[block:callout]
{
  "type": "danger",
  "body": "**The following information is deprecated and for historical reference only. It describes features that have been redesigned and no longer operate as described below.**",
  "title": "Deprecated Content"
}
[/block]
# InstantSend (original)
[block:callout]
{
  "type": "warning",
  "body": "**Please see [here for details of the current InstantSend design](core-guide-dash-features-instantsend).**"
}
[/block]
Dash Core's InstantSend feature provides a way to lock transaction inputs and enable secure, instantaneous transactions. Since Dash Core 0.13.0, any qualifying transaction is automatically upgraded to InstantSend by the network without a need for the sending wallet to explicitly request it. For these simple transactions (those containing 4 or fewer inputs), the previous requirement for a special InstantSend transaction fee was also removed. The criteria for determining eligibility can be found in the lists of limitations below.

The following video provides an overview with a good introduction to the details including the InstantSend vulnerability that was fixed in Dash Core 0.12.2. Some specific points in the video are listed here for quick reference:

 - 2:00 - InstantSend restrictions
 - 5:00 - Masternode quorum creation
 - 6:00 - Input locking
 - 7:45 - Quorum score calculation / Requirement for block confirmations
 - 9:00 - Description of Dash Core pre-0.12.2 InstantSend vulnerability
 - 13:00 - Description of vulnerability fix / Post Dash Core 0.12.2 operation

[block:embed]
{
  "html": false,
  "url": "https://www.youtube-nocookie.com/embed/n4PELomRiFY?rel=0;start=0",
  "title": "Matt Robertson - Instant Send - The Dash Conference - YouTube",
  "favicon": "https://www.youtube-nocookie.com/favicon.ico",
  "iframe": true,
  "width": "",
  "height": ""
}
[/block]
*InstantSend Data Flow*

| **InstantSend Client** | **Direction**  | **Peers**   | **Description** |
| --- | :---: | --- | --- |
| [`inv` message](core-ref-p2p-network-data-messages#section-inv) (ix)          | → |                         | Client sends inventory for transaction lock request
|                             | ← | [`getdata` message](core-ref-p2p-network-data-messages#section-getdata) (ix)  | Peer responds with request for transaction lock request
| `ix` message                | → |                         | Client sends InstantSend transaction lock request
|                             | ← | [`inv` message](core-ref-p2p-network-data-messages#section-inv) (txlvote) | Masternodes in the [quorum](core-guide-dash-features-masternode-quorums#section-quorum-selection) respond with votes
| [`getdata` message](core-ref-p2p-network-data-messages#section-getdata) (txlvote) | → |                         | Client requests vote
|                             | ← | `txlvote` message       | Peer responds with vote

Once an InstantSend lock has been requested, the `instantsend` field of various RPCs (e.g. the [`getmempoolentry` RPC](core-api-ref-remote-procedure-calls-blockchain#section-getmempoolentry)) is set to `true`. Then, if a sufficient number of votes approve the transaction lock, the InstantSend transaction is approved the `instantlock` field of the RPC is set to `true`.

If an InstantSend transaction is a valid transaction but does not receive a transaction lock, it reverts to being a standard transaction.

There are a number of limitations on InstantSend transactions:

* The lock request will timeout 15 seconds after the first vote is seen (`INSTANTSEND_LOCK_TIMEOUT_SECONDS`)
* The lock request will fail if it has not been locked after 60 seconds (`INSTANTSEND_FAILED_TIMEOUT_SECONDS`)
* A “per-input” fee of 0.0001 DASH per input is required for non-simple transactions (inputs >=5) since they place a higher load on the masternodes. This fee was most recently decreased by [DIP-0001](https://github.com/dashpay/dips/blob/master/dip-0001.md).
* To be used in an InstantSend transaction, an input must have at least the number confirmations (block depth) indicated by the table below

| **Network** | **Confirmations Required** |
| --- | --- |
| Mainnet | 6 Blocks |
| Testnet | 2 Blocks |
| Regtest | 2 Blocks |
| Devnet  | 2 Blocks |

There are some further limitations on Automatic InstantSend transactions:

* DIP3 must be active
* Spork 16 must be enabled
* Mempool usage must be lower than 10% (`AUTO_IX_MEMPOOL_THRESHOLD`). As of Dash Core 0.13.0, this corresponds to a mempool size of 30 MB (`DEFAULT_MAX_MEMPOOL_SIZE` = 300 MB).

**Historical Note**

Prior to Dash Core 0.13.0, `instantsend` and `instantlock` values were not available via RPC. At that time, the InstantSend system worked as described below.

Once a sufficient number of votes approved the transaction lock, the InstantSend transaction was approved and showed 5 confirmations (`DEFAULT_INSTANTSEND_DEPTH`).

NOTE: The 5 "pseudo-confirmations" were shown to convey confidence that the transaction was secure from double-spending and DID NOT indicate the transaction had already been confirmed to a block depth of 5 in the blockchain.

# Masternode Payment (original)
[block:callout]
{
  "type": "warning",
  "body": "**The following information is for historical reference only. It describes the masternode payment process that was used prior to the deterministic masternode list update in Dash Core v0.13 that implemented DIP3.**\n\nPlease see [here for details of the current system](core-guide-dash-features-masternode-payment)"
}
[/block]

Prior to DIP3, the masternode payment process operated as described below.

Masternode payment uses a verifiable process to determine which masternode is paid in each block. When a new block is processed, a quorum of `MNPAYMENTS_SIGNATURES_TOTAL` (10) masternodes vote on the next masternode payee. The quorum is calculated deterministically based on the distance between masternode's hash and the block's proof of work.

Each member of the quorum issues a 'mnw' message that is relayed to the network. The payee is selected from a subset of masternodes made up of 10% of eligible nodes that have been waiting the longest since their last payment. The winner is then determined based on a number of parameters including the distance between the its hash and the block's proof of work. For additional detail, reference this [Official Documentation Payment Logic page](https://docs.dash.org/en/0.12.3/masternodes/understanding.html#payment-logic).

Nodes receiving a `mnw` message verify the validity of the message before relaying it to their peers. If the message is invalid, the sending node may be treated as misbehaving and have its ban score increased.

# Masternode Sync (original)
[block:callout]
{
  "type": "warning",
  "body": "**The following information is for historical reference only. It describes the masternode sync process that was used prior to the deterministic masternode list update in Dash Core v0.13 that implemented DIP3.**\n\nPlease see [here for details of the current system](core-guide-dash-features-masternode-sync)"
}
[/block]
## Initial Sync

This diagram shows the order in which P2P messages are sent to perform masternode synchronization initially after startup.

![Masternode Sync (Initial)](https://dash-docs.github.io/img/dev/en-masternode-sync-initial.svg)

The following table details the data flow of P2P messages exchanged during initial masternode synchronization before the activation of DIP3 and Spork 15.

| **Syncing Node Message** | **Direction**  | **Masternode Response**   | **Description** |
| --- | :---: | --- | --- |
| **1. Sporks** |   |  |  |
| [`getsporks` message](core-ref-p2p-network-control-messages#section-getsporks)                            | → |                           | Syncing node requests sporks
|                                                | ← | [`spork` message](core-ref-p2p-network-control-messages#section-spork)(s)        |
| **2. Masternode List** |   |  | Sync Masternode list from other connected clients |
| `dseg` message                                 | → |                           | Syncing node requests masternode list
|                                                | ← | [`ssc` message](core-ref-p2p-network-masternode-messages#section-ssc) | Number of entries in masternode list (MASTERNODE_SYNC_LIST)<br><br>Only sent if requesting entire list
|                                                | ← | [`inv` message](core-ref-p2p-network-data-messages#section-inv)(s) (mnb)         | MSG_MASTERNODE_ANNOUNCE
|                                                | ← | [`inv` message](core-ref-p2p-network-data-messages#section-inv)(s) (mnp)         | MSG_MASTERNODE_PING
| [`getdata` message](core-ref-p2p-network-data-messages#section-getdata)(s) (mnb) | → |                           | (Optional)
| [`getdata` message](core-ref-p2p-network-data-messages#section-getdata)(s) (mnp)     | → |                           | (Optional)
|                                                | ← | `mnb` message(s)          | (If requested) Masternode announce message
|                                                | ← | `mnp` message(s)          | (If requested) Masternode ping message
| **3. Masternode payments** |   |  | Ask node for all payment votes it has (new nodes will only return votes for future payments) |
| `mnget` message                                | → |                           | Syncing node requests masternode payment sync
|                                                | ← | [`ssc` message](core-ref-p2p-network-masternode-messages#section-ssc) | Number of entries in masternode payment list
|                                                | ← | [`inv` message](core-ref-p2p-network-data-messages#section-inv)(s) (mnw)         | MSG_MASTERNODE_PAYMENT_VOTE
| [`getdata` message](core-ref-p2p-network-data-messages#section-getdata)(s) (mnw) | → |                           | (Optional)
|                                                | ← | `mnw` message(s)          | (If requested) Masternode payment vote message
| **4. Governance** |   |  | See [Governance sync](core-guide-dash-features-governance) |

## Ongoing Sync

Once a masternode completes an initial full sync, continuing synchronization is maintained by the exchange of P2P messages with other nodes. This diagram shows an overview of the messages exchanged to keep the masternode list, masternode payments, and governance objects synchronized between masternodes.

![Masternode Sync (Ongoing)](https://dash-docs.github.io/img/dev/en-masternode-sync-ongoing.svg)

**Recurring Ping**
[block:callout]
{
  "type": "warning",
  "body": "Note: Deprecated following activation of DIP3."
}
[/block]
Each masternode issues a ping (`mnp` message) periodically to notify the network that it is still online. Masternodes that do not issue a ping for 3 hours will be put into the `MASTERNODE_NEW_START_REQUIRED` state and will need to issue a masternode announce (`mnb` message).

**Masternode List**
[block:callout]
{
  "type": "warning",
  "body": "Note: Deprecated following activation of DIP3."
}
[/block]
After the initial masternode list has been received, it is kept current by a combination of the periodic `mnp` messages received from other masternodes, the `mnb` messages sent by masternodes as they come online, and `mnv` messages to verify that other masternodes are valid.

Also, `dseg` messages can be sent to request masternode info when messages are received that have been signed by an unrecognized masternode (most masternode/governance messages include a `vin` value that can be used to verify the masternode's unspent 1000 Dash).

Unsynchronized peers may send a `dseg` message to request the entire masternode list.

**Masternode Payment**
[block:callout]
{
  "type": "warning",
  "body": "Note: Deprecated following activation of DIP3."
}
[/block]
After the initial masternode payment synchronization, payment information is kept current via the `mnw` messages relayed on the network. Unsynchronized peers may send a `mnget` message to request masternode payment sync.

## Sync Schedule

Prior to the deterministic masternode system introduced by DIP3 in Dash Core 0.13, the following additional sync actions were also required.

| **Period (seconds)** | **Action** | **Description** |
| --- | --- | --- |
| 1   | MN Check                  | ![Warning icon](https://dash-docs.github.io/img/icons/icon_warning.svg) _Deprecated following activation of DIP3 and Spork 15_<br><br>Check the state of each masternode that is still funded and not banned. The action occurs once per second, but individual masternodes are only checked at most every 5 seconds (only a subset of masternodes are checked each time it runs) (masternodeman.cpp) |
| 60  | MN Check/Remove           | ![Warning icon](https://dash-docs.github.io/img/icons/icon_warning.svg) _Deprecated following activation of DIP3 and Spork 15_<br><br>Remove spent masternodes and check the state of inactive ones (masternodeman.cpp) |
| 60  | MN Payment Check/Remove   | ![Warning icon](https://dash-docs.github.io/img/icons/icon_warning.svg) _Deprecated following activation of DIP3 and Spork 15_<br><br>Remove old masternode payment votes/blocks (masternode-payments.cpp) |
| 300 | Full verification         | ![Warning icon](https://dash-docs.github.io/img/icons/icon_warning.svg) _Deprecated following activation of DIP3 and Spork 15_<br><br>Verify masternodes via direct requests (`mnv` messages - note time constraints in the Developer Reference section) (masternodeman.cpp) |
| 600 | Manage State              | ![Warning icon](https://dash-docs.github.io/img/icons/icon_warning.svg) _Deprecated following activation of DIP3 and Spork 15_<br><br>Sends masternode pings (`mnp` message). Also sends initial masternode broadcast (`mnb` message) for local masternodes. (activemasternode.cpp) |