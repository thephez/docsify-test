Take note that for both types of broadcasting, mechanisms are in place to punish misbehaving <<glossary:peers>> who take up bandwidth and computing resources by sending false information. If a peer gets a banscore above the `-banscore=<n>` threshold (100 by default), they will be banned for the number of seconds defined by `-bantime=<n>`, which is 86,400 by default (24 hours).

| Type | Misbehavior | Ban Score | Description |
| ---- | ----------- | --------- | ----------- |
| Net | GetBlockTxn Index Error | **100** | Peer relayed a [`getblocktxn` message](core-ref-p2p-network-data-messages#section-getblocktxn) with out-of-bound indices
| Net | Bloom Filter Service | **100** | Bloom filter message received from peer that has bloom filter commands disabled by default (protocol version > 70201) (`filterload` message, [`filteradd` message](core-ref-p2p-network-control-messages#section-filteradd), or [`filterclear` message](core-ref-p2p-network-control-messages#section-filterclear))
| Net | Block Rejected | 1 | Peer rejected the block it requested from us
| Net | Duplicate Version | 1 | Duplicate [`version` message](core-ref-p2p-network-control-messages#section-version) received
| Net | Wrong Devnet | **100** | Peer responded with the wrong devnet version (`version` message)
| Net | Wrong Devnet | 1 | Peer connected using the wrong devnet version (`version` message)
| Net | No Version | 1 | Received a message prior to receiving a [`version` message](core-ref-p2p-network-control-messages#section-version)
| Net | No Verack | 1 | After sending [`version` message](core-ref-p2p-network-control-messages#section-version), received a message other than a [`verack` message](core-ref-p2p-network-control-messages#section-verack) back first
| Net | Address List Size | 20 | More than 1000 addresses received (`addr` message)
| Net | Inventory List | 20 | More than `MAX_INV_SZ` (50000) inventories received (`inv` message)
| Net | Get Data Size | 20 | More than `MAX_INV_SZ` (50000) inventories requested (`getdata` message)
| Net | Orphan Transaction | **Varies** | Peer relayed an invalid orphan transaction. Ban score varies from 0-100 based on the specific reason (values set by `AcceptToMemoryPoolWorker()`)
| Net | Bad Transaction | **Varies** | Transaction rejected from the mempool
| Net | Invalid Header | **Varies** | Invalid block header received from peer (`cmpctblock` message)
| Net | Invalid CompactBlock | **100** | Invalid compact block /non-matching block transactions received from peer (`cmpctblock` message)
| Net | Header List Size | 20 | More than `MAX_HEADERS_RESULTS` (2000) headers received (`headers` message)
| Net | Header List Sequence | 20 | Non-continous headers sequence received (`headers` message)
| Net | Invalid Block | **Varies** | Invalid block header received from peer
| Net | Bloom Filter Size | **100** | Maximum script element size (520) exceeded (`filterload` message or [`filteradd` message](core-ref-p2p-network-control-messages#section-filteradd))
| Net | MN List Diff | 1 | Failed to get masternode list diff (`getmnlistd` message)
| Net | Unrequested MN List Diff | **100** | Peer provided an unrequested masternode list diff (`mnlistdiff` message)
| InstantSend | Invalid Lock Message | **100** | Invalid TXID or inputs in lock message (`islock` message)
| InstantSend | Verify Error | 20 | Peer relayed a message that failed to verify
| LLMQ ChainLock | Invalid | 10 | Invalid ChainLock message (`clsig` message)
| LLMQ Commitment | Null QcTx | **100** | Peer relayed a block with a null commitment
| LLMQ Commitment | Invalid LLMQ Type | **100** | Peer relayed a block containing an invalid LLMQ Type
| LLMQ Commitment | Invalid Height | **100** | Peer relayed a block that is not the first block in the DKG interval
| LLMQ Commitment | Invalid Commitment | **100** | Peer relayed a block with an invalid quorum commitment
| LLMQ DKG | Empty Message | **100** | Peer relayed a message with no payload
| LLMQ DKG | Invalid LLMQ Type | **100** | Peer relayed a message for an incorrect LLMQ Type
| LLMQ DKG | Invalid Message | **100** | Peer relayed a message that could not be deserialized
| LLMQ DKG | Preverify Failed | **100** | Peer relayed a message that could not be pre-verified
| LLMQ DKG | Signature  | **100** | Peer relayed a message with an invalid signature
| LLMQ DKG | Full Verify Failed | **100** | Peer relayed a message that failed full verification
| LLMQ Signing | Too Many Messages | **100** | Maximum message count exceed in [`qsigsesann` message](core-ref-p2p-network-quorum-messages#section-qsigsesann), [`qsigsinv` message](core-ref-p2p-network-quorum-messages#section-qsigsinv), [`qgetsigs` message](core-ref-p2p-network-quorum-messages#section-qgetsigs), or [`qbsigs` message](core-ref-p2p-network-quorum-messages#section-qbsigs)
| LLMQ Signing | Signature  | **100** | Peer relayed a message with an invalid recovered signature or signature share
| Masternode Authentication | Duplicate Message | **100** | Only 1 message allowed (`mnauth` message)
| Masternode Authentication | Signature | **100** | Peer relayed a message with an invalid signature (`mnauth` message)
| Masternode Authentication | Invalid MN | 10 | Peer not in the valid masternode list (`mnauth` message)
| Governance | Sync | 20 | Requesting a governance sync too frequently (`govsync` message with empty hash)
| Governance | Invalid Object | 20 | Peer relayed an invalid governance object (`govobj` message)
| Governance | Invalid Vote | 20 | Peer relayed an invalid/invalid old vote(`govobjvote` message)
| Governance | Unsupported Vote Signal | 20 | Vote signal outside the accepted range (see [`govobjvote` message](core-ref-p2p-network-governance-messages#section-govobjvote))
| PrivateSend | Signature  | 10 | Peer relayed a message with an invalid signature (`dsq` message)
| Spork | Invalid Time | **100** | Peer relayed a spork with a timestamp too far in the future (`spork` message)
| Spork | Signature  | **100** | Peer relayed a spork with an invalid signature (`spork` message)