# GetBestBlockHash

The [`getbestblockhash` RPC](core-api-ref-remote-procedure-calls-blockchain#section-getbestblockhash) returns the header hash of the most recent block on the best block chain.

*Parameters: none*

*Result---hash of the tip from the best block chain*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string (hex) | Required<br>(exactly 1) | The hash of the block header from the most recent block on the best block chain, encoded as hex in RPC byte order

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet getbestblockhash
```

Result:

``` text
00000bafbc94add76cb75e2ec92894837288a481e5c005f6563d91623bf8bc2c
```

*See also*

* [GetBlock](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getblock): gets a block with a particular header hash from the local block database either as a JSON object or as a serialized block.
* [GetBlockHash](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getblockhash): returns the header hash of a block at the given height in the local best block chain.

# GetBestChainLock

The [`getbestchainlock` RPC](core-api-ref-remote-procedure-calls-blockchain#section-getbestchainlock) returns the block hash of the best chainlock.

Throws an error if there is no known chainlock yet.

*Parameters: none*

*Result*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object/null | Required<br>(exactly 1) | An object containing the requested block, or JSON `null` if an error occurred
→<br>`blockhash` | string (hex) | Required<br>(exactly 1) | The hash of the block encoded as hex in RPC byte order.
→<br>`height` | number (int) | Required<br>(exactly 1) | The height of this block on its block chain
→<br>`known_block` | boolean | Required<br>(exactly 1) | True if the block is known by this node

*Example from Dash Core 0.15.0*

``` bash
dash-cli -testnet getbestchainlock
```

Result:
``` json
{
  "blockhash": "000000000036ab34d3005941d4224fc5887526355c98b769e27e5ece05f48860",
  "height": 182106,
  "known_block": true
}
```

*See also: none*

# GetBlock

The [`getblock` RPC](core-api-ref-remote-procedure-calls-blockchain#section-getblock) gets a block with a particular header hash from the local block database either as a JSON object or as a serialized block.

*Parameter #1---block hash*

Name | Type | Presence | Description
--- | --- | --- | ---
Block Hash | string (hex) | Required<br>(exactly 1) | The hash of the header of the block to get, encoded as hex in RPC byte order

*Parameter #2---whether to get JSON or hex output*

Name | Type | Presence | Description
--- | --- | --- | ---
Verbosity | number (int) | Optional<br>(0 or 1) | Set to one of the following verbosity levels:<br>• `0` - Get the block in serialized block format;<br>• `1` - Get the decoded block as a JSON object (default)<br>• `2` - Get the decoded block as a JSON object with transaction details

*Result (if verbosity was `0`)---a serialized block*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string (hex)/null | Required<br>(exactly 1) | The requested block as a serialized block, encoded as hex, or JSON `null` if an error occurred

*Result (if verbosity was `1` or omitted)---a JSON block with transaction hashes*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object/null | Required<br>(exactly 1) | An object containing the requested block, or JSON `null` if an error occurred
→<br>`hash` | string (hex) | Required<br>(exactly 1) | The hash of this block's block header encoded as hex in RPC byte order.  This is the same as the hash provided in parameter #1
→<br>`confirmations` | number (int) | Required<br>(exactly 1) | The number of confirmations the transactions in this block have, starting at 1 when this block is at the tip of the best block chain.  This score will be -1 if the the block is not part of the best block chain
→<br>`size` | number (int) | Required<br>(exactly 1) | The size of this block in serialized block format, counted in bytes
→<br>`height` | number (int) | Required<br>(exactly 1) | The height of this block on its block chain
→<br>`version` | number (int) | Required<br>(exactly 1) | This block's version number.  See [block version numbers](core-ref-block-chain-block-headers#section-block-versions)
→<br>`versionHex` | string (hex) | Required<br>(exactly 1) | _Added in Bitcoin Core 0.13.0_<br><br>The block version formatted in hexadecimal
→<br>`merkleroot` | string (hex) | Required<br>(exactly 1) | The merkle root for this block, encoded as hex in RPC byte order
→<br>`tx` | array | Required<br>(exactly 1) | An array containing the TXIDs of all transactions in this block.  The transactions appear in the array in the same order they appear in the serialized block
→ →<br>TXID | string (hex) | Required<br>(1 or more) | The TXID of a transaction in this block, encoded as hex in RPC byte order
→<br>`cbTx` | object | Required<br>(exactly 1) | Coinbase special transaction details
→ →<br>`version` | number (int) | Required<br>(exactly 1) | The version of the Coinbase special transaction (CbTx)
→ →<br>`height` | number (int) | Required<br>(exactly 1) | The height of this block on its block chain
→ →<br>`merkleRootMNList` | string (hex) | Required<br>(exactly 1) | The merkle root for the masternode list
→ →<br>`merkleRootQuorums` | string (hex) | Required<br>(exactly 1) | The merkle root for the quorum list
→<br>`time` | number (int) | Required<br>(exactly 1) | The value of the *time* field in the block header, indicating approximately when the block was created
→<br>`mediantime` | number (int) | Required<br>(exactly 1) | *Added in Bitcoin Core 0.12.0*<br><br>The median block time in Unix epoch time  
→<br>`nonce` | number (int) | Required<br>(exactly 1) | The nonce which was successful at turning this particular block into one that could be added to the best block chain
→<br>`bits` | string (hex) | Required<br>(exactly 1) | The value of the *nBits* field in the block header, indicating the target threshold this block's header had to pass
→<br>`difficulty` | number (real) | Required<br>(exactly 1) | The estimated amount of work done to find this block relative to the estimated amount of work done to find block 0
→<br>`chainwork` | string (hex) | Required<br>(exactly 1) | The estimated number of block header hashes miners had to check from the genesis block to this block, encoded as big-endian hex
→<br>`previousblockhash` | string (hex) | Optional<br>(0 or 1) | The hash of the header of the previous block, encoded as hex in RPC byte order.  Not returned for genesis block
→<br>`nextblockhash` | string (hex) | Optional<br>(0 or 1) | The hash of the next block on the best block chain, if known, encoded as hex in RPC byte order

*Result (if verbosity was `2`---a JSON block with full transaction details*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object/null | Required<br>(exactly 1) | An object containing the requested block, or JSON `null` if an error occurred
→<br>`hash` | string (hex) | Required<br>(exactly 1) | The hash of this block's block header encoded as hex in RPC byte order.  This is the same as the hash provided in parameter #1
→<br>`confirmations` | number (int) | Required<br>(exactly 1) | The number of confirmations the transactions in this block have, starting at 1 when this block is at the tip of the best block chain.  This score will be -1 if the the block is not part of the best block chain
→<br>`size` | number (int) | Required<br>(exactly 1) | The size of this block in serialized block format, counted in bytes
→<br>`height` | number (int) | Required<br>(exactly 1) | The height of this block on its block chain
→<br>`version` | number (int) | Required<br>(exactly 1) | This block's version number.  See [block version numbers](core-ref-block-chain-block-headers#section-block-versions)
→<br>`versionHex` | string (hex) | Required<br>(exactly 1) | _Added in Bitcoin Core 0.13.0_<br><br>The block version formatted in hexadecimal
→<br>`merkleroot` | string (hex) | Required<br>(exactly 1) | The merkle root for this block, encoded as hex in RPC byte order
→<br>`tx` | array | Required<br>(exactly 1) | An array containing the TXIDs of all transactions in this block.  The transactions appear in the array in the same order they appear in the serialized block
→ →<br>`txid` | string (hex) | Required<br>(exactly 1) | The transaction's TXID encoded as hex in RPC byte order
→ →<br>`size` | number (int) | Required<br>(exactly 1) | *Added in Bitcoin Core 0.12.0*<br><br>The serialized transaction size
→ →<br>`version` | number (int) | Required<br>(exactly 1) | The transaction format version number
→ →<br>`type` | number (int) | Required<br>(exactly 1) | *Added in Dash Core 0.13.0.0*<br><br>The transaction format type
→ →<br>`locktime` | number (int) | Required<br>(exactly 1) | The transaction's locktime: either a Unix epoch date or block height; see the [locktime parsing rules](core-guide-transactions-locktime-and-sequence-number#locktime_parsing_rules)
→ →<br>`vin` | array | Required<br>(exactly 1) | An array of objects with each object being an input vector (vin) for this transaction.  Input objects will have the same order within the array as they have in the transaction, so the first input listed will be input 0
→ → →<br>Input | object | Required<br>(1 or more) | An object describing one of this transaction's inputs.  May be a regular input or a coinbase
→ → → →<br>`txid` | string | Optional<br>(0 or 1) | The TXID of the outpoint being spent, encoded as hex in RPC byte order.  Not present if this is a coinbase transaction
→ → → →<br>`vout` | number (int) | Optional<br>(0 or 1) | The output index number (vout) of the outpoint being spent.  The first output in a transaction has an index of `0`.  Not present if this is a coinbase transaction
→ → → →<br>`scriptSig` | object | Optional<br>(0 or 1) | An object describing the signature script of this input.  Not present if this is a coinbase transaction
→ → → → →<br>`asm` | string | Required<br>(exactly 1) | The signature script in decoded form with non-data-pushing opcodes listed
→ → → → →<br>`hex` | string (hex) | Required<br>(exactly 1) | The signature script encoded as hex
→ → → →<br>`coinbase` | string (hex) | Optional<br>(0 or 1) | The coinbase (similar to the hex field of a scriptSig) encoded as hex.  Only present if this is a coinbase transaction
→ → → →<br>`value` | number (Dash) | Optional<br>(exactly 1) | The number of Dash paid to this output.  May be `0`.<br><br>Only present if `spentindex` enabled
→ → → →<br>`valueSat` | number (duffs) | Optional<br>(exactly 1) | The number of duffs paid to this output.  May be `0`.<br><br>Only present if `spentindex` enabled
→ → → → →<br>`addresses` | string : array | Optional<br>(0 or 1) | The P2PKH or P2SH addresses used in this transaction, or the computed P2PKH address of any pubkeys in this transaction.  This array will not be returned for `nulldata` or `nonstandard` script types.<br><br>Only present if `spentindex` enabled
→ → → → → →<br>Address | string | Required<br>(1 or more) | A P2PKH or P2SH address
→ → → →<br>`sequence` | number (int) | Required<br>(exactly 1) | The input sequence number
→ →<br>`vout` | array | Required<br>(exactly 1) | An array of objects each describing an output vector (vout) for this transaction.  Output objects will have the same order within the array as they have in the transaction, so the first output listed will be output 0
→ → →<br>Output | object | Required<br>(1 or more) | An object describing one of this transaction's outputs
→ → → →<br>`value` | number (Dash) | Required<br>(exactly 1) | The number of Dash paid to this output.  May be `0`
→ → → →<br>`valueSat` | number (duffs) | Required<br>(exactly 1) | The number of duffs paid to this output.  May be `0`
→ → → →<br>`n` | number (int) | Required<br>(exactly 1) | The output index number of this output within this transaction
→ → → →<br>`scriptPubKey` | object | Required<br>(exactly 1) | An object describing the pubkey script
→ → → → →<br>`asm` | string | Required<br>(exactly 1) | The pubkey script in decoded form with non-data-pushing opcodes listed
→ → → → →<br>`hex` | string (hex) | Required<br>(exactly 1) | The pubkey script encoded as hex
→ → → → →<br>`reqSigs` | number (int) | Optional<br>(0 or 1) | The number of signatures required; this is always `1` for P2PK, P2PKH, and P2SH (including P2SH multisig because the redeem script is not available in the pubkey script).  It may be greater than 1 for bare multisig.  This value will not be returned for `nulldata` or `nonstandard` script types (see the `type` key below)
→ → → → →<br>`type` | string | Optional<br>(0 or 1) | The type of script.  This will be one of the following:<br>• `pubkey` for a P2PK script<br>• `pubkeyhash` for a P2PKH script<br>• `scripthash` for a P2SH script<br>• `multisig` for a bare multisig script<br>• `nulldata` for nulldata scripts<br>• `nonstandard` for unknown scripts
→ → → → →<br>`addresses` | string : array | Optional<br>(0 or 1) | The P2PKH or P2SH addresses used in this transaction, or the computed P2PKH address of any pubkeys in this transaction.  This array will not be returned for `nulldata` or `nonstandard` script types
→ → → → → →<br>Address | string | Required<br>(1 or more) | A P2PKH or P2SH address
→ →<br>`extraPayloadSize` | number (int) | Optional<br>(0 or 1) | *Added in Dash Core 0.13.0.0*<br><br>Size of the DIP2 extra payload. Only present if it's a DIP2 special transaction
→ →<br>`extraPayload` | string (hex) | Optional<br>(0 or 1) | *Added in Dash Core 0.13.0.0*<br><br>Hex encoded DIP2 extra payload data. Only present if it's a DIP2 special transaction
→ →<br>`instantlock` | bool | Required<br>(exactly 1) | If set to `true`, this transaction is locked (by InstantSend or a ChainLock)
→ →<br>`instantlock_internal` | bool | Required<br>(exactly 1) | If set to `true`, this transaction has an InstantSend lock
→<br>`cbTx` | object | Required<br>(exactly 1) | Coinbase special transaction details
→ →<br>`version` | number (int) | Required<br>(exactly 1) | The version of the Coinbase special transaction (CbTx)
→ →<br>`height` | number (int) | Required<br>(exactly 1) | The height of this block on its block chain
→ →<br>`merkleRootMNList` | string (hex) | Required<br>(exactly 1) | The merkle root for the masternode list
→ →<br>`merkleRootQuorums` | string (hex) | Required<br>(exactly 1) | The merkle root for the quorum list
→<br>`time` | number (int) | Required<br>(exactly 1) | The value of the *time* field in the block header, indicating approximately when the block was created
→<br>`mediantime` | number (int) | Required<br>(exactly 1) | *Added in Bitcoin Core 0.12.0*<br><br>The median block time in Unix epoch time  
→<br>`nonce` | number (int) | Required<br>(exactly 1) | The nonce which was successful at turning this particular block into one that could be added to the best block chain
→<br>`bits` | string (hex) | Required<br>(exactly 1) | The value of the *nBits* field in the block header, indicating the target threshold this block's header had to pass
→<br>`difficulty` | number (real) | Required<br>(exactly 1) | The estimated amount of work done to find this block relative to the estimated amount of work done to find block 0
→<br>`chainwork` | string (hex) | Required<br>(exactly 1) | The estimated number of block header hashes miners had to check from the genesis block to this block, encoded as big-endian hex
→<br>`previousblockhash` | string (hex) | Optional<br>(0 or 1) | The hash of the header of the previous block, encoded as hex in RPC byte order.  Not returned for genesis block
→<br>`nextblockhash` | string (hex) | Optional<br>(0 or 1) | The hash of the next block on the best block chain, if known, encoded as hex in RPC byte order
<br>`chainlock` | bool | Required<br>(exactly 1) | *Added in Dash Core 0.14.0*<br><br>If set to `true`, this transaction is in a block that is locked (not susceptible to a chain re-org)

*Example from Dash Core 0.15.0*

Get a block in raw hex:

``` bash
dash-cli -testnet getblock \
            00000000007b0fb99e36713cf08012482478ee496e6dcb4007ad2e806306e62b \
            0
```

Result (wrapped):

``` text
00000020272e374a06c87a0ce0e6ee1a0754c98b9ec2493e7c0ac7ba41a07300\
00000000568b3c4156090db4d8db5447762e95dd1d4c921c96801a9086720ded\
85266325916cc05caa94001c5caf359501030005000100000000000000000000\
00000000000000000000000000000000000000000000ffffffff2703ae50011a\
4d696e656420627920416e74506f6f6c2021000b01201da9196f000000000700\
0000ffffffff02809e4730000000001976a914cbd7bfcc50351180132b2c0698\
cb90ad74c473c788ac809e4730000000001976a91488a060bc2dfe05780ae4dc\
b6c98b12436c35a93988ac00000000460200ae50010078e5c08b39960887bf95\
185c381bdb719e60b6925fa12af78a8824fade927387c757acb6bac63da84f92\
45e20cfd5d830382ac634d434725ca6349ab5db920a3
```

Get the same block in JSON:

``` bash
dash-cli -testnet getblock \
            00000000007b0fb99e36713cf08012482478ee496e6dcb4007ad2e806306e62b
```

Result:

``` json
{
  "hash": "00000000007b0fb99e36713cf08012482478ee496e6dcb4007ad2e806306e62b",
  "confirmations": 73083,
  "size": 310,
  "height": 86190,
  "version": 536870912,
  "versionHex": "20000000",
  "merkleroot": "25632685ed0d7286901a80961c924c1ddd952e764754dbd8b40d0956413c8b56",
  "tx": [
    "25632685ed0d7286901a80961c924c1ddd952e764754dbd8b40d0956413c8b56"
  ],
  "cbTx": {
    "version": 2,
    "height": 86190,
    "merkleRootMNList": "877392defa24888af72aa15f92b6609e71db1b385c1895bf870896398bc0e578",
    "merkleRootQuorums": "a320b95dab4963ca2547434d63ac8203835dfd0ce245924fa83dc6bab6ac57c7"
  },
  "time": 1556114577,
  "mediantime": 1556113720,
  "nonce": 2503323484,
  "bits": "1c0094aa",
  "difficulty": 440.8261075201009,
  "chainwork": "0000000000000000000000000000000000000000000000000045ab6f9403a8e7",
  "previousblockhash": "000000000073a041bac70a7c3e49c29e8bc954071aeee6e00c7ac8064a372e27",
  "nextblockhash": "00000000001c6c962639a1aad4cd069f315560a824d489418dc1f26b50a58aed",
  "chainlock": true
}
```

Get the same block in JSON with transaction details:

``` bash
dash-cli -testnet getblock \
            00000000007b0fb99e36713cf08012482478ee496e6dcb4007ad2e806306e62b 2
```

Result:

``` json
{
  "hash": "00000000007b0fb99e36713cf08012482478ee496e6dcb4007ad2e806306e62b",
  "confirmations": 73084,
  "size": 310,
  "height": 86190,
  "version": 536870912,
  "versionHex": "20000000",
  "merkleroot": "25632685ed0d7286901a80961c924c1ddd952e764754dbd8b40d0956413c8b56",
  "tx": [
    {
      "txid": "25632685ed0d7286901a80961c924c1ddd952e764754dbd8b40d0956413c8b56",
      "version": 3,
      "type": 5,
      "size": 229,
      "locktime": 0,
      "vin": [
        {
          "coinbase": "03ae50011a4d696e656420627920416e74506f6f6c2021000b01201da9196f0000000007000000",
          "sequence": 4294967295
        }
      ],
      "vout": [
        {
          "value": 8.10000000,
          "valueSat": 810000000,
          "n": 0,
          "scriptPubKey": {
            "asm": "OP_DUP OP_HASH160 cbd7bfcc50351180132b2c0698cb90ad74c473c7 OP_EQUALVERIFY OP_CHECKSIG",
            "hex": "76a914cbd7bfcc50351180132b2c0698cb90ad74c473c788ac",
            "reqSigs": 1,
            "type": "pubkeyhash",
            "addresses": [
              "yeuGUfPMrbEqAS2Pw1wonYgEPbM4LAA9LK"
            ]
          }
        },
        {
          "value": 8.10000000,
          "valueSat": 810000000,
          "n": 1,
          "scriptPubKey": {
            "asm": "OP_DUP OP_HASH160 88a060bc2dfe05780ae4dcb6c98b12436c35a939 OP_EQUALVERIFY OP_CHECKSIG",
            "hex": "76a91488a060bc2dfe05780ae4dcb6c98b12436c35a93988ac",
            "reqSigs": 1,
            "type": "pubkeyhash",
            "addresses": [
              "yYmrsYP3XYMZr1cGtha3QzmuNB1C7CfyhV"
            ]
          }
        }
      ],
      "extraPayloadSize": 70,
      "extraPayload": "0200ae50010078e5c08b39960887bf95185c381bdb719e60b6925fa12af78a8824fade927387c757acb6bac63da84f9245e20cfd5d830382ac634d434725ca6349ab5db920a3",
      "cbTx": {
        "version": 2,
        "height": 86190,
        "merkleRootMNList": "877392defa24888af72aa15f92b6609e71db1b385c1895bf870896398bc0e578",
        "merkleRootQuorums": "a320b95dab4963ca2547434d63ac8203835dfd0ce245924fa83dc6bab6ac57c7"
      },
      "instantlock": false,
      "chainlock": false
      "hex": "03000500010000000000000000000000000000000000000000000000000000000000000000ffffffff2703ae50011a4d696e656420627920416e74506f6f6c2021000b01201da9196f0000000007000000ffffffff02809e4730000000001976a914cbd7bfcc50351180132b2c0698cb90ad74c473c788ac809e4730000000001976a91488a060bc2dfe05780ae4dcb6c98b12436c35a93988ac00000000460200ae50010078e5c08b39960887bf95185c381bdb719e60b6925fa12af78a8824fade927387c757acb6bac63da84f9245e20cfd5d830382ac634d434725ca6349ab5db920a3"
    }
  ],
  "cbTx": {
    "version": 2,
    "height": 86190,
    "merkleRootMNList": "877392defa24888af72aa15f92b6609e71db1b385c1895bf870896398bc0e578",
    "merkleRootQuorums": "a320b95dab4963ca2547434d63ac8203835dfd0ce245924fa83dc6bab6ac57c7"
  },
  "time": 1556114577,
  "mediantime": 1556113720,
  "nonce": 2503323484,
  "bits": "1c0094aa",
  "difficulty": 440.8261075201009,
  "chainwork": "0000000000000000000000000000000000000000000000000045ab6f9403a8e7",
  "previousblockhash": "000000000073a041bac70a7c3e49c29e8bc954071aeee6e00c7ac8064a372e27",
  "nextblockhash": "00000000001c6c962639a1aad4cd069f315560a824d489418dc1f26b50a58aed",
  "chainlock": true
}
```

*See also*

* [GetBlockHash](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getblockhash): returns the header hash of a block at the given height in the local best block chain.
* [GetBestBlockHash](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getbestblockhash): returns the header hash of the most recent block on the best block chain.

# GetBlockChainInfo

The [`getblockchaininfo` RPC](core-api-ref-remote-procedure-calls-blockchain#section-getblockchaininfo) provides information about the current state of the block chain.

*Parameters: none*

*Result---A JSON object providing information about the block chain*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | Information about the current state of the local block chain
→<br>`chain` | string | Required<br>(exactly 1) | The name of the block chain.  One of `main` for mainnet, `test` for testnet, or `regtest` for regtest
→<br>`blocks` | number (int) | Required<br>(exactly 1) | The number of validated blocks in the local best block chain.  For a new node with just the hardcoded genesis block, this will be 0
→<br>`headers` | number (int) | Required<br>(exactly 1) | The number of validated headers in the local best headers chain.  For a new node with just the hardcoded genesis block, this will be zero.  This number may be higher than the number of *blocks*
→<br>`bestblockhash` | string (hex) | Required<br>(exactly 1) | The hash of the header of the highest validated block in the best block chain, encoded as hex in RPC byte order.  This is identical to the string returned by the [`getbestblockhash` RPC](core-api-ref-remote-procedure-calls-blockchain#section-getbestblockhash)
→<br>`difficulty` | number (real) | Required<br>(exactly 1) | The difficulty of the highest-height block in the best block chain
→<br>`mediantime` | number (int) | Required<br>(exactly 1) | *Added in Bitcoin Core 0.12.0*<br><br>The median time of the 11 blocks before the most recent block on the blockchain.  Used for validating transaction locktime under BIP113
→<br>`verificationprogress` | number (real) | Required<br>(exactly 1) | Estimate of what percentage of the block chain transactions have been verified so far, starting at 0.0 and increasing to 1.0 for fully verified.  May slightly exceed 1.0 when fully synced to account for transactions in the memory pool which have been verified before being included in a block
→<br>`chainwork` | string (hex) | Required<br>(exactly 1) | The estimated number of block header hashes checked from the genesis block to this block, encoded as big-endian hex
→<br>`pruned` | bool | Required<br>(exactly 1) | *Added in Bitcoin Core 0.11.0*<br><br>Indicates if the blocks are subject to pruning
→<br>`pruneheight` | number (int) | Optional<br>(0 or 1) | *Added in Bitcoin Core 0.11.0*<br><br>The lowest-height complete block stored if pruning is activated
→<br>`softforks` | array | Required<br>(exactly 1) | *Added in Bitcoin Core 0.12.0*<br><br>An array of objects each describing a current or previous soft fork
→ →<br>Softfork | object | Required<br>(0 or more) | A specific softfork
→ → →<br>`id` | string | Required<br>(exactly 1) | The name of the softfork
→ → →<br>`version` | numeric<br>(int) | Required<br>(exactly 1) | The block version used for the softfork
→ → →<br>`enforce` | string : object | Optional<br>(0 or 1) | The progress toward enforcing the softfork rules for new-version blocks
→ → → →<br>`status` | bool | Required<br>(exactly 1) | Indicates if the threshold was reached
→ → → →<br>`found` | numeric<br>(int) | Optional<br>(0 or 1) | Number of blocks that support the softfork
→ → → →<br>`required` | numeric<br>(int) | Optional<br>(0 or 1) | Number of blocks that are required to reach the threshold
→ → → →<br>`window` | numeric<br>(int) | Optional<br>(0 or 1) | The maximum size of examined window of recent blocks
→ → →<br>`reject` | object | Optional<br>(0 or 1) | The progress toward enforcing the softfork rules for new-version blocks
→ → → →<br>`status` | bool | Optional<br>(0 or 1) | Indicates if the threshold was reached
→ → → →<br>`found` | numeric<br>(int) | Optional<br>(0 or 1) | Number of blocks that support the softfork
→ → → →<br>`required` | numeric<br>(int) | Optional<br>(0 or 1) | Number of blocks that are required to reach the threshold
→ → → →<br>`window` | numeric<br>(int) | Optional<br>(0 or 1) | The maximum size of examined window of recent blocks
→<br>`bip9_softforks` | object | Required<br>(exactly 1) | *Added in Bitcoin Core 0.12.1*<br><br>The status of BIP9 softforks in progress
→ →<br>Name | string : object | Required<br>(0 or more) | A specific BIP9 softfork
→ → →<br>`status` | string | Required<br>(exactly 1) | Set to one of the following reasons:<br>• `defined` if voting hasn't started yet<br>• `started` if the voting has started <br>• `locked_in` if the voting was successful but the softfork hasn't been activated yet<br>• `active` if the softfork was activated<br>• `failed` if the softfork has not receieved enough votes
→ → →<br>`bit` | numeric<br>(int) | Optional<br>(0 or 1) | The bit (0-28) in the block version field used to signal this softfork.  Field is only shown when status is `started`
→ → →<br>`startTime` | numeric<br>(int) | Required<br>(exactly 1) | The Unix epoch time when the softfork voting begins
→ → →<br>`timeout` | numeric<br>(int) | Required<br>(exactly 1) | The Unix epoch time at which the deployment is considered failed if not yet locked in
→ → →<br>`since` | numeric<br>(int) | Required<br>(exactly 1) | *Added in Bitcoin Core 0.14.0*<br><br>The height of the first block to which the status applies
→ → →<br>`statistics` | string : object | Required<br>(exactly 1) | *Added in Dash Core 0.15.0*<br><br>Numeric statistics about BIP9 signaling for a softfork (only for \started\" status)"
→ → → →<br>`period` | numeric<br>(int) | Optional<br>(0 or 1) | *Added in Dash Core 0.15.0*<br><br>The length in blocks of the BIP9 signaling period.  Field is only shown when status is `started`
→ → → →<br>`threshold` | numeric<br>(int) | Optional<br>(0 or 1) | *Added in Dash Core 0.15.0*<br><br>The number of blocks with the version bit set required to activate the feature.  Field is only shown when status is `started`
→ → → →<br>`elapsed` | numeric<br>(int) | Optional<br>(0 or 1) | *Added in Dash Core 0.15.0*<br><br>The number of blocks elapsed since the beginning of the current period.  Field is only shown when status is `started`
→ → → →<br>`count` | numeric<br>(int) | Optional<br>(0 or 1) | *Added in Dash Core 0.15.0*<br><br>The number of blocks with the version bit set in the current period.  Field is only shown when status is `started`
→<br>`possible` | bool | Optional<br>(0 or 1) | *Added in Bitcoin Core 0.11.0*<br><br>Returns false if there are not enough blocks left in this period to pass activation threshold.  Field is only shown when status is `started`

*Example from Dash Core 0.15.0*

``` bash
dash-cli -testnet getblockchaininfo
```

Result:

``` json
{
  "chain": "test",
  "blocks": 160508,
  "headers": 160508,
  "bestblockhash": "0000000008ae87c2999faa79c74727ab2a15783fcab515cc940a6c14dfa921a8",
  "difficulty": 24.71182965485547,
  "mediantime": 1566479773,
  "verificationprogress": 0.9999986039171913,
  "chainwork": "0000000000000000000000000000000000000000000000000077db2024e1810b",
  "pruned": false,
  "softforks": [
    {
      "id": "bip34",
      "version": 2,
      "reject": {
        "status": true
      }
    },
    {
      "id": "bip66",
      "version": 3,
      "reject": {
        "status": true
      }
    },
    {
      "id": "bip65",
      "version": 4,
      "reject": {
        "status": true
      }
    }
  ],
  "bip9_softforks": {
    "csv": {
      "status": "active",
      "startTime": 1544655600,
      "timeout": 1576191600,
      "since": 8064
    },
    "dip0001": {
      "status": "active",
      "startTime": 1544655600,
      "timeout": 1576191600,
      "since": 4400
    },
    "dip0003": {
      "status": "active",
      "startTime": 1544655600,
      "timeout": 1576191600,
      "since": 7000
    },
    "dip0008": {
      "status": "active",
      "startTime": 1553126400,
      "timeout": 1584748800,
      "since": 78800
    },
    "bip147": {
      "status": "active",
      "startTime": 1544655600,
      "timeout": 1576191600,
      "since": 4300
    }
  }
}
```

*See also*

* [GetMiningInfo](/docs/core-api-ref-remote-procedure-calls-mining#section-getmininginfo): returns various mining-related information.
* [GetNetworkInfo](/docs/core-api-ref-remote-procedure-calls-network#section-getnetworkinfo): returns information about the node's connection to the network.
* [GetWalletInfo](/docs/core-api-ref-remote-procedure-calls-wallet#section-getwalletinfo): provides information about the wallet.

# GetBlockCount

The [`getblockcount` RPC](core-api-ref-remote-procedure-calls-blockchain#section-getblockcount) returns the number of blocks in the local best block chain.

*Parameters: none*

*Result---the number of blocks in the local best block chain*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | number (int) | Required<br>(exactly 1) | The number of blocks in the local best block chain.  For a new node with only the hardcoded genesis block, this number will be 0

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet getblockcount
```

Result:

``` text
4627
```

*See also*

* [GetBlockHash](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getblockhash): returns the header hash of a block at the given height in the local best block chain.
* [GetBlock](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getblock): gets a block with a particular header hash from the local block database either as a JSON object or as a serialized block.

# GetBlockHash

The [`getblockhash` RPC](core-api-ref-remote-procedure-calls-blockchain#section-getblockhash) returns the header hash of a block at the given height in the local best block chain.

*Parameter---a block height*

Name | Type | Presence | Description
--- | --- | --- | ---
Block Height | number (int) | Required<br>(exactly 1) | The height of the block whose header hash should be returned.  The height of the hardcoded genesis block is 0

*Result---the block header hash*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string (hex)/null | Required<br>(exactly 1) | The hash of the block at the requested height, encoded as hex in RPC byte order, or JSON `null` if an error occurred

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet getblockhash 4000
```

Result:

``` text
00000ce22113f3eb8636e225d6a1691e132fdd587aed993e1bc9b07a0235eea4
```

*See also*

* [GetBlock](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getblock): gets a block with a particular header hash from the local block database either as a JSON object or as a serialized block.
* [GetBestBlockHash](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getbestblockhash): returns the header hash of the most recent block on the best block chain.

# GetBlockHashes

*Added in Dash Core 0.12.1*

The [`getblockhashes` RPC](core-api-ref-remote-procedure-calls-blockchain#section-getblockhashes) returns array of hashes of blocks within the timestamp range provided (requires `timestampindex` to be enabled).

*Parameter #1---high block timestamp*

Name | Type | Presence | Description
--- | --- | --- | ---
Block Timestamp | number (int) | Required<br>(exactly 1) | The block timestamp for the newest block hash that should be returned.

*Parameter #2---low block timestamp*

Name | Type | Presence | Description
--- | --- | --- | ---
Block Timestamp | number (int) | Required<br>(exactly 1) | The block timestamp for the oldest block hash that should be returned.

*Result---the block header hashes in the give time range*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | The hashes of the blocks in the requested time range
→<br>`hash` | string (hex) | Required<br>(1 or more) | The hash of a block in the chain, encoded as hex in RPC byte order

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet getblockhashes 1507555793 1507554793
```

Result:

``` json
[
  "0000000010a16c6fbc6bd5cdc238c2beabcda334e97fde1500d59be4e6fc4b89",
  "000000009910885e811230c403e55aac6547d6df04ee671b2e8348524f73cab8",
  "000000004bbb3828db1c4d4491760336cec215087819ab656336f30d4095e3d2",
  "00000000ad2df2149aca2261a9a87c41e139dfe8f73d91db7ec0c1837fee21a0",
  "0000000074068a9e3a271d165da3deb28bc3f8c751dde97f460d8078d92a9d06"
]
```

*See also*

* [GetBlock](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getblock): gets a block with a particular header hash from the local block database either as a JSON object or as a serialized block.
* [GetBlockHash](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getblockhash): returns the header hash of a block at the given height in the local best block chain.
* [GetBestBlockHash](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getbestblockhash): returns the header hash of the most recent block on the best block chain.

# GetBlockHeader

*Added in Bitcoin Core 0.12.0*

The [`getblockheader` RPC](core-api-ref-remote-procedure-calls-blockchain#section-getblockheader) gets a block header with a particular header hash from the local block database either as a JSON object or as a serialized block header.

*Parameter #1---header hash*

Name | Type | Presence | Description
--- | --- | --- | ---
Header Hash | string (hex) | Required<br>(exactly 1) | The hash of the block header to get, encoded as hex in RPC byte order

*Parameter #2---JSON or hex output*

Name | Type | Presence | Description
--- | --- | --- | ---
Format | bool | Optional<br>(0 or 1) | Set to `false` to get the block header in serialized block format; set to `true` (the default) to get the decoded block header as a JSON object

*Result (if format was `false`)---a serialized block header*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string (hex)/null | Required<br>(exactly 1) | The requested block header as a serialized block, encoded as hex, or JSON `null` if an error occurred

*Result (if format was `true` or omitted)---a JSON block header*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object/null | Required<br>(exactly 1) | An object containing the requested block, or JSON `null` if an error occurred
→<br>`hash` | string (hex) | Required<br>(exactly 1) | The hash of this block's block header encoded as hex in RPC byte order.  This is the same as the hash provided in parameter #1
→<br>`confirmations` | number (int) | Required<br>(exactly 1) | The number of confirmations the transactions in this block have, starting at 1 when this block is at the tip of the best block chain.  This score will be -1 if the the block is not part of the best block chain
→<br>`height` | number (int) | Required<br>(exactly 1) | The height of this block on its block chain
→<br>`version` | number (int) | Required<br>(exactly 1) | This block's version number.  See [block version numbers](core-ref-block-chain-block-headers#section-block-versions)
→<br>`merkleroot` | string (hex) | Required<br>(exactly 1) | The merkle root for this block, encoded as hex in RPC byte order
→<br>`time` | number (int) | Required<br>(exactly 1) | The time of the block  
→<br>`mediantime` | number (int) | Required<br>(exactly 1) | The computed median time of the previous 11 blocks.  Used for validating transaction locktime under BIP113
→<br>`nonce` | number (int) | Required<br>(exactly 1) | The nonce which was successful at turning this particular block into one that could be added to the best block chain
→<br>`bits` | string (hex) | Required<br>(exactly 1) | The value of the *nBits* field in the block header, indicating the target threshold this block's header had to pass
→<br>`difficulty` | number (real) | Required<br>(exactly 1) | The estimated amount of work done to find this block relative to the estimated amount of work done to find block 0
→<br>`chainwork` | string (hex) | Required<br>(exactly 1) | The estimated number of block header hashes miners had to check from the genesis block to this block, encoded as big-endian hex
→<br>`previousblockhash` | string (hex) | Optional<br>(0 or 1) | The hash of the header of the previous block, encoded as hex in RPC byte order.  Not returned for genesis block
→<br>`nextblockhash` | string (hex) | Optional<br>(0 or 1) | The hash of the next block on the best block chain, if known, encoded as hex in RPC byte order

*Changes from Bitcoin - Following items not present in Dash result*

Name | Type | Presence | Description
--- | --- | --- | ---
→<br>`versionHex` | number (hex) | Required<br>(exactly 1) | This block's hex version number.  See [block version numbers](core-ref-block-chain-block-headers#section-block-versions)

*Example from Dash Core 0.12.2*

Get a block header in raw hex:

``` bash
dash-cli -testnet getblockheader \
            00000000eb0af5aec7b673975a22593dc0cc763f71ba8de26292410273437078 \
            false
```

Result (wrapped):

``` text
01000020f61396cfd2747e94cfa088fe1f7875d8171accc22d6e5616edca0cb8\
00000000c31eb96ee1d9e78d61a601371a348c19e4e59698d0ff7869334b72cb\
7ffb76893b41d6593016011d09b2aa3c

```

Get the same block in JSON:

``` bash
dash-cli -testnet getblockheader \
            00000000eb0af5aec7b673975a22593dc0cc763f71ba8de26292410273437078
```

Result:

``` json

{
  "hash": "00000000eb0af5aec7b673975a22593dc0cc763f71ba8de26292410273437078",
  "confirmations": 7,
  "height": 4635,
  "version": 536870913,
  "merkleroot": "8976fb7fcb724b336978ffd09896e5e4198c341a3701a6618de7d9e16eb91ec3",
  "time": 1507213627,
  "mediantime": 1507213022,
  "nonce": 1017819657,
  "bits": "1d011630",
  "difficulty": 0.920228600314536,
  "chainwork": "000000000000000000000000000000000000000000000000000001e06428c09a",
  "previousblockhash": "00000000b80ccaed16566e2dc2cc1a17d875781ffe88a0cf947e74d2cf9613f6",
  "nextblockhash": "000000003b1aa290db62ae7cfb4dbb67c8e1402a40ef387587f930b8ec3b45db"
}

```

*See also*

* [GetBlock](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getblock): gets a block with a particular header hash from the local block database either as a JSON object or as a serialized block.
* [GetBlockHash](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getblockhash): returns the header hash of a block at the given height in the local best block chain.
* [GetBlockHashes](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getblockhashes): returns array of hashes of blocks within the timestamp range provided (requires `timestampindex` to be enabled).
* [GetBlockHeaders](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getblockheaders): returns an array of items with information about the requested number of blockheaders starting from the requested hash.
* [GetBestBlockHash](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getbestblockhash): returns the header hash of the most recent block on the best block chain.

# GetBlockHeaders

*Added in Dash Core 0.12.1*

The [`getblockheaders` RPC](core-api-ref-remote-procedure-calls-blockchain#section-getblockheaders) returns an array of items with information about the requested number of blockheaders starting from the requested hash.

*Parameter #1---header hash*

Name | Type | Presence | Description
--- | --- | --- | ---
Header Hash | string (hex) | Required<br>(exactly 1) | The hash of the block header to get, encoded as hex in RPC byte order

*Parameter #2---number of headers to return*

Name | Type | Presence | Description
--- | --- | --- | ---
Count | number | Optional<br>(exactly 1) | The number of block headers to get

*Parameter #3---JSON or hex output*

Name | Type | Presence | Description
--- | --- | --- | ---
Verbose | bool | Optional<br>(0 or 1) | Set to `false` to get the block headers in serialized block format; set to `true` (the default) to get the decoded block headers as a JSON object

*Result (if format was `false`)---a serialized block header*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | The requested block header(s) as a serialized block
→<br>`header` | string (hex) | Required<br>(1 or more) | The block header encoded as hex in RPC byte order

*Result (if format was `true` or omitted)---a JSON block header*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | An array of objects each containing a block header, or JSON `null` if an error occurred
→<br>Block Header | object/null | Required<br>(exactly 1) | An object containing a block header
→ →<br>`hash` | string (hex) | Required<br>(exactly 1) | The hash of this block's block header encoded as hex in RPC byte order.  This is the same as the hash provided in parameter #1
→ →<br>`confirmations` | number (int) | Required<br>(exactly 1) | The number of confirmations the transactions in this block have, starting at 1 when this block is at the tip of the best block chain.  This score will be -1 if the the block is not part of the best block chain
→ →<br>`height` | number (int) | Required<br>(exactly 1) | The height of this block on its block chain
→ →<br>`version` | number (int) | Required<br>(exactly 1) | This block's version number.  See [block version numbers](core-ref-block-chain-block-headers#section-block-versions)
→ →<br>`merkleroot` | string (hex) | Required<br>(exactly 1) | The merkle root for this block, encoded as hex in RPC byte order
→ →<br>`time` | number (int) | Required<br>(exactly 1) | The time of the block
→ →<br>`mediantime` | number (int) | Required<br>(exactly 1) | The computed median time of the previous 11 blocks.  Used for validating transaction locktime under BIP113
→ →<br>`nonce` | number (int) | Required<br>(exactly 1) | The nonce which was successful at turning this particular block into one that could be added to the best block chain
→ →<br>`bits` | string (hex) | Required<br>(exactly 1) | The value of the *nBits* field in the block header, indicating the target threshold this block's header had to pass
→ →<br>`difficulty` | number (real) | Required<br>(exactly 1) | The estimated amount of work done to find this block relative to the estimated amount of work done to find block 0
→<br>`chainwork` | string (hex) | Required<br>(exactly 1) | The estimated number of block header hashes miners had to check from the genesis block to this block, encoded as big-endian hex
→ →<br>`previousblockhash` | string (hex) | Optional<br>(0 or 1) | The hash of the header of the previous block, encoded as hex in RPC byte order.  Not returned for genesis block
→ →<br>`nextblockhash` | string (hex) | Optional<br>(0 or 1) | The hash of the next block on the best block chain, if known, encoded as hex in RPC byte order

*Example from Dash Core 0.12.2*

Get two block headers in raw hex:

``` bash
dash-cli -testnet getblockheaders \
            0000000010a16c6fbc6bd5cdc238c2beabcda334e97fde1500d59be4e6fc4b89 \
            2 false
```

Result (wrapped):

``` text
[
  "010000207216dc7b7c898ba3fc0b39d1fd16756b97b1e07e3eb5c64d1510a64b0000000\
   0bb64e58a0be4276bf3e9c366bba960953ef9e47a8f62342476be56a5dfa7a2670276db\
   59eae1001d0735577e",
  "01000020894bfce6e49bd50015de7fe934a3cdabbec238c2cdd56bbc6f6ca1100000000\
   0edb2a018d535de70b0622a3303dc329dcb315e7507d074c0c641501c58d88aa08576db\
   59c5db001d03cf8986"
]
```

Get the same two block headers in JSON:

``` bash
dash-cli -testnet getblockheader \
            00000000eb0af5aec7b673975a22593dc0cc763f71ba8de26292410273437078 \
            2 true
```

Result:

``` json
[
  {
    "hash": "0000000010a16c6fbc6bd5cdc238c2beabcda334e97fde1500d59be4e6fc4b89",
    "confirmations": 20,
    "height": 6802,
    "version": 536870913,
    "merkleroot": "67a2a7dfa556be762434628f7ae4f93e9560a9bb66c3e9f36b27e40b8ae564bb",
    "time": 1507554818,
    "mediantime": 1507554058,
    "nonce": 2119644423,
    "bits": "1d00e1ea",
    "difficulty": 1.1331569664903,
    "chainwork": "0000000000000000000000000000000000000000000000000000092c7b511197",
    "previousblockhash": "000000004ba610154dc6b53e7ee0b1976b7516fdd1390bfca38b897c7bdc1672",
    "nextblockhash": "000000009910885e811230c403e55aac6547d6df04ee671b2e8348524f73cab8"
  },
  {
    "hash": "000000009910885e811230c403e55aac6547d6df04ee671b2e8348524f73cab8",
    "confirmations": 19,
    "height": 6803,
    "version": 536870913,
    "merkleroot": "a08ad8581c5041c6c074d007755e31cb9d32dc03332a62b070de35d518a0b2ed",
    "time": 1507554949,
    "mediantime": 1507554181,
    "nonce": 2257178371,
    "bits": "1d00dbc5",
    "difficulty": 1.164838875953147,
    "chainwork": "0000000000000000000000000000000000000000000000000000092da5851d38",
    "previousblockhash": "0000000010a16c6fbc6bd5cdc238c2beabcda334e97fde1500d59be4e6fc4b89",
    "nextblockhash": "000000004bbb3828db1c4d4491760336cec215087819ab656336f30d4095e3d2"
  }
]
```

*See also*

* [GetBlock](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getblock): gets a block with a particular header hash from the local block database either as a JSON object or as a serialized block.
* [GetBlockHash](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getblockhash): returns the header hash of a block at the given height in the local best block chain.
* [GetBlockHashes](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getblockhashes): returns array of hashes of blocks within the timestamp range provided (requires `timestampindex` to be enabled).
* [GetBlockHeader](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getblockheader): gets a block header with a particular header hash from the local block database either as a JSON object or as a serialized block header.
* [GetBestBlockHash](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getbestblockhash): returns the header hash of the most recent block on the best block chain.

# GetBlockStats

The [`getblockstats` RPC](core-api-ref-remote-procedure-calls-blockchain#section-getblockstats) computes per block statistics for a given window.

All amounts are in duffs.

It won't work for some heights with pruning. It won't work without `-txindex` for
`utxo_size_inc`, `*fee` or `*feerate` stats.

*Parameter #1---hash_or_height*

Name | Type | Presence | Description
--- | --- | --- | ---
hash_or_height | string or numeric | Required<br>(exactly 1) | The block hash or height of the target block

*Parameter #2---stats*

Name | Type | Presence | Description
--- | --- | --- | ---
stats | array | optional | Values to plot, by default all values (see result below)

*Result---a JSON object containing the requested statistics*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object/null | Required<br>(exactly 1) | An object containing stats for the requested block, or JSON `null` if an error occurred
→<br>`avgfee` | numeric | Required<br>(exactly 1) | Average fee in the block
→<br>`avgfeerate` | numeric | Required<br>(exactly 1) | Average feerate (in duffs per byte)
→<br>`avgtxsize` | numeric | Required<br>(exactly 1) | Average transaction size
→<br>`blockhash` | string (hex) | Required<br>(exactly 1) | The block hash (to check for potential reorgs)
→<br>`height` | numeric | Required<br>(exactly 1) | The height of the block
→<br>`ins` | numeric | Required<br>(exactly 1) | The number of inputs (excluding coinbase)
→<br>`maxfee` | numeric | Required<br>(exactly 1) | Maximum fee in the block
→<br>`maxfeerate` | numeric | Required<br>(exactly 1) | Maximum feerate (in duffs per byte)
→<br>`maxtxsize` | numeric | Required<br>(exactly 1) | Maximum transaction size
→<br>`medianfee` | numeric | Required<br>(exactly 1) | Truncated median fee in the block
→<br>`medianfeerate` | numeric | Required<br>(exactly 1) | Truncated median feerate (in duffs per byte)
→<br>`mediantime` | numeric | Required<br>(exactly 1) | The block median time past
→<br>`mediantxsize` | numeric | Required<br>(exactly 1) | Truncated median transaction size
→<br>`minfee` | numeric | Required<br>(exactly 1) | Minimum fee in the block
→<br>`minfeerate` | numeric | Required<br>(exactly 1) | Minimum feerate (in duffs per byte)
→<br>`mintxsize` | numeric | Required<br>(exactly 1) | Minimum transaction size
→<br>`outs` | numeric | Required<br>(exactly 1) | The number of outputs
→<br>`subsidy` | numeric | Required<br>(exactly 1) | The block subsidy
→<br>`time` | number (real) | Required<br>(exactly 1) | The block time
→<br>`total_out` | numeric | Required<br>(exactly 1) | Total amount in all outputs (excluding coinbase and thus reward [i.e. subsidy + totalfee])
→<br>`total_size` | numeric | Required<br>(exactly 1) | Total size of all non-coinbase transactions
→<br>`totalfee` | numeric | Required<br>(exactly 1) | The fee total
→<br>`txs` | numeric | Required<br>(exactly 1) | The number of transactions (excluding coinbase)
→<br>`utxo_increase` | numeric | Required<br>(exactly 1) | The increase/decrease in the number of unspent outputs
→<br>`utxo_size_inc` | numeric | Required<br>(exactly 1) | The increase/decrease in size for the utxo index (not discounting op_return and similar)

*Example from Dash Core 0.15.0*

``` bash
dash-cli getblockstats 1000 '["blockhash","subsidy", "txs"]'
```

Result:
``` json
{
  "blockhash": "000004e906762c8c70583418d46915b4271fa83c29d5b88544d05e09e3f3621d",
  "subsidy": 50000000000,
  "txs": 1
}
```

*See also: none*

# GetChainTips

The [`getchaintips` RPC](core-api-ref-remote-procedure-calls-blockchain#section-getchaintips) returns information about the highest-height block (tip) of each local block chain.

*Parameters: none*

*Result---an array of block chain tips*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | An array of JSON objects, with each object describing a chain tip.  At least one tip---the local best block chain---will always be present
→<br>Tip | object | Required<br>(1 or more) | An object describing a particular chain tip.  The first object will always describe the active chain (the local best block chain)
→ →<br>`height` | number (int) | Required<br>(exactly 1) | The height of the highest block in the chain.  A new node with only the genesis block will have a single tip with height of 0
→ →<br>`hash` | string (hex) | Required<br>(exactly 1) | The hash of the highest block in the chain, encoded as hex in RPC byte order
→<br>`difficulty` | number (real) | Required<br>(exactly 1) | The difficulty of the highest-height block in the best block chain (Added in Dash Core 0.12.1)
→<br>`chainwork` | string (hex) | Required<br>(exactly 1) | The estimated number of block header hashes checked from the genesis block to this block, encoded as big-endian hex (Added in Dash Core 0.12.1)
→ →<br>`branchlen` | number (int) | Required<br>(exactly 1) | The number of blocks that are on this chain but not on the main chain.  For the local best block chain, this will be `0`; for all other chains, it will be at least `1`
→ →<br>`forkpoint` | string (hex) | Required<br>(exactly 1) | *Added in Dash Core 0.12.3*<br><br>Block hash of the last common block between this tip and the main chain
→ →<br>`status` | string | Required<br>(exactly 1) | The status of this chain.  Valid values are:<br>• `active` for the local best block chain<br>• `invalid` for a chain that contains one or more invalid blocks<br>• `headers-only` for a chain with valid headers whose corresponding blocks both haven't been validated and aren't stored locally<br>• `valid-headers` for a chain with valid headers whose corresponding blocks are stored locally, but which haven't been fully validated<br>• `valid-fork` for a chain which is fully validated but which isn't part of the local best block chain (it was probably the local best block chain at some point)<br>• `unknown` for a chain whose reason for not being the active chain is unknown

*Example from Dash Core 0.12.3*

``` bash
dash-cli -testnet getchaintips
```

``` json
[
  {
    "height": 110192,
    "hash": "000000000c6007f40c3b68a77b0e1319a89c0504ae1b391d071cf49fa7591dee",
    "difficulty": 18.38631407059958,
    "chainwork": "000000000000000000000000000000000000000000000000002cbd2546718747",
    "branchlen": 0,
    "forkpoint": "000000000c6007f40c3b68a77b0e1319a89c0504ae1b391d071cf49fa7591dee",
    "status": "active"
  }
]
```

*See also*

* [GetBestBlockHash](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getbestblockhash): returns the header hash of the most recent block on the best block chain.
* [GetBlock](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getblock): gets a block with a particular header hash from the local block database either as a JSON object or as a serialized block.
* [GetBlockChainInfo](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getblockchaininfo): provides information about the current state of the block chain.

# GetChainTxStats

The [`getchaintxstats` RPC](core-api-ref-remote-procedure-calls-blockchain#section-getchaintxstats) compute statistics about the total number and rate of transactions in the chain.

*Parameter #1---nblocks*

Name | Type | Presence | Description
--- | --- | --- | ---
nblocks | number (int) | Optional | Size of the window in number of blocks (default: one month).

*Parameter #2---blockhash*

Name | Type | Presence | Description
--- | --- | --- | ---
blockhash | string | Optional | The hash of the block that ends the window.

*Result--statistics about transactions*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | Object containing transaction statistics
→<br>`time` | number (int) | Required<br>(exactly 1) | The timestamp for the statistics in UNIX format
→<br>`txcount` | number (int) | Required<br>(exactly 1) | The total number of transactions in the chain up to that point
→<br>`txrate` | number (int) | Required<br>(exactly 1) | The average rate of transactions per second in the window

*Example from Dash Core 0.15.0*

``` bash
dash-cli -testnet getchaintxstats
```

Result:
``` json
{
  "time": 1566416832,
  "txcount": 1353139,
  "txrate": 0.04107376448354556
}
```

*See also: none*

# GetDifficulty

The [`getdifficulty` RPC](core-api-ref-remote-procedure-calls-blockchain#section-getdifficulty) returns the proof-of-work difficulty as a multiple of the minimum difficulty.

*Parameters: none*

*Result---the current difficulty*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | number (real) | Required<br>(exactly 1) | The difficulty of creating a block with the same target threshold (nBits) as the highest-height block in the local best block chain.  The number is a a multiple of the minimum difficulty

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet getdifficulty
```

Result:

``` text
1.069156225528583
```

*See also*

* [GetNetworkHashPS](/docs/core-api-ref-remote-procedure-calls-mining#section-getnetworkhashps): returns the estimated network hashes per second based on the last n blocks.
* [GetMiningInfo](/docs/core-api-ref-remote-procedure-calls-mining#section-getmininginfo): returns various mining-related information.

# GetMemPoolAncestors

*Added in Dash Core 0.12.3*

The [`getmempoolancestors` RPC](core-api-ref-remote-procedure-calls-blockchain#section-getmempoolancestors) returns all in-mempool ancestors for a transaction in the mempool.

*Parameter #1---a transaction identifier (TXID)*

Name | Type | Presence | Description
--- | --- | --- | ---
TXID | string (hex) | Required<br>(exactly 1) | The TXID of a transaction in the memory pool, encoded as hex in RPC byte order

*Parameter #2---desired output format*

Name | Type | Presence | Description
--- | --- | --- | ---
Format | bool | Optional<br>(0 or 1) | Set to `true` to get json objects describing each transaction in the memory pool; set to `false` (the default) to only get an array of TXIDs

*Result---list of ancestor transactions*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | An array of TXIDs belonging to transactions in the memory pool.  The array may be empty if there are no transactions in the memory pool
→<br>TXID | string | Optional<br>(0 or more) | The TXID of a transaction in the memory pool, encoded as hex in RPC byte order

*Result (format: `true`)---a JSON object describing each transaction*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | A object containing transactions currently in the memory pool.  May be empty
→<br>TXID | string : object | Optional<br>(0 or more) | The TXID of a transaction in the memory pool, encoded as hex in RPC byte order
→ →<br>`size` | number (int) | Required<br>(exactly 1) | The size of the serialized transaction in bytes
→ →<br>`fee` | number (bitcoins) | Required<br>(exactly 1) | The transaction fee paid by the transaction in decimal bitcoins
→ →<br>`modifiedfee` | number (bitcoins) | Required<br>(exactly 1) | The transaction fee with fee deltas used for mining priority in decimal bitcoins
→ →<br>`time` | number (int) | Required<br>(exactly 1) | The time the transaction entered the memory pool, Unix epoch time format
→ →<br>`height` | number (int) | Required<br>(exactly 1) | The block height when the transaction entered the memory pool
→ →<br>`descendantcount` | number (int) | Required<br>(exactly 1) | The number of in-mempool descendant transactions (including this one)
→ →<br>`descendantsize` | number (int) | Required<br>(exactly 1) | The size of in-mempool descendants (including this one)
→ →<br>`descendantfees` | number (int) | Required<br>(exactly 1) | The modified fees (see `modifiedfee` above) of in-mempool descendants (including this one)
→ →<br>`ancestorcount` | number (int) | Required<br>(exactly 1) | The number of in-mempool ancestor transactions (including this one)
→ →<br>`ancestorsize` | number (int) | Required<br>(exactly 1) | The size of in-mempool ancestors (including this one)
→ →<br>`ancestorfees` | number (int) | Required<br>(exactly 1) | The modified fees (see `modifiedfee` above) of in-mempool ancestors (including this one)
→ →<br>`depends` | array | Required<br>(exactly 1) | An array holding TXIDs of unconfirmed transactions this transaction depends upon (parent transactions).  Those transactions must be part of a block before this transaction can be added to a block, although all transactions may be included in the same block.  The array may be empty
→ → →<br>Depends TXID | string | Optional (0 or more) | The TXIDs of any unconfirmed transactions this transaction depends upon, encoded as hex in RPC byte order

*Examples from Dash Core 0.12.3*

The default (`false`):

``` bash
dash-cli getmempoolancestors 49a512c3d567effd4f605a6023df8b4b523\
ac0ae7bccbaeed1c8a7db1e05e15a
```

Result:

``` json
[
  "d1eefe8a006e2c21b55bc97c1f5b10000d63aa6a777bb11abc0daf62e4296660"
]
```

Verbose output (`true`):

``` bash
dash-cli getmempoolancestors 49a512c3d567effd4f605a6023df8b4b523\
ac0ae7bccbaeed1c8a7db1e05e15a true
```

Result:

``` json
{
  "d1eefe8a006e2c21b55bc97c1f5b10000d63aa6a777bb11abc0daf62e4296660": {
    "size": 963,
    "fee": 0.00000966,
    "modifiedfee": 0.00000966,
    "time": 1519160516,
    "height": 79045,
    "descendantcount": 2,
    "descendantsize": 1189,
    "descendantfees": 1192,
    "ancestorcount": 1,
    "ancestorsize": 963,
    "ancestorfees": 966,
    "depends": [
    ]
  }
}
```

*See also*

* [GetMemPoolDescendants](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getmempooldescendants): returns all in-mempool descendants for a transaction in the mempool.
* [GetRawMemPool](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getrawmempool): returns all transaction identifiers (TXIDs) in the memory pool as a JSON array, or detailed information about each transaction in the memory pool as a JSON object.

# GetMemPoolDescendants

*Added in Dash Core 0.12.3*

The [`getmempooldescendants` RPC](core-api-ref-remote-procedure-calls-blockchain#section-getmempooldescendants) returns all in-mempool descendants for a transaction in the mempool.

*Parameter #1---a transaction identifier (TXID)*

Name | Type | Presence | Description
--- | --- | --- | ---
TXID | string (hex) | Required<br>(exactly 1) | The TXID of a transaction in the memory pool, encoded as hex in RPC byte order

*Parameter #2---desired output format*

Name | Type | Presence | Description
--- | --- | --- | ---
Format | bool | Optional<br>(0 or 1) | Set to `true` to get json objects describing each transaction in the memory pool; set to `false` (the default) to only get an array of TXIDs

*Result---list of descendant transactions*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | An array of TXIDs belonging to transactions in the memory pool.  The array may be empty if there are no transactions in the memory pool
→<br>TXID | string | Optional<br>(0 or more) | The TXID of a transaction in the memory pool, encoded as hex in RPC byte order

*Result (format: `true`)---a JSON object describing each transaction*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | A object containing transactions currently in the memory pool.  May be empty
→<br>TXID | string : object | Optional<br>(0 or more) | The TXID of a transaction in the memory pool, encoded as hex in RPC byte order
→ →<br>`size` | number (int) | Required<br>(exactly 1) | The size of the serialized transaction in bytes
→ →<br>`fee` | number (bitcoins) | Required<br>(exactly 1) | The transaction fee paid by the transaction in decimal bitcoins
→ →<br>`modifiedfee` | number (bitcoins) | Required<br>(exactly 1) | The transaction fee with fee deltas used for mining priority in decimal bitcoins
→ →<br>`time` | number (int) | Required<br>(exactly 1) | The time the transaction entered the memory pool, Unix epoch time format
→ →<br>`height` | number (int) | Required<br>(exactly 1) | The block height when the transaction entered the memory pool
→ →<br>`descendantcount` | number (int) | Required<br>(exactly 1) | The number of in-mempool descendant transactions (including this one)
→ →<br>`descendantsize` | number (int) | Required<br>(exactly 1) | The size of in-mempool descendants (including this one)
→ →<br>`descendantfees` | number (int) | Required<br>(exactly 1) | The modified fees (see `modifiedfee` above) of in-mempool descendants (including this one)
→ →<br>`ancestorcount` | number (int) | Required<br>(exactly 1) | The number of in-mempool ancestor transactions (including this one)
→ →<br>`ancestorsize` | number (int) | Required<br>(exactly 1) | The size of in-mempool ancestors (including this one)
→ →<br>`ancestorfees` | number (int) | Required<br>(exactly 1) | The modified fees (see `modifiedfee` above) of in-mempool ancestors (including this one)
→ →<br>`depends` | array | Required<br>(exactly 1) | An array holding TXIDs of unconfirmed transactions this transaction depends upon (parent transactions).  Those transactions must be part of a block before this transaction can be added to a block, although all transactions may be included in the same block.  The array may be empty
→ → →<br>Depends TXID | string | Optional (0 or more) | The TXIDs of any unconfirmed transactions this transaction depends upon, encoded as hex in RPC byte order

*Examples from Dash Core 0.14.0*

The default (`false`):

``` bash
dash-cli getmempooldescendants 49a512c3d567effd4f605a6023df8b4b5\
23ac0ae7bccbaeed1c8a7db1e05e15a
```

Result:

``` json
[
  "49a512c3d567effd4f605a6023df8b4b523ac0ae7bccbaeed1c8a7db1e05e15a"
]
```

Verbose output (`true`):

``` bash
dash-cli getmempooldescendants 49a512c3d567effd4f605a6023df8b4b5\
23ac0ae7bccbaeed1c8a7db1e05e15a true
```

Result:

``` json
{
  "49a512c3d567effd4f605a6023df8b4b523ac0ae7bccbaeed1c8a7db1e05e15a": {
    "size": 226,
    "fee": 0.00000226,
    "modifiedfee": 0.00000226,
    "time": 1519160551,
    "height": 79046,
    "descendantcount": 1,
    "descendantsize": 226,
    "descendantfees": 226,
    "ancestorcount": 2,
    "ancestorsize": 1189,
    "ancestorfees": 1192,
    "depends": [
      "d1eefe8a006e2c21b55bc97c1f5b10000d63aa6a777bb11abc0daf62e4296660"
    ]
  }
}
```

*See also*

* [GetMemPoolAncestors](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getmempoolancestors): returns all in-mempool ancestors for a transaction in the mempool.
* [GetRawMemPool](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getrawmempool): returns all transaction identifiers (TXIDs) in the memory pool as a JSON array, or detailed information about each transaction in the memory pool as a JSON object.

# GetMemPoolEntry

*Added in Dash Core 0.14.0*

The [`getmempoolentry` RPC](core-api-ref-remote-procedure-calls-blockchain#section-getmempoolentry) returns mempool data for given transaction (must be in mempool).

*Parameter #1---a transaction identifier (TXID)*

Name | Type | Presence | Description
--- | --- | --- | ---
TXID | string (hex) | Required<br>(exactly 1) | The TXID of a transaction in the memory pool, encoded as hex in RPC byte order

*Result ---a JSON object describing the transaction*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | A object containing transactions currently in the memory pool.  May be empty
→<br>`size` | number (int) | Required<br>(exactly 1) | The size of the serialized transaction in bytes
→<br>`fee` | number (bitcoins) | Required<br>(exactly 1) | The transaction fee paid by the transaction in decimal bitcoins
→<br>`modifiedfee` | number (bitcoins) | Required<br>(exactly 1) | The transaction fee with fee deltas used for mining priority in decimal bitcoins
→<br>`time` | number (int) | Required<br>(exactly 1) | The time the transaction entered the memory pool, Unix epoch time format
→<br>`height` | number (int) | Required<br>(exactly 1) | The block height when the transaction entered the memory pool
→<br>`descendantcount` | number (int) | Required<br>(exactly 1) | The number of in-mempool descendant transactions (including this one)
→<br>`descendantsize` | number (int) | Required<br>(exactly 1) | The size of in-mempool descendants (including this one)
→<br>`descendantfees` | number (int) | Required<br>(exactly 1) | The modified fees (see `modifiedfee` above) of in-mempool descendants (including this one)
→<br>`ancestorcount` | number (int) | Required<br>(exactly 1) | The number of in-mempool ancestor transactions (including this one)
→<br>`ancestorsize` | number (int) | Required<br>(exactly 1) | The size of in-mempool ancestors (including this one)
→<br>`ancestorfees` | number (int) | Required<br>(exactly 1) | The modified fees (see `modifiedfee` above) of in-mempool ancestors (including this one)
→<br>`depends` | array | Required<br>(exactly 1) | An array holding TXIDs of unconfirmed transactions this transaction depends upon (parent transactions).  Those transactions must be part of a block before this transaction can be added to a block, although all transactions may be included in the same block.  The array may be empty
→ →<br>Depends TXID | string | Optional (0 or more) | The TXIDs of any unconfirmed transactions this transaction depends upon, encoded as hex in RPC byte order
→<br>`instantlock` | bool | Required<br>(exactly 1) | True if this transaction was locked via InstantSend

*Examples from Dash Core 0.12.3*

``` bash
dash-cli getmempoolentry d1eefe8a006e2c21b55bc97c1f5b10000d63aa6\
a777bb11abc0daf62e4296660
```

Result:

``` json
{
  "size": 372,
  "fee": 0.00000375,
  "modifiedfee": 0.00000375,
  "time": 1566315602,
  "height": 159320,
  "descendantcount": 1,
  "descendantsize": 372,
  "descendantfees": 375,
  "ancestorcount": 1,
  "ancestorsize": 372,
  "ancestorfees": 375,
  "depends": [
  ],
  "instantlock": true
}
```

*See also*

* [GetMemPoolAncestors](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getmempoolancestors): returns all in-mempool ancestors for a transaction in the mempool.
* [GetMemPoolDescendants](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getmempooldescendants): returns all in-mempool descendants for a transaction in the mempool.
* [GetRawMemPool](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getrawmempool): returns all transaction identifiers (TXIDs) in the memory pool as a JSON array, or detailed information about each transaction in the memory pool as a JSON object.

# GetMemPoolInfo

The [`getmempoolinfo` RPC](core-api-ref-remote-procedure-calls-blockchain#section-getmempoolinfo) returns information about the node's current transaction memory pool.

*Parameters: none*

*Result---information about the transaction memory pool*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | A object containing information about the memory pool
→<br>`size` | number (int) | Required<br>(exactly 1) | The number of transactions currently in the memory pool
→<br>`bytes` | number (int) | Required<br>(exactly 1) | The total number of bytes in the transactions in the memory pool
→<br>`usage` | number (int) | Required<br>(exactly 1) | *Added in Bitcoin Core 0.11.0*<br><br>Total memory usage for the mempool in bytes
→<br>`maxmempool` | number (int) | Required<br>(exactly 1) | *Added in Bitcoin Core 0.12.0*<br><br>Maximum memory usage for the mempool in bytes
→<br>`mempoolminfee` | number (int) | Required<br>(exactly 1) | *Added in Bitcoin Core 0.12.0*<br><br>The lowest fee per kilobyte paid by any transaction in the memory pool
→<br>`instantsendlocks` | number (int) | Required<br>(exactly 1) | *Added in Dash Core 0.15.0*<br><br>Number of unconfirmed InstantSend locks

*Example from Dash Core 0.15.0*

``` bash
dash-cli -testnet getmempoolinfo
```

Result:

``` json
{
  "size": 1,
  "bytes": 666,
  "usage": 1936,
  "maxmempool": 300000000,
  "mempoolminfee": 0.00000000,
  "instantsendlocks": 1
}
```

*See also*

* [GetBlockChainInfo](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getblockchaininfo): provides information about the current state of the block chain.
* [GetRawMemPool](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getrawmempool): returns all transaction identifiers (TXIDs) in the memory pool as a JSON array, or detailed information about each transaction in the memory pool as a JSON object.
* [GetTxOutSetInfo](/docs/core-api-ref-remote-procedure-calls-blockchain#section-gettxoutsetinfo): returns statistics about the confirmed unspent transaction output (UTXO) set. Note that this call may take some time and that it only counts outputs from confirmed transactions---it does not count outputs from the memory pool.

# GetRawMemPool

The [`getrawmempool` RPC](core-api-ref-remote-procedure-calls-blockchain#section-getrawmempool) returns all transaction identifiers (TXIDs) in the memory pool as a JSON array, or detailed information about each transaction in the memory pool as a JSON object.

*Parameter---desired output format*

Name | Type | Presence | Description
--- | --- | --- | ---
Format | bool | Optional<br>(0 or 1) | Set to `true` to get verbose output describing each transaction in the memory pool; set to `false` (the default) to only get an array of TXIDs for transactions in the memory pool

*Result (format `false`)---an array of TXIDs*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | An array of TXIDs belonging to transactions in the memory pool.  The array may be empty if there are no transactions in the memory pool
→<br>TXID | string | Optional<br>(0 or more) | The TXID of a transaction in the memory pool, encoded as hex in RPC byte order

*Result (format: `true`)---a JSON object describing each transaction*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | A object containing transactions currently in the memory pool.  May be empty
→<br>TXID | string : object | Optional<br>(0 or more) | The TXID of a transaction in the memory pool, encoded as hex in RPC byte order
→ →<br>`size` | number (int) | Required<br>(exactly 1) | The size of the serialized transaction in bytes
→ →<br>`fee` | amount (Dash) | Required<br>(exactly 1) | The transaction fee paid by the transaction in decimal Dash
→ →<br>`modifiedfee` | amount (Dash) | Required<br>(exactly 1) | *Added in Bitcoin Core 0.12.0*<br><br>The transaction fee with fee deltas used for mining priority in decimal Dash
→ →<br>`time` | number (int) | Required<br>(exactly 1) | The time the transaction entered the memory pool, Unix epoch time format
→ →<br>`height` | number (int) | Required<br>(exactly 1) | The block height when the transaction entered the memory pool
→ →<br>`descendantcount` | number (int) | Required<br>(exactly 1) | *Added in Bitcoin Core 0.12.0*<br><br>The number of in-mempool descendant transactions (including this one)
→ →<br>`descendantsize` | number (int) | Required<br>(exactly 1) | *Added in Bitcoin Core 0.12.0*<br><br>The size of in-mempool descendants (including this one)
→ →<br>`descendantfees` | number (int) | Required<br>(exactly 1) | *Added in Bitcoin Core 0.12.0*<br><br>The modified fees (see `modifiedfee` above) of in-mempool descendants (including this one)
→ →<br>`ancestorcount` | number (int) | Required<br>(exactly 1) | *Added in Dash Core 0.12.3 / Bitcoin Core 0.13.0*<br><br>The number of in-mempool ancestor transactions (including this one)
→ →<br>`ancestorsize` | number (int) | Required<br>(exactly 1) | *Added in Dash Core 0.12.3 / Bitcoin Core 0.13.0*<br><br>The size of in-mempool ancestors (including this one)
→ →<br>`ancestorfees` | number (int) | Required<br>(exactly 1) | *Added in Dash Core 0.12.3 / Bitcoin Core 0.13.0*<br><br>The modified fees (see `modifiedfee` above) of in-mempool ancestors (including this one)
→ →<br>`depends` | array | Required<br>(exactly 1) | An array holding TXIDs of unconfirmed transactions this transaction depends upon (parent transactions).  Those transactions must be part of a block before this transaction can be added to a block, although all transactions may be included in the same block.  The array may be empty
→ → →<br>Depends TXID | string | Optional (0 or more) | The TXIDs of any unconfirmed transactions this transaction depends upon, encoded as hex in RPC byte order
→ →<br>`instantlock` | bool | Required<br>(exactly 1) | *Added in Dash Core 0.12.3*<br><br>Set to `true` for locked InstantSend transactions (masternode quorum has locked the transaction inputs via `txlvote` messages). Set to `false` if the masternodes have not approved the InstantSend transaction

*Examples from Dash Core 0.12.3*

The default (`false`):

``` bash
dash-cli getrawmempool
```

Result:

``` json
[
  "9dc994e03e387ff2d2709fbe86edede9f3d7aaddea7f75694495e415561b22fe"
]
```

Verbose output (`true`):

``` bash
dash-cli getrawmempool true
```

Result:

``` json
{
  "3bf4985183ddebcb6b1d58fa04dae9728a8f2bf20be298d81e38a8bd2f50ea47": {
    "size": 225,
    "fee": 0.00000226,
    "modifiedfee": 0.00000226,
    "time": 1566315512,
    "height": 159318,
    "descendantcount": 1,
    "descendantsize": 225,
    "descendantfees": 226,
    "ancestorcount": 2,
    "ancestorsize": 5760,
    "ancestorfees": 5780,
    "depends": [
      "1b8fdb3ce371c1274ee60df807d631320e7efaf30e7867584eb44b8ec4c19d12"
    ],
    "instantlock": true
  },
  "1b8fdb3ce371c1274ee60df807d631320e7efaf30e7867584eb44b8ec4c19d12": {
    "size": 5535,
    "fee": 0.00005554,
    "modifiedfee": 0.00005554,
    "time": 1566315441,
    "height": 159318,
    "descendantcount": 2,
    "descendantsize": 5760,
    "descendantfees": 5780,
    "ancestorcount": 1,
    "ancestorsize": 5535,
    "ancestorfees": 5554,
    "depends": [
    ],
    "instantlock": true
  }
}
```

*See also*

* [GetMemPoolInfo](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getmempoolinfo): returns information about the node's current transaction memory pool.
* [GetMemPoolEntry](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getmempoolentry): returns mempool data for given transaction (must be in mempool).
* [GetTxOutSetInfo](/docs/core-api-ref-remote-procedure-calls-blockchain#section-gettxoutsetinfo): returns statistics about the confirmed unspent transaction output (UTXO) set. Note that this call may take some time and that it only counts outputs from confirmed transactions---it does not count outputs from the memory pool.

# GetMerkleBlocks

*Added in Dash Core 0.15.0*

The [`getmerkleblocks` RPC](core-api-ref-remote-procedure-calls-blockchain#section-getmerkleblocks) returns an array of hex-encoded merkleblocks for <count> blocks starting from <hash> which match <filter>.

*Parameter #1---filter*

Name | Type | Presence | Description
--- | --- | --- | ---
filter | string | Required<br>(exactly 1) | The hex encoded bloom filter

*Parameter #2---hash*

Name | Type | Presence | Description
--- | --- | --- | ---
hash | string | Required<br>(exactly 1) | The block hash

*Parameter #3---count*

Name | Type | Presence | Description
--- | --- | --- | ---
count | number (int) | Optional<br>Default/max=2000 |

*Result---the list of merkleblocks*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | An array of merkleblocks
→<br>Merkle Block | string (hex) | Optional<br>(1 or more) | A serialized, hex-encoded merkleblock

*Example from Dash Core 0.15.0*

``` bash
dash-cli getmerkleblocks \
	"2303028005802040100040000008008400048141010000f8400420800080025004000004130000000000000001" \
	"00000000007e1432d2af52e8463278bf556b55cf5049262f25634557e2e91202"
	2000
```

Result (truncated):
``` json
[
  "000000202c...aefc440107",
  "0000002058...9a17830103"
]
```

*See also: none*

# GetSpecialTxes

*Added in Dash Core 0.13.1*

The [`getspecialtxes` RPC](core-api-ref-remote-procedure-calls-blockchain#section-getspecialtxes) returns an array of special transactions found in the specified block

*Parameter #1---Block hash*

Name | Type | Presence | Description
--- | --- | --- | ---
`blockhash` | string | Required<br>(exactly 1) | The block hash.

*Parameter #2---Special transaction type*

Name | Type | Presence | Description
--- | --- | --- | ---
type | int | Optional<br>(0 or 1) | Filter special txes by type, -1 means all types (default: -1)

*Parameter #3---Result limit*

Name | Type | Presence | Description
--- | --- | --- | ---
count | int | Optional<br>(0 or 1) | The number of transactions to return (default: 10)

*Parameter #4---Results to skip*

Name | Type | Presence | Description
--- | --- | --- | ---
skip | int | Optional<br>(0 or 1) | The number of transactions to skip (default: 0)

*Parameter #5---Verbosity*

Name | Type | Presence | Description
--- | --- | --- | ---
verbosity | int | Optional<br>(0 or 1) | 0 for hashes, 1 for hex-encoded data, and 2 for JSON object<br>(default: 0)

*Result (if `verbosity` was `0`)---An array of transaction IDs*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string (hex): array | Required<br>(exactly 1) | Array of special transaction hashes

*Result (if `verbosity` was `1`)---An array of serialized transactions*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string (hex): array | Required<br>(exactly 1) | Array of serialized, hex-encoded data for the special transaction(s)

*Result (if `verbosity` was `2`)---An array of JSON objects*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string (hex): array of ojbects | Required<br>(exactly 1) | Array of special transaction objects in the format of the [`getrawtransaction` RPC](core-api-ref-remote-procedure-calls-raw-transaction#section-getrawtransaction)

*Example from Dash Core 0.13.1*

List of Special Transactions hashes.

``` bash
dash-cli -testnet getspecialtxes \
0000003db0006ecaccdf5ae2cfa9d94406ef40ff65b9ec34668b87fca3da9226
```

Result:
``` json
[
  "1572a15f56307e413afe3cb7ea0017a1a3fd6d89c6c5f258cc17b2888a8e7fff",
  "89a6dc42063e4a792ec225db64dd9426742a5d1738e8821625d2ab920a6187b2",
  "fa3b3b0d3522becb02ddd15dd075f3d6ecc6a5a50b43c6c9f6d4703a9a8509d5"
]
```

List of Provider Registration Special Transactions (type: 1) in serialized, hex-encoded format.

``` bash
dash-cli -testnet getspecialtxes \
0000003db0006ecaccdf5ae2cfa9d94406ef40ff65b9ec34668b87fca3da9226 1 10 0 1
```

Result:
``` json
[
  "0300010001ea721d7420a9b58025894d08f9fecc73b7b87ed09277fa99dad5aa028ea357e1000000006b48304502210093c409672eed335f80630d7108c108d0b85ebe4d8be0758f8a3745f617c4b57302203175063605552c89f6de7f3dadc1773d5ef773b7cc0ccf98e6c5555ea75ba307012102b21d19fec95d9863c5b12fafeb60530e1cfc51d83f49ea9bca7192abb8b83e46feffffff01c4bb609a010000001976a9142efe9f9d3b36b133364d3cccbd27db75a0fbdcb788ac00000000fd120101000000000031567fbaf591ae9d2d0e9050bebce6a311cfd900616f851a3a630aa65e53f6940000000000000000000000000000ffffad3d1ee74a43c1ad3af209f75deaeb9216fc8339fd48d376f9b007ffa44583c9908f4aaca8dd97990c56043e475723f90940ef5fd7d493152540f25f58fb8c965ee5e1be4f850a661476c1ad3af209f75deaeb9216fc8339fd48d376f9b0e8031976a91454bbd7bd7c21553612d60ab16579e51efbcb135288acc281e8bf5a0dd22dfc9f1edeef9ef248f965a79210d997da37fb3e1dec76d1a4412096809bc005c860a0215cb008e3044b972688443b0b7a31ac5a04b728e63b1b5c5489e33dd666435f93c646523ad8a1d935a58957026749cbd0a9bf7e09a77388",
  "03000100012354b77c0f261f3d5b8424cbe67c2f27130f01c531732a08b8ae3f28aaa1b1fb000000006a473044022058323d3d9114492a7a7d350d5e3127d2dc550563968319987079c98f19ed519202204160cfe81adf1f41301136a3cbe03697baa1b14c3103b66bd839ace503dbf2630121026f83a8dad6b4695b136c399405b31d4031fd6631c469673d71eda479157ef9dcfeffffff0106b8609a010000001976a9142a855dc127bfdd5cc0ab73b71ff126e49aa409c488ac00000000fd1201010000000000b60dcb8bab5aba47435942c36ca4ee74ea5b662f4d7c7b528ce341915b2d5aec0100000000000000000000000000ffffad3d1ee74a428d3433cb6b9a1a29fdf07613172bbfdab744889689e308c9d2d8a3cb35f9d7bb7220b1eca82c952b82111119670dacae18a509628c775287e4e796128cd6379b80dffd7d8d3433cb6b9a1a29fdf07613172bbfdab744889610271976a91454bbd7bd7c21553612d60ab16579e51efbcb135288ac512010a2b992d7d5c1e1f999852855cc55162800025cfdf3b56c74e4734a2d97411f858532607cbd8848452dab1f216650def1d11a5abf3fa464c9ffcc7fec894a012a4b70ee5d3b983b5fe640f04a7f3e4fe67fbb5b7cccb71afa37888ad6cca48e"
]
```

List of Coinbase Special Transactions (type: 5) in JSON format.

``` bash
dash-cli -testnet getspecialtxes \
00000000007b0fb99e36713cf08012482478ee496e6dcb4007ad2e806306e62b 5 10 0 2
```

Result:
``` json
[
  {
    "txid": "25632685ed0d7286901a80961c924c1ddd952e764754dbd8b40d0956413c8b56",
    "size": 229,
    "version": 3,
    "type": 5,
    "locktime": 0,
    "vin": [
      {
        "coinbase": "03ae50011a4d696e656420627920416e74506f6f6c2021000b01201da9196f0000000007000000",
        "sequence": 4294967295
      }
    ],
    "vout": [
      {
        "value": 8.10000000,
        "valueSat": 810000000,
        "n": 0,
        "scriptPubKey": {
          "asm": "OP_DUP OP_HASH160 cbd7bfcc50351180132b2c0698cb90ad74c473c7 OP_EQUALVERIFY OP_CHECKSIG",
          "hex": "76a914cbd7bfcc50351180132b2c0698cb90ad74c473c788ac",
          "reqSigs": 1,
          "type": "pubkeyhash",
          "addresses": [
            "yeuGUfPMrbEqAS2Pw1wonYgEPbM4LAA9LK"
          ]
        }
      },
      {
        "value": 8.10000000,
        "valueSat": 810000000,
        "n": 1,
        "scriptPubKey": {
          "asm": "OP_DUP OP_HASH160 88a060bc2dfe05780ae4dcb6c98b12436c35a939 OP_EQUALVERIFY OP_CHECKSIG",
          "hex": "76a91488a060bc2dfe05780ae4dcb6c98b12436c35a93988ac",
          "reqSigs": 1,
          "type": "pubkeyhash",
          "addresses": [
            "yYmrsYP3XYMZr1cGtha3QzmuNB1C7CfyhV"
          ]
        }
      }
    ],
    "extraPayloadSize": 70,
    "extraPayload": "0200ae50010078e5c08b39960887bf95185c381bdb719e60b6925fa12af78a8824fade927387c757acb6bac63da84f9245e20cfd5d830382ac634d434725ca6349ab5db920a3",
    "cbTx": {
      "version": 2,
      "height": 86190,
      "merkleRootMNList": "877392defa24888af72aa15f92b6609e71db1b385c1895bf870896398bc0e578",
      "merkleRootQuorums": "a320b95dab4963ca2547434d63ac8203835dfd0ce245924fa83dc6bab6ac57c7"
    },
    "instantlock": false,
    "chainlock": false
  }
]
```

*See also:*

* [GetRawTransaction](/docs/core-api-ref-remote-procedure-calls-raw-transaction#section-getrawtransaction): gets a hex-encoded serialized transaction or a JSON object describing the transaction. By default, Dash Core only stores complete transaction data for UTXOs and your own transactions, so the RPC may fail on historic transactions unless you use the non-default `txindex=1` in your Dash Core startup settings.

# GetSpentInfo

*Added in Dash Core 0.12.1*

The [`getspentinfo` RPC](core-api-ref-remote-procedure-calls-blockchain#section-getspentinfo) returns the txid and index where an output is spent (requires `spentindex` to be enabled).

*Parameter #1---the TXID of the output*

Name | Type | Presence | Description
--- | --- | --- | ---
TXID | string (hex) | Required<br>(exactly 1) | The TXID of the transaction containing the relevant output, encoded as hex in RPC byte order

*Parameter #2---the start block height*

Name | Type | Presence | Description
--- | --- | --- | ---
Index | number (int) | Required<br>(exactly 1) | The block height to begin looking in

*Result---the TXID and spending input index*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object/null | Required<br>(exactly 1) | Information about the spent output.  If output wasn't found or if an error occurred, this will be JSON `null`
→<br>`txid` | string | Required<br>(exactly 1) | The output txid
→<br>`index` | number | Required<br>(exactly 1) | The spending input index

*Example from Dash Core 0.12.2*

Get the txid and index where an output is spent:

``` bash
dash-cli getspentinfo \
  '''
    {
      "txid": "0456aaf51a8df21dd47c2a06ede046a5bf7403bcb95d14d1d71b178c189fb933", \
      "index": 0
    }
```

Result:

``` json
{
  "txid": "14e874421350840e9d43965967c5a989e7d41ad361ef37484ee67d01d433ecfa",
  "index": 1,
  "height": 7742
}
```

*See also: none*

# GetTxOut

The [`gettxout` RPC](core-api-ref-remote-procedure-calls-blockchain#section-gettxout) returns details about an unspent transaction output (UTXO).

*Parameter #1---the TXID of the output to get*

Name | Type | Presence | Description
--- | --- | --- | ---
TXID | string (hex) | Required<br>(exactly 1) | The TXID of the transaction containing the output to get, encoded as hex in RPC byte order

*Parameter #2---the output index number (vout) of the output to get*

Name | Type | Presence | Description
--- | --- | --- | ---
Vout | number (int) | Required<br>(exactly 1) | The output index number (vout) of the output within the transaction; the first output in a transaction is vout 0

*Parameter #3---whether to display unconfirmed outputs from the memory pool*

Name | Type | Presence | Description
--- | --- | --- | ---
Unconfirmed | bool | Optional<br>(0 or 1) | Set to `true` to display unconfirmed outputs from the memory pool; set to `false` (the default) to only display outputs from confirmed transactions

*Result---a description of the output*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object/null | Required<br>(exactly 1) | Information about the output.  If output wasn't found, if it was spent, or if an error occurred, this will be JSON `null`
→<br>`bestblock` | string (hex) | Required<br>(exactly 1) | The hash of the header of the block on the local best block chain which includes this transaction.  The hash will encoded as hex in RPC byte order.  If the transaction is not part of a block, the string will be empty
→<br>`confirmations` | number (int) | Required<br>(exactly 1) | The number of confirmations received for the transaction containing this output or `0` if the transaction hasn't been confirmed yet
→<br>`value` | number (Dash) | Required<br>(exactly 1) | The amount of Dash spent to this output.  May be `0`
→<br>`scriptPubKey` | string : object | Optional<br>(0 or 1) | An object with information about the pubkey script.  This may be `null` if there was no pubkey script
→ →<br>`asm` | string | Required<br>(exactly 1) | The pubkey script in decoded form with non-data-pushing opcodes listed
→ →<br>`hex` | string (hex) | Required<br>(exactly 1) | The pubkey script encoded as hex
→ →<br>`reqSigs` | number (int) | Optional<br>(0 or 1) | The number of signatures required; this is always `1` for P2PK, P2PKH, and P2SH (including P2SH multisig because the redeem script is not available in the pubkey script).  It may be greater than 1 for bare multisig.  This value will not be returned for `nulldata` or `nonstandard` script types (see the `type` key below)
→ →<br>`type` | string | Optional<br>(0 or 1) | The type of script.  This will be one of the following:<br>• `pubkey` for a P2PK script<br>• `pubkeyhash` for a P2PKH script<br>• `scripthash` for a P2SH script<br>• `multisig` for a bare multisig script<br>• `nulldata` for nulldata scripts<br>• `nonstandard` for unknown scripts
→ →<br>`addresses` | string : array | Optional<br>(0 or 1) | The P2PKH or P2SH addresses used in this transaction, or the computed P2PKH address of any pubkeys in this transaction.  This array will not be returned for `nulldata` or `nonstandard` script types
→ → →<br>Address | string | Required<br>(1 or more) | A P2PKH or P2SH address
→<br>`coinbase` | bool | Required<br>(exactly 1) | Set to `true` if the transaction output belonged to a coinbase transaction; set to `false` for all other transactions.  Coinbase transactions need to have 101 confirmations before their outputs can be spent

*Example from Dash Core 0.15.0*

Get the UTXO from the following transaction from the first output index ("0"),
searching the memory pool if necessary.

``` bash
dash-cli -testnet gettxout \
  e0a06b47f0de6f3851a228d5ac377ac38b495adf04298c43e951e679c5b0aa8f \
  0 true
```

Result:

``` json
{
  "bestblock": "000000005651f6d7859793dee07d476a2f2a7338e66bbb41caf4b544c5b0318d",
  "confirmations": 2,
  "value": 25.00000000,
  "scriptPubKey": {
    "asm": "OP_DUP OP_HASH160 b66266c5017a759817f3bb99e8d9124bf5bb2e74 OP_EQUALVERIFY OP_CHECKSIG",
    "hex": "76a914b66266c5017a759817f3bb99e8d9124bf5bb2e7488ac",
    "reqSigs": 1,
    "type": "pubkeyhash",
    "addresses": [
      "ycwoiAibTjpwnoCZSX7S4kiB2H8wULw9qo"
    ]
  },
  "coinbase": false
}
```

*See also*

* [GetRawTransaction](/docs/core-api-ref-remote-procedure-calls-raw-transaction#section-getrawtransaction): gets a hex-encoded serialized transaction or a JSON object describing the transaction. By default, Dash Core only stores complete transaction data for UTXOs and your own transactions, so the RPC may fail on historic transactions unless you use the non-default `txindex=1` in your Dash Core startup settings.
* [GetTransaction](/docs/core-api-ref-remote-procedure-calls-wallet#section-gettransaction): gets detailed information about an in-wallet transaction.

# GetTxOutProof

The [`gettxoutproof` RPC](core-api-ref-remote-procedure-calls-blockchain#section-gettxoutproof) returns a hex-encoded proof that one or more specified transactions were included in a block.

NOTE: By default this function only works when there is an
unspent output in the UTXO set for this transaction. To make it always work,
you need to maintain a transaction index, using the `-txindex` command line option, or
specify the block in which the transaction is included in manually (by block header hash).

*Parameter #1---the transaction hashes to prove*

Name | Type | Presence | Description
--- | --- | --- | ---
TXIDs | array | Required<br>(exactly 1) | A JSON array of txids to filter
→<br>`txid` | string | Required<br>(1 or more) | TXIDs of the transactions to generate proof for.  All transactions must be in the same block

*Parameter #2---the block to look for txids in*

Name | Type | Presence | Description
--- | --- | --- | ---
Header hash | string | Optional<br>(0 or 1) | If specified, looks for txid in the block with this hash

*Result---serialized, hex-encoded data for the proof*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string | Required<br>(exactly 1) | A string that is a serialized, hex-encoded data for the proof

*Example from Dash Core 0.12.2*

Get the hex-encoded proof that "txid" was included in block 000000012d774f3c7668f32bc448efeb93b317f312dd863679de3a007d47817f:

``` bash
dash-cli gettxoutproof \
  '''
    [
      "e0a06b47f0de6f3851a228d5ac377ac38b495adf04298c43e951e679c5b0aa8f"
    ]
  ''' \
  '000000012d774f3c7668f32bc448efeb93b317f312dd863679de3a007d47817f'
```

Result (wrapped):

``` text
01000020ed72cc6a7294782a7711d8fa7ef74716ef062dc50bb0820f7eec923801000000\
aa5d17c5128043803b67c7ab03e4d3ffbc9604b54f877f1c5cf9ed3adeaa19b2cd7ed659\
f838011d10a70a480200000002033c89c2baecba9fc983c85dcf365c2d9cc93aca1dee2e\
5ac18124464056542e8faab0c579e651e9438c2904df5a498bc37a37acd528a251386fde\
f0476ba0e00105
```

*See also*

* [VerifyTxOutProof](/docs/core-api-ref-remote-procedure-calls-blockchain#section-verifytxoutproof): verifies that a proof points to one or more transactions in a block, returning the transactions the proof commits to and throwing an RPC error if the block is not in our best block chain.
* [`merkleblock` message](core-ref-p2p-network-data-messages#section-merkleblock): A description of the
  format used for the proof.

# GetTxOutSetInfo

The [`gettxoutsetinfo` RPC](core-api-ref-remote-procedure-calls-blockchain#section-gettxoutsetinfo) returns statistics about the confirmed unspent transaction output (UTXO) set. Note that this call may take some time and that it only counts outputs from confirmed transactions---it does not count outputs from the memory pool.

*Parameters: none*

*Result---statistics about the UTXO set*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | Information about the UTXO set
→<br>`height` | number (int) | Required<br>(exactly 1) | The height of the local best block chain.  A new node with only the hardcoded genesis block will have a height of 0
→<br>`bestblock` | string (hex) | Required<br>(exactly 1) | The hash of the header of the highest block on the local best block chain, encoded as hex in RPC byte order
→<br>`transactions` | number (int) | Required<br>(exactly 1) | The number of transactions with unspent outputs
→<br>`txouts` | number (int) | Required<br>(exactly 1) | The number of unspent transaction outputs
→<br>`bogosize` | number (int) | Required<br>(exactly 1) | A meaningless metric for UTXO set size
→<br>`hash_serialized_2` | string (hex) | Required<br>(exactly 1) | A SHA256(SHA256()) hash of the serialized UTXO set; useful for comparing two nodes to see if they have the same set (they should, if they always used the same serialization format and currently have the same best block).  The hash is encoded as hex in RPC byte order
→<br>`disk_size` | number (int) | Required<br>(exactly 1) | The estimated size of the chainstate on disk
→<br>`total_amount` | number (Dash) | Required<br>(exactly 1) | The total amount of Dash in the UTXO set

*Example from Dash Core 0.15.0*

``` bash
dash-cli -testnet gettxoutsetinfo
```

Result:

``` json
{
  "height": 159358,
  "bestblock": "0000000000a705ef74a1fc134ea1eba49af8eead40b3df1fc4fb40f5940a0d60",
  "transactions": 187542,
  "txouts": 366996,
  "bogosize": 28344374,
  "hash_serialized_2": "d7326bdc2d9cb7d91580bfd47d6c2972ab1776c2c33c787873a5fd01986c9377",
  "disk_size": 21513509,
  "total_amount": 7517185.08574437
}
```

*See also*

* [GetBlockChainInfo](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getblockchaininfo): provides information about the current state of the block chain.
* [GetMemPoolInfo](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getmempoolinfo): returns information about the node's current transaction memory pool.

# PreciousBlock

*Added in Dash Core 0.12.3 / Bitcoin Core 0.14.0*

The [`preciousblock` RPC](core-api-ref-remote-procedure-calls-blockchain#section-preciousblock) treats a block as if it were received before others with the same work. A later `preciousblock` call can override the effect of an earlier one. The effects of `preciousblock` are not retained across restarts.

*Parameter #1---the block hash*

Name | Type | Presence | Description
--- | --- | --- | ---
Header Hash | string (hex) | Required<br>(exactly 1) | The hash of the block to mark as precious

*Result---`null` or error on failure*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | JSON `null`.  The JSON-RPC error field will be set only if you entered an invalid block hash

*Example from Dash Core 0.12.3*

``` bash
dash-cli preciousblock 00000000034d77e287b63922a94f12e8c4ab9e\
1d8056060fd51f6153ea5dc757
```

Result (no output from `dash-cli` because result is set to `null`).

# PruneBlockChain

*Added in Dash Core 0.12.3 / Bitcoin Core 0.14.0*

The [`pruneblockchain` RPC](core-api-ref-remote-procedure-calls-blockchain#section-pruneblockchain) prunes the blockchain up to a specified height or timestamp. The `-prune` option needs to be enabled (disabled by default).

*Parameter #1---the block height or timestamp*

Name | Type | Presence | Description
--- | --- | --- | ---
Height | number (int) | Required<br>(exactly 1) | The block height to prune up to. May be set to a particular height, or a unix timestamp to prune blocks whose block time is at least 2 hours older than the provided timestamp

*Result---the height of the last block pruned*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | number (int) | Required<br>(exactly 1) | The height of the last block pruned

*Examples from Dash Core 0.12.3*

``` bash
dash-cli pruneblockchain 413555
```

Result:

``` text
413555
```

*See also*

* [ImportPrunedFunds](/docs/core-api-ref-remote-procedure-calls-wallet#section-importprunedfunds): imports funds without the need of a rescan. Meant for use with pruned wallets.

# VerifyChain

The [`verifychain` RPC](core-api-ref-remote-procedure-calls-blockchain#section-verifychain) verifies each entry in the local block chain database.

*Parameter #1---how thoroughly to check each block*

Name | Type | Presence | Description
--- | --- | --- | ---
Check Level | number (int) | Optional<br>(0 or 1) | How thoroughly to check each block, from 0 to 4.  Default is the level set with the `-checklevel` command line argument; if that isn't set, the default is `3`.  Each higher level includes the tests from the lower levels<br><br>Levels are:<br>**0.** Read from disk to ensure the files are accessible<br>**1.**  Ensure each block is valid<br>**2.** Make sure undo files can be read from disk and are in a valid format<br>**3.** Test each block undo to ensure it results in correct state<br>**4.** After undoing blocks, reconnect them to ensure they reconnect correctly

*Parameter #2---the number of blocks to check*

Name | Type | Presence | Description
--- | --- | --- | ---
Number Of Blocks | number (int) | Optional<br>(0 or 1) | The number of blocks to verify.  Set to `0` to check all blocks.  Defaults to the value of the `-checkblocks` command-line argument; if that isn't set, the default is `288`

*Result---verification results*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | bool | Required<br>(exactly 1) | Set to `true` if verified; set to `false` if verification failed for any reason

*Example from Dash Core 0.12.2*

Verify the most recent 400 blocks in the most through way:

``` bash
dash-cli -testnet verifychain 4 400
```

Result (took < 1 second on a mobile workstation; it would've taken much longer on mainnet):

``` json
true
```

*See also*

* [GetBlockChainInfo](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getblockchaininfo): provides information about the current state of the block chain.
* [GetTxOutSetInfo](/docs/core-api-ref-remote-procedure-calls-blockchain#section-gettxoutsetinfo): returns statistics about the confirmed unspent transaction output (UTXO) set. Note that this call may take some time and that it only counts outputs from confirmed transactions---it does not count outputs from the memory pool.

# VerifyTxOutProof

The [`verifytxoutproof` RPC](core-api-ref-remote-procedure-calls-blockchain#section-verifytxoutproof) verifies that a proof points to one or more transactions in a block, returning the transactions the proof commits to and throwing an RPC error if the block is not in our best block chain.

*Parameter #1---The hex-encoded proof generated by gettxoutproof*

Name | Type | Presence | Description
--- | --- | --- | ---
`proof` | string | Required | A hex-encoded proof

*Result---txid(s) which the proof commits to*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string | Required<br>(exactly 1) | The txid(s) which the proof commits to, or empty array if the proof is invalid

*Example from Dash Core 0.12.2*

Verify a proof:

``` bash
dash-cli verifytxoutproof \
01000020ed72cc6a7294782a7711d8fa7ef74716ef062dc50bb0820f7eec923801000000\
aa5d17c5128043803b67c7ab03e4d3ffbc9604b54f877f1c5cf9ed3adeaa19b2cd7ed659\
f838011d10a70a480200000002033c89c2baecba9fc983c85dcf365c2d9cc93aca1dee2e\
5ac18124464056542e8faab0c579e651e9438c2904df5a498bc37a37acd528a251386fde\
f0476ba0e00105
```

Result:

``` json
[
"e0a06b47f0de6f3851a228d5ac377ac38b495adf04298c43e951e679c5b0aa8f"
]
```

*See also*

* [GetTxOutProof](/docs/core-api-ref-remote-procedure-calls-blockchain#section-gettxoutproof): returns a hex-encoded proof that one or more specified transactions were included in a block.
* [`merkleblock` message](core-ref-p2p-network-data-messages#section-merkleblock): A description of the format used for the proof.