# CreateMultiSig

The [`createmultisig` RPC](core-api-ref-remote-procedure-calls-utility#section-createmultisig) creates a P2SH multi-signature address.

*Parameter #1---the number of signatures required*

Name | Type | Presence | Description
--- | --- | --- | ---
Required | number (int) | Required<br>(exactly 1) | The minimum (*m*) number of signatures required to spend this m-of-n multisig script

*Parameter #2---the full public keys, or addresses for known public keys*

Name | Type | Presence | Description
--- | --- | --- | ---
Keys Or Addresses | array | Required<br>(exactly 1) | An array of strings with each string being a public key or address
→<br>Key Or Address | string | Required<br>(1 or more) | A public key against which signatures will be checked.  If wallet support is enabled, this may be a P2PKH address belonging to the wallet---the corresponding public key will be substituted.  There must be at least as many keys as specified by the Required parameter, and there may be more keys

*Result---P2SH address and hex-encoded redeem script*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | An object describing the multisig address
→<br>`address` | string (base58) | Required<br>(exactly 1) | The P2SH address for this multisig redeem script
→<br>`redeemScript` | string (hex) | Required<br>(exactly 1) | The multisig redeem script encoded as hex

*Example from Dash Core 0.12.2*

Creating a 2-of-3 P2SH multisig address by mixing two P2PKH addresses and
one full public key:

``` bash
dash-cli -testnet createmultisig 2 '''
  [
    "yNpezfFDfoikDuT1f4iK75AiLp2YLPsGAb",
    "0311f97539724e0de38fb1ff79f5148e5202459d06ed07193ab18c730274fd0d88",
    "yVJj7TB3ZhMcSP2wo65ZFNqy23BQH9tT87"
  ]
'''
```

Result:

``` json
{
  "address": "8uJLxDxk2gEMbidF5vT8XLS2UCgQmVcroW",
  "redeemScript": "522102eacba539d92eb88d4e73bb32749d79f53f6e8d7947ac40a71bd4b26c13b6ec29210311f97539724e0de38fb1ff79f5148e5202459d06ed07193ab18c730274fd0d882103251f25a5c0291446d801ba6df122f67a7dd06c60a9b332b7b29cc94f3b8f57d053ae"
}
```

*See also*

* [AddMultiSigAddress](/docs/core-api-ref-remote-procedure-calls-wallet#section-addmultisigaddress): adds a P2SH multisig address to the wallet.
* [DecodeScript](/docs/core-api-ref-remote-procedure-calls-raw-transaction#section-decodescript): decodes a hex-encoded P2SH redeem script.

# EstimateFee

The [`estimatefee` RPC](core-api-ref-remote-procedure-calls-utility#section-estimatefee) estimates the transaction fee per kilobyte that needs to be paid for a transaction to begin confirmation within a certain number of blocks.

*Parameter #1---how many blocks the transaction may wait before being included*

Name | Type | Presence | Description
--- | --- | --- | ---
Blocks | number (int) | Required<br>(exactly 1) | The maximum number of blocks a transaction should have to wait before it is predicted to be included in a block. Has to be between 1 and 25 blocks

*Result---the fee the transaction needs to pay per kilobyte*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | number (Dash) | Required<br>(exactly 1) | The estimated fee the transaction should pay in order to be included within the specified number of blocks.  If the node doesn't have enough information to make an estimate, the value `-1` will be returned

*Examples from Dash Core 0.12.2*

``` bash
dash-cli estimatefee 6
```

Result:

``` json
0.00044345
```

Requesting data the node can't calculate (out of range):

``` bash
dash-cli estimatefee 100
```

Result:

``` json
-1
```

*See also*

* [SetTxFee](/docs/core-api-ref-remote-procedure-calls-wallet#section-settxfee): sets the transaction fee per kilobyte paid by transactions created by this wallet.

# EstimateSmartFee

The [`estimatesmartfee` RPC](core-api-ref-remote-procedure-calls-utility#section-estimatesmartfee) estimates the transaction fee per kilobyte that needs to be paid for a transaction to begin confirmation within a certain number of blocks and returns the number of blocks for which the estimate is valid.

*Parameter #1---how many confirmations the transaction may wait before being included*

Name | Type | Presence | Description
--- | --- | --- | ---
conf_target | number (int) | Required<br>(exactly 1) | Confirmation target in blocks (1 - 1008)

*Parameter #2---estimate mode*

Name | Type | Presence | Description
--- | --- | --- | ---
estimate_mode | string | Optional<br>Default=<br>`CONSERVATIVE` | The fee estimate mode. Whether to return a more conservative estimate which also satisfies a longer history. A conservative estimate potentially returns a higher feerate and is more likely to be sufficient for the desired target, but is not as responsive to short term drops in the prevailing fee market.  Must be one of:<br>`UNSET` (defaults to `CONSERVATIVE`)<br>`ECONOMICAL`<br>`CONSERVATIVE`

*Result---the fee the transaction needs to pay per kilobyte*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | JSON Object containing estimate information
→<br>`feerate` | number (Dash) | Optional<br>(0 or 1) | The estimated fee the transaction should pay in order to be included within the specified number of blocks.  If the node doesn't have enough information to make an estimate, this field will not be returned
→<br>`error` | JSON array (strings) | Optional<br>(0 or 1) | Errors encountered during processing
→<br>`blocks` | number | Required<br>(exactly 1) | Block number where the estimate was found

*Examples from Dash Core 0.15.0*

``` bash
dash-cli estimatesmartfee 6
```

Result:

``` json
{
  "feerate": 0.00044345,
  "blocks": 6
}
```

Requesting data the node can't calculate (out of range):

``` bash
dash-cli estimatesmartfee 2
```

Result:

``` json
{
  "errors": [
    "Insufficient data or no feerate found"
  ],
  "blocks": 2
}
```

*See also*

* [SetTxFee](/docs/core-api-ref-remote-procedure-calls-wallet#section-settxfee): sets the transaction fee per kilobyte paid by transactions created by this wallet.

# SignMessageWithPrivKey

*Added in Dash Core 0.12.3 / Bitcoin Core 0.13.0*

The [`signmessagewithprivkey` RPC](core-api-ref-remote-procedure-calls-utility#section-signmessagewithprivkey) signs a message with a given private key.

*Parameter #1---the private key to sign with*

Name | Type | Presence | Description
--- | --- | --- | ---
Private Key | string (base58) | Required<br>(exactly 1) | The private key to sign the message with encoded in base58check using wallet import format (WIF)

*Parameter #2---the message to sign*

Name | Type | Presence | Description
--- | --- | --- | ---
Message | string | Required<br>(exactly 1) | The message to sign

*Result---the message signature*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string (base64) | Required<br>(exactly 1) | The signature of the message, encoded in base64.

*Example from Dash Core 0.12.3*

Sign a the message "Hello, World!" using the following private key:

``` bash
dash-cli signmessagewithprivkey cNKbZBqUCjuBRSnAJWwFWxKESJ5Lw\
G4uxBSJ1UeBNBGVRupFKr6S "Hello, World!"
```

Result:

``` text
IBx8jxFjutPlcZcFdQPlA2n/B4yTrYhH43qYJURKRj7LWhSD0ERE/nnRLOnXi/gwULUcqfqOKqnqkSvuJjlgEvc=
```

*See also*

* [SignMessage](/docs/core-api-ref-remote-procedure-calls-wallet#section-signmessage): signs a message with the private key of an address.
* [VerifyMessage](/docs/core-api-ref-remote-procedure-calls-utility#section-verifymessage): verifies a signed message.

# ValidateAddress

The [`validateaddress` RPC](core-api-ref-remote-procedure-calls-utility#section-validateaddress) returns information about the given Dash address.

*Parameter #1---a P2PKH or P2SH address*

Name | Type | Presence | Description
--- | --- | --- | ---
Address | string (base58) | Required<br>(exactly 1) | The P2PKH or P2SH address to validate encoded in base58check format

*Result---information about the address*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | Information about the address
→<br>`isvalid` | bool | Required<br>(exactly 1) | Set to `true` if the address is a valid P2PKH or P2SH address; set to `false` otherwise
→<br>`address` | string (base58) | Optional<br>(0 or 1) | The Dash address given as parameter
→<br>`scriptPubKey` | string (hex) | Optional<br>(0 or 1) | The hex encoded scriptPubKey generated by the address  
→<br>`ismine` | bool | Optional<br>(0 or 1) | Set to `true` if the address belongs to the wallet; set to false if it does not.  Only returned if wallet support enabled
→<br>`iswatchonly` | bool | Optional<br>(0 or 1) | Set to `true` if the address is watch-only.  Otherwise set to `false`.  Only returned if address is in the wallet
→<br>`isscript` | bool | Optional<br>(0 or 1) | Set to `true` if a P2SH address; otherwise set to `false`.  Only returned if the address is in the wallet
→<br>`script` | string | Optional<br>(0 or 1) | Only returned for P2SH addresses belonging to this wallet. This is the type of script:<br>• `pubkey` for a P2PK script inside P2SH<br>• `pubkeyhash` for a P2PKH script inside P2SH<br>• `multisig` for a multisig script inside P2SH<br>• `nonstandard` for unknown scripts
→<br>`hex` | string (hex) | Optional<br>(0 or 1) | Only returned for P2SH addresses belonging to this wallet.  This is the redeem script encoded as hex
→<br>`addresses` | array | Optional<br>(0 or 1) | Only returned for P2SH addresses belonging to the wallet.  A P2PKH addresses used in this script, or the computed P2PKH addresses of any pubkeys in this script.  This array will be empty for `nonstandard` script types
→ →<br>Address | string | Optional<br>(0 or more) | A P2PKH address
→<br>`sigsrequired` | number (int) | Optional<br>(0 or 1) | Only returned for multisig P2SH addresses belonging to the wallet.  The number of signatures required by this script
→<br>`pubkey` | string (hex) | Optional<br>(0 or 1) | The public key corresponding to this address.  Only returned if the address is a P2PKH address in the wallet
→<br>`iscompressed` | bool | Optional<br>(0 or 1) | Set to `true` if a compressed public key or set to `false` if an uncompressed public key.  Only returned if the address is a P2PKH address in the wallet
→<br>`account` | string | Optional<br>(0 or 1) | *Deprecated: will be removed in a later version of Bitcoin Core*<br><br>The account this address belong to.  May be an empty string for the default account.  Only returned if the address belongs to the wallet
→<br>`timestamp` | number (int) | Optional<br>(0 or 1) | *Added in Dash Core 0.12.3*<br><br>The creation time of the key if available in seconds since epoch (Jan 1 1970 GMT)
→<br>`hdkeypath` | string | Optional<br>(0 or 1) | *Added in Bitcoin Core 0.13.0*<br><br>The HD keypath if the key is HD and available  
→<br>`hdmasterkeyid` | string (hash160) | Optional<br>(0 or 1) | *Added in Bitcoin Core 0.13.0*<br><br>The Hash160 of the HD master public key  

*Example from Dash Core 0.12.3*

Validate the following P2PKH address from the wallet:

``` bash
dash-cli validateaddress yNpezfFDfoikDuT1f4iK75AiLp2YLPsGAb
```

Result:

``` json
{
  "isvalid": true,
  "address": "yNpezfFDfoikDuT1f4iK75AiLp2YLPsGAb",
  "scriptPubKey": "76a9141b767409bd8717b56cfcb00747811432ab1aa8a788ac",
  "ismine": true,
  "iswatchonly": false,
  "isscript": false,
  "pubkey": "02eacba539d92eb88d4e73bb32749d79f53f6e8d7947ac40a71bd4b26c13b6ec29",
  "iscompressed": true,
  "account": "Msig 1",
  "timestamp": 0
}
```

Validate the following P2SH multisig address from the wallet:

``` bash
dash-cli -testnet validateaddress 8uJLxDxk2gEMbidF5vT8XLS2UCgQmVcroW
```

Result:

``` json
{
  "isvalid": true,
  "address": "8uJLxDxk2gEMbidF5vT8XLS2UCgQmVcroW",
  "scriptPubKey": "a914a33155e490d146e656a9bac2cbee9c625ef42f0a87",
  "ismine": true,
  "iswatchonly": false,
  "isscript": true,
  "script": "multisig",
  "hex": "522102eacba539d92eb88d4e73bb32749d79f53f6e8d7947ac40a71bd4b26c13b6ec29210311f97539724e0de38fb1ff79f5148e5202459d06ed07193ab18c730274fd0d882103251f25a5c0291446d801ba6df122f67a7dd06c60a9b332b7b29cc94f3b8f57d053ae",
  "addresses": [
    "yNpezfFDfoikDuT1f4iK75AiLp2YLPsGAb",
    "yWAk1cDVvsRdPYjnzcFkySJux75yaCE7xz",
    "yVJj7TB3ZhMcSP2wo65ZFNqy23BQH9tT87"
  ],
  "sigsrequired": 2,
  "account": "test account"
  "timestamp": 0  
}
```

*See also*

* [ImportAddress](/docs/core-api-ref-remote-procedure-calls-wallet#section-importaddress): adds an address or pubkey script to the wallet without the associated private key, allowing you to watch for transactions affecting that address or pubkey script without being able to spend any of its outputs.
* [GetNewAddress](/docs/core-api-ref-remote-procedure-calls-wallet#section-getnewaddress): returns a new Dash address for receiving payments. If an account is specified, payments received with the address will be credited to that account.

# VerifyMessage

The [`verifymessage` RPC](core-api-ref-remote-procedure-calls-utility#section-verifymessage) verifies a signed message.

*Parameter #1---the address corresponding to the signing key*

Name | Type | Presence | Description
--- | --- | --- | ---
Address | string (base58) | Required<br>(exactly 1) | The P2PKH address corresponding to the private key which made the signature.  A P2PKH address is a hash of the public key corresponding to the private key which made the signature.  When the ECDSA signature is checked, up to four possible ECDSA public keys will be reconstructed from from the signature; each key will be hashed and compared against the P2PKH address provided to see if any of them match.  If there are no matches, signature validation will fail

*Parameter #2---the signature*

Name | Type | Presence | Description
--- | --- | --- | ---
Signature | string (base64) | Required<br>(exactly 1) | The signature created by the signer encoded as base-64 (the format output by the [`signmessage` RPC](core-api-ref-remote-procedure-calls-wallet#section-signmessage))

*Parameter #3---the message*

Name | Type | Presence | Description
--- | --- | --- | ---
Message | string | Required<br>(exactly 1) | The message exactly as it was signed (e.g. no extra whitespace)

*Result: `true`, `false`, or an error*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | boolean | Required<br>(exactly 1) | Set to `true` if the message was signed by a key corresponding to the provided P2PKH address; set to `false` if it was not signed by that key; set to JSON `null` if an error occurred

*Example from Dash Core 0.12.2*

Check the signature on the message created in the example for
`signmessage`:

``` bash
dash-cli -testnet verifymessage \
  yNpezfFDfoikDuT1f4iK75AiLp2YLPsGAb \
  H4XULzfHCf16In2ECk9Ta9QxQPq639zQto2JA3OLlo3JbUdrClvJ89+A1z+Z9POd6l8LJhn1jGpQYF8mX4jkQvE= \
  'Hello, World!'
```

Result:

``` json
true
```

*See also*

* [SignMessage](/docs/core-api-ref-remote-procedure-calls-wallet#section-signmessage): signs a message with the private key of an address.