# CombineRawTransaction

The [`combinerawtransaction` RPC](core-api-ref-remote-procedure-calls-raw-transaction#section-combinerawtransaction) combine multiple partially signed transactions into one transaction.

The combined transaction may be another partially signed transaction or a
fully signed transaction.

*Parameter #1---txs*

Name | Type | Presence | Description
--- | --- | --- | ---
txs | string | Required | A json array of hex strings of partially signed transactions

*Result---hex-encoded raw transaction with signature(s)*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string | Required<br>(Exactly 1) | The resulting raw transaction in serialized transaction format encoded as hex.

*Example from Dash Core 0.15.0*

The following example shows a fully signed two input transaction being assembled
by combining two partially signed transactions. The first hex-encoded string is
the transaction with only the first input signed. The second hex-encoded string is
the transaction with only the second input signed.

``` bash
dash-cli combinerawtransaction '[
 "0200000002fdb27b4f2b80a5fd3b96602618a6ccf7bdde504bf90989610b19ed6ecd769520010000006b483045022100f8770316327966fb1972338d14db8d38048455da8b62f6350b117c797cea459e02206c63c103cf53ce1d24a313b3e6853913fa14febafd733e683b6eb46a7beec0fa012103c67d86944315838aea7ec80d390b5d09b91b62483370d4979da5ccf7a7df77a9ffffffff0d052e9b13c53bb342d772767732ffe4fa9f1c150629d3fa79655267baa7c86a0100000000ffffffff0200ca9a3b000000001976a9144139b147b5cef5fef5bcdb02fcdf55e426f74dbb88ac00daf89a000000001976a91465f53f2095c99ce152ff3bc8a8f027d8a77cbdcb88ac00000000",
 "0200000002fdb27b4f2b80a5fd3b96602618a6ccf7bdde504bf90989610b19ed6ecd7695200100000000ffffffff0d052e9b13c53bb342d772767732ffe4fa9f1c150629d3fa79655267baa7c86a010000006b4830450221008c3abc11ea84cc98cf674afc5b6d3d078d672768d289f2ab976ea4b2a49129fc022008470458b1b179800e7f5348196d510d2f147e69fe836c94135fc5c620312acd012102912ba98d6641f79864d04d41523167f5db267e45d1633e9243a0be7efb675719ffffffff0200ca9a3b000000001976a9144139b147b5cef5fef5bcdb02fcdf55e426f74dbb88ac00daf89a000000001976a91465f53f2095c99ce152ff3bc8a8f027d8a77cbdcb88ac00000000"
]'
```

Result:
``` bash
0200000002fdb27b4f2b80a5fd3b96602618a6ccf7bdde504bf90989610b19ed6ecd7695\
20010000006b483045022100f8770316327966fb1972338d14db8d38048455da8b62f635\
0b117c797cea459e02206c63c103cf53ce1d24a313b3e6853913fa14febafd733e683b6e\
b46a7beec0fa012103c67d86944315838aea7ec80d390b5d09b91b62483370d4979da5cc\
f7a7df77a9ffffffff0d052e9b13c53bb342d772767732ffe4fa9f1c150629d3fa796552\
67baa7c86a010000006b4830450221008c3abc11ea84cc98cf674afc5b6d3d078d672768\
d289f2ab976ea4b2a49129fc022008470458b1b179800e7f5348196d510d2f147e69fe83\
6c94135fc5c620312acd012102912ba98d6641f79864d04d41523167f5db267e45d1633e\
9243a0be7efb675719ffffffff0200ca9a3b000000001976a9144139b147b5cef5fef5bc\
db02fcdf55e426f74dbb88ac00daf89a000000001976a91465f53f2095c99ce152ff3bc8\
a8f027d8a77cbdcb88ac00000000
```

*See also:*

* [CreateRawTransaction](/docs/core-api-ref-remote-procedure-calls-raw-transaction#section-createrawtransaction): creates an unsigned serialized transaction that spends a previous output to a new output with a P2PKH or P2SH address. The transaction is not stored in the wallet or transmitted to the network.
* [DecodeRawTransaction](/docs/core-api-ref-remote-procedure-calls-raw-transaction#section-decoderawtransaction): decodes a serialized transaction hex string into a JSON object describing the transaction.
* [SignRawTransaction](/docs/core-api-ref-remote-procedure-calls-raw-transaction#section-signrawtransaction): signs a transaction in the serialized transaction format using private keys stored in the wallet or provided in the call.
* [SendRawTransaction](/docs/core-api-ref-remote-procedure-calls-raw-transaction#section-sendrawtransaction): validates a transaction and broadcasts it to the peer-to-peer network.
* [Serialized Transaction Format](core-ref-transactions-raw-transaction-format)

# CreateRawTransaction

The [`createrawtransaction` RPC](core-api-ref-remote-procedure-calls-raw-transaction#section-createrawtransaction) creates an unsigned serialized transaction that spends a previous output to a new output with a P2PKH or P2SH address. The transaction is not stored in the wallet or transmitted to the network.

*Parameter #1---Inputs*

Name | Type | Presence | Description
--- | --- | --- | ---
Transactions | array | Required<br>(exactly 1) | An array of objects, each one to be used as an input to the transaction
→ Input | object | Required<br>(1 or more) | An object describing a particular input
→ →<br>`txid` | string (hex) | Required<br>(exactly 1) | The TXID of the outpoint to be spent encoded as hex in RPC byte order
→ →<br>`vout` | number (int) | Required<br>(exactly 1) | The output index number (vout) of the outpoint to be spent; the first output in a transaction is index `0`
→ →<br>`Sequence` | number (int) | Optional<br>(0 or 1) | Added in Dash Core 0.12.3.0.<br><br>The sequence number to use for the input

*Parameter #2---P2PKH or P2SH addresses and amounts*

Name | Type | Presence | Description
--- | --- | --- | ---
Outputs | object | Required<br>(exactly 1) | The addresses and amounts to pay
→<br>Address/Amount | string : number (Dash) | Required<br>(1 or more) | A key/value pair with the address to pay as a string (key) and the amount to pay that address (value) in Dash
→<br>Data/Hex | data : hex | Required<br>(1 or more) | A key/value pair where the key is 'data' and the value is hex encoded data

*Parameter #3---locktime*

Name | Type | Presence | Description
--- | --- | --- | ---
Locktime | numeric (int) | Optional<br>(0 or 1) | *Added in Bitcoin Core 0.12.0*<br><br>Indicates the earliest time a transaction can be added to the block chain

*Result---the unsigned raw transaction in hex*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string | Required<br>(Exactly 1) | The resulting unsigned raw transaction in serialized transaction format encoded as hex.  If the transaction couldn't be generated, this will be set to JSON `null` and the JSON-RPC error field may contain an error message

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet createrawtransaction '''
  [
    {
      "txid": "061ec99eb641ffdeaa05a1a724a255103bebc445b15c6c8c028b19c08608496b",
      "vout" : 1
    }
  ]''' \
  '{"ySutkc49Khpz1HQN8AfWNitVBLwqtyaxvv": 800, "yY6AmGopsZS31wy1JLHR9P6AC6owFaXwuh":74.99}' '0'

```

Result (wrapped):

``` text
01000000016b490886c0198b028c6c5cb145c4eb3b1055a224a7a105aadeff41b69ec91e06\
0100000000ffffffff0200205fa0120000001976a914485485425fa99504ec1638ac4213f3\
cfc9f32ef388acc0a8f9be010000001976a914811eacc14db8ebb5b64486dc43400c0226b4\
28a488ac00000000
```

*See also*

* [CombineRawTransaction](/docs/core-api-ref-remote-procedure-calls-raw-transaction#section-combinerawtransaction): combine multiple partially signed transactions into one transaction.
* [DecodeRawTransaction](/docs/core-api-ref-remote-procedure-calls-raw-transaction#section-decoderawtransaction): decodes a serialized transaction hex string into a JSON object describing the transaction.
* [SignRawTransaction](/docs/core-api-ref-remote-procedure-calls-raw-transaction#section-signrawtransaction): signs a transaction in the serialized transaction format using private keys stored in the wallet or provided in the call.
* [SendRawTransaction](/docs/core-api-ref-remote-procedure-calls-raw-transaction#section-sendrawtransaction): validates a transaction and broadcasts it to the peer-to-peer network.
* [Serialized Transaction Format](core-ref-transactions-raw-transaction-format)

# DecodeRawTransaction

The [`decoderawtransaction` RPC](core-api-ref-remote-procedure-calls-raw-transaction#section-decoderawtransaction) decodes a serialized transaction hex string into a JSON object describing the transaction.

*Parameter #1---serialized transaction in hex*

Name | Type | Presence | Description
--- | --- | --- | ---
Serialized Transaction | string (hex) | Required<br>(exactly 1) | The transaction to decode in serialized transaction format

*Result---the decoded transaction*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | An object describing the decoded transaction, or JSON `null` if the transaction could not be decoded
→<br>`txid` | string (hex) | Required<br>(exactly 1) | The transaction's TXID encoded as hex in RPC byte order
→<br>`size` | number (int) | Required<br>(exactly 1) | *Added in Bitcoin Core 0.12.0*<br><br>The serialized transaction size
→<br>`version` | number (int) | Required<br>(exactly 1) | The transaction format version number
→<br>`type` | number (int) | Required<br>(exactly 1) | *Added in Dash Core 0.13.0.0*<br><br>The transaction format type
→<br>`locktime` | number (int) | Required<br>(exactly 1) | The transaction's locktime: either a Unix epoch date or block height; see the [locktime parsing rules](core-guide-transactions-locktime-and-sequence-number#locktime_parsing_rules)
→<br>`vin` | array | Required<br>(exactly 1) | An array of objects with each object being an input vector (vin) for this transaction.  Input objects will have the same order within the array as they have in the transaction, so the first input listed will be input 0
→ →<br>Input | object | Required<br>(1 or more) | An object describing one of this transaction's inputs.  May be a regular input or a coinbase
→ → →<br>`txid` | string | Optional<br>(0 or 1) | The TXID of the outpoint being spent, encoded as hex in RPC byte order.  Not present if this is a coinbase transaction
→ → →<br>`vout` | number (int) | Optional<br>(0 or 1) | The output index number (vout) of the outpoint being spent.  The first output in a transaction has an index of `0`.  Not present if this is a coinbase transaction
→ → →<br>`scriptSig` | object | Optional<br>(0 or 1) | An object describing the signature script of this input.  Not present if this is a coinbase transaction
→ → → →<br>`asm` | string | Required<br>(exactly 1) | The signature script in decoded form with non-data-pushing opcodes listed
→ → → →<br>`hex` | string (hex) | Required<br>(exactly 1) | The signature script encoded as hex
→ → →<br>`coinbase` | string (hex) | Optional<br>(0 or 1) | The coinbase (similar to the hex field of a scriptSig) encoded as hex.  Only present if this is a coinbase transaction
→ → →<br>`value` | number (Dash) | Optional<br>(exactly 1) | The number of Dash paid to this output.  May be `0`.<br><br>Only present if `spentindex` enabled
→ → →<br>`valueSat` | number (duffs) | Optional<br>(exactly 1) | The number of duffs paid to this output.  May be `0`.<br><br>Only present if `spentindex` enabled
→ → → →<br>`addresses` | string : array | Optional<br>(0 or 1) | The P2PKH or P2SH addresses used in this transaction, or the computed P2PKH address of any pubkeys in this transaction.  This array will not be returned for `nulldata` or `nonstandard` script types.<br><br>Only present if `spentindex` enabled
→ → → → →<br>Address | string | Required<br>(1 or more) | A P2PKH or P2SH address
→ → →<br>`sequence` | number (int) | Required<br>(exactly 1) | The input sequence number
→<br>`vout` | array | Required<br>(exactly 1) | An array of objects each describing an output vector (vout) for this transaction.  Output objects will have the same order within the array as they have in the transaction, so the first output listed will be output 0
→ →<br>Output | object | Required<br>(1 or more) | An object describing one of this transaction's outputs
→ → →<br>`value` | number (Dash) | Required<br>(exactly 1) | The number of Dash paid to this output.  May be `0`
→ → →<br>`valueSat` | number (duffs) | Required<br>(exactly 1) | The number of duffs paid to this output.  May be `0`
→ → →<br>`n` | number (int) | Required<br>(exactly 1) | The output index number of this output within this transaction
→ → →<br>`scriptPubKey` | object | Required<br>(exactly 1) | An object describing the pubkey script
→ → → →<br>`asm` | string | Required<br>(exactly 1) | The pubkey script in decoded form with non-data-pushing opcodes listed
→ → → →<br>`hex` | string (hex) | Required<br>(exactly 1) | The pubkey script encoded as hex
→ → → →<br>`reqSigs` | number (int) | Optional<br>(0 or 1) | The number of signatures required; this is always `1` for P2PK, P2PKH, and P2SH (including P2SH multisig because the redeem script is not available in the pubkey script).  It may be greater than 1 for bare multisig.  This value will not be returned for `nulldata` or `nonstandard` script types (see the `type` key below)
→ → → →<br>`type` | string | Optional<br>(0 or 1) | The type of script.  This will be one of the following:<br>• `pubkey` for a P2PK script<br>• `pubkeyhash` for a P2PKH script<br>• `scripthash` for a P2SH script<br>• `multisig` for a bare multisig script<br>• `nulldata` for nulldata scripts<br>• `nonstandard` for unknown scripts
→ → → →<br>`addresses` | string : array | Optional<br>(0 or 1) | The P2PKH or P2SH addresses used in this transaction, or the computed P2PKH address of any pubkeys in this transaction.  This array will not be returned for `nulldata` or `nonstandard` script types
→ → → → →<br>Address | string | Required<br>(1 or more) | A P2PKH or P2SH address
→<br>`extraPayloadSize` | number (int) | Optional<br>(0 or 1) | *Added in Dash Core 0.13.0.0*<br><br>Size of the DIP2 extra payload. Only present if it's a DIP2 special transaction
→<br>`extraPayload` | string (hex) | Optional<br>(0 or 1) | *Added in Dash Core 0.13.0.0*<br><br>Hex encoded DIP2 extra payload data. Only present if it's a DIP2 special transaction

*Example from Dash Core 0.13.0*

Decode a signed one-input, two-output transaction:

``` bash
dash-cli decoderawtransaction 02000000015d0b26079696875e9fc3cb480420aae3c8\
b1da628fbb14cc718066df7fe7c5fd010000006a47304402202cfa683981898ad9adb89534\
23a38f7185ed41e163aa195d608fbe5bc3034910022034e2376aaed1c6576c0dad79d626ee\
27f706baaed86dabb105979c3e6f6e1cb9012103d14eb001cf0908f3a2333d171f6236497a\
82318a6a6f649b4d7fd8e5c8922e08feffffff021e3f4b4c000000001976a914b02ae52066\
542b4aec5cf45c7cae3183d7bd322788ac00f90295000000001976a914252c9de3a0ebd5c9\
5886187b24969d4ccdb5576e88ac943d0000
```

Result:

``` json
{
  "txid": "f4de3be04efa18e203c9d0b7ad11bb2517f5889338918ed300a374f5bd736ed7",
  "size": 225,
  "version": 2,
  "type": 0,
  "locktime": 15764,
  "vin": [
    {
      "txid": "fdc5e77fdf668071cc14bb8f62dab1c8e3aa200448cbc39f5e87969607260b5d",
      "vout": 1,
      "scriptSig": {
        "asm": "304402202cfa683981898ad9adb8953423a38f7185ed41e163aa195d608fbe5bc3034910022034e2376aaed1c6576c0dad79d626ee27f706baaed86dabb105979c3e6f6e1cb9[ALL] 03d14eb001cf0908f3a2333d171f6236497a82318a6a6f649b4d7fd8e5c8922e08",
        "hex": "47304402202cfa683981898ad9adb8953423a38f7185ed41e163aa195d608fbe5bc3034910022034e2376aaed1c6576c0dad79d626ee27f706baaed86dabb105979c3e6f6e1cb9012103d14eb001cf0908f3a2333d171f6236497a82318a6a6f649b4d7fd8e5c8922e08"
      },
      "sequence": 4294967294
    }
  ],
  "vout": [
    {
      "value": 12.79999774,
      "valueSat": 1279999774,
      "n": 0,
      "scriptPubKey": {
        "asm": "OP_DUP OP_HASH160 b02ae52066542b4aec5cf45c7cae3183d7bd3227 OP_EQUALVERIFY OP_CHECKSIG",
        "hex": "76a914b02ae52066542b4aec5cf45c7cae3183d7bd322788ac",
        "reqSigs": 1,
        "type": "pubkeyhash",
        "addresses": [
          "ycNwAN4DQ7Xnw5XLKg84SR4U1GE22FfLNQ"
        ]
      }
    },
    {
      "value": 25.00000000,
      "valueSat": 2500000000,
      "n": 1,
      "scriptPubKey": {
        "asm": "OP_DUP OP_HASH160 252c9de3a0ebd5c95886187b24969d4ccdb5576e OP_EQUALVERIFY OP_CHECKSIG",
        "hex": "76a914252c9de3a0ebd5c95886187b24969d4ccdb5576e88ac",
        "reqSigs": 1,
        "type": "pubkeyhash",
        "addresses": [
          "yPi1JKw5fn8bMFsCCtnkGagogW6GXwGktZ"
        ]
      }
    }
  ],
  "instantlock": true
}
```

Decode a coinbase special transaction (CbTx):

``` bash
dash-cli decoderawtransaction 03000500010000000000000000000000000000000000\
000000000000000000000000000000ffffffff2703ae50011a4d696e656420627920416e74\
506f6f6c2021000b01201da9196f0000000007000000ffffffff02809e4730000000001976\
a914cbd7bfcc50351180132b2c0698cb90ad74c473c788ac809e4730000000001976a91488\
a060bc2dfe05780ae4dcb6c98b12436c35a93988ac00000000460200ae50010078e5c08b39\
960887bf95185c381bdb719e60b6925fa12af78a8824fade927387c757acb6bac63da84f92\
45e20cfd5d830382ac634d434725ca6349ab5db920a3
```

Result:

``` json
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
  "instantlock_internal": false,  
  "chainlock": false
}
```

*See also*

* [CombineRawTransaction](/docs/core-api-ref-remote-procedure-calls-raw-transaction#section-combinerawtransaction): combine multiple partially signed transactions into one transaction.
* [CreateRawTransaction](/docs/core-api-ref-remote-procedure-calls-raw-transaction#section-createrawtransaction): creates an unsigned serialized transaction that spends a previous output to a new output with a P2PKH or P2SH address. The transaction is not stored in the wallet or transmitted to the network.
* [SignRawTransaction](/docs/core-api-ref-remote-procedure-calls-raw-transaction#section-signrawtransaction): signs a transaction in the serialized transaction format using private keys stored in the wallet or provided in the call.
* [SendRawTransaction](/docs/core-api-ref-remote-procedure-calls-raw-transaction#section-sendrawtransaction): validates a transaction and broadcasts it to the peer-to-peer network.

# DecodeScript

The [`decodescript` RPC](core-api-ref-remote-procedure-calls-raw-transaction#section-decodescript) decodes a hex-encoded P2SH redeem script.

*Parameter #1---a hex-encoded redeem script*

Name | Type | Presence | Description
--- | --- | --- | ---
Redeem Script | string (hex) | Required<br>(exactly 1) | The redeem script to decode as a hex-encoded serialized script

*Result---the decoded script*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | An object describing the decoded script, or JSON `null` if the script could not be decoded
→<br>`asm` | string | Required<br>(exactly 1) | The redeem script in decoded form with non-data-pushing opcodes listed.  May be empty
→<br>`reqSigs` | number (int) | Optional<br>(0 or 1) | The number of signatures required; this is always `1` for P2PK or P2PKH within P2SH.  It may be greater than 1 for P2SH multisig.  This value will not be returned for `nonstandard` script types (see the `type` key above)
→<br>`type` | string | Optional<br>(0 or 1) | The type of script.  This will be one of the following:<br>• `pubkey` for a P2PK script inside P2SH<br>• `pubkeyhash` for a P2PKH script inside P2SH<br>• `multisig` for a multisig script inside P2SH<br>• `nonstandard` for unknown scripts
→<br>`addresses` | array | Optional<br>(0 or 1) | A P2PKH addresses used in this script, or the computed P2PKH addresses of any pubkeys in this script.  This array will not be returned for `nonstandard` script types
→ →<br>Address | string | Required<br>(1 or more) | A P2PKH address
→<br>`p2sh` | string (hex) | Required<br>(exactly 1) | The P2SH address of this redeem script

*Example from Dash Core 0.12.2*

A 2-of-3 P2SH multisig pubkey script:

``` bash
dash-cli -testnet decodescript 522102eacba539d92eb88d4e73bb32\
749d79f53f6e8d7947ac40a71bd4b26c13b6ec29210311f97539724e0de38fb1\
ff79f5148e5202459d06ed07193ab18c730274fd0d882103251f25a5c0291446\
d801ba6df122f67a7dd06c60a9b332b7b29cc94f3b8f57d053ae
```

Result:

``` json
{
  "asm": "2 02eacba539d92eb88d4e73bb32749d79f53f6e8d7947ac40a71bd4b26c13b6ec29 0311f97539724e0de38fb1ff79f5148e5202459d06ed07193ab18c730274fd0d88 03251f25a5c0291446d801ba6df122f67a7dd06c60a9b332b7b29cc94f3b8f57d0 3 OP_CHECKMULTISIG",
  "reqSigs": 2,
  "type": "multisig",
  "addresses": [
    "yNpezfFDfoikDuT1f4iK75AiLp2YLPsGAb",
    "yWAk1cDVvsRdPYjnzcFkySJux75yaCE7xz",
    "yVJj7TB3ZhMcSP2wo65ZFNqy23BQH9tT87"
  ],
  "p2sh": "8uJLxDxk2gEMbidF5vT8XLS2UCgQmVcroW"
}
```

*See also*

* [CreateMultiSig](/docs/core-api-ref-remote-procedure-calls-utility#section-createmultisig): creates a P2SH multi-signature address.

﻿

# FundRawTransaction

*Requires wallet support.*

The [`fundrawtransaction` RPC](core-api-ref-remote-procedure-calls-raw-transaction#section-fundrawtransaction) adds inputs to a transaction until it has enough in value to meet its out value.  This will not modify existing inputs, and will add one change output to the outputs.
Note that inputs which were signed may need to be resigned after completion since in/outputs have been added.  The inputs added will not be signed, use signrawtransaction for that.
All existing inputs must have their previous output transaction be in the wallet.

*Parameter #1---The hex string of the raw transaction*

Name | Type | Presence | Description
--- | --- | --- | ---
Hexstring | string (hex) | Required<br>(exactly 1) | The hex string of the raw transaction

*Parameter #2---Additional options*

Note: For backwards compatibility, passing in a `true` instead of an object will result in {\"includeWatching\":true}\n"

Name | Type | Presence | Description
--- | --- | --- | ---
Options | Object | Optional<br>(0 or 1) | *Added in Bitcoin Core 0.13.0*<br><br>Additional options
→ <br>`changeAddress` | string | Optional<br>(0 or 1) | The bitcoin address to receive the change. If not set, the address is chosen from address pool
→ <br>`changePosition` | nummeric (int) | Optional<br>(0 or 1) | The index of the change output. If not set, the change position is randomly chosen
`includeWatching` | bool | Optional<br>(0 or 1) | Inputs from watch-only addresses are also considered. The default is `false`
→ <br>`lockUnspent` | bool | Optional<br>(0 or 1) | The selected outputs are locked after running the rpc call. The default is `false`
→ <br>`reserveChangeKey` | bool | Optional<br>(0 or 1) | *Deprecated and ignored in Dash Core 0.15.0*<br><br>Reserves the change output key from the keypool. The default is `true`. Before Bitcoin Core 0.14.0, the used keypool key was never marked as change-address key and directly returned to the keypool (leading to address reuse).  
→ <br>`feeRate` | numeric (bitcoins) | Optional<br>(0 or 1) | The specific feerate  you are willing to pay(BTC per KB). If not set, the wallet determines the fee
→ <br>`subtractFeeFromOutputs` | array | Optional<br>(0 or 1) | A json array of integers. The fee will be equally deducted from the amount of each specified output. The outputs are specified by their zero-based index, before any change output is added.
→ →<br>Output index | numeric (int) | Optional<br>(0 or more) | A output index number (vout) from which the fee should be subtracted. If multiple vouts are provided, the total fee will be divided by the number of vouts listed and each vout will have that amount subtracted from it.

*Result---information about the created transaction*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | An object including information about the created transaction
→ <br>hex | string (hex) | Required<br>(Exactly 1) | The resulting unsigned raw transaction in serialized transaction format encoded as hex
→ <br>fee | numeric (bitcoins) | Required<br>(Exactly 1) | Fee in BTC the resulting transaction pays
→ <br>changepos | numeric (int) | Required<br>(Exactly 1) | The position of the added change output, or `-1` if no change output was added

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet fundrawtransaction 01000000000100205fa012000000\
1976a914485485425fa99504ec1638ac4213f3cfc9f32ef388ac00000000

```

Result:

``` text
{
  "hex": "01000000016b490886c0198b028c6c5cb145c4eb3b1055a224a7a105aadeff41b69ec91e060100000000feffffff023e1207bf010000001976a914bd652a167e7ad674f7815dc549bea9c57a7f919b88ac00205fa0120000001976a914485485425fa99504ec1638ac4213f3cfc9f32ef388ac00000000",
  "changepos": 0,
  "fee": 0.00000226
}
```

*See also*

* [CreateRawTransaction](/docs/core-api-ref-remote-procedure-calls-raw-transaction#section-createrawtransaction): creates an unsigned serialized transaction that spends a previous output to a new output with a P2PKH or P2SH address. The transaction is not stored in the wallet or transmitted to the network.
* [DecodeRawTransaction](/docs/core-api-ref-remote-procedure-calls-raw-transaction#section-decoderawtransaction): decodes a serialized transaction hex string into a JSON object describing the transaction.
* [SignRawTransaction](/docs/core-api-ref-remote-procedure-calls-raw-transaction#section-signrawtransaction): signs a transaction in the serialized transaction format using private keys stored in the wallet or provided in the call.
* [SendRawTransaction](/docs/core-api-ref-remote-procedure-calls-raw-transaction#section-sendrawtransaction): validates a transaction and broadcasts it to the peer-to-peer network.
* [Serialized Transaction Format](core-ref-transactions-raw-transaction-format)

﻿

# GetRawTransaction

The [`getrawtransaction` RPC](core-api-ref-remote-procedure-calls-raw-transaction#section-getrawtransaction) gets a hex-encoded serialized transaction or a JSON object describing the transaction. By default, Dash Core only stores complete transaction data for UTXOs and your own transactions, so the RPC may fail on historic transactions unless you use the non-default `txindex=1` in your Dash Core startup settings.

Note: By default this function only works for mempool transactions. If the
`-txindex` option is enabled, it also works for blockchain transactions. For now,
it also works for transactions with unspent outputs although this feature is
deprecated.

[block:callout]
{
  "type": "warning",
  "title": "Reindex note",
  "body": "If you begin using `txindex=1` after downloading the block chain, you must rebuild your indexes by starting Dash Core with the option  `-reindex`.  This may take several hours to complete, during which time your node will not process new blocks or transactions. This reindex only needs to be done once."
}
[/block]

*Parameter #1---the TXID of the transaction to get*

Name | Type | Presence | Description
--- | --- | --- | ---
TXID | string (hex) | Required<br>(exactly 1) | The TXID of the transaction to get, encoded as hex in RPC byte order

*Parameter #2---whether to get the serialized or decoded transaction*

Name | Type | Presence | Description
--- | --- | --- | ---
Format | bool | Optional<br>(0 or 1) | *Updated in Dash Core 0.12.3 / Bitcoin Core 0.14.0*<br><br>Set to `false` (the default) to return the serialized transaction as hex.  Set to `true` to return a decoded transaction.  Before 0.12.3, use `0` and `1`, respectively

*Result (if transaction not found)---`null`*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | If the transaction wasn't found, the result will be JSON `null`.  This can occur because the transaction doesn't exist in the block chain or memory pool, or because it isn't part of the transaction index.  See the Dash Core `-help` entry for `-txindex`

*Result (if verbose=`false`)---the serialized transaction*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string (hex) | Required<br>(exactly 1) | If the transaction was found, this will be the serialized transaction encoded as hex

*Result (if verbose=`true`)---the decoded transaction*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | If the transaction was found, this will be an object describing it
→<br>`txid` | string (hex) | Required<br>(exactly 1) | The transaction's TXID encoded as hex in RPC byte order
→<br>`size` | number (int) | Required<br>(exactly 1) | *Added in Bitcoin Core 0.12.0*<br><br>The serialized transaction size
→<br>`version` | number (int) | Required<br>(exactly 1) | The transaction format version number
→<br>`type` | number (int) | Required<br>(exactly 1) | *Added in Dash Core 0.13.0.0*<br><br>The transaction format type
→<br>`locktime` | number (int) | Required<br>(exactly 1) | The transaction's locktime: either a Unix epoch date or block height; see the [locktime parsing rules](core-guide-transactions-locktime-and-sequence-number#locktime_parsing_rules)
→<br>`vin` | array | Required<br>(exactly 1) | An array of objects with each object being an input vector (vin) for this transaction.  Input objects will have the same order within the array as they have in the transaction, so the first input listed will be input 0
→ →<br>Input | object | Required<br>(1 or more) | An object describing one of this transaction's inputs.  May be a regular input or a coinbase
→ → →<br>`txid` | string | Optional<br>(0 or 1) | The TXID of the outpoint being spent, encoded as hex in RPC byte order.  Not present if this is a coinbase transaction
→ → →<br>`vout` | number (int) | Optional<br>(0 or 1) | The output index number (vout) of the outpoint being spent.  The first output in a transaction has an index of `0`.  Not present if this is a coinbase transaction
→ → →<br>`scriptSig` | object | Optional<br>(0 or 1) | An object describing the signature script of this input.  Not present if this is a coinbase transaction
→ → → →<br>`asm` | string | Required<br>(exactly 1) | The signature script in decoded form with non-data-pushing opcodes listed
→ → → →<br>`hex` | string (hex) | Required<br>(exactly 1) | The signature script encoded as hex
→ → →<br>`coinbase` | string (hex) | Optional<br>(0 or 1) | The coinbase (similar to the hex field of a scriptSig) encoded as hex.  Only present if this is a coinbase transaction
→ → →<br>`value` | number (Dash) | Optional<br>(exactly 1) | The number of Dash paid to this output.  May be `0`.<br><br>Only present if `spentindex` enabled
→ → →<br>`valueSat` | number (duffs) | Optional<br>(exactly 1) | The number of duffs paid to this output.  May be `0`.<br><br>Only present if `spentindex` enabled
→ → → →<br>`addresses` | string : array | Optional<br>(0 or 1) | The P2PKH or P2SH addresses used in this transaction, or the computed P2PKH address of any pubkeys in this transaction.  This array will not be returned for `nulldata` or `nonstandard` script types.<br><br>Only present if `spentindex` enabled
→ → → → →<br>Address | string | Required<br>(1 or more) | A P2PKH or P2SH address
→ → →<br>`sequence` | number (int) | Required<br>(exactly 1) | The input sequence number
→<br>`vout` | array | Required<br>(exactly 1) | An array of objects each describing an output vector (vout) for this transaction.  Output objects will have the same order within the array as they have in the transaction, so the first output listed will be output 0
→ →<br>Output | object | Required<br>(1 or more) | An object describing one of this transaction's outputs
→ → →<br>`value` | number (Dash) | Required<br>(exactly 1) | The number of Dash paid to this output.  May be `0`
→ → →<br>`valueSat` | number (duffs) | Required<br>(exactly 1) | The number of duffs paid to this output.  May be `0`
→ → →<br>`n` | number (int) | Required<br>(exactly 1) | The output index number of this output within this transaction
→ → →<br>`scriptPubKey` | object | Required<br>(exactly 1) | An object describing the pubkey script
→ → → →<br>`asm` | string | Required<br>(exactly 1) | The pubkey script in decoded form with non-data-pushing opcodes listed
→ → → →<br>`hex` | string (hex) | Required<br>(exactly 1) | The pubkey script encoded as hex
→ → → →<br>`reqSigs` | number (int) | Optional<br>(0 or 1) | The number of signatures required; this is always `1` for P2PK, P2PKH, and P2SH (including P2SH multisig because the redeem script is not available in the pubkey script).  It may be greater than 1 for bare multisig.  This value will not be returned for `nulldata` or `nonstandard` script types (see the `type` key below)
→ → → →<br>`type` | string | Optional<br>(0 or 1) | The type of script.  This will be one of the following:<br>• `pubkey` for a P2PK script<br>• `pubkeyhash` for a P2PKH script<br>• `scripthash` for a P2SH script<br>• `multisig` for a bare multisig script<br>• `nulldata` for nulldata scripts<br>• `nonstandard` for unknown scripts
→ → → →<br>`addresses` | string : array | Optional<br>(0 or 1) | The P2PKH or P2SH addresses used in this transaction, or the computed P2PKH address of any pubkeys in this transaction.  This array will not be returned for `nulldata` or `nonstandard` script types
→ → → → →<br>Address | string | Required<br>(1 or more) | A P2PKH or P2SH address
→<br>`extraPayloadSize` | number (int) | Optional<br>(0 or 1) | *Added in Dash Core 0.13.0.0*<br><br>Size of the DIP2 extra payload. Only present if it's a DIP2 special transaction
→<br>`extraPayload` | string (hex) | Optional<br>(0 or 1) | *Added in Dash Core 0.13.0.0*<br><br>Hex encoded DIP2 extra payload data. Only present if it's a DIP2 special transaction
→<br>`hex` | string (hex) | Required<br>(exactly 1) | The serialized, hex-encoded data for the provided `txid`   
→<br>`blockhash` | string (hex) | Optional<br>(0 or 1) | If the transaction has been included in a block on the local best block chain, this is the hash of that block encoded as hex in RPC byte order
→<br>`height` | number (int) | Optional<br>(0 or 1) | The block height where the transaction was mined
→<br>`confirmations` | number (int) | Required<br>(exactly 1) | If the transaction has been included in a block on the local best block chain, this is how many confirmations it has.  Otherwise, this is `0`
→<br>`time` | number (int) | Optional<br>(0 or 1) | If the transaction has been included in a block on the local best block chain, this is the block header time of that block (may be in the future)
→<br>`blocktime` | number (int) | Optional<br>(0 or 1) | This field is currently identical to the time field described above
→<br>`instantlock` | bool | Required<br>(exactly 1) | If set to `true`, this transaction is locked (by InstantSend or a ChainLock)
→<br>`instantlock_internal` | bool | Required<br>(exactly 1) | If set to `true`, this transaction has an InstantSend lock
→<br>`chainlock` | bool | Required<br>(exactly 1) | *Added in Dash Core 0.14.0*<br><br>If set to `true`, this transaction is in a block that is locked (not susceptible to a chain re-org)

*Examples from Dash Core 0.14.0*

A classical transaction in serialized transaction format:

``` bash
dash-cli getrawtransaction \
  f4de3be04efa18e203c9d0b7ad11bb2517f5889338918ed300a374f5bd736ed7
```

Result (wrapped):

``` text
02000000015d0b26079696875e9fc3cb480420aae3c8b1da628fbb14cc718066\
df7fe7c5fd010000006a47304402202cfa683981898ad9adb8953423a38f7185\
ed41e163aa195d608fbe5bc3034910022034e2376aaed1c6576c0dad79d626ee\
27f706baaed86dabb105979c3e6f6e1cb9012103d14eb001cf0908f3a2333d17\
1f6236497a82318a6a6f649b4d7fd8e5c8922e08feffffff021e3f4b4c000000\
001976a914b02ae52066542b4aec5cf45c7cae3183d7bd322788ac00f9029500\
0000001976a914252c9de3a0ebd5c95886187b24969d4ccdb5576e88ac943d0000
```

Get the same transaction in JSON:

``` bash
dash-cli getrawtransaction \
f4de3be04efa18e203c9d0b7ad11bb2517f5889338918ed300a374f5bd736ed7 \
1
```

Result:

``` json
{
  "txid": "f4de3be04efa18e203c9d0b7ad11bb2517f5889338918ed300a374f5bd736ed7",
  "version": 2,
  "type": 0,
  "size": 225,
  "locktime": 15764,
  "vin": [
    {
      "txid": "fdc5e77fdf668071cc14bb8f62dab1c8e3aa200448cbc39f5e87969607260b5d",
      "vout": 1,
      "scriptSig": {
        "asm": "304402202cfa683981898ad9adb8953423a38f7185ed41e163aa195d608fbe5bc3034910022034e2376aaed1c6576c0dad79d626ee27f706baaed86dabb105979c3e6f6e1cb9[ALL] 03d14eb001cf0908f3a2333d171f6236497a82318a6a6f649b4d7fd8e5c8922e08",
        "hex": "47304402202cfa683981898ad9adb8953423a38f7185ed41e163aa195d608fbe5bc3034910022034e2376aaed1c6576c0dad79d626ee27f706baaed86dabb105979c3e6f6e1cb9012103d14eb001cf0908f3a2333d171f6236497a82318a6a6f649b4d7fd8e5c8922e08"
      },
      "value": 37.80000000,
      "valueSat": 3780000000,
      "address": "yTsGq4wV8WF5GKLaYV2C43zrkr2sfTtysT",
      "sequence": 4294967294
    }
  ],
  "vout": [
    {
      "value": 12.79999774,
      "valueSat": 1279999774,
      "n": 0,
      "scriptPubKey": {
        "asm": "OP_DUP OP_HASH160 b02ae52066542b4aec5cf45c7cae3183d7bd3227 OP_EQUALVERIFY OP_CHECKSIG",
        "hex": "76a914b02ae52066542b4aec5cf45c7cae3183d7bd322788ac",
        "reqSigs": 1,
        "type": "pubkeyhash",
        "addresses": [
          "ycNwAN4DQ7Xnw5XLKg84SR4U1GE22FfLNQ"
        ]
      },
      "spentTxId": "85b40136f077cded9587022645fde82389e2f01b0bec697d8cd22ccab930f3d3",
      "spentIndex": 19,
      "spentHeight": 20631
    },
    {
      "value": 25.00000000,
      "valueSat": 2500000000,
      "n": 1,
      "scriptPubKey": {
        "asm": "OP_DUP OP_HASH160 252c9de3a0ebd5c95886187b24969d4ccdb5576e OP_EQUALVERIFY OP_CHECKSIG",
        "hex": "76a914252c9de3a0ebd5c95886187b24969d4ccdb5576e88ac",
        "reqSigs": 1,
        "type": "pubkeyhash",
        "addresses": [
          "yPi1JKw5fn8bMFsCCtnkGagogW6GXwGktZ"
        ]
      },
      "spentTxId": "90aa8902dbab71c07a5ac06dfe45c5e5fa1f032788b5f916951d0969c9feef39",
      "spentIndex": 1,
      "spentHeight": 24630
    }
  ],
  "hex": "02000000015d0b26079696875e9fc3cb480420aae3c8b1da628fbb14cc718066df7fe7c5fd010000006a47304402202cfa683981898ad9adb8953423a38f7185ed41e163aa195d608fbe5bc3034910022034e2376aaed1c6576c0dad79d626ee27f706baaed86dabb105979c3e6f6e1cb9012103d14eb001cf0908f3a2333d171f6236497a82318a6a6f649b4d7fd8e5c8922e08feffffff021e3f4b4c000000001976a914b02ae52066542b4aec5cf45c7cae3183d7bd322788ac00f90295000000001976a914252c9de3a0ebd5c95886187b24969d4ccdb5576e88ac943d0000",
  "blockhash": "0000000005f395d62a40ef9f2a13000bd4076e2131c8671db8333a5b31e4403f",
  "height": 15765,
  "confirmations": 153657,
  "time": 1546278750,
  "blocktime": 1546278750,
  "instantlock": true,
  "instantlock_internal": false,
  "chainlock": true
}
```

A special transaction (CbTx) in serialized transaction format:

``` bash
dash-cli getrawtransaction \
  25632685ed0d7286901a80961c924c1ddd952e764754dbd8b40d0956413c8b56
```

Result (wrapped):

``` text
030005000100000000000000000000000000000000000000000000000000000000000\
00000ffffffff2703ae50011a4d696e656420627920416e74506f6f6c2021000b0120\
1da9196f0000000007000000ffffffff02809e4730000000001976a914cbd7bfcc503\
51180132b2c0698cb90ad74c473c788ac809e4730000000001976a91488a060bc2dfe\
05780ae4dcb6c98b12436c35a93988ac00000000460200ae50010078e5c08b3996088\
7bf95185c381bdb719e60b6925fa12af78a8824fade927387c757acb6bac63da84f92\
45e20cfd5d830382ac634d434725ca6349ab5db920a3
```

Get the same transaction in JSON:

``` bash
dash-cli getrawtransaction \
25632685ed0d7286901a80961c924c1ddd952e764754dbd8b40d0956413c8b56 \
1
```

Result:

``` json
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
      },
      "spentTxId": "1790b286922d1a439bdc056939bc902a222f9d66ee63d8427805617eedf835bd",
      "spentIndex": 83,
      "spentHeight": 94680
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
  "hex": "03000500010000000000000000000000000000000000000000000000000000000000000000ffffffff2703ae50011a4d696e656420627920416e74506f6f6c2021000b01201da9196f0000000007000000ffffffff02809e4730000000001976a914cbd7bfcc50351180132b2c0698cb90ad74c473c788ac809e4730000000001976a91488a060bc2dfe05780ae4dcb6c98b12436c35a93988ac00000000460200ae50010078e5c08b39960887bf95185c381bdb719e60b6925fa12af78a8824fade927387c757acb6bac63da84f9245e20cfd5d830382ac634d434725ca6349ab5db920a3",
  "blockhash": "00000000007b0fb99e36713cf08012482478ee496e6dcb4007ad2e806306e62b",
  "height": 86190,
  "confirmations": 83233,
  "time": 1556114577,
  "blocktime": 1556114577,
  "instantlock": true,
  "instantlock_internal": false,
  "chainlock": true
}
```

*See also*

* [GetSpecialTxes](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getspecialtxes): returns an array of special transactions found in the specified block
* [GetTransaction](/docs/core-api-ref-remote-procedure-calls-wallet#section-gettransaction): gets detailed information about an in-wallet transaction.

# SendRawTransaction

The [`sendrawtransaction` RPC](core-api-ref-remote-procedure-calls-raw-transaction#section-sendrawtransaction) validates a transaction and broadcasts it to the peer-to-peer network.

*Parameter #1---a serialized transaction to broadcast*

Name | Type | Presence | Description
--- | --- | --- | ---
Transaction | string (hex) | Required<br>(exactly 1) | The serialized transaction to broadcast encoded as hex

*Parameter #2--whether to allow high fees*

Name | Type | Presence | Description
--- | --- | --- | ---
Allow High Fees | bool | Optional<br>(0 or 1) | Set to `true` to allow the transaction to pay a high transaction fee.  Set to `false` (the default) to prevent Bitcoin Core from broadcasting the transaction if it includes a high fee.  Transaction fees are the sum of the inputs minus the sum of the outputs, so this high fees check helps ensures user including a change address to return most of the difference back to themselves

*Parameter #3--whether to use InstantSend*

Name | Type | Presence | Description
--- | --- | --- | ---
Use InstantSend | bool | Optional<br>(0 or 1) | *Depcrecated and ignored since Dash Core 0.15.0*

*Result---a TXID or error message*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null/string (hex) | Required<br>(exactly 1) | If the transaction was accepted by the node for broadcast, this will be the TXID of the transaction encoded as hex in RPC byte order.  If the transaction was rejected by the node, this will set to `null`, the JSON-RPC error field will be set to a code, and the JSON-RPC message field may contain an informative error message

*Examples from Dash Core 0.12.2*

Broadcast a signed transaction:

``` bash
dash-cli -testnet sendrawtransaction 01000000016b490886c0198b\
028c6c5cb145c4eb3b1055a224a7a105aadeff41b69ec91e0601000000694630\
43022033a61c56fa0867ed67b76b023204a9dc0ee6b0d63305dc5f65fe943354\
45ff2f021f712f55399d5238fc7146497c431fc4182a1de0b96fc22716e0845f\
561d542e012102eacba539d92eb88d4e73bb32749d79f53f6e8d7947ac40a71b\
d4b26c13b6ec29ffffffff0200205fa0120000001976a914485485425fa99504\
ec1638ac4213f3cfc9f32ef388acc0a8f9be010000001976a914811eacc14db8\
ebb5b64486dc43400c0226b428a488ac00000000
```

Result:

``` text
2f124cb550d9967b81914b544dea3783de23e85d67a9816f9bada665ecfe1cd5
```

*See also*

* [CombineRawTransaction](/docs/core-api-ref-remote-procedure-calls-raw-transaction#section-combinerawtransaction): combine multiple partially signed transactions into one transaction.
* [CreateRawTransaction](/docs/core-api-ref-remote-procedure-calls-raw-transaction#section-createrawtransaction): creates an unsigned serialized transaction that spends a previous output to a new output with a P2PKH or P2SH address. The transaction is not stored in the wallet or transmitted to the network.
* [DecodeRawTransaction](/docs/core-api-ref-remote-procedure-calls-raw-transaction#section-decoderawtransaction): decodes a serialized transaction hex string into a JSON object describing the transaction.
* [SignRawTransaction](/docs/core-api-ref-remote-procedure-calls-raw-transaction#section-signrawtransaction): signs a transaction in the serialized transaction format using private keys stored in the wallet or provided in the call.

# SignRawTransaction

The [`signrawtransaction` RPC](core-api-ref-remote-procedure-calls-raw-transaction#section-signrawtransaction) signs a transaction in the serialized transaction format using private keys stored in the wallet or provided in the call.

*Parameter #1---the transaction to sign*

Name | Type | Presence | Description
--- | --- | --- | ---
Transaction | string (hex | Required<br>(exactly 1) | The transaction to sign as a serialized transaction

*Parameter #2---unspent transaction output details*

Name | Type | Presence | Description
--- | --- | --- | ---
Dependencies | array | Optional<br>(0 or 1) | The previous outputs being spent by this transaction
→<br>Output | object | Optional<br>(0 or more) | An output being spent
→ →<br>`txid` | string (hex) | Required<br>(exactly 1) | The TXID of the transaction the output appeared in.  The TXID must be encoded in hex in RPC byte order
→ →<br>`vout` | number (int) | Required<br>(exactly 1) | The index number of the output (vout) as it appeared in its transaction, with the first output being 0
→ →<br>`scriptPubKey` | string (hex) | Required<br>(exactly 1) | The output's pubkey script encoded as hex
→ →<br>`redeemScript` | string (hex) | Optional<br>(0 or 1) | If the pubkey script was a script hash, this must be the corresponding redeem script
→ →<br>`amount` | numeric | Required<br>(exactly 1) | The amount of Dash spent

*Parameter #3---private keys for signing*

Name | Type | Presence | Description
--- | --- | --- | ---
Private Keys | array | Optional<br>(0 or 1) | An array holding private keys.  If any keys are provided, only they will be used to sign the transaction (even if the wallet has other matching keys).  If this array is empty or not used, and wallet support is enabled, keys from the wallet will be used
→<br>Key | string (base58) | Required<br>(1 or more) | A private key in base58check format to use to create a signature for this transaction

*Parameter #4---signature hash type*

Name | Type | Presence | Description
--- | --- | --- | ---
SigHash | string | Optional<br>(0 or 1) | The type of signature hash to use for all of the signatures performed.  (You must use separate calls to the [`signrawtransaction` RPC](core-api-ref-remote-procedure-calls-raw-transaction#section-signrawtransaction) if you want to use different signature hash types for different signatures.  The allowed values are: `ALL`, `NONE`, `SINGLE`, `ALL|ANYONECANPAY`, `NONE|ANYONECANPAY`, and `SINGLE|ANYONECANPAY`

*Result---the transaction with any signatures made*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | The results of the signature
→<br>`hex` | string (hex) | Required<br>(exactly 1) | The resulting serialized transaction encoded as hex with any signatures made inserted.  If no signatures were made, this will be the same transaction provided in parameter #1
→<br>`complete` | bool | Required<br>(exactly 1) | The value `true` if transaction is fully signed; the value `false` if more signatures are required

*Example from Dash Core 0.12.2*

Sign the hex generated in the example section for the `createrawtransaction`
RPC:

``` bash
dash-cli -testnet signrawtransaction 01000000016b490886c0198b028c6c5cb14\
5c4eb3b1055a224a7a105aadeff41b69ec91e060100000000ffffffff0200205fa012000\
0001976a914485485425fa99504ec1638ac4213f3cfc9f32ef388acc0a8f9be010000001\
976a914811eacc14db8ebb5b64486dc43400c0226b428a488ac00000000
```

Result:

``` json
{
  "hex": "01000000016b490886c0198b028c6c5cb145c4eb3b1055a224a7a105aadeff41b69ec91e060100000069463043022033a61c56fa0867ed67b76b023204a9dc0ee6b0d63305dc5f65fe94335445ff2f021f712f55399d5238fc7146497c431fc4182a1de0b96fc22716e0845f561d542e012102eacba539d92eb88d4e73bb32749d79f53f6e8d7947ac40a71bd4b26c13b6ec29ffffffff0200205fa0120000001976a914485485425fa99504ec1638ac4213f3cfc9f32ef388acc0a8f9be010000001976a914811eacc14db8ebb5b64486dc43400c0226b428a488ac00000000",
  "complete": true
}
```

*See also*

* [CombineRawTransaction](/docs/core-api-ref-remote-procedure-calls-raw-transaction#section-combinerawtransaction): combine multiple partially signed transactions into one transaction.
* [CreateRawTransaction](/docs/core-api-ref-remote-procedure-calls-raw-transaction#section-createrawtransaction): creates an unsigned serialized transaction that spends a previous output to a new output with a P2PKH or P2SH address. The transaction is not stored in the wallet or transmitted to the network.
* [DecodeRawTransaction](/docs/core-api-ref-remote-procedure-calls-raw-transaction#section-decoderawtransaction): decodes a serialized transaction hex string into a JSON object describing the transaction.
* [SendRawTransaction](/docs/core-api-ref-remote-procedure-calls-raw-transaction#section-sendrawtransaction): validates a transaction and broadcasts it to the peer-to-peer network.