Dash Core's <<glossary:PrivateSend>> feature provides a way to improve privacy by performing coin-mixing without relinquishing custodial access. For additional details, reference this [Official Documentation PrivateSend page](https://docs.dash.org/en/stable/introduction/features.html#privatesend).

The following video provides an overview with a good introduction to the details:


[block:embed]
{
  "html": false,
  "url": "https://www.youtube-nocookie.com/embed/vgCId3wJc5Y?rel=0",
  "title": "How Dash's 'PrivateSend' Works Under the Hood - YouTube",
  "favicon": "https://www.youtube-nocookie.com/favicon.ico",
  "iframe": true,
  "width": "100%",
  "height": "350px"
}
[/block]
# PrivateSend Wallet Preparation

The <<glossary:wallet>> completes two operations in this phase:

1. Split value into inputs matching the PrivateSend <<glossary:denominations>> by sending <<glossary:transactions>> to itself

2. Split value into <<glossary:inputs>> to use for collateral by sending transactions to itself

**Note**: Both these operations incur the standard <<glossary:transaction fee>> like any other transaction

**Creating Denominations**

The PrivateSend denominations include a bit mapping to easily differentiate them. The [`dsa` message](core-ref-p2p-network-privatesend-messages#section-dsa) and [`dsq` message](core-ref-p2p-network-privatesend-messages#section-dsq) use this bit shifted integer instead of sending the actual denomination. The table below lists the bit, its associated integer value used in P2P messages, and the actual Dash value.

| **Bit** | **Denom. (Integer)** | **Denomination (DASH)** |
| --- | --- | --- |
| 0   |  1 | 10.0001              |
| 1   |  2 | 01.00001             |
| 2   |  4 | 00.100001            |
| 3   |  8 | 00.0100001           |
| 4   | 16 | 00.00100001          |

Protocol version 70213 added a 5th denomination (0.001 DASH).

[Example Testnet denomination creation transaction](https://testnet-insight.dashevo.org/insight/tx/f0174fc87d68a18617c2990df4d9455c0459c601d2d6473934357a66f9b8b70a)

**Creating Collaterals**

PrivateSend collaterals are used to pay mixing fees, but are kept separate from the denominations to maximize privacy. Since protocol version 70213, the minimum collateral fee is 1/10 of the smallest denomination for all mixing sessions regardless of denomination. In Dash Core, collaterals are created with enough value to pay 4 collateral fees (4 x 0.001 DASH). ([Dash Core Reference](https://github.com/dashpay/dash/blob/262454791c4b4f783b2533d1b16b757a71eb5f7d/src/privatesend.h#L413))

In protocol version 70208, collateral inputs can be anything from 2x the minimum collateral amount to the maximum collateral amount (currently defined as 4x the minimum collateral). In protocol versions > 70208, Dash Core can use any <<glossary:input>> from 1x the minimum collateral amount to the maximum collateral amount.

[Example Testnet collateral creation transaction](https://testnet-insight.dashevo.org/insight/tx/8f9b15973983876f7ce4eb2c32b09690dfb0432d2caf6c6df516196a8d17689f)

[Example Testnet collateral payment transaction](https://testnet-insight.dashevo.org/insight/tx/de51e6f7c5ef75aad0dbb0a808ef4873d7ef6d67b25f3a658d5a241db4f3eeeb)

# PrivateSend Mixing

The mixing phase involves exchanging a sequence of messages with a <<glossary:masternode>> so it can construct a mixing transaction with inputs from the clients in its mixing pool.

*PrivateSend Data Flow*

|   | **Clients** | **Direction**  | **Masternode**   | **Description** |
| --- | --- | :---: | --- | --- |
| 0 | | | | Client determines whether to join an existing mixing pool or create a new one |
| 1 | [`dsa` message](core-ref-p2p-network-privatesend-messages#section-dsa)                            | → |                            | Client asks to join mixing pool or have the masternode create a new one
| 2 |                                                | ← | [`dssu` message](core-ref-p2p-network-privatesend-messages#section-dssu)       | Masternode provides a mixing pool status update (Typical - State: `POOL_STATE_QUEUE`, Message: `MSG_NOERR`)
| 3 |                                                | ← | [`dsq` message](core-ref-p2p-network-privatesend-messages#section-dsq)        | Masternode notifies clients when it is ready to mix
| 4 | [`dsi` message](core-ref-p2p-network-privatesend-messages#section-dsi)                                 | → |                       | Upon receiving a [`dsq` message](core-ref-p2p-network-privatesend-messages#section-dsq) with the Ready bit set, clients each provide a list of their inputs (unsigned) to be mixed, collateral, and a list of outputs where mixed funds should be sent
| 5 |                                                | ← | [`dssu` message](core-ref-p2p-network-privatesend-messages#section-dssu)       | Masternode provides a mixing pool status update (typical - State: `POOL_STATE_ACCEPTING_ENTRIES`, Message: `MSG_ENTRIES_ADDED`)
| 6 |                                                | ← | [`dsf` message](core-ref-p2p-network-privatesend-messages#section-dsf)        | Masternode sends the final transaction containing all clients inputs (unsigned) and all client outputs to each client for verification
| 7 |                                                | ← | [`dssu` message](core-ref-p2p-network-privatesend-messages#section-dssu)       | Masternode provides a mixing pool status update (Typical - State: `POOL_STATE_SIGNING`, Message: `MSG_NOERR`)
| 8 | [`dss` message](core-ref-p2p-network-privatesend-messages#section-dss)                                 | → |                       | After verifying the final transaction, clients each sign their own inputs and send them back
| 9 |                                                | ← | [`dsc` message](core-ref-p2p-network-privatesend-messages#section-dsc)        | Masternode verifies the signed inputs, creates a [`dstx` message](core-ref-p2p-network-privatesend-messages#section-dstx) to broadcast the transaction, and notifies clients that the mixing transaction is complete (Typical - Message: `MSG_SUCCESS`)
| 10 |                                                | ← | [`inv` message](core-ref-p2p-network-data-messages#section-inv)        | Masternode broadcasts a `dstx` inventory message
| 11 | [`getdata` message](core-ref-p2p-network-data-messages#section-getdata) (dstx)                                 | → |            | (Optional)

**Additional notes**

_**Step 0 - Pool Selection**_
  * Existing mixing pool information is derived from the Queue messages seen by the client
  * Dash Core attempts to join an existing mixing pool and only requests creation of a new one if that fails, although this is not a requirement that alternative implementations would be required to follow

_**Step 1 - Pool Request**_

  * The [`dsa` message](core-ref-p2p-network-privatesend-messages#section-dsa) contains a collateral transaction
    * This transaction uses a collateral <<glossary:input>> created in the [Wallet Preparation](#privatesend-wallet-preparation) phase
    * The collateral is a signed <<glossary:transaction>> that pays the collateral back to a client <<glossary:address>> minus a fee of 0.001 DASH

_**Step 3 - Queue**_

  * A masternode broadcasts [`dsq` messages](core-ref-p2p-network-privatesend-messages#section-dsq) when it starts a new queue. These message are relayed by all <<glossary:peers>>.
  * As of protocol version 70214, mixing sessions have a variable number of participants defined by the range `nPoolMinParticipants` (3) to `nPoolMaxParticipants` (5). Prior protocol version mixing sessions always contained exactly 3 participants
  * Once the masternode has received valid [`dsa` messages](core-ref-p2p-network-privatesend-messages#section-dsa) from the appropriate number of clients (`nSessionMaxParticipants`), it sends a [`dsq` message](core-ref-p2p-network-privatesend-messages#section-dsq) with the ready bit set
    * Clients must respond to the Queue ready within 30 seconds or risk forfeiting the collateral they provided in the [`dsa` message](core-ref-p2p-network-privatesend-messages#section-dsa) (Step 1) ([Dash Core Reference](https://github.com/dashpay/dash/blob/e9f7142ed01c0d7b53ef8b5f6f3f6375a68ef422/src/privatesend.h#L23))

_**Step 4 - Inputs**_

  * The collateral transaction can be the same in the [`dsi` message](core-ref-p2p-network-privatesend-messages#section-dsi) as the one in the [`dsa` message](core-ref-p2p-network-privatesend-messages#section-dsa) (Step 1) as long as it has not been spent
  * Each client can provide up to 9 (`PRIVATESEND_ENTRY_MAX_SIZE`) inputs (and an equal number of outputs) to be mixed ([Dash Core Reference](https://github.com/dashpay/dash/blob/e9f7142ed01c0d7b53ef8b5f6f3f6375a68ef422/src/privatesend.h#L29))
  * This is the only message in the PrivateSend process that contains enough information to link a wallet's PrivateSend inputs with its outputs
    * This message is sent directly between a client and the mixing masternode (not relayed across the Dash network) so no other clients see it

_**Step 6 - Final Transaction (unsigned)**_

  * Once the masternode has received valid [`dsi` messages](core-ref-p2p-network-privatesend-messages#section-dsi) from all clients, it creates the final transaction and sends a [`dsf` message](core-ref-p2p-network-privatesend-messages#section-dsf)
    * Inputs/outputs are ordered deterministically as defined by [BIP-69](https://github.com/dashevo/bips/blob/master/bip-0069.mediawiki#Abstract) to avoid leaking any data ([Dash Core Reference](https://github.com/dashpay/dash/blob/e596762ca22d703a79c6880a9d3edb1c7c972fd3/src/privatesend-server.cpp#L321-#L322))
    * Clients must sign their inputs to the Final Transaction within 15 seconds or risk forfeiting the collateral they provided in the [`dsi` message](core-ref-p2p-network-privatesend-messages#section-dsi) (Step 4) ([Dash Core Reference](https://github.com/dashpay/dash/blob/e9f7142ed01c0d7b53ef8b5f6f3f6375a68ef422/src/privatesend.h#L24))

_**Step 10 - Final Transaction broadcast**_

  * Prior to protocol version 70213, masternodes could only send a single un-mined [`dstx` message](core-ref-p2p-network-privatesend-messages#section-dstx) at a time. As of protocol version 70213, up to 5 (`MASTERNODE_MAX_MIXING_TXES`) un-mined [`dstx` messages](core-ref-p2p-network-privatesend-messages#section-dstx) per masternode are allowed.

_**General**_

  With the exception of the [`dsq` message](core-ref-p2p-network-privatesend-messages#section-dsq) and the [`dstx` message](core-ref-p2p-network-privatesend-messages#section-dstx) (which need to be public and do not expose any private information), all PrivateSend P2P messages are sent directly between the mixing masternode and relevant client(s).

# PrivateSend Fees

**Mixing Fees**

* If mixing completes successfully, Dash Core charges the collateral randomly in 1/10 mixing transactions to pay miners ([Dash Core Reference](https://github.com/dashpay/dash/blob/e596762ca22d703a79c6880a9d3edb1c7c972fd3/src/privatesend-server.cpp#L458-#L478))
* Clients that abuse the mixing system by failing to respond to [`dsq` messages](core-ref-p2p-network-privatesend-messages#section-dsq) or [`dsf` messages](core-ref-p2p-network-privatesend-messages#section-dsf) within the timeout periods may forfeit their collateral. Dash Core charges the abuse fee in 2/3 cases ([Dash Core Reference](https://github.com/dashpay/dash/blob/e596762ca22d703a79c6880a9d3edb1c7c972fd3/src/privatesend-server.cpp#L397-#L398))

**Sending Fees**

To maintain privacy when using PrivateSend, PrivateSend transactions must fully spend all inputs to a single output (with the remainder becoming the fee - i.e. no <<glossary:change output>>). This can result in large fees depending on the value being sent.

For example, an extreme case is sending the minimum non-dust value (546 duffs) via PrivateSend. This results in an extremely large transaction fee because the minimum PrivateSend denomination (0.0100001 DASH or 1,000,010 duffs) must be fully spent with no change. This results in a fee of 0.00999464 DASH and a sent value of only 0.00000546 DASH as shown by the calculation below.

1000010 duffs (smallest PrivateSend denomination) - 546 duffs (value to send) = 999464 duffs (fee)

[Example Testnet PrivateSend transaction spending 546 duffs](https://testnet-insight.dashevo.org/insight/address/yWWNYVEQ84RM1xXJekj62wJPF3h1TKh9fS)