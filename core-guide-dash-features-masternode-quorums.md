Dash's <<glossary:masternode>> quorums are used to facilitate the operation of masternode provided features in a decentralized, deterministic way. The original quorums (used largely for <<glossary:InstantSend>> and masternode payments) were ephemeral and used for a single purpose (e.g. voting on one specific InstantSend transaction).

Dash Core 0.14 (protocol version 70214) introduced the <<glossary:Long-Living Masternode Quorum>>  (LLMQ) system described in detail by [DIP6](https://github.com/dashpay/dips/blob/master/dip-0006.md). These LLMQs are deterministic subsets of the global deterministic masternode list that are formed via a distributed key generation (DKG) protocol and remain active for a long periods of time (e.g. hours to days).

The main task of LLMQs is to perform threshold signing of consensus-related messages (e.g. <<glossary:ChainLocks>>).

# LLMQ Creation (DKG)

The following table details the data flow of P2P messages exchanged during the distributed key generation (DKG) protocol used to establish an LLMQ.
[block:callout]
{
  "type": "info",
  "body": "Note: With the exception of the final step (`qfcommit` message broadcast), the message exchanges happen only between masternodes participating in the DKG process via the [Intra-Quorum communication process](https://github.com/dashpay/dips/blob/master/dip-0006.md#intra-quorum-communication) described in the DIP.",
  "title": "Intra-Quorum Communication"
}
[/block]
*Quorum DKG Data Flow*

| **Masternode** | **Direction**  | **Peers**   | **Description** |
| --- | :---: | --- | --- |
| **[Initialization Phase](https://github.com/dashpay/dips/blob/master/dip-0006.md#1-initialization-phase)**| | | **Deterministically evaluate if quorum participation necessary** |
| | | | Each quorum participant establishes connections to a set of quorum participants [as described in DIP6](https://github.com/dashpay/dips/blob/master/dip-0006.md#building-the-set-of-deterministic-connections) |
| **[Contribution Phase](https://github.com/dashpay/dips/blob/master/dip-0006.md#2-contribution-phase)** | | | **Send quorum contributions (intra-quorum communication)** |
|`inv` message (qcontrib)                        | → |                              | Masternode sends inventory for its quorum contribution _to other masternodes in the quorum_
|                                                | ← | [`getdata` message](core-ref-p2p-network-data-messages#section-getdata) (qcontrib) | Peer(s) respond with request for quorum contribution
| [`qcontrib` message](core-ref-p2p-network-quorum-messages#section-qcontrib)                             | → |                              | Masternode sends the requested quorum contribution
| **[Complaining Phase](https://github.com/dashpay/dips/blob/master/dip-0006.md#3-complaining-phase)** | | | **Send complaints for any peers with invalid or missing contributions (intra-quorum communication)** |
|`inv` message (qcomplaint)                      | → |                              | Masternode sends inventory for any complaints _to other masternodes in the quorum_
|                                                | ← | [`getdata` message](core-ref-p2p-network-data-messages#section-getdata) (qcomplaint) | Peer(s) respond with request for quorum complaints
| [`qcomplaint` message](core-ref-p2p-network-quorum-messages#section-qcomplaint)                           | → |                              | Masternode sends the requested complaints
| **[Justification Phase](https://github.com/dashpay/dips/blob/master/dip-0006.md#4-justification-phase)** | | | **Send justification responses for any complaints against own contributions (intra-quorum communication)** |
|`inv` message (qjustify)                        | → |                              | Masternode sends inventory for any justifications _to other masternodes in the quorum_
|                                                | ← | [`getdata` message](core-ref-p2p-network-data-messages#section-getdata) (qjustify) | Peer(s) respond with request for quorum justifications
| [`qjustify` message](core-ref-p2p-network-quorum-messages#section-qjustify)                             | → |                              | Masternode sends the requested justifications
| **[Commitment Phase](https://github.com/dashpay/dips/blob/master/dip-0006.md#5-commitment-phase)** | | | **Send premature commitment containing the quorum public key (intra-quorum communication)** |
|`inv` message (qpcommit)                        | → |                              | Masternode sends inventory for its premature commitment _to other masternodes in the quorum_
|                                                | ← | [`getdata` message](core-ref-p2p-network-data-messages#section-getdata) (qpcommit) | Peer(s) respond with request for quorum premature commitment
| [`qpcommit` message](core-ref-p2p-network-quorum-messages#section-qpcommit)                             | → |                              | Masternode sends the requested premature commitment
| **[Finalization Phase](https://github.com/dashpay/dips/blob/master/dip-0006.md#6-finalization-phase)** | | | **Send final commitment containing the quorum public key** |
|`inv` message (qfcommit)                        | → |                              | Masternode sends inventory for its premature commitment **to all peers**
|                                                | ← | [`getdata` message](core-ref-p2p-network-data-messages#section-getdata) (qfcommit) | Peer(s) respond with request for quorum final commitment
| [`qfcommit` message](core-ref-p2p-network-quorum-messages#section-qfcommit)                             | → |                              | Masternode sends the requested final commitment

# LLMQ Signing Session

The following table details the data flow of P2P messages exchanged during an LLMQ signing session. These sessions take advantage of BLS threshold signatures to enable quorums to sign arbitrary messages. For example, Dash Core 0.14 uses this capability to create the quorum signature found in the [`clsig` message](core-ref-p2p-network-instantsend-messages#section-clsig) that enables <<glossary:ChainLocks>>.

Please read [DIP7 LLMQ Signing Requests / Sessions](https://github.com/dashpay/dips/blob/master/dip-0007.md) for additional details.

*LLMQ Signing Session Data Flow*

| **Masternode** | **Direction**  | **Peers**   | **Description** |
| --- | :---: | --- | --- |
| **[Siging Request Phase](https://github.com/dashpay/dips/blob/master/dip-0007.md#signing-request)** | | | Request quorum signing of a message (e.g. InstantSend transaction input) (intra-quorum communication) |
| [`qsigsesann` message](core-ref-p2p-network-quorum-messages#section-qsigsesann)                             | → |                              | Masternode sends a signing session announcement _to other masternodes in the quorum_
| **[Share Propagation Phase](https://github.com/dashpay/dips/blob/master/dip-0007.md#propagating-signature-shares)** | | | Members exchange signature shares within the quorum (intra-quorum communication) |
| [`qsigsinv` message](core-ref-p2p-network-quorum-messages#section-qsigsinv)                             | → |                              | Masternode sends one or more quorum signature share inventories _to other masternodes in the quorum_<br>_May occur multiple times in this phase_
|                                                | ← | [`qgetsigs` message](core-ref-p2p-network-quorum-messages#section-qgetsigs) (qcontrib) | Peer(s) respond with request for signature shares<br>_May occur multiple times in this phase_
| [`qbsigs` message](core-ref-p2p-network-quorum-messages#section-qbsigs)                             | → |                              | Masternode sends the requested batched signature share(s)<br>_May occur multiple times in this phase_
| **[Threshold Signature Recovery Phase](https://github.com/dashpay/dips/blob/master/dip-0007.md#recovered-threshold-signatures)** | | | A recovered signature is created by a quorum member once valid signature shares from at least the threshold number of members have been received |
| [`qsigrec` message](core-ref-p2p-network-quorum-messages#section-qsigrec)                             | → |                              | Masternode sends the quorum recovered signature **to all peers** (except those that have asked to be excluded via a [`qsendrecsigs` message](core-ref-p2p-network-quorum-messages#section-qsendrecsigs))

Note the following timeouts defined by Dash Core related to signing sessions:

| Parameter | Timeout, sec | Description |
| --- | --- | --- |
| `SESSION_NEW_SHARES_TIMEOUT` | 60 | Time to wait for new shares |
| `SIG_SHARE_REQUEST_TIMEOUT` | 5 | Time to wait for a requested share before requesting from a different node |
| `SESSION_TOTAL_TIMEOUT` | 300 | Time to wait for session to complete |

# Quorum Selection

| Quorum Type | Members | Consensus | Description |
| ----------- | ------- | --------- | ----------- |
| Classic<br>(non-LLMQ) InstantSend | 10      | Majority  | A set of 10 masternodes are selected for _each_ input of the InstantSend transaction. A majority (6+) of them must agree to lock the input. If all inputs in the transaction can be locked, it becomes a successful InstantSend.
| MN Payments | 10      | Majority | A set of 10 masternodes are selected for each block. A majority (6+) of them must agree on the masternode payee for the next block.
| MN Broadcast | 10      | Majority | _Deprecated by DIP3 (deterministic masternode list) in Dash Core 0.13._<br><br>If a majority (6+) of nodes agree, a new `mnb` message is not required.