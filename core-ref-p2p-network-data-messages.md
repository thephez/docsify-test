The following network messages all request or provide data related to transactions and blocks.

![Overview Of P2P Protocol Data Request And Reply Messages](https://dash-docs.github.io/img/dev/en-p2p-data-messages.svg)

Many of the data messages use <<glossary:inventories>> as unique identifiers for <<glossary:transactions>> and <<glossary:blocks>>.  Inventories have a simple 36-byte structure:

| Bytes | Name            | Data Type | Description
|-------|-----------------|-----------|-------------
| 4     | type identifier | uint32_t  | The type of object which was hashed.  See list of type identifiers below.
| 32    | hash            | char[32]  | SHA256(SHA256()) hash of the object in internal byte order.

The currently-available type identifiers are:

| Type Identifier | Name                                                                          | Description
|-----------------|-------------------------------------------------------------------------------|---------------
| 1               | `<<glossary:MSG_TX>>`                                     | The hash is a TXID.
| 2               | `<<glossary:MSG_BLOCK>>`                            | The hash is of a block header.
| 3               | `<<glossary:MSG_FILTERED_BLOCK>>` | The hash is of a block header; identical to `MSG_BLOCK`. When used in a [`getdata` message](core-ref-p2p-network-data-messages#section-getdata), this indicates the response should be a [`merkleblock` message](core-ref-p2p-network-data-messages#section-merkleblock) rather than a [`block` message](core-ref-p2p-network-data-messages#section-block) (but this only works if a bloom filter was previously configured).  **Only for use in [`getdata` messages](core-ref-p2p-network-data-messages#section-getdata).**
| 4               | `<<glossary:MSG_LEGACY_TXLOCK_REQUEST>>` | `MSG_TXLOCK_REQUEST` prior to Dash Core 0.15.0. The hash is an Instant Send transaction lock request. Transactions received this way are automatically converted to a standard [`tx` message](core-ref-p2p-network-data-messages#section-tx) as of Dash Core 0.15.0.
| 6               | `<<glossary:MSG_SPORK>>`                            | The hash is Spork ID.
| 16               | `<<glossary:MSG_DSTX>>`                              | The hash is Private Send (Dark Send) Broadcast TX.
| 17               | `<<glossary:MSG_GOVERNANCE_OBJECT>>`                                     | The hash is a Governance Object.
| 18               | `<<glossary:MSG_GOVERNANCE_OBJECT_VOTE>>`                                     | The hash is a Governance Object Vote.
| 20               | `<<glossary:MSG_CMPCT_BLOCK>>`                                     | The hash is of a block header; identical to `MSG_BLOCK`. When used in a [`getdata` message](core-ref-p2p-network-data-messages#section-getdata), this indicates the response should be a [`cmpctblock` message](core-ref-p2p-network-data-messages#section-cmpctblock). **Only for use in [`getdata` messages](core-ref-p2p-network-data-messages#section-getdata).**
| 21               | `<<glossary:MSG_QUORUM_FINAL_COMMITMENT>>`                | The hash is a long-living masternode quorum final commitment.<br>_Added in 0.13.0_
| 23               | `<<glossary:MSG_QUORUM_CONTRIB>>`                                     | The hash is a long-living masternode quorum contribution.<br>_Added in 0.14.0_
| 24               | `<<glossary:MSG_QUORUM_COMPLAINT>>`                                     | The hash is a long-living masternode quorum complaint.<br>_Added in 0.14.0_
| 25               | `<<glossary:MSG_QUORUM_JUSTIFICATION>>`                   | The hash is a long-living masternode quorum justification.<br>_Added in 0.14.0_
| 26               | `<<glossary:MSG_QUORUM_PREMATURE_COMMITMENT>>`    | The hash is a long-living masternode quorum premature commitment.<br>_Added in 0.14.0_
| 28               | `<<glossary:MSG_QUORUM_RECOVERED_SIG>>`                        | The hash is a long-living masternode quorum recovered signature.<br>_Added in 0.14.0_
| 29               | `<<glossary:MSG_CLSIG>>`                                     | The hash is a ChainLock signature.<br>_Added in 0.14.0_
| 30               | `<<glossary:MSG_ISLOCK>>`                                   | The hash is an LLMQ-based InstantSend lock.<br>_Added in 0.14.0_

The deprecated type identifiers are:

| Type Identifier | Name                                                                          | Description
|-----------------|-------------------------------------------------------------------------------|---------------
| 5               | `<<glossary:MSG_TXLOCK_VOTE>>`          | **Deprecated in 0.15.0**<br><br>The hash is an Instant Send transaction vote.
| 7               | `<<glossary:MSG_MASTERNODE_PAYMENT_VOTE>>`                                     | **Deprecated in 0.14.0**<br><br>The hash is a Masternode Payment Vote.
| 8               | `<<glossary:MSG_MASTERNODE_PAYMENT_BLOCK>>`                                     | **Deprecated in 0.14.0**<br><br>The hash is a Masternode Payment Block.
| 8               | `MSG_MASTERNODE_SCANNING_ERROR`                                             | Replaced by `MSG_MASTERNODE_PAYMENT_BLOCK`
| 9               | `<<glossary:MSG_BUDGET_VOTE>>`          | Deprecated
| 10               | `<<glossary:MSG_BUDGET_PROPOSAL>>`                                     | Deprecated
| 11               | `<<glossary:MSG_BUDGET_FINALIZED>>`                                     | Deprecated
| 12               | `<<glossary:MSG_BUDGET_FINALIZED_VOTE>>`                                     | Deprecated
| 13               | `<<glossary:MSG_MASTERNODE_QUORUM>>`                                     | Not Implemented
| 14               | `<<glossary:MSG_MASTERNODE_ANNOUNCE>>`                                     | **Deprecated in 0.14.0**<br><br>The hash is a Masternode Broadcast.
| 15               | `<<glossary:MSG_MASTERNODE_PING>>`                                     | **Deprecated in 0.14.0**<br><br>The hash is a Masternode Ping.
| 19               | `<<glossary:MSG_MASTERNODE_VERIFY>>`                                     | **Deprecated in 0.14.0**<br><br>The hash is a Masternode Verify.
| 22               | `MSG_QUORUM_DUMMY_COMMITMENT`                                     | **Deprecated in 0.14.0**<br><br>Temporarily used on Testnet only.
| 27               | `<<glossary:MSG_QUORUM_DEBUG_STATUS>>`                            | **Deprecated in 0.14.0**<br><br>Temporarily used on Testnet only.

Type identifier zero and type identifiers greater than those shown in the table above are reserved for future implementations. Dash Core ignores all inventories with one of these unknown types.

# Block

The [`block` message](core-ref-p2p-network-data-messages#section-block) transmits a single <<glossary:serialized block>> in the format described in the [serialized blocks section](core-ref-block-chain-serialized-blocks). See that section for an example hexdump.  It can be sent for two different reasons:

1. **GetData Response:** Nodes will always send it in response to a [`getdata` message](core-ref-p2p-network-data-messages#section-getdata) that requests the block with an <<glossary:inventory>> type of `MSG_BLOCK` (provided the node has that block available for relay).

2. **Unsolicited:** Some miners will send unsolicited [`block` messages](core-ref-p2p-network-data-messages#section-block) broadcasting their newly-mined blocks to all of their <<glossary:peers>>. Many <<glossary:mining>> pools do the same thing, although some may be misconfigured to send the block from multiple nodes, possibly sending the same block to some peers more than once.

# Blocktxn

*Added in protocol version 70209 of Dash Core as described by BIP152*

The [`blocktxn` message](core-ref-p2p-network-data-messages#section-blocktxn) sends requested <<glossary:block>> <<glossary:transactions>> to a node which previously requested them with a [`getblocktxn` message](core-ref-p2p-network-data-messages#section-getblocktxn). It is defined as a message containing a serialized `BlockTransactions` message.

Upon receipt of a properly-formatted requested [`blocktxn` message](core-ref-p2p-network-data-messages#section-blocktxn), <<glossary:nodes>> should:

1. Attempt to reconstruct the full block by taking the prefilledtxn transactions from the original [`cmpctblock` message](core-ref-p2p-network-data-messages#section-cmpctblock) and placing them in the marked positions
2. For each short transaction ID from the original [`cmpctblock` message](core-ref-p2p-network-data-messages#section-cmpctblock), in order, find the corresponding transaction (from either the [`blocktxn` message](core-ref-p2p-network-data-messages#section-blocktxn) or from other sources)
3. Place each short transaction ID in the first available position in the block
4. Once the block has been reconstructed, it shall be processed as normal.

**Short transaction IDs are expected to occasionally collide. Nodes must not be penalized for such collisions.**

The structure of `BlockTransactions` is defined below.

| Bytes    | Name                 | Data Type            | Encoding | Description|
|----------|----------------------|----------------------|----------|------------|
| 32       | blockhash            | Binary blob          | The output from a double-SHA256 of the block header, as used elsewhere | The blockhash of the block which the transactions being provided are in
| 1 or 3   | transactions<br>_length | CompactSize          | As used to encode array lengths elsewhere | The number of transactions provided
| *Varies* | transactions         | List of transactions | As encoded in [`tx` messages](core-ref-p2p-network-data-messages#section-tx) in response to `getdata MSG_TX` | The transactions provided

The following annotated hexdump shows a [`blocktxn` message](core-ref-p2p-network-data-messages#section-blocktxn).  (The message header has been omitted.)

``` text
182327cb727da7d60541da793831fd0ab0509e79c8cd
3d654cdf3a0100000000 ....................... Block Hash

01 ......................................... Transactions Provided: 1

Transaction(s)
| Transaction 1
| | 01000000 ................................ Transaction Version: 1
| | 01 ...................................... Input count: 1
| |
| | Transaction input #1
| | |
| | | 0952617a516d956e2ecee71a6adc249f
| | | 4bb757adcc409452ab98c8e55c31e62a ..... Outpoint TXID
| | | 00000000 ............................. Outpoint index number: 0
| | |
| | | 6b ................................... Bytes in sig. script: 107
| | | 483045022100d10edf447252e1e69ff1
| | | 77330bb2c889a50be02e00cc5d79c0d0
| | | 79ae56518fc40220245d36905dc950fc
| | | d55694cfde8cde3109dc80b12aca3a6e
| | | 332033802ee36e1b01210272cc6e7660
| | | 2648831d8e80fca8eb24369cd0f23ff0
| | | 79cf20ae9d9beee05de6db ............... Secp256k1 signature
| | |
| | | ffffffff ............................. Sequence number: UINT32_MAX
| |
| | 02 ..................................... Number of outputs: 02
| |
| | Transaction output #1
| | | 0be0f50500000000 ..................... Duffs (0.99999755 Dash)
| | |
| | | 19 ................................... Bytes in pubkey script: 25
| | | | 76 ................................. OP_DUP
| | | | a9 ................................. OP_HASH160
| | | | 14 ................................. Push 20 bytes as data
| | | | | 923d91ed359f650eec6ea8b9030b340d
| | | | | ea63d590 ......................... PubKey hash
| | | | 88 ................................. OP_EQUALVERIFY
| | | | ac ................................. OP_CHECKSIG
| |
| | [...] .................................. 1 more tx output omitted
| |
| | 00000000 ............................... locktime: 0 (a block height)
```

# CmpctBlock

*Added in protocol version 70209 of Dash Core as described by BIP152*

The [`cmpctblock` message](core-ref-p2p-network-data-messages#section-cmpctblock) is a reply to a [`getdata` message](core-ref-p2p-network-data-messages#section-getdata) which requested a <<glossary:block>> using the <<glossary:inventory>> type `MSG_CMPCT_BLOCK`. If the requested block was recently announced and is close to the tip of the best chain of the receiver and after having sent the requesting <<glossary:peer>> a [`sendcmpct` message](core-ref-p2p-network-control-messages#section-sendcmpct), nodes respond with a [`cmpctblock` message](core-ref-p2p-network-data-messages#section-cmpctblock) containing data for the block.

**If the requested block is too old, the node responds with a *full non-compact block***

Upon receipt of a [`cmpctblock` message](core-ref-p2p-network-data-messages#section-cmpctblock), after sending a [`sendcmpct` message](core-ref-p2p-network-control-messages#section-sendcmpct), nodes should calculate the short transaction ID for each <<glossary:unconfirmed transaction>> they have available (i.e. in their mempool) and compare each to each short transaction ID in the [`cmpctblock` message](core-ref-p2p-network-data-messages#section-cmpctblock). After finding already-available transactions, nodes which do not have all transactions available to reconstruct the full block should request the missing transactions using a [`getblocktxn` message](core-ref-p2p-network-data-messages#section-getblocktxn).

A node must not send a [`cmpctblock` message](core-ref-p2p-network-data-messages#section-cmpctblock) unless they are able to respond to a [`getblocktxn` message](core-ref-p2p-network-data-messages#section-getblocktxn) which requests every transaction in the block. A node must not send a [`cmpctblock` message](core-ref-p2p-network-data-messages#section-cmpctblock) without having validated that the <<glossary:header>> properly commits to each transaction in the block, and properly builds on top of the existing, fully-validated chain with a valid proof-of-work either as a part of the current most-work valid chain, or building directly on top of it. A node may send a [`cmpctblock` message](core-ref-p2p-network-data-messages#section-cmpctblock) before validating that each transaction in the block validly spends existing UTXO set entries.

The [`cmpctblock` message](core-ref-p2p-network-data-messages#section-cmpctblock) contains a vector of `PrefilledTransaction` whose structure is defined below. A `PrefilledTransaction` is used in `HeaderAndShortIDs` to provide a list of a few transactions explicitly.

| Bytes    | Name                 | Data Type            | Encoding | Description|
|----------|----------------------|----------------------|----------|------------|
| 1 or 3   | index                | CompactSize          | Compact Size, differentially encoded since the last PrefilledTransaction in a list | The index into the block at which this transaction is
| *Varies* | tx                   | Transaction          | As encoded in [`tx` messages](core-ref-p2p-network-data-messages#section-tx) sent in response to `getdata MSG_TX` | Transaction which is in the block at index `index`

The [`cmpctblock` message](core-ref-p2p-network-data-messages#section-cmpctblock) is compromised of a serialized `HeaderAndShortIDs` structure which is defined below. A `HeaderAndShortIDs` structure is used to relay a block header, the short transactions IDs used for matching already-available transactions, and a select few transactions which we expect a peer may be missing.

| Bytes    | Name                 | Data Type            | Encoding | Description|
|----------|----------------------|----------------------|----------|------------|
| 80       | header               | Block header         | First 80 bytes of the block as defined by the encoding used by [`block` messages](core-ref-p2p-network-data-messages#section-block) | The header of the block being provided
| 8        | nonce                | uint64_t             | Little Endian | A nonce for use in short transaction ID calculations
| 1 or 3   | shortids_<br>length  | CompactSize          | As used to encode array lengths elsewhere | The number of short transaction IDs in `shortids` (i.e. block tx count - `prefilledtxn`<br>`_length`)
| *Varies* | shortids  | List of 6-byte integers | Little Endian | The short transaction IDs calculated from the transactions which were not provided explicitly in `prefilledtxn`
| 1 or 3   | prefilledtxn<br>_length | CompactSize       | As used to encode array lengths elsewhere | The number of prefilled transactions in `prefilledtxn` (i.e. block tx count - `shortids`<br>`_length`)
| *Varies* | prefilledtxn     | List of Prefilled<br>Transactions | As defined by `Prefilled`<br>`Transaction` definition below | Used to provide the coinbase transaction and a select few which we expect a peer may be missing

## **Short Transaction ID calculation**

Short transaction IDs are used to represent a transaction without sending a full 256-bit hash. They are calculated as follows,

* A single-SHA256 hashing the <<glossary:block header>> with the nonce appended (in little-endian)
* Running SipHash-2-4 with the input being the transaction ID and the keys (k0/k1) set to the first two little-endian 64-bit integers from the above hash, respectively.
* Dropping the 2 most significant bytes from the SipHash output to make it 6 bytes.

The following annotated hexdump shows a [`cmpctblock` message](core-ref-p2p-network-data-messages#section-cmpctblock). (The message header has been omitted.)

``` text
00000020981178a4342cec6316296b2ad84c9b7cdf9f
2688e5d0fe1a0003cd0000000000f64870f52a3d0125
1336c9464961216732b25fbf288a51f25a0e81bffb20
e9600194d85a64a50d1cc02b0181 ................ Block Header

3151b67e5b418b9d ............................ Nonce

01 .......................................... Short IDs Length: 1
483edcd3c799 ................................ Short IDs

01 .......................................... Prefilled Transaction Length: 1

Prefilled Transactions
| 00 ........................................ Index: 0
|
| Transaction 1 (Coinbase)
| | 01000000 ................................ Transaction Version: 1
| | 01 ...................................... Input count: 1
| |
| | Transaction input #1
| | |
| | | 00000000000000000000000000000000
| | | 00000000000000000000000000000000 ..... Outpoint TXID
| | | ffffffff ............................. Outpoint index number: UINT32_MAX
| | |
| | | 13 ................................... Bytes in sig. script: 19
| | | 03daaf010e2f5032506f6f6c2d74444153482f Secp256k1 signature
| | |
| | | ffffffff ............................. Sequence number: UINT32_MAX
| |
| | 04 ..................................... Number of outputs: 04
| |
| | Transaction output #1
| | | ffe5654200000000 ..................... Duffs (11.13974271 Dash)
| | |
| | | 19 ................................... Bytes in pubkey script: 25
| | | | 76 ................................. OP_DUP
| | | | a9 ................................. OP_HASH160
| | | | 14 ................................. Push 20 bytes as data
| | | | | b885cb21ad12e593c1a46d814df47ccb
| | | | | 450a7d84 ......................... PubKey hash
| | | | 88 ................................. OP_EQUALVERIFY
| | | | ac ................................. OP_CHECKSIG
| |
| | [...] .................................. 3 more tx outputs omitted
| |
| | 00000000 ............................... locktime: 0 (a block height)
```

# GetBlocks

The [`getblocks` message](core-ref-p2p-network-data-messages#section-getblocks) requests an [`inv` message](core-ref-p2p-network-data-messages#section-inv) that provides <<glossary:block header>> hashes starting from a particular point in the <<glossary:block chain>>. It allows a <<glossary:peer>> which has been disconnected or started for the first time to get the data it needs to request the blocks it hasn't seen.

Peers which have been disconnected may have <<glossary:stale blocks>> in their locally-stored block chain, so the [`getblocks` message](core-ref-p2p-network-data-messages#section-getblocks) allows the requesting peer to provide the receiving peer with multiple <<glossary:header>> hashes at heights on their local chain. This allows the receiving peer to find, within that list, the last header hash they had in common and reply with all subsequent header hashes.

**Note:** the receiving peer itself may respond with an [`inv` message](core-ref-p2p-network-data-messages#section-inv) containing header hashes of stale blocks.  It is up to the requesting peer to poll all of its peers to find the best block chain.

If the receiving peer does not find a common header hash within the list, it will assume the last common block was the <<glossary:genesis block>> (block zero), so it will reply with in [`inv` message](core-ref-p2p-network-data-messages#section-inv) containing header hashes starting with block one (the first block after the genesis block).

| Bytes    | Name                 | Data Type        | Description
|----------|----------------------|------------------|----------------
| 4        | version              | uint32_t         | The protocol version number; the same as sent in the [`version` message](core-ref-p2p-network-control-messages#section-version).
| *Varies* | hash count           | compactSize uint | The number of header hashes provided not including the stop hash.  There is no limit except that the byte size of the entire message must be below the [`MAX_SIZE`](https://github.com/dashpay/dash/blob/c31ba8ba4c07e72620bd71753f2103ca103bb1c2/src/serialize.h#L26) limit; typically from 1 to 200 hashes are sent.
| *Varies* | block header hashes  | char[32]         | One or more block header hashes (32 bytes each) in internal byte order.  Hashes should be provided in reverse order of block height, so highest-height hashes are listed first and lowest-height hashes are listed last.
| 32       | stop hash            | char[32]         | The header hash of the last header hash being requested; set to all zeroes to request an [`inv` message](core-ref-p2p-network-data-messages#section-inv) with all subsequent header hashes (a maximum of 500 will be sent as a reply to this message; if you need more than 500, you will need to send another [`getblocks` message](core-ref-p2p-network-data-messages#section-getblocks) with a higher-height header hash as the first entry in block header hash field).

The following annotated hexdump shows a [`getblocks` message](core-ref-p2p-network-data-messages#section-getblocks).  (The message header has been omitted.)

``` text
71110100 ........................... Protocol version: 70001
02 ................................. Hash count: 2

d39f608a7775b537729884d4e6633bb2
105e55a16a14d31b0000000000000000 ... Hash #1

5c3e6403d40837110a2e8afb602b1c01
714bda7ce23bea0a0000000000000000 ... Hash #2

00000000000000000000000000000000
00000000000000000000000000000000 ... Stop hash
```

# GetBlockTxn

*Added in protocol version 70209 of Dash Core as described by BIP152*

The [`getblocktxn` message](core-ref-p2p-network-data-messages#section-getblocktxn) requests a [`blocktxn` message](core-ref-p2p-network-data-messages#section-blocktxn) for any transactions that it has not seen after a compact block is received. It is defined as a message containing a serialized `BlockTransactionsRequest` message. Upon receipt of a properly-formatted [`getblocktxn` message](core-ref-p2p-network-data-messages#section-getblocktxn), <<glossary:nodes>> which recently provided the sender of such a message with a [`cmpctblock` message](core-ref-p2p-network-data-messages#section-cmpctblock) for the block hash identified in this message must respond with either an appropriate [`blocktxn` message](core-ref-p2p-network-data-messages#section-blocktxn), or a full block message.

A [`blocktxn` message](core-ref-p2p-network-data-messages#section-blocktxn) response must contain exactly and only each transaction which is present in the appropriate block at the index specified in the [`getblocktxn` message](core-ref-p2p-network-data-messages#section-getblocktxn) indexes list, in the order requested.

The structure of `BlockTransactionsRequest` is defined below.

| Bytes    | Name            | Data Type            | Encoding | Description|
|----------|-----------------|----------------------|----------|------|
| 32       | blockhash       | Binary blob          | The output from a double-SHA256 of the block header, as used elsewhere | The blockhash of the block which the transactions being requested are in
| *Varies* | indexes_length  | CompactSize uint     | As used to encode array lengths elsewhere | The number of transactions requested
| *Varies* | indexes         | CompactSize uint[]   | Differentially encoded | Vector of compactSize containing the indexes of the transactions being requested in the block.

The following annotated hexdump shows a [`getblocktxn` message](core-ref-p2p-network-data-messages#section-getblocktxn).  (The message header has been omitted.)

``` text
182327cb727da7d60541da793831fd0a
b0509e79c8cd3d654cdf3a0100000000 ... Block Hash

01 ................................. Index length: 1
01 ................................. Index: 1
```

# GetData

The [`getdata` message](core-ref-p2p-network-data-messages#section-getdata) requests one or more data objects from another <<glossary:node>>. The objects are requested by an inventory, which the requesting node typically previously received by way of an [`inv` message](core-ref-p2p-network-data-messages#section-inv).

The response to a [`getdata` message](core-ref-p2p-network-data-messages#section-getdata) can be a [`tx` message](core-ref-p2p-network-data-messages#section-tx), [`block` message](core-ref-p2p-network-data-messages#section-block), [`merkleblock` message](core-ref-p2p-network-data-messages#section-merkleblock), `ix` message, `txlvote` message, `mnw` message, `mnb` message, `mnp` message, [`dstx` message](core-ref-p2p-network-privatesend-messages#section-dstx), [`govobj` message](core-ref-p2p-network-governance-messages#section-govobj), [`govobjvote` message](core-ref-p2p-network-governance-messages#section-govobjvote), `mnv` message, [`notfound` message](core-ref-p2p-network-data-messages#section-notfound), or [`cmpctblock` message](core-ref-p2p-network-data-messages#section-cmpctblock).

This message cannot be used to request arbitrary data, such as historic transactions no longer in the memory pool or relay set. Full nodes may not even be able to provide older <<glossary:blocks>> if they've pruned old transactions from their block database. For this reason, the [`getdata` message](core-ref-p2p-network-data-messages#section-getdata) should usually only be used to request data from a node which previously advertised it had that data by sending an [`inv` message](core-ref-p2p-network-data-messages#section-inv).

The format and maximum size limitations of the [`getdata` message](core-ref-p2p-network-data-messages#section-getdata) are identical to the [`inv` message](core-ref-p2p-network-data-messages#section-inv); only the message header differs.

# GetHeaders

*Added in protocol version 70077.*

The [`getheaders` message](core-ref-p2p-network-data-messages#section-getheaders) requests a [`headers` message](core-ref-p2p-network-data-messages#section-headers) that provides block headers starting from a particular point in the <<glossary:block chain>>. It allows a <<glossary:peer>> which has been disconnected or started for the first time to get the <<glossary:headers>> it hasn’t seen yet.

The [`getheaders` message](core-ref-p2p-network-data-messages#section-getheaders) is nearly identical to the [`getblocks` message](core-ref-p2p-network-data-messages#section-getblocks), with one minor difference: the `inv` reply to the [`getblocks` message](core-ref-p2p-network-data-messages#section-getblocks) will include no more than 500 <<glossary:block header>> hashes; the `headers` reply to the [`getheaders` message](core-ref-p2p-network-data-messages#section-getheaders) will include as many as 2,000 block headers.

# GetMNListD

*Added in protocol version 70213*

The [`getmnlistd` message](core-ref-p2p-network-data-messages#section-getmnlistd) requests a [`mnlistdiff` message](core-ref-p2p-network-data-messages#section-mnlistdiff) that provides either:

  1. A full <<glossary:masternode>> list (if `baseBlockHash` is all-zero)
  2. An update to a previously requested masternode list

| Bytes | Name | Data type | Required | Description |
| ---------- | ----------- | --------- | -------- | -------- |
| 32 | baseBlockHash | uint256 | Required | Hash of a block the requester already has a valid masternode list of.<br>_Note: Can be all-zero to indicate that a full masternode list is requested._
| 32 | blockHash | uint256 | Required | Hash of the block for which the masternode list diff is requested

The following annotated hexdump shows a [`getmnlistd` message](core-ref-p2p-network-data-messages#section-getmnlistd). (The message header has been omitted.)

``` text
000001ee5108348a2c59396da29dc576
9b2a9bb303d7577aee9cd95136c49b9b ........... Base block hash

0000030f51f12e7069a7aa5f1bc9085d
db3fe368976296fd3b6d73fdaf898cc0 ........... Block hash
```

# Headers

*Added in protocol version 31800 (of Bitcoin).*

The [`headers` message](core-ref-p2p-network-data-messages#section-headers) sends block headers to a <<glossary:node>> which previously requested certain <<glossary:headers>> with a [`getheaders` message](core-ref-p2p-network-data-messages#section-getheaders). A headers message can be empty.

| Bytes    | Name    | Data Type        | Description
|----------|---------|------------------|-----------------
| *Varies* | count   | compactSize uint | Number of block headers up to a maximum of 2,000.  Note: headers-first sync assumes the sending node will send the maximum number of headers whenever possible.
| *Varies* | headers | block_header     | Block headers: each 80-byte block header is in the format described in the [block headers section](core-ref-block-chain-block-headers) with an additional 0x00 suffixed.  This 0x00 is called the transaction count, but because the headers message doesn't include any transactions, the transaction count is always zero.

The following annotated hexdump shows a [`headers` message](core-ref-p2p-network-data-messages#section-headers).  (The message header has been omitted.)

``` text
01 ................................. Header count: 1

02000000 ........................... Block version: 2
b6ff0b1b1680a2862a30ca44d346d9e8
910d334beb48ca0c0000000000000000 ... Hash of previous block's header
9d10aa52ee949386ca9385695f04ede2
70dda20810decd12bc9b048aaab31471 ... Merkle root
24d95a54 ........................... Unix time: 1415239972
30c31b18 ........................... Target (bits)
fe9f0864 ........................... Nonce

00 ................................. Transaction count (0x00)
```

# Inv

The [`inv` message](core-ref-p2p-network-data-messages#section-inv) (inventory message) transmits one or more <<glossary:inventories>> of objects known to the transmitting <<glossary:peer>>.  It can be sent unsolicited to announce new <<glossary:transactions>> or <<glossary:blocks>>, or it can be sent in reply to a [`getblocks` message](core-ref-p2p-network-data-messages#section-getblocks) or [`mempool` message](core-ref-p2p-network-data-messages#section-mempool).

The receiving peer can compare the inventories from an [`inv` message](core-ref-p2p-network-data-messages#section-inv) against the inventories it has already seen, and then use a follow-up message to request unseen objects.

| Bytes    | Name      | Data Type             | Description
|----------|-----------|-----------------------|-----------------
| *Varies* | count     | compactSize uint      | The number of inventory entries.
| *Varies* | inventory | inventory             | One or more inventory entries up to a maximum of 50,000 entries.

The following annotated hexdump shows an [`inv` message](core-ref-p2p-network-data-messages#section-inv) with two inventory entries.  (The message header has been omitted.)

``` text
02 ................................. Count: 2

0f000000 ........................... Type: MSG_MASTERNODE_PING
dd6cc6c11211793b239c2e311f1496e2
2281b200b35233eaae465d2aa3c9d537 ... Hash (mnp)

05000000 ........................... Type: MSG_TXLOCK_VOTE
afc5b2f418f8c06c477a7d071240f5ee
ab17057f9ce4b50c2aef4fadf3729a2e ... Hash (txlvote)
```

# MemPool

*Added in protocol version 60002 (of Bitcoin).*

The [`mempool` message](core-ref-p2p-network-data-messages#section-mempool) requests the <<glossary:TXIDs>> of transactions that the receiving <<glossary:node>> has verified as valid but which have not yet appeared in a <<glossary:block>>. That is, transactions which are in the receiving node's memory pool. The response to the [`mempool` message](core-ref-p2p-network-data-messages#section-mempool) is one or more [`inv` messages](core-ref-p2p-network-data-messages#section-inv) containing the TXIDs in the usual <<glossary:inventory>> format.

Sending the [`mempool` message](core-ref-p2p-network-data-messages#section-mempool) is mostly useful when a program first connects to the network. Full nodes can use it to quickly gather most or all of the unconfirmed transactions available on the network; this is especially useful for miners trying to gather transactions for their transaction fees. SPV clients can set a filter before sending a `mempool` to only receive transactions that match that filter; this allows a recently-started client to get most or all unconfirmed transactions related to its wallet.

The `inv` response to the [`mempool` message](core-ref-p2p-network-data-messages#section-mempool) is, at best, one node's view of the network---not a complete list of every <<glossary:unconfirmed transaction>> on the network. Here are some additional reasons the list might not be complete:

* Before Bitcoin Core 0.9.0, the response to the [`mempool` message](core-ref-p2p-network-data-messages#section-mempool) was only one [`inv` message](core-ref-p2p-network-data-messages#section-inv). An [`inv` message](core-ref-p2p-network-data-messages#section-inv) is limited to 50,000 inventories, so a node with a memory pool larger than 50,000 entries would not send everything.  Later versions of Bitcoin Core send as many [`inv` messages](core-ref-p2p-network-data-messages#section-inv) as needed to reference its complete memory pool.

* The [`mempool` message](core-ref-p2p-network-data-messages#section-mempool) is not currently fully compatible with the [`filterload` message](core-ref-p2p-network-control-messages#section-filterload)'s `BLOOM_UPDATE_ALL` and `BLOOM_UPDATE_P2PUBKEY_ONLY` flags. Mempool transactions are not sorted like in-block transactions, so a transaction (tx2) spending an <<glossary:output>> can appear before the transaction (tx1) containing that output, which means the automatic filter update mechanism won't operate until the second-appearing transaction (tx1) is seen---missing the first-appearing transaction (tx2). It has been proposed in [Bitcoin Core issue #2381](https://github.com/bitcoin/bitcoin/issues/2381) that the transactions should be sorted before being processed by the filter.

There is no payload in a [`mempool` message](core-ref-p2p-network-data-messages#section-mempool).  See the [message header section](core-ref-p2p-network-message-headers) for an example of a message without a payload.

# MerkleBlock

*Added in protocol version 70001 as described by BIP37.*

The [`merkleblock` message](core-ref-p2p-network-data-messages#section-merkleblock) is a reply to a [`getdata` message](core-ref-p2p-network-data-messages#section-getdata) which requested a <<glossary:block>> using the inventory type `MSG_MERKLEBLOCK`.  It is only part of the reply: if any matching transactions are found, they will be sent separately as [`tx` messages](core-ref-p2p-network-data-messages#section-tx).

If a filter has been previously set with the [`filterload` message](core-ref-p2p-network-control-messages#section-filterload), the [`merkleblock` message](core-ref-p2p-network-data-messages#section-merkleblock) will contain the <<glossary:TXIDs>> of any transactions in the requested block that matched the filter, as well as any parts of the block's <<glossary:merkle tree>> necessary to connect those transactions to the block header's <<glossary:merkle root>>. The message also contains a complete copy of the <<glossary:block header>> to allow the client to hash it and confirm its <<glossary:proof of work>>.

| Bytes    | Name               | Data Type        | Description
|----------|--------------------|------------------|----------------
| 80       | block header       | block_header     | The block header in the format described in the [block header section](core-ref-block-chain-block-headers).
| 4        | transaction count  | uint32_t         | The number of transactions in the block (including ones that don't match the filter).
| *Varies* | hash count         | compactSize uint | The number of hashes in the following field.
| *Varies* | hashes             | char[32]         | One or more hashes of both transactions and merkle nodes in internal byte order.  Each hash is 32 bytes.
| *Varies* | flag byte count    | compactSize uint | The number of flag bytes in the following field.
| *Varies* | flags              | byte[]           | A sequence of bits packed eight in a byte with the least significant bit first.  May be padded to the nearest byte boundary but must not contain any more bits than that.  Used to assign the hashes to particular nodes in the merkle tree as described below.

The annotated hexdump below shows a [`merkleblock` message](core-ref-p2p-network-data-messages#section-merkleblock) which corresponds to the examples below.  (The message header has been omitted.)

``` text
01000000 ........................... Block version: 1
82bb869cf3a793432a66e826e05a6fc3
7469f8efb7421dc88067010000000000 ... Hash of previous block's header
7f16c5962e8bd963659c793ce370d95f
093bc7e367117b3c30c1f8fdd0d97287 ... Merkle root
76381b4d ........................... Time: 1293629558
4c86041b ........................... nBits: 0x04864c * 256**(0x1b-3)
554b8529 ........................... Nonce

07000000 ........................... Transaction count: 7
04 ................................. Hash count: 4

3612262624047ee87660be1a707519a4
43b1c1ce3d248cbfc6c15870f6c5daa2 ... Hash #1
019f5b01d4195ecbc9398fbf3c3b1fa9
bb3183301d7a1fb3bd174fcfa40a2b65 ... Hash #2
41ed70551dd7e841883ab8f0b16bf041
76b7d1480e4f0af9f3d4c3595768d068 ... Hash #3
20d2a7bc994987302e5b1ac80fc425fe
25f8b63169ea78e68fbaaefa59379bbf ... Hash #4

01 ................................. Flag bytes: 1
1d ................................. Flags: 1 0 1 1 1 0 0 0
```

Note: when fully decoded, the above [`merkleblock` message](core-ref-p2p-network-data-messages#section-merkleblock) provided the TXID for a single transaction that matched the filter. In the <<glossary:network>> traffic dump this output was taken from, the full transaction belonging to that TXID was sent immediately after the [`merkleblock` message](core-ref-p2p-network-data-messages#section-merkleblock) as a [`tx` message](core-ref-p2p-network-data-messages#section-tx).

## Parsing A MerkleBlock Message

As seen in the annotated hexdump above, the [`merkleblock` message](core-ref-p2p-network-data-messages#section-merkleblock) provides three special data types: a transaction count, a list of hashes, and a list of one-bit flags.

You can use the transaction count to construct an empty <<glossary:merkle tree>>. We'll call each entry in the tree a node; on the bottom are TXID nodes---the hashes for these nodes are <<glossary:TXIDs>>; the remaining nodes (including the <<glossary:merkle root>>) are non-TXID nodes---they may actually have the same hash as a TXID, but we treat them differently.

![Example Of Parsing A MerkleBlock Message](https://dash-docs.github.io/img/dev/animated-en-merkleblock-parsing.gif)

Keep the hashes and flags in the order they appear in the [`merkleblock` message](core-ref-p2p-network-data-messages#section-merkleblock). When we say "next flag" or "next hash", we mean the next flag or hash on the list, even if it's the first one we've used so far.

Start with the merkle root node and the first flag. The table below describes how to evaluate a flag based on whether the node being processed is a TXID node or a non-TXID node. Once you apply a flag to a node, never apply another flag to that same node or reuse that same flag again.

| Flag  | TXID Node                                                                                | Non-TXID Node
|-------|------------------------------------------------------------------------------------------|----
| **0** | Use the next hash as this node's TXID, but this transaction didn't match the filter.     | Use the next hash as this node's hash.  Don't process any descendant nodes.
| **1** | Use the next hash as this node's TXID, and mark this transaction as matching the filter. | The hash needs to be computed.  Process the left child node to get its hash; process the right child node to get its hash; then concatenate the two hashes as 64 raw bytes and hash them to get this node's hash.

Any time you begin processing a node for the first time, evaluate the next flag. Never use a flag at any other time.

When processing a child node, you may need to process its children (the grandchildren of the original node) or further-descended nodes before returning to the parent node. This is expected---keep processing depth first until you reach a TXID node or a non-TXID node with a flag of 0.

After you process a TXID node or a non-TXID node with a flag of 0, stop processing flags and begin to ascend the tree. As you ascend, compute the hash of any nodes for which you now have both child hashes or for which you now have the sole child hash. See the [merkle tree section](core-ref-block-chain-block-headers#section-merkle-trees) for hashing instructions. If you reach a node where only the left hash is known, descend into its right child (if present) and further descendants as necessary.

However, if you find a node whose left and right children both have the same hash, fail.  This is related to CVE-2012-2459.

Continue descending and ascending until you have enough information to obtain the hash of the merkle root node. If you run out of flags or hashes before that condition is reached, fail. Then perform the following checks (order doesn't matter):

* Fail if there are unused hashes in the hashes list.

* Fail if there are unused flag bits---except for the minimum number of bits necessary to pad up to the next full byte.

* Fail if the hash of the merkle root node is not identical to the merkle root in the <<glossary:block header>>.

* Fail if the block header is invalid. Remember to ensure that the hash of the header is less than or equal to the <<glossary:target threshold>> encoded by the nBits header field. Your program should also, of course, attempt to ensure the header belongs to the best block chain and that the user knows how many confirmations this block has.

For a detailed example of parsing a [`merkleblock` message](core-ref-p2p-network-data-messages#section-merkleblock), please see the corresponding [merkle block examples section](core-example-p2p-network-parsing-a-merkleblock).

## Creating A MerkleBlock Message

It's easier to understand how to create a [`merkleblock` message](core-ref-p2p-network-data-messages#section-merkleblock) after you understand how to parse an already-created message, so we recommend you read the parsing section above first.

Create a complete merkle tree with <<glossary:TXIDs>> on the bottom row and all the other hashes calculated up to the <<glossary:merkle root>> on the top row. For each transaction that matches the filter, track its TXID node and all of its ancestor nodes.

![Example Of Creating A MerkleBlock Message](https://dash-docs.github.io/img/dev/animated-en-merkleblock-creation.gif)

Start processing the tree with the <<glossary:merkle root>> node. The table below describes how to process both TXID nodes and non-TXID nodes based on whether the node is a match, a match ancestor, or neither a match nor a match ancestor.

|                                      | TXID Node                                                              | Non-TXID Node
|--------------------------------------|------------------------------------------------------------------------|----
| **Neither Match Nor Match Ancestor** | Append a 0 to the flag list; append this node's TXID to the hash list. | Append a 0 to the flag list; append this node's hash to the hash list.  Do not descend into its child nodes.
| **Match Or Match Ancestor**          | Append a 1 to the flag list; append this node's TXID to the hash list. | Append a 1 to the flag list; process the left child node.  Then, if the node has a right child, process the right child.  Do not append a hash to the hash list for this node.

Any time you begin processing a node for the first time, a flag should be appended to the flag list. Never put a flag on the list at any other time, except when processing is complete to pad out the flag list to a byte boundary.

When processing a child node, you may need to process its children (the grandchildren of the original node) or further-descended nodes before returning to the parent node. This is expected---keep processing depth first until you reach a TXID node or a node which is neither a TXID nor a match ancestor.

After you process a TXID node or a node which is neither a TXID nor a match ancestor, stop processing and begin to ascend the tree until you find a node with a right child you haven't processed yet. Descend into that right child and process it.

After you fully process the merkle root node according to the instructions in the table above, processing is complete.  Pad your flag list to a byte boundary and construct the [`merkleblock` message](core-ref-p2p-network-data-messages#section-merkleblock) using the template near the beginning of this subsection.

# MnListDiff

*Added in protocol version 70213*

The [`mnlistdiff` message](core-ref-p2p-network-data-messages#section-mnlistdiff) is a reply to a [`getmnlistd` message](core-ref-p2p-network-data-messages#section-getmnlistd) which requested either a full <<glossary:masternode>> list or a diff for a range of <<glossary:blocks>>.

| Bytes | Name | Data<br>type | Required | Description |
| ---------- | ----------- | --------- | -------- | -------- |
| 32 | baseBlockHash | uint256 | Required | Hash of a block the requester already has a valid masternode list of. Can be all-zero to indicate that a full masternode list is requested.
| 32 | blockHash | uint256 | Required | Hash of the block for which the masternode list diff is requested
| 4 | totalTransactions | uint32_t  | Required | Number of total transactions in `blockHash`
| 1-9 | merkleHashes<br>Count | compactSize uint | Required | Number of Merkle hashes
| variable | merkleHashes | vector | Required | Merkle hashes in depth-first order
| 1-9 | merkleFlags<br>Count | compactSize uint | Required | Number of Merkle flag bytes
| variable | merkleFlags | vector<uint8_t> | Required | Merkle flag bits, packed per 8 in a byte, least significant bit first
| variable | cbTx | CTransaction | Required | The fully serialized coinbase transaction of `blockHash`
| 1-9 | deletedMNsCount | compactSize uint | Required | Number of ProRegTx hashes which were deleted after baseBlockHash
| variable | deletedMNs | vector | Required | A list of ProRegTx hashes for masternode which were deleted after `baseBlockHash`
| variable | mnList | vector | Required | The list of Simplified Masternode List (SML) entries which were added or updated since `baseBlockHash`
| 1-9 | deletedQuorums<br>Count | compactSize uint | Required | *Added in protocol version 70214*<br><br>Number of LLMQs which were deleted from the active set after `baseBlockHash` |
| variable | deletedQuorums | (uint8_t+uint256)[] | Required | *Added in protocol version 70214*<br><br>A list of LLMQ type and quorum hashes for LLMQs which were deleted after `baseBlockHash` |
| 1-9 | newQuorumsCount | compactSize uint | Required | *Added in protocol version 70214*<br><br>Number of new LLMQs which were added to the active set since `baseBlockHash` |
| variable | newQuorums | qfcommit[] | Required | *Added in protocol version 70214*<br><br>The list of LLMQ commitments for the LLMQs which were added since `baseBlockHash` |

Simplified Masternode List (SML) Entry

| Bytes | Name | Data type | Description |
| ---------- | ----------- | -------- | -------- |
| 32 | proRegTxHash | uint256 | The hash of the ProRegTx that identifies the masternode
| 32 | confirmedHash | uint256 | The hash of the block at which the masternode got confirmed
| 16 | ipAddress | byte[] | IPv6 address in network byte order. Only IPv4 mapped addresses are allowed (to be extended in the future)
| 2 | port | uint_16 | Port (network byte order)
| 48 | pubKeyOperator | BLSPubKey | The operators public key
| 20 |keyIDVoting | CKeyID | The public key hash used for voting.
| 1 | isValid | bool | True if a masternode is not PoSe-banned

The following annotated hexdump shows a [`mnlistdiff` message](core-ref-p2p-network-data-messages#section-mnlistdiff). (The message header has been omitted.)

``` text
000001ee5108348a2c59396da29dc576
9b2a9bb303d7577aee9cd95136c49b9b ........... Base block hash

0000030f51f12e7069a7aa5f1bc9085d
db3fe368976296fd3b6d73fdaf898cc0 ........... Block hash

05000000 ................................... Transactions: 5

04 ......................................... Merkle hash count: 4

4488a599e5d61709664c32305befd58b
ef29e33bc6e718af0233f938557a57a9 ........... Merkle hash 1
5c8119b7b136d94e477a0d2917d5f724
5ff299cc6e31994f6236a8fb34fec88f ........... Merkle hash 2
905efa3e6743c889823f00147d36d12f
d12ad401c19089f0affcabd423deef67 ........... Merkle hash 3
3f3a7f84d7ad33214994b5aecf4c1e19
2cb65b86750b1377e069073d1eba477a ........... Merkle hash 4

01 ......................................... Merkle flag count: 1
0f ......................................... Flags: 0 0 0 0 1 1 1 1

[...]....................................... Coinbase Tx (Not shown)

00 ......................................... Deleted masternodes: 0

02 ......................................... Masternode list entries: 2

00 ......................................... Deleted quorums: 0

00 ......................................... New quorums: 0

Masternode List
| Masternode 1
| | 01040eb32f760490054543356cff4638
| | 65633439dd073cffa570305eb086f70e ....... ProRegTx hash
| |
| | 000001ee5108348a2c59396da29dc576
| | 9b2a9bb303d7577aee9cd95136c49b9b ....... Confirmed block hash
| |
| | 00000000000000000000000000000000 ....... IP Address: ::ffff:0.0.0.0
| | 0000 ................................... Port: 0
| |
| | 0000000000000000000000000000000000000000
| | 0000000000000000000000000000000000000000
| | 0000000000000000 ....................... Operator public key (BLS)
| | c2ae01fb4084cbc3bc31e7f59b36be228a320404 Voting pubkey hash (ECDSA)
| |
| | 0 ...................................... Valid (0 - No)
|
| Masternode 2
| | f7737beb39779971e9bc59632243e13f
| | c5fc9ada93b69bf48c2d4c463296cd5a ....... ProRegTx hash
| |
| | 0000030f51f12e7069a7aa5f1bc9085d
| | db3fe368976296fd3b6d73fdaf898cc0 ....... Confirmed block hash
| |
| | 000000000000000000000000cf9af40d ....... IP Address: ::ffff:207.154.244.13
| | 4e1f ................................... Port: 19999
| |
| | 88d719278eef605d9c19037366910b59bc28d437
| | de4a8db4d76fda6d6985dbdf10404fb9bb5cd0e8
| | c22f4a914a6c5566 ....................... Operator public key (BLS)
| | 43ce12751c4ba45dcdfe2c16cefd61461e17a54d Voting pubkey hash (ECDSA)
| |
| | 1 ...................................... Valid (1 - Yes)
```

# NotFound

*Added in protocol version 70001.*

The [`notfound` message](core-ref-p2p-network-data-messages#section-notfound) is a reply to a [`getdata` message](core-ref-p2p-network-data-messages#section-getdata) which requested an object the receiving <<glossary:node>> does not have available for relay. (Nodes are not expected to relay historic transactions which are no longer in the memory pool or relay set. Nodes may also have pruned spent transactions from older <<glossary:blocks>>, making them unable to send those blocks.)

The format and maximum size limitations of the [`notfound` message](core-ref-p2p-network-data-messages#section-notfound) are identical to the [`inv` message](core-ref-p2p-network-data-messages#section-inv); only the message header differs.

# Tx

The [`tx` message](core-ref-p2p-network-data-messages#section-tx) transmits a single transaction in the <<glossary:raw transaction>> format. It can be sent in a variety of situations;

* **Transaction Response:** Dash Core will send it in response to a [`getdata` message](core-ref-p2p-network-data-messages#section-getdata) that requests the transaction with an <<glossary:inventory>>  type of `MSG_TX`.

* **MerkleBlock Response:** Dash Core will send it in response to a [`getdata` message](core-ref-p2p-network-data-messages#section-getdata) that requests a <<glossary:merkle block>> with an <<glossary:inventory>> type of `MSG_MERKLEBLOCK`. (This is in addition to sending a [`merkleblock` message](core-ref-p2p-network-data-messages#section-merkleblock).) Each [`tx` message](core-ref-p2p-network-data-messages#section-tx) in this case provides a matched transaction from that <<glossary:block>>.

For an example hexdump of the raw transaction format, see the [raw transaction section](core-ref-transactions-raw-transaction-format).