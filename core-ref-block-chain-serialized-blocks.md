Under current <<glossary:consensus rules>>, a <<glossary:block>> is not valid unless its serialized size is less than or equal to 2 MB. All fields described below are counted towards the serialized size.

| Bytes    | Name         | Data Type        | Description
| - | - | - | - |
| 80       | block header | block_header     | The <<glossary:block header>> in the format described in the [block header section](core-ref-block-chain-block-headers).
| *Varies* | txn_count    | <<glossary:compactSize uint>> | The total number of transactions in this block, including the coinbase transaction.
| *Varies* | txns         | <<glossary:raw transaction>>  | Every transaction in this block, one after another, in raw transaction format.  Transactions must appear in the data stream in the same order their TXIDs appeared in the first row of the merkle tree.  See the [merkle tree section](core-ref-block-chain-block-headers#section-merkle-trees) for details.

The first transaction in a block must be a <<glossary:coinbase transaction>> which should collect and spend any <<glossary:transaction fee>> paid by transactions included in this block.

Until the coin limit (~18 million Dash) is hit, all blocks are entitled to receive a block subsidy of newly created Dash value. The newly created value should be spent in the coinbase transaction.

The block subsidy declines by ~7.1% per year until all Dash is mined. Subsidy calculations are performed by the Dash Core [GetBlockSubsidy()](https://github.com/dashpay/dash/blob/9ed9474a9eb007bba70278ce19df68e84aeeb712/src/main.cpp#L1741) function.

Together, the transaction fees and block subsidy are called the <<glossary:block reward>>. A coinbase transaction is invalid if it tries to spend more value than is available from the block reward.

The block reward is divided into three parts: <<glossary:miner>>, <<glossary:masternode>>, and <<glossary:superblock>>.

| Payee | Subsidy | Description |
| ----- | -------- | ----------- |
| Miner | 45% | Payment for mining
| Masternode | 45% | Payment for masternode services ([PrivateSend](core-guide-dash-features-privatesend), [InstantSend](core-guide-dash-features-instantsend), [Governance](https://docs.dash.org/en/stable/introduction/features.html#decentralized-governance), etc.)
| Superblock | 10% | Payment for maintenance/expansion of the ecosystem (Core development, marketing, integration, etc.)