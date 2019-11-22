# AbandonTransaction

*Added in Bitcoin Core 0.12.0*

The [`abandontransaction` RPC](core-api-ref-remote-procedure-calls-wallet#section-abandontransaction) marks an in-wallet transaction and all its in-wallet descendants as abandoned. This allows their inputs to be respent.

*Parameter #1---a transaction identifier (TXID)*

Name | Type | Presence | Description
--- | --- | --- | ---
TXID | string (hex) | Required<br>(exactly 1) | The TXID of the transaction that you want to abandon.  The TXID must be encoded as hex in RPC byte order

*Result---`null` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | JSON `null` when the transaction and all descendants were abandoned

*Example from Dash Core 0.12.2*

Abandons the transaction on your node.

``` bash
dash-cli abandontransaction fa3970c341c9f5de6ab13f128cbfec58d732e736a505fe32137ad551c799ecc4
```

Result (no output from `dash-cli` because result is set to `null`).

*See also*

* [SendRawTransaction](/docs/core-api-ref-remote-procedure-calls-raw-transaction#section-sendrawtransaction): validates a transaction and broadcasts it to the peer-to-peer network.

# AbortRescan

The [`abortrescan` RPC](core-api-ref-remote-procedure-calls-wallet#section-abortrescan) Stops current wallet rescan

Stops current wallet rescan triggered e.g. by an [`importprivkey` RPC](core-api-ref-remote-procedure-calls-wallet#section-importprivkey) call.

*Parameters: none*

*Result---`true` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | `true` when the command was successful or `false` if not successful.

*Example from Dash Core 0.15.0*

Abort the running wallet rescan

``` bash
dash-cli -testnet abortrescan
```

Result:
``` text
true
```

*See also: none*

# AddMultiSigAddress

*Requires wallet support.*

The [`addmultisigaddress` RPC](core-api-ref-remote-procedure-calls-wallet#section-addmultisigaddress) adds a P2SH multisig address to the wallet.

*Parameter #1---the number of signatures required*

Name | Type | Presence | Description
--- | --- | --- | ---
Required | number (int) | Required<br>(exactly 1) | The minimum (*m*) number of signatures required to spend this m-of-n multisig script

*Parameter #2---the full public keys, or addresses for known public keys*

Name | Type | Presence | Description
--- | --- | --- | ---
Keys Or Addresses | array | Required<br>(exactly 1) | An array of strings with each string being a public key or address
→<br>Key Or Address | string | Required<br>(1 or more) | A public key against which signatures will be checked.  Alternatively, this may be a P2PKH address belonging to the wallet---the corresponding public key will be substituted.  There must be at least as many keys as specified by the Required parameter, and there may be more keys

*Parameter #3---the account name*

Name | Type | Presence | Description
--- | --- | --- | ---
Account | string | Optional<br>(0 or 1) | The account name in which the address should be stored.  Default is the default account, \\" (an empty string)"

*Result---a P2SH address printed and stored in the wallet*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string (base58) | Required<br>(exactly 1) | The P2SH multisig address.  The address will also be added to the wallet, and outputs paying that address will be tracked by the wallet

*Example from Dash Core 0.12.2*

Adding a 2-of-3 P2SH multisig address to the "test account" by mixing
two P2PKH addresses and one full public key:

``` bash
dash-cli -testnet addmultisigaddress 2 '''
  [
    "yNpezfFDfoikDuT1f4iK75AiLp2YLPsGAb",
    "0311f97539724e0de38fb1ff79f5148e5202459d06ed07193ab18c730274fd0d88",
    "yVJj7TB3ZhMcSP2wo65ZFNqy23BQH9tT87"
  ]
''' \
 'test account'
```

Result:

``` text
8uJLxDxk2gEMbidF5vT8XLS2UCgQmVcroW
```

(New P2SH multisig address also stored in wallet.)

*See also*

* [CreateMultiSig](/docs/core-api-ref-remote-procedure-calls-utility#section-createmultisig): creates a P2SH multi-signature address.
* [DecodeScript](/docs/core-api-ref-remote-procedure-calls-raw-transaction#section-decodescript): decodes a hex-encoded P2SH redeem script.

# BackupWallet

*Requires wallet support.*

The [`backupwallet` RPC](core-api-ref-remote-procedure-calls-wallet#section-backupwallet) safely copies `wallet.dat` to the specified file, which can be a directory or a path with filename.

*Parameter #1---destination directory or filename*

Name | Type | Presence | Description
--- | --- | --- | ---
Destination | string | Required<br>(exactly 1) | A filename or directory name.  If a filename, it will be created or overwritten.  If a directory name, the file `wallet.dat` will be created or overwritten within that directory

*Result---`null` or error*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | Always `null` whether success or failure.  The JSON-RPC error and message fields will be set if a failure occurred

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet backupwallet /tmp/backup.dat
```

*See also*

* [DumpWallet](/docs/core-api-ref-remote-procedure-calls-wallet#section-dumpwallet): creates or overwrites a file with all wallet keys in a human-readable format.
* [ImportWallet](/docs/core-api-ref-remote-procedure-calls-wallet#section-importwallet): imports private keys from a file in wallet dump file format (see the [`dumpwallet` RPC](core-api-ref-remote-procedure-calls-wallet#section-dumpwallet)). These keys will be added to the keys currently in the wallet.  This call may need to rescan all or parts of the block chain for transactions affecting the newly-added keys, which may take several minutes.

# DumpHDInfo

The [`dumphdinfo` RPC](core-api-ref-remote-procedure-calls-wallet#section-dumphdinfo) returns an object containing sensitive private info about this HD wallet

*Parameters: none*

*Result---HD wallet information*

Name | Type | Presence | Description
--- | --- | --- | ---
Result | object | Required<br>(exactly 1) | An object containing sensitive private info about this HD wallet.
→ <br>`hdseed` | string | Required<br>(exactly 1) | The BIP-32 HD seed (in hex)
→ <br>`mnemonic` | string | Required<br>(exactly 1) | The BIP-39 mnemonic for this HD wallet (English words)
→ <br>`mnemonicpassphrase` | string | Required<br>(exactly 1) | The BIP-39 mnemonic passphrase for this HD wallet (may be empty)

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet dumphdinfo
```

Result (truncated for security reasons):
``` json
{
  "hdseed": "20c63c3fb298ebd52de3 ...",
  "mnemonic": "cost circle shiver ...",
  "mnemonicpassphrase": ""
}
```

*See also: none*

# DumpPrivKey

*Requires wallet support. Requires an unlocked wallet or an
unencrypted wallet.*

The [`dumpprivkey` RPC](core-api-ref-remote-procedure-calls-wallet#section-dumpprivkey) returns the wallet-import-format (WIP) private key corresponding to an address. (But does not remove it from the wallet.)

*Parameter #1---the address corresponding to the private key to get*

Name | Type | Presence | Description
--- | --- | --- | ---
P2PKH Address | string (base58) | Required<br>(exactly 1) | The P2PKH address corresponding to the private key you want returned.  Must be the address corresponding to a private key in this wallet

*Result---the private key*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string (base58) | Required<br>(exactly 1) | The private key encoded as base58check using wallet import format

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet dumpprivkey ycBuREgSskHHkWLxDa9A5WppCki6PfFycL
```

Result:

``` text
cQZZ4awQvcXXyES3CmUJqSgeTobQm9t9nyUr337kvUtsWsnvvMyw
```

*See also*

* [ImportPrivKey](/docs/core-api-ref-remote-procedure-calls-wallet#section-importprivkey): adds a private key to your wallet. The key should be formatted in the wallet import format created by the [`dumpprivkey` RPC](core-api-ref-remote-procedure-calls-wallet#section-dumpprivkey).
* [DumpWallet](/docs/core-api-ref-remote-procedure-calls-wallet#section-dumpwallet): creates or overwrites a file with all wallet keys in a human-readable format.

# DumpWallet

*Requires wallet support.  Requires an unlocked wallet or an unencrypted
wallet.*

The [`dumpwallet` RPC](core-api-ref-remote-procedure-calls-wallet#section-dumpwallet) creates or overwrites a file with all wallet keys in a human-readable format.

*Parameter #1---a filename*

Name | Type | Presence | Description
--- | --- | --- | ---
Filename | string | Required<br>(exactly 1) | The file in which the wallet dump will be placed.  May be prefaced by an absolute file path.  An existing file with that name will be overwritten

*Result---information about exported wallet*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | An object describing dumped wallet file
→<br>`dashcoreversion` | string | Required<br>(exactly 1) | Dash Core build details
→<br>`lastblockheight` | int | Required<br>(exactly 1) | Height of the most recent block received
→<br>`lastblockhash` | string (hex) | Required<br>(exactly 1) | Hash of the most recent block received
→<br>`lastblocktime` | string | Required<br>(exactly 1) | Timestamp of the most recent block received
→<br>`keys` | int | Required<br>(exactly 1) | Number of keys dumped
→<br>`file` | string | Required<br>(exactly 1) | Name of the file the wallet was dumped to
→<br>`warning` | string | Required<br>(exactly 1) | Warning to not share the file due to it containing the private keys

*Example from Dash Core 0.13.0*

Create a wallet dump and then print its first 10 lines.

``` bash
dash-cli -testnet dumpwallet /tmp/dump.txt
head /tmp/dump.txt
```

Results:

``` json
{
  "dashcoreversion": "v0.13.0.0",
  "lastblockheight": 250186,
  "lastblockhash": "0000000000a82fb1890de5da4740d0671910a436fe6fc4503a3e553adef073b4",
  "lastblocktime": "2018-10-23T12:50:44Z",
  "keys": 8135,
  "file": "/tmp/dump.txt",
  "warning": "/tmp/dump.txt file contains all private keys from this wallet. Do not share it with anyone!"
}
```

Results (the first 10 lines of the file):

``` bash
>>>>>>>> Wallet dump created by Dash Core v0.13.0.0
>>>>>>>> * Created on 2018-10-23T12:55:38Z
>>>>>>>> * Best block at time of backup was 250186 (0000000000a82fb1890de5da4740d0671910a436fe6fc4503a3e553adef073b4),
>>>>>>>>   mined on 2018-10-23T12:50:44Z

cQZZ4awQvcXXyES3CmUJqSgeTobQm9t9nyUr337kvUtsWsnvvMyw 2017-11-28T18:21:36Z label=test%20label # addr=ycBuREgSskHHkWLxDa9A5WppCki6PfFycL
cTBRPnJoPjEMh67v1zes437v8Po5bFLDWKgEudTJMhVaLs1ZVGJe 2017-11-28T18:21:37Z reserve=1 # addr=yNsWkgPLN1u7p5dfWYnasYdgirU2J3tjUj
cRkkwrFnQUrih3QiT87sNy1AxyfjzqVYSyVYuL3qnJcSiQfE4QJa 2017-11-28T18:21:37Z reserve=1 # addr=yRkHzRbRKn8gBp5826mbaBvxLuBBNDVQg3
cQM7KoqQjHCCTrDhnfBEY1vpW9W65zRvaQeTb41UbFb6WX8Q8UkQ 2017-11-28T18:21:37Z reserve=1 # addr=yVEdefApUYiDLHApvvWCK5afTtJeQada8Y
cTGSKYaQTQabnjNSwCqpjYXiucVujTXiwp9dzmJV9cNAiayAJusi 2017-11-28T18:21:37Z reserve=1 # addr=ybQYgp21ZyZK8JuMLb2CVwG4TaWrXVXD5M
```

*See also*

* [BackupWallet](/docs/core-api-ref-remote-procedure-calls-wallet#section-backupwallet): safely copies `wallet.dat` to the specified file, which can be a directory or a path with filename.
* [ImportWallet](/docs/core-api-ref-remote-procedure-calls-wallet#section-importwallet): imports private keys from a file in wallet dump file format (see the [`dumpwallet` RPC](core-api-ref-remote-procedure-calls-wallet#section-dumpwallet)). These keys will be added to the keys currently in the wallet.  This call may need to rescan all or parts of the block chain for transactions affecting the newly-added keys, which may take several minutes.

# EncryptWallet

*Requires wallet support.*

The [`encryptwallet` RPC](core-api-ref-remote-procedure-calls-wallet#section-encryptwallet) encrypts the wallet with a passphrase.  This is only to enable encryption for the first time. After encryption is enabled, you will need to enter the passphrase to use private keys.
[block:callout]
{
  "type": "warning",
  "body": "**Warning:** if using this RPC on the command line, remember that your shell probably saves your command lines (including the value of the passphrase parameter). In addition, there is no RPC to completely disable encryption. If you want to return to an unencrypted wallet, you must create a new wallet and restore your data from a backup made with the [`dumpwallet` RPC](core-api-ref-remote-procedure-calls-wallet#section-dumpwallet)."
}
[/block]
*Parameter #1---a passphrase*

Name | Type | Presence | Description
--- | --- | --- | ---
Passphrase | string | Required<br>(exactly 1) | The passphrase to use for the encrypted wallet.  Must be at least one character

*Result---a notice (with program shutdown)*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string | Required<br>(exactly 1) | A notice that the server is stopping and that you need to make a new backup.  The wallet is now encrypted

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet encryptwallet "test"
```

Result:

``` text
Wallet encrypted; Dash Core server stopping, restart to run with encrypted wallet.
The keypool has been flushed and a new HD seed was generated (if you are using
HD). You need to make a new backup.

```

*See also*

* [WalletPassphrase](/docs/core-api-ref-remote-procedure-calls-wallet#section-walletpassphrase): stores the wallet decryption key in memory for the indicated number of seconds. Issuing the `walletpassphrase` command while the wallet is already unlocked will set a new unlock time that overrides the old one.
* [WalletLock](/docs/core-api-ref-remote-procedure-calls-wallet#section-walletlock): removes the wallet encryption key from memory, locking the wallet. After calling this method, you will need to call `walletpassphrase` again before being able to call any methods which require the wallet to be unlocked.
* [WalletPassphraseChange](/docs/core-api-ref-remote-procedure-calls-wallet#section-walletpassphrasechange): changes the wallet passphrase from 'old passphrase' to 'new passphrase'.

# GetAccount

*Requires wallet support.*

The [`getaccount` RPC](core-api-ref-remote-procedure-calls-wallet#section-getaccount) returns the name of the account associated with the given address.
[block:callout]
{
  "type": "warning",
  "body": "**Warning:** `setaccount` will be removed in a later version of Dash Core.  Use the RPCs listed in the See Also subsection below instead."
}
[/block]
*Parameter #1---a Dash address*

Name | Type | Presence | Description
--- | --- | --- | ---
Address | string (base58) | Required<br>(exactly 1) | A P2PKH or P2SH Dash address belonging either to a specific account or the default account (\\")"

*Result---an account name*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string | Required<br>(exactly 1) | The name of an account, or an empty string (\\", the default account)"

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet getaccount yMTFRnrfJ4NpnYVeidDNHVwT7uuNsVjevq
```

Result:

``` text
doc test
```

*See also*

* [GetAddressesByAccount](/docs/core-api-ref-remote-procedure-calls-wallet#section-getaddressesbyaccount): returns a list of every address assigned to a particular account.

# GetAccountAddress

*Requires wallet support.*

The [`getaccountaddress` RPC](core-api-ref-remote-procedure-calls-wallet#section-getaccountaddress) returns the current Dash address for receiving payments to this account. If the account doesn't exist, it creates both the account and a new address for receiving payment.  Once a payment has been received to an address, future calls to this RPC for the same account will return a different address.
[block:callout]
{
  "type": "warning",
  "body": "**Warning:** `getaccountaddress` will be removed in a later version of Dash Core.  Use the RPCs listed in the See Also subsection below instead."
}
[/block]
*Parameter #1---an account name*

Name | Type | Presence | Description
--- | --- | --- | ---
Account | string | Required<br>(exactly 1) | The name of an account.  Use an empty string (\\") for the default account.  If the account doesn't exist, it will be created"

*Result---a Dash address*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string (base58) | Required<br>(exactly 1) | An address, belonging to the account specified, which has not yet received any payments

*Example from Dash Core 0.12.2*

Get an address for the default account:

``` bash
dash-cli -testnet getaccountaddress ""
```

Result:

``` text
yNUQ6RzTpNj5GP5ebdRcusJ7K9JJKx6VvV
```

*See also*

* [GetNewAddress](/docs/core-api-ref-remote-procedure-calls-wallet#section-getnewaddress): returns a new Dash address for receiving payments. If an account is specified, payments received with the address will be credited to that account.
* [GetRawChangeAddress](/docs/core-api-ref-remote-procedure-calls-wallet#section-getrawchangeaddress): returns a new Dash address for receiving change. This is for use with raw transactions, not normal use.
* [GetAddressesByAccount](/docs/core-api-ref-remote-procedure-calls-wallet#section-getaddressesbyaccount): returns a list of every address assigned to a particular account.

# GetAddressesByAccount

*Requires wallet support.*

The [`getaddressesbyaccount` RPC](core-api-ref-remote-procedure-calls-wallet#section-getaddressesbyaccount) returns a list of every address assigned to a particular account.

![Warning icon](https://dash-docs.github.io/img/icons/icon_warning.svg) **Warning:** `getaddressesbyaccount` will be removed in a later version of Dash
Core.  Use the RPCs listed in the See Also subsection below instead.

*Parameter #1---the account name*

Name | Type | Presence | Description
--- | --- | --- | ---
Account | string | Required<br>(exactly 1) | The name of the account containing the addresses to get.  To get addresses from the default account, pass an empty string (\\")"

*Result---a list of addresses*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | An array containing all addresses belonging to the specified account.  If the account has no addresses, the array will be empty
Address | string (base58) | Optional<br>(1 or more) | A P2PKH or P2SH address belonging to the account

*Example from Dash Core 0.12.2*

Get the addresses assigned to the account "doc test":

``` bash
dash-cli -testnet getaddressesbyaccount "doc test"
```

Result:

``` json
[
  "yMTFRnrfJ4NpnYVeidDNHVwT7uuNsVjevq",
  "yhT2HS1SxvXkMVdAdf6RNtGPfuVFvwZi35"
]
```

*See also*

* [GetAccount](/docs/core-api-ref-remote-procedure-calls-wallet#section-getaccount): returns the name of the account associated with the given address.
* [GetBalance](/docs/core-api-ref-remote-procedure-calls-wallet#section-getbalance): gets the balance in decimal dash across all accounts or for a particular account.

# GetBalance

*Requires wallet support.*

The [`getbalance` RPC](core-api-ref-remote-procedure-calls-wallet#section-getbalance) gets the balance in decimal dash across all accounts or for a particular account.

*Parameter #1---an account name*

Name | Type | Presence | Description
--- | --- | --- | ---
Account | string | Optional<br>(0 or 1) | *Deprecated: will be removed in a later version of Bitcoin Core*<br><br>The name of an account to get the balance for.  An empty string (\\") is the default account.  The string `*` will get the balance for all accounts (this is the default behavior)"

*Parameter #2---the minimum number of confirmations*

Name | Type | Presence | Description
--- | --- | --- | ---
Confirmations | number (int) | Optional<br>(0 or 1) | The minimum number of confirmations an externally-generated transaction must have before it is counted towards the balance.  Transactions generated by this node are counted immediately.  Typically, externally-generated transactions are payments to this wallet and transactions generated by this node are payments to other wallets.  Use `0` to count unconfirmed transactions.  Default is `1`

*Parameter #3---whether to add the balance from transactions locked via InstantSend*

Name | Type | Presence | Description
--- | --- | --- | ---
addlocked | bool | Optional<br>(exactly 1) | Add the balance from InstantSend locked transactions

*Parameter #4---whether to include watch-only addresses*

Name | Type | Presence | Description
--- | --- | --- | ---
Include Watch-Only | bool | Optional<br>(0 or 1) | If set to `true`, include watch-only addresses in details and calculations as if they were regular addresses belonging to the wallet.  If set to `false` (the default), treat watch-only addresses as if they didn't belong to this wallet

*Result---the balance in dash*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | number (dash) | Required<br>(exactly 1) | The balance of the account (or all accounts) in dash

*Examples from Dash Core 0.13.0*

Get the balance for the main ("") account, including transactions with
at least five confirmations and those spent to watch-only addresses in
that account. Do not include InstantSend locked transactions.

``` bash
dash-cli -testnet getbalance "" 3 false true
```

Result:

``` json
0.00000000
```

Get the balance for the main ("") account, including transactions with
at least one confirmation and those spent to watch-only addresses in
that account. Include the balance from InstantSend locked transactions.

``` bash
dash-cli -testnet getbalance "" 3 true true
```

Result:

``` json
1.00000000
```

*See also*

* [ListAccounts](/docs/core-api-ref-remote-procedure-calls-wallet#section-listaccounts): lists accounts and their balances.
* [GetReceivedByAccount](/docs/core-api-ref-remote-procedure-calls-wallet#section-getreceivedbyaccount): returns the total amount received by addresses in a particular account from transactions with the specified number of confirmations.  It does not count coinbase transactions.
* [GetReceivedByAddress](/docs/core-api-ref-remote-procedure-calls-wallet#section-getreceivedbyaddress): returns the total amount received by the specified address in transactions with the specified number of confirmations. It does not count coinbase transactions.

# GetNewAddress

*Requires wallet support.*

The [`getnewaddress` RPC](core-api-ref-remote-procedure-calls-wallet#section-getnewaddress) returns a new Dash address for receiving payments. If an account is specified, payments received with the address will be credited to that account.

*Parameter #1---an account name*

Name | Type | Presence | Description
--- | --- | --- | ---
Account | string | Optional<br>(0 or 1) | The name of the account to put the address in.  The default is the default account, an empty string (\\")"

*Result---a dash address never previously returned*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string (base58) | Required<br>(exactly 1) | A P2PKH address which has not previously been returned by this RPC.  The address will be marked as a receiving address in the wallet.  The address may already have been part of the keypool, so other RPCs such as the [`dumpwallet` RPC](core-api-ref-remote-procedure-calls-wallet#section-dumpwallet) may have disclosed it previously.  If the wallet is unlocked, its keypool will also be filled to its max (by default, 100 unused keys).  If the wallet is locked and its keypool is empty, this RPC will fail

*Example from Dash Core 0.12.2*

Create a new address in the "doc test" account:

``` bash
dash-cli -testnet getnewaddress "doc test"
```

Result:

``` text
yPuNTqCGzXtU3eEV5jHvhhJkzEPyJLmVkb
```

*See also*

* [GetAccountAddress](/docs/core-api-ref-remote-procedure-calls-wallet#section-getaccountaddress): returns the current Dash address for receiving payments to this account. If the account doesn't exist, it creates both the account and a new address for receiving payment.  Once a payment has been received to an address, future calls to this RPC for the same account will return a different address.
* [GetRawChangeAddress](/docs/core-api-ref-remote-procedure-calls-wallet#section-getrawchangeaddress): returns a new Dash address for receiving change. This is for use with raw transactions, not normal use.
* [GetBalance](/docs/core-api-ref-remote-procedure-calls-wallet#section-getbalance): gets the balance in decimal dash across all accounts or for a particular account.

# GetRawChangeAddress

*Requires wallet support.*

The [`getrawchangeaddress` RPC](core-api-ref-remote-procedure-calls-wallet#section-getrawchangeaddress) returns a new Dash address for receiving change. This is for use with raw transactions, not normal use.

*Parameters: none*

*Result---a P2PKH address which can be used in raw transactions*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string (base58) | Required<br>(exactly 1) | A P2PKH address which has not previously been returned by this RPC.  The address will be removed from the keypool but not marked as a receiving address, so RPCs such as the [`dumpwallet` RPC](core-api-ref-remote-procedure-calls-wallet#section-dumpwallet) will show it as a change address.  The address may already have been part of the keypool, so other RPCs such as the [`dumpwallet` RPC](core-api-ref-remote-procedure-calls-wallet#section-dumpwallet) may have disclosed it previously.  If the wallet is unlocked, its keypool will also be filled to its max (by default, 100 unused keys).  If the wallet is locked and its keypool is empty, this RPC will fail

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet getrawchangeaddress
```

Result:

``` text
yXBr9BiJmugTzHPgByDmvjJMAkvhTmXVJ8
```

*See also*

* [GetNewAddress](/docs/core-api-ref-remote-procedure-calls-wallet#section-getnewaddress): returns a new Dash address for receiving payments. If an account is specified, payments received with the address will be credited to that account.
* [GetAccountAddress](/docs/core-api-ref-remote-procedure-calls-wallet#section-getaccountaddress): returns the current Dash address for receiving payments to this account. If the account doesn't exist, it creates both the account and a new address for receiving payment.  Once a payment has been received to an address, future calls to this RPC for the same account will return a different address.

# GetReceivedByAccount

*Requires wallet support.*

The [`getreceivedbyaccount` RPC](core-api-ref-remote-procedure-calls-wallet#section-getreceivedbyaccount) returns the total amount received by addresses in a particular account from transactions with the specified number of confirmations.  It does not count coinbase transactions.

![Warning icon](https://dash-docs.github.io/img/icons/icon_warning.svg) **Warning:** `getreceivedbyaccount` will be removed in a later version of Dash
Core.  Use the RPCs listed in the See Also subsection below instead.

*Parameter #1---the account name*

Name | Type | Presence | Description
--- | --- | --- | ---
Account | string | Required<br>(exactly 1) | The name of the account containing the addresses to get.  For the default account, use an empty string (\\")"

*Parameter #2---the minimum number of confirmations*

Name | Type | Presence | Description
--- | --- | --- | ---
Confirmations | number (int) | Optional<br>(0 or 1) | The minimum number of confirmations an externally-generated transaction must have before it is counted towards the balance.  Transactions generated by this node are counted immediately.  Typically, externally-generated transactions are payments to this wallet and transactions generated by this node are payments to other wallets.  Use `0` to count unconfirmed transactions.  Default is `1`

*Parameter #3---whether to include transactions locked via InstantSend*

Name | Type | Presence | Description
--- | --- | --- | ---
addlocked | bool | Optional<br>(exactly 1) | Add the balance from InstantSend locked transactions (default=false)

*Result---the number of dash received*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | number (dash) | Required<br>(exactly 1) | The number of dash received by the account.  May be `0`

*Example from Dash Core 0.12.2*

Get the dash received by the "doc test" account with six or more
confirmations:

``` bash
dash-cli -testnet getreceivedbyaccount "doc test" 6
```

Result:

``` json
0.30000000
```

*See also*

* [GetReceivedByAddress](/docs/core-api-ref-remote-procedure-calls-wallet#section-getreceivedbyaddress): returns the total amount received by the specified address in transactions with the specified number of confirmations. It does not count coinbase transactions.
* [GetAddressesByAccount](/docs/core-api-ref-remote-procedure-calls-wallet#section-getaddressesbyaccount): returns a list of every address assigned to a particular account.
* [ListAccounts](/docs/core-api-ref-remote-procedure-calls-wallet#section-listaccounts): lists accounts and their balances.

# GetReceivedByAddress

*Requires wallet support.*

![Warning icon](https://dash-docs.github.io/img/icons/icon_warning.svg) Note: This RPC only returns a balance for addresses contained in the local wallet.

The [`getreceivedbyaddress` RPC](core-api-ref-remote-procedure-calls-wallet#section-getreceivedbyaddress) returns the total amount received by the specified address in transactions with the specified number of confirmations. It does not count coinbase transactions.

*Parameter #1---the address*

Name | Type | Presence | Description
--- | --- | --- | ---
Address | string | Required<br>(exactly 1) | __Only works for addresses contained in the local wallet__<br><br>The address whose transactions should be tallied

*Parameter #2---the minimum number of confirmations*

Name | Type | Presence | Description
--- | --- | --- | ---
Confirmations | number (int) | Optional<br>(0 or 1) | The minimum number of confirmations an externally-generated transaction must have before it is counted towards the balance.  Transactions generated by this node are counted immediately.  Typically, externally-generated transactions are payments to this wallet and transactions generated by this node are payments to other wallets.  Use `0` to count unconfirmed transactions.  Default is `1`

*Parameter #3---whether to include transactions locked via InstantSend*

Name | Type | Presence | Description
--- | --- | --- | ---
addlocked | bool | Optional<br>(exactly 1) | Add the balance from InstantSend locked transactions

*Result---the amount of dash received*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | number (dash) | Required<br>(exactly 1) | The amount of dash received by the address, excluding coinbase transactions.  May be `0`

*Example from Dash Core 0.13.0*

Get the dash received for a particular address, only counting
transactions with six or more confirmations (ignore InstantSend locked transactions):

``` bash
dash-cli -testnet getreceivedbyaddress yYoCWcjbykWsQJ7MVJrTMeQd8TZe5N4Q7g 6
```

Result:

``` json
0.00000000
```

Get the dash received for a particular address, only counting
transactions with six or more confirmations (include InstantSend locked transactions):

``` bash
dash-cli -testnet getreceivedbyaddress yYoCWcjbykWsQJ7MVJrTMeQd8TZe5N4Q7g 6 true
```

Result:

``` json
0.30000000
```

*See also*

* [GetReceivedByAccount](/docs/core-api-ref-remote-procedure-calls-wallet#section-getreceivedbyaccount): returns the total amount received by addresses in a particular account from transactions with the specified number of confirmations.  It does not count coinbase transactions.
* [GetAddressesByAccount](/docs/core-api-ref-remote-procedure-calls-wallet#section-getaddressesbyaccount): returns a list of every address assigned to a particular account.
* [ListAccounts](/docs/core-api-ref-remote-procedure-calls-wallet#section-listaccounts): lists accounts and their balances.

# GetTransaction

*Requires wallet support.*

The [`gettransaction` RPC](core-api-ref-remote-procedure-calls-wallet#section-gettransaction) gets detailed information about an in-wallet transaction.

*Parameter #1---a transaction identifier (TXID)*

Name | Type | Presence | Description
--- | --- | --- | ---
TXID | string (hex) | Required<br>(exactly 1) | The TXID of the transaction to get details about.  The TXID must be encoded as hex in RPC byte order

*Parameter #2---whether to include watch-only addresses in details and calculations*

Name | Type | Presence | Description
--- | --- | --- | ---
Include Watch-Only | bool | Optional<br>(0 or 1) | If set to `true`, include watch-only addresses in details and calculations as if they were regular addresses belonging to the wallet.  If set to `false` (the default), treat watch-only addresses as if they didn't belong to this wallet

*Result---a description of the transaction*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | An object describing how the transaction affects the wallet
→<br>`amount` | number (dash) | Required<br>(exactly 1) | A positive number of dash if this transaction increased the total wallet balance; a negative number of dash if this transaction decreased the total wallet balance, or `0` if the transaction had no net effect on wallet balance
→<br>`fee` | number (dash) | Optional<br>(0 or 1) | If an outgoing transaction, this is the fee paid by the transaction reported as negative dash
→ <br>`confirmations` | number (int) | Required<br>(exactly 1) | The number of confirmations the transaction has received.  Will be `0` for unconfirmed and `-1` for conflicted
→ <br>`instantlock` | bool | Required<br>(exactly 1) | Current transaction lock state (InstantSend and/or ChainLock)
→ <br>`instantlock_internal` | bool | Required<br>(exactly 1) | Current InstantSend transaction lock state
→ <br>`chainlock` | bool | Required<br>(exactly 1) | *Added in Dash Core 0.14.0*<br><br>If set to `true`, this transaction is in a block that is locked (not susceptible to a chain re-org)  
→ <br>`generated` | bool | Optional<br>(0 or 1) | Set to `true` if the transaction is a coinbase.  Not returned for regular transactions
→ <br>`blockhash` | string (hex) | Optional<br>(0 or 1) | The hash of the block on the local best block chain which includes this transaction, encoded as hex in RPC byte order.  Only returned for confirmed transactions
→ <br>`blockindex` | number (int) | Optional<br>(0 or 1) | The index of the transaction in the block that includes it.  Only returned for confirmed transactions
→ <br>`blocktime` | number (int) | Optional<br>(0 or 1) | The block header time (Unix epoch time) of the block on the local best block chain which includes this transaction.  Only returned for confirmed transactions
→ <br>`txid` | string (hex) | Required<br>(exactly 1) | The TXID of the transaction, encoded as hex in RPC byte order
→ <br>`walletconflicts` | array | Required<br>(exactly 1) | An array containing the TXIDs of other transactions that spend the same inputs (UTXOs) as this transaction.  Array may be empty
→ →<br>TXID | string (hex) | Optional<br>(0 or more) | The TXID of a conflicting transaction, encoded as hex in RPC byte order
→ <br>`time` | number (int) | Required<br>(exactly 1) | A Unix epoch time when the transaction was added to the wallet
→ <br>`timereceived` | number (int) | Required<br>(exactly 1) | A Unix epoch time when the transaction was detected by the local node, or the time of the block on the local best block chain that included the transaction
→ <br>`abandoned` | bool | Optional<br>(0 or 1) | `true` if the transaction has been abandoned (inputs are respendable). Only available for the 'send' category of transactions.
→ <br>`comment` | string | Optional<br>(0 or 1) | For transaction originating with this wallet, a locally-stored comment added to the transaction.  Only returned if a comment was added
→ <br>`to` | string | Optional<br>(0 or 1) | For transaction originating with this wallet, a locally-stored comment added to the transaction identifying who the transaction was sent to.  Only returned if a comment-to was added
→<br>`DS` | bool | Optional<br>(0 or 1) | Set to 1 if a PrivateSend transaction
→<br>`details` | array | Required<br>(exactly 1) | An array containing one object for each input or output in the transaction which affected the wallet
→ → <br>`involvesWatchonly` | bool | Optional<br>(0 or 1) | Set to `true` if the input or output involves a watch-only address.  Otherwise not returned
→ →<br>`account` | string | Required<br>(exactly 1) | The account which the payment was credited to or debited from.  May be an empty string (\\") for the default account"
→ →<br>`address` | string (base58) | Optional<br>(0 or 1) | If an output, the address paid (may be someone else's address not belonging to this wallet).  If an input, the address paid in the previous output.  May be empty if the address is unknown, such as when paying to a non-standard pubkey script
→ →<br>`category` | string | Required<br>(exactly 1) | Set to one of the following values:<br>• `send` if sending payment normally<br>• `privatesend` if sending PrivateSend payment<br>• `receive` if this wallet received payment in a regular transaction<br>• `generate` if a matured and spendable coinbase<br>• `immature` if a coinbase that is not spendable yet<br>• `orphan` if a coinbase from a block that's not in the local best block chain
→ →<br>`amount` | number (dash) | Required<br>(exactly 1) | A negative dash amount if sending payment; a positive dash amount if receiving payment (including coinbases)
→ →<br>`label` | string | Optional<br>(0 or 1) | An optional comment for the address/transaction
→ →<br>`vout` | number (int) | Required<br>(exactly 1) | For an output, the output index (vout) for this output in this transaction.  For an input, the output index for the output being spent in its transaction.  Because inputs list the output indexes from previous transactions, more than one entry in the details array may have the same output index
→ →<br>`fee` | number (dash) | Optional<br>(0 or 1) | If sending payment, the fee paid as a negative dash value.  May be `0`.  Not returned if receiving payment
→ →<br>`abandoned` | bool | Optional<br>(0 or 1) | *Added in Bitcoin Core 0.12.1*<br><br>Indicates if a transaction is was abandoned:<br>• `true` if it was abandoned (inputs are respendable)<br>• `false`  if it was not abandoned<br>Only returned by *send* category payments
→<br>`hex` | string (hex) | Required<br>(exactly 1) | The transaction in serialized transaction format

*Example from Dash Core 0.14.0*

``` bash
dash-cli -testnet gettransaction \
  c099c882745ad150e9b2a55ef5818683c7ef597e1e5fc20856c67eabc3778ccc
```

Result:
``` json
{
  "amount": -50.00000000,
  "fee": -0.00030000,
  "confirmations": 3064,
  "instantlock": false,
  "instantlock_internal": false,
  "chainlock": false,
  "blockhash": "00000a01007be2912c3123085534b58d341cb5e5980b967e8dcc021089487a1e",
  "blockindex": 1,
  "blocktime": 1553290594,
  "trusted": true,
  "txid": "c099c882745ad150e9b2a55ef5818683c7ef597e1e5fc20856c67eabc3778ccc",
  "walletconflicts": [
  ],
  "time": 1553290584,
  "timereceived": 1553290584,
  "details": [
    {
      "account": "",
      "address": "ycCsAUKsjdmoP4qAXiS1cjYA4ixM48zJWe",
      "category": "send",
      "amount": -50.00000000,
      "label": "Electrum",
      "vout": 1,
      "fee": -0.00030000,
      "abandoned": false
    }
  ],
  "hex": "0200000003aac865dba0e98fe32533df6bc3eaac160d04bb02966584fb61fc8d7788e09537010000006a47304402202d537257f23ab42b3e14f2ab533f39bb4586aa1b29a1f833f718a59493c8a601022019c6c156c20e66ef256519592b3c977b64d417c94aea4dca20cf18522a138993012103c67d86944315838aea7ec80d390b5d09b91b62483370d4979da5ccf7a7df77a9feffffff47833a270d2e2bac47bc5dc0df576c3a68b01bedbc89692060ac4113a6f9cb67010000006a4730440220442c19a913b10edc533bf63310f5294d6d91eec0eb9c510a3c6b0f33333f27320220501d5093ecdf603b8af9734e21d5de4710c8500309bfa4acdda243a294442b2c012103c67d86944315838aea7ec80d390b5d09b91b62483370d4979da5ccf7a7df77a9feffffffdcfd2d0fb30d79ffeadab8832e65be2310b67043ff3d74deac9a9cb825acda67000000006b483045022100cae8c025d3bec82903f356a5ec38d78a141447b6562e3aceac901f5fcc6f8567022076407835937514d6690c81c0c3b97f92d2b0ae9749249affaf539ead825692f4012102d6be44ab930ff67f084fbaf47a38b539b8d5da65c010952a972c9e524b6009dffeffffff0204fe2b00000000001976a914e3b0093477c2f629430d0a7b5813fe8b0153b0fd88ac00f2052a010000001976a914ae4365dedb1836ba215b9149602e0787a23376d288ac42010100"
}
```

*See also*

* [GetRawTransaction](/docs/core-api-ref-remote-procedure-calls-raw-transaction#section-getrawtransaction): gets a hex-encoded serialized transaction or a JSON object describing the transaction. By default, Dash Core only stores complete transaction data for UTXOs and your own transactions, so the RPC may fail on historic transactions unless you use the non-default `txindex=1` in your Dash Core startup settings.

# GetUnconfirmedBalance

*Requires wallet support.*

The [`getunconfirmedbalance` RPC](core-api-ref-remote-procedure-calls-wallet#section-getunconfirmedbalance) returns the wallet's total unconfirmed balance.

*Parameters: none*

*Result---the balance of unconfirmed transactions paying this wallet*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | number (dash) | Required<br>(exactly 1) | The total number of dash paid to this wallet in unconfirmed transactions

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet getunconfirmedbalance
```

Result (no unconfirmed incoming payments):

``` json
0.00000000
```

*See also*

* [GetBalance](/docs/core-api-ref-remote-procedure-calls-wallet#section-getbalance): gets the balance in decimal dash across all accounts or for a particular account.

# GetWalletInfo

*Requires wallet support.*

The [`getwalletinfo` RPC](core-api-ref-remote-procedure-calls-wallet#section-getwalletinfo) provides information about the wallet.

*Parameters: none*

*Result---information about the wallet*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | An object describing the wallet
→<br>`walletversion` | number (int) | Required<br>(exactly 1) | The version number of the wallet
→<br>`balance` | number (dash) | Required<br>(exactly 1) | The total confirmed balance of the wallet.  The same as returned by the [`getbalance` RPC](core-api-ref-remote-procedure-calls-wallet#section-getbalance) with default parameters
→<br>`privatesendbalance` | number (dash) | Required<br>(exactly 1) | *Added in Dash Core 0.12.3*<br><br>The total PrivateSend balance of the wallet
→<br>`unconfirmed_balance` | number (dash) | Required<br>(exactly 1) | The total unconfirmed balance of the wallet.  The same as returned by the [`getunconfirmedbalance` RPC](core-api-ref-remote-procedure-calls-wallet#section-getunconfirmedbalance) with default parameters
→<br>`immature_balance` | number (dash) | Required<br>(exactly 1) | The total immature balance of the wallet.  This includes mining/masternode rewards that cannot be spent yet
→<br>`txcount` | number (int) | Required<br>(exactly 1) | The total number of transactions in the wallet (both spends and receives)
→<br>`keypoololdest` | number (int) | Required<br>(exactly 1) | The date as Unix epoch time when the oldest key in the wallet key pool was created; useful for only scanning blocks created since this date for transactions
→<br>`keypoolsize` | number (int) | Required<br>(exactly 1) | The number of keys in the wallet keypool
→<br>`keypoolsize_hd_internal` | number (int) | Optional<br>(0 or 1) | How many new keys are pre-generated for internal use (used for change outputs, only appears if the wallet is using this feature, otherwise external keys are used)
→<br>`keys_left` | number (int) | Required<br>(exactly 1) | The number of unused keys left since the last automatic backup
→<br>`unlocked_until` | number (int) | Optional<br>(0 or 1) | Only returned if the wallet was encrypted with the [`encryptwallet` RPC](core-api-ref-remote-procedure-calls-wallet#section-encryptwallet). A Unix epoch date when the wallet will be locked, or `0` if the wallet is currently locked
→<br>`paytxfee` | number (int) | Required<br>(exactly 1) | The transaction fee configuration, set in DASH/kB
→<br>`hdchainid` | string (hash) | Optional<br>(0 or 1) | The ID of the HD chain
→<br>`hdaccountcount` | number (int) | Optional<br>(0 or 1) | How many accounts of the HD chain are in this wallet
→ →<br>`hdaccountcountindex` | number (int) | Optional<br>(0 or 1) | The index of the account
→ →<br>`hdexternalkeyindex` | number (int) | Optional<br>(0 or 1) | Current external child key index
→ →<br>`hdinternalkeyindex` | number (int) | Optional<br>(0 or 1) | Current internal child key index

*Example from Dash Core 0.12.3*

``` bash
dash-cli -testnet getwalletinfo
```

Result:

``` json
{
  "walletversion": 61000,
  "balance": 3000.00000000,
  "privatesend_balance": 413.20413200,  
  "unconfirmed_balance": 10.10000000,
  "immature_balance": 11.25000000,
  "txcount": 267,
  "keypoololdest": 1508428379,
  "keypoolsize": 999,
  "keys_left": 978,
  "unlocked_until": 0,
  "paytxfee": 0.00000000
}
```

*See also*

* [ListTransactions](/docs/core-api-ref-remote-procedure-calls-wallet#section-listtransactions): returns the most recent transactions that affect the wallet.

# ImportAddress

*Requires wallet support.*

The [`importaddress` RPC](core-api-ref-remote-procedure-calls-wallet#section-importaddress) adds an address or pubkey script to the wallet without the associated private key, allowing you to watch for transactions affecting that address or pubkey script without being able to spend any of its outputs.

*Parameter #1---the address or pubkey script to watch*

Name | Type | Presence | Description
--- | --- | --- | ---
Address or Script | string (base58 or hex) | Required<br>(exactly 1) | Either a P2PKH or P2SH address encoded in base58check, or a pubkey script encoded as hex

*Parameter #2---The account into which to place the address or pubkey script*

Name | Type | Presence | Description
--- | --- | --- | ---
Label | string | Optional<br>(0 or 1) | An optional label.  Default is an empty string(\\")"

*Parameter #3---whether to rescan the block chain*

Name | Type | Presence | Description
--- | --- | --- | ---
Rescan | bool | Optional<br>(0 or 1) | Set to `true` (the default) to rescan the entire local block database for transactions affecting any address or pubkey script in the wallet (including transaction affecting the newly-added address or pubkey script).  Set to `false` to not rescan the block database (rescanning can be performed at any time by restarting Dash Core with the `-rescan` command-line argument).  Rescanning may take several minutes.

*Parameter #4---whether to rescan the block chain*

Name | Type | Presence | Description
--- | --- | --- | ---
P2SH | bool | Optional<br>(0 or 1) | Add the P2SH version of the script as well

*Result---`null` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | If the address or pubkey script is added to the wallet (or is already part of the wallet), JSON `null` will be returned

*Example from Dash Core 0.12.2*

Add an address, rescanning the local block database for any transactions
matching it.

``` bash
dash-cli -testnet importaddress \
  yg89Yt5Tjzs9nRpX3wJCuvr7KuQvgkvmeC "watch-only test" true
```

Result:

(No output; success.)

Show that the address has been added:

``` bash
dash-cli -testnet getaccount yg89Yt5Tjzs9nRpX3wJCuvr7KuQvgkvmeC
```

Result:

``` text
watch-only test
```

*See also*

* [ImportPrivKey](/docs/core-api-ref-remote-procedure-calls-wallet#section-importprivkey): adds a private key to your wallet. The key should be formatted in the wallet import format created by the [`dumpprivkey` RPC](core-api-ref-remote-procedure-calls-wallet#section-dumpprivkey).
* [ListReceivedByAddress](/docs/core-api-ref-remote-procedure-calls-wallet#section-listreceivedbyaddress): lists the total number of dash received by each address.

# ImportElectrumWallet

The [`importelectrumwallet` RPC](core-api-ref-remote-procedure-calls-wallet#section-importelectrumwallet) imports keys from an Electrum wallet export file (.csv or .json)

*Parameter #1---file name*

Name | Type | Presence | Description
--- | --- | --- | ---
File Name | string | Required<br>(exactly 1) | The Electrum wallet export file (should be in csv or json format)

*Parameter #2---index*

Name | Type | Presence | Description
--- | --- | --- | ---
Index | number (int) | Optional<br>(0 or 1) | Rescan the wallet for transactions starting from this block index (default: 0)

*Result---`null` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | If the Electrum keys are imported successfully, JSON `null` will be returned

*Example from Dash Core 0.12.2*

``` bash
dash-cli importelectrumwallet /tmp/electrum-export.csv
```

(Success: no result displayed.)

*See also: none*

# ImportMulti

*Added in Dash Core 0.12.3 / Bitcoin Core 0.14.0*

*Requires wallet support.  Wallet must be unlocked.*

The [`importmulti` RPC](core-api-ref-remote-procedure-calls-wallet#section-importmulti) imports addresses or scripts (with private keys, public keys, or P2SH redeem scripts) and optionally performs the minimum necessary rescan for all imports.

*Parameter #1---the addresses/scripts to import*

Name | Type | Presence | Description
--- | --- | --- | ---
Imports | array | Required<br>(exactly 1) | An array of JSON objects, each one being an address or script to be imported
→ Import | object | Required<br>(1 or more) | A JSON object describing a particular import
→ →<br>`scriptPubKey` | string (hex) | Optional<br>(0 or 1) | The script (string) to be imported.  Must have either this field or `address` below
→ →<br>`address` | string (base58) | Optional<br>(0 or 1) | The P2PKH or P2SH address to be imported.  Must have either this field or `scriptPubKey` above
→ →<br>`timestamp` | number (int) / string | Required<br>(exactly 1) | The creation time of the key in Unix epoch time or the string “now” to substitute the current synced block chain time. The timestamp of the oldest key will determine how far back block chain rescans need to begin. Specify `now` to bypass scanning for keys which are known to never have been used.  Specify `0` to scan the entire block chain. Blocks up to 2 hours before the earliest key creation time will be scanned
→ →<br>`redeemscript` | string | Optional<br>(0 or 1) | A redeem script. Only allowed if either the `address` field is a P2SH address or the `scriptPubKey` field is a P2SH scriptPubKey
→ →<br>`pubkeys` | array | Optional<br>(0 or 1) | Array of strings giving pubkeys that must occur in the scriptPubKey or redeemscript
→ →<br>`keys` | array | Optional<br>(0 or 1) | Array of strings giving private keys whose corresponding public keys must occur in the scriptPubKey or redeemscript
→ →<br>`internal` | bool | Optional<br>(0 or 1) | Stating whether matching outputs should be treated as change rather than incoming payments. The default is `false`
→ →<br>`watchonly` | bool | Optional<br>(0 or 1) | Stating whether matching outputs should be considered watched even when they're not spendable. This is only allowed if keys are empty. The default is `false`
→ →<br>`label` | string | Optional<br>(0 or 1) | Label to assign to the address, only allowed with `internal` set to `false`. The default is an empty string (“”)  

*Parameter #2---options regarding the import*

Name | Type | Presence | Description
--- | --- | --- | ---
Option | object | Optional<br>(0 or 1) | JSON object with options regarding the import
→ <br>`rescan` | bool | Optional<br>(0 or 1) | Set to `true` (the default) to rescan the entire local block chain for transactions affecting any imported address or script. Set to `false` to not rescan after the import. Rescanning may take a considerable amount of time and may require re-downloading blocks if using block chain pruning

*Result---execution result*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | An array of JSON objects, with each object describing the execution result of each import
→ Result | object | Required<br>(1 or more) | A JSON object describing the execution result of an imported address or script
→ → <br>`success` | string | Required<br>(exactly 1) | Displays `true` if the import has been successful or `false` if it failed
→ → <br>`error` | string : object | Optional<br>(0 or 1) | A JSON object containing details about the error. Only displayed if the import fails
→ → → <br>`code` | number (int) | Optional<br>(0 or 1) | The error code  
→ → → <br>`message` | string | Optional<br>(0 or 1) | The error message    

*Example from Dash Core 0.12.3*

Import the address `ycCsAUKsjdmoP4qAXiS1cjYA4ixM48zJWe` (giving it a label and scanning the entire block chain) and the scriptPubKey `76a9146cf5870411edc31ba5630d61c7cddff55b884fda88ac` (giving a specific timestamp and label):

``` bash
dash-cli importmulti '
  [
    {
      "scriptPubKey" : { "address": "ycCsAUKsjdmoP4qAXiS1cjYA4ixM48zJWe" },
      "timestamp" : 0,
      "label" : "Personal"
    },
    {
      "scriptPubKey" : "76a9146cf5870411edc31ba5630d61c7cddff55b884fda88ac",
      "timestamp" : 1493912405,
      "label" : "TestFailure"
    }
  ]' '{ "rescan": true }'
```

Result (scriptPubKey import failed because `internal` was not set to `true`):

``` json
[
  {
    "success": true
  },
  {
    "success": false,
    "error": {
      "code": -8,
      "message": "Internal must be set for hex scriptPubKey"
    }
  }
]
```

*See also*

* [ImportPrivKey](/docs/core-api-ref-remote-procedure-calls-wallet#section-importprivkey): adds a private key to your wallet. The key should be formatted in the wallet import format created by the [`dumpprivkey` RPC](core-api-ref-remote-procedure-calls-wallet#section-dumpprivkey).
* [ImportAddress](/docs/core-api-ref-remote-procedure-calls-wallet#section-importaddress): adds an address or pubkey script to the wallet without the associated private key, allowing you to watch for transactions affecting that address or pubkey script without being able to spend any of its outputs.
* [ImportWallet](/docs/core-api-ref-remote-procedure-calls-wallet#section-importwallet): imports private keys from a file in wallet dump file format (see the [`dumpwallet` RPC](core-api-ref-remote-procedure-calls-wallet#section-dumpwallet)). These keys will be added to the keys currently in the wallet.  This call may need to rescan all or parts of the block chain for transactions affecting the newly-added keys, which may take several minutes.

# ImportPrivKey

*Requires wallet support.  Wallet must be unlocked.*

The [`importprivkey` RPC](core-api-ref-remote-procedure-calls-wallet#section-importprivkey) adds a private key to your wallet. The key should be formatted in the wallet import format created by the [`dumpprivkey` RPC](core-api-ref-remote-procedure-calls-wallet#section-dumpprivkey).

*Parameter #1---the private key to import*

Name | Type | Presence | Description
--- | --- | --- | ---
Private Key | string (base58) | Required<br>(exactly 1) | The private key to import into the wallet encoded in base58check using wallet import format (WIF)

*Parameter #2---the account into which the key should be placed*

Name | Type | Presence | Description
--- | --- | --- | ---
Account | string | Optional<br>(0 or 1) | The name of an account to which transactions involving the key should be assigned.  The default is the default account, an empty string (\\")"

*Parameter #3---whether to rescan the block chain*

Name | Type | Presence | Description
--- | --- | --- | ---
Rescan | bool | Optional<br>(0 or 1) | Set to `true` (the default) to rescan the entire local block database for transactions affecting any address or pubkey script in the wallet (including transaction affecting the newly-added address for this private key).  Set to `false` to not rescan the block database (rescanning can be performed at any time by restarting Dash Core with the `-rescan` command-line argument).  Rescanning may take several minutes.  Notes: if the address for this key is already in the wallet, the block database will not be rescanned even if this parameter is set

*Result---`null` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | If the private key is added to the wallet (or is already part of the wallet), JSON `null` will be returned

*Example from Dash Core 0.12.2*

Import the private key for the address
ycBuREgSskHHkWLxDa9A5WppCki6PfFycL, giving it a label and scanning the
entire block chain:

``` bash
dash-cli -testnet importprivkey \
              cQZZ4awQvcXXyES3CmUJqSgeTobQm9t9nyUr337kvUtsWsnvvMyw \
              "test label" \
              true
```

(Success: no result displayed.)

*See also*

* [DumpPrivKey](/docs/core-api-ref-remote-procedure-calls-wallet#section-dumpprivkey): returns the wallet-import-format (WIP) private key corresponding to an address. (But does not remove it from the wallet.)
* [ImportAddress](/docs/core-api-ref-remote-procedure-calls-wallet#section-importaddress): adds an address or pubkey script to the wallet without the associated private key, allowing you to watch for transactions affecting that address or pubkey script without being able to spend any of its outputs.
* [ImportPubKey](/docs/core-api-ref-remote-procedure-calls-wallet#section-importpubkey): imports a public key (in hex) that can be watched as if it were in your wallet but cannot be used to spend
* [ImportWallet](/docs/core-api-ref-remote-procedure-calls-wallet#section-importwallet): imports private keys from a file in wallet dump file format (see the [`dumpwallet` RPC](core-api-ref-remote-procedure-calls-wallet#section-dumpwallet)). These keys will be added to the keys currently in the wallet.  This call may need to rescan all or parts of the block chain for transactions affecting the newly-added keys, which may take several minutes.

# ImportPrunedFunds

*Added in Dash Core 0.12.3 / Bitcoin Core 0.13.0*

*Requires wallet support.*

The [`importprunedfunds` RPC](core-api-ref-remote-procedure-calls-wallet#section-importprunedfunds) imports funds without the need of a rescan. Meant for use with pruned wallets. Corresponding address or script must previously be included in wallet.
The end-user is responsible to import additional transactions that subsequently spend the imported
outputs or rescan after the point in the blockchain the transaction is included.

*Parameter #1---the raw transaction to import*

Name | Type | Presence | Description
--- | --- | --- | ---
Raw Transaction | string<br>(hex) | Required<br>(exactly 1) | A raw transaction in hex funding an already-existing address in wallet

*Parameter #2---the tx out proof that cointains the transaction*

Name | Type | Presence | Description
--- | --- | --- | ---
TX Out Proof | string<br>(hex) | Required<br>(exactly 1) | The hex output from gettxoutproof that contains the transaction

*Result---`null` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | If the funds are added to wallet, JSON `null` will be returned

*Example from Dash Core 0.12.3*

``` bash
bitcoin-cli importprunedfunds "txhex" "txoutproof"
```

(Success: no result displayed.)

*See also*

* [ImportPrivKey](/docs/core-api-ref-remote-procedure-calls-wallet#section-importprivkey): adds a private key to your wallet. The key should be formatted in the wallet import format created by the [`dumpprivkey` RPC](core-api-ref-remote-procedure-calls-wallet#section-dumpprivkey).
* [RemovePrunedFunds](/docs/core-api-ref-remote-procedure-calls-wallet#section-removeprunedfunds): deletes the specified transaction from the wallet. Meant for use with pruned wallets and as a companion to importprunedfunds.

# ImportPubKey

The [`importpubkey` RPC](core-api-ref-remote-procedure-calls-wallet#section-importpubkey) imports a public key (in hex) that can be watched as if it were in your wallet but cannot be used to spend

*Parameter #1---the public key to import*

Name | Type | Presence | Description
--- | --- | --- | ---
Private Key | string (hex) | Required<br>(exactly 1) | The public key to import

*Parameter #2---the account into which the key should be placed*

Name | Type | Presence | Description
--- | --- | --- | ---
Label | string | Optional<br>(0 or 1) | The label the key should be assigned

*Parameter #3---whether to rescan the block chain*

Name | Type | Presence | Description
--- | --- | --- | ---
Rescan | bool | Optional<br>(0 or 1) | Set to `true` (the default) to rescan the entire local block database for transactions affecting any address or pubkey script in the wallet.  Set to `false` to not rescan the block database (rescanning can be performed at any time by restarting Dash Core with the `-rescan` command-line argument).  Rescanning may take several minutes.  Notes: if the address for this key is already in the wallet, the block database will not be rescanned even if this parameter is set

*Result---`null` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | If the public key is added to the wallet (or is already part of the wallet), JSON `null` will be returned

*Example from Dash Core 0.12.2*

Import the public key for the address, giving it a label and scanning the
entire block chain:

``` bash
dash-cli -testnet importpubkey \
    0210c1349657c1253d3d64d1b31d3500b09335bf12b8df061666e216f550a43249 \
    "test label" \
    true
```

(Success: no result displayed.)

*See also:*

* [ImportAddress](/docs/core-api-ref-remote-procedure-calls-wallet#section-importaddress): adds an address or pubkey script to the wallet without the associated private key, allowing you to watch for transactions affecting that address or pubkey script without being able to spend any of its outputs.
* [ImportPrivKey](/docs/core-api-ref-remote-procedure-calls-wallet#section-importprivkey): adds a private key to your wallet. The key should be formatted in the wallet import format created by the [`dumpprivkey` RPC](core-api-ref-remote-procedure-calls-wallet#section-dumpprivkey).
* [ImportWallet](/docs/core-api-ref-remote-procedure-calls-wallet#section-importwallet): imports private keys from a file in wallet dump file format (see the [`dumpwallet` RPC](core-api-ref-remote-procedure-calls-wallet#section-dumpwallet)). These keys will be added to the keys currently in the wallet.  This call may need to rescan all or parts of the block chain for transactions affecting the newly-added keys, which may take several minutes.

# ImportWallet

*Requires wallet support. Requires an unlocked wallet or an
unencrypted wallet.*

The [`importwallet` RPC](core-api-ref-remote-procedure-calls-wallet#section-importwallet) imports private keys from a file in wallet dump file format (see the [`dumpwallet` RPC](core-api-ref-remote-procedure-calls-wallet#section-dumpwallet)). These keys will be added to the keys currently in the wallet.  This call may need to rescan all or parts of the block chain for transactions affecting the newly-added keys, which may take several minutes.

*Parameter #1---the file to import*

Name | Type | Presence | Description
--- | --- | --- | ---
Filename | string | Required<br>(exactly 1) | The file to import.  The path is relative to Dash Core's working directory

*Result---`null` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | If all the keys in the file are added to the wallet (or are already part of the wallet), JSON `null` will be returned

*Example from Dash Core 0.12.2*

Import the file shown in the example subsection of the [`dumpwallet` RPC](core-api-ref-remote-procedure-calls-wallet#section-dumpwallet).

``` bash
dash-cli -testnet importwallet /tmp/dump.txt
```

(Success: no result displayed.)

*See also*

* [DumpWallet](/docs/core-api-ref-remote-procedure-calls-wallet#section-dumpwallet): creates or overwrites a file with all wallet keys in a human-readable format.
* [ImportPrivKey](/docs/core-api-ref-remote-procedure-calls-wallet#section-importprivkey): adds a private key to your wallet. The key should be formatted in the wallet import format created by the [`dumpprivkey` RPC](core-api-ref-remote-procedure-calls-wallet#section-dumpprivkey).

# KeePass

The [`keepass` RPC](core-api-ref-remote-procedure-calls-wallet#section-keepass) provides commands for configuring and managing KeePass authentication

*Parameter #1---Command mode*

Name | Type | Presence | Description
--- | --- | --- | ---
`mode` | string | Required (exactly 1) | The command mode to use:<br>`genkey`,<br>`init`,<br>`setpassphrase`

*Command Options*

Mode | Description
--- | --- | --- |
`genkey` | Generates a base64 encoded 256 bit AES key that can be used for the communication with KeePassHttp. This is only necessary for manual configuration.
`init` | Sets up the association between Dash Core and KeePass by generating an AES key and sending an association message to KeePassHttp. This will trigger KeePass to ask for an Id for the association. Returns the association and the base64 encoded string for the AES key.
`setpassphrase` | Updates the passphrase in KeePassHttp to a new value. This should match the passphrase you intend to use for the wallet. Please note that the standard RPC commands walletpassphrasechange and the wallet encryption from the QT GUI already send the updates to KeePassHttp, so this is only necessary for manual manipulation of the password.

**Command Mode - `genkey`**

*Result---the new key*

Name | Type | Presence | Description
--- | --- | --- | ---
Result | string (base64) | Required (exactly 1) | The new key

*Example from Dash Core 0.12.2*

Manually generate a key

``` bash
dash-cli -testnet keepass genkey
```

Result:
``` bash
Generated Key: dNjo+J8Jb30txbJiKq4s9H6vEgWq/whb1w9bb2cTOFo=
```  

**Command Mode - `init`**

*Result---initialization response*

Name | Type | Presence | Description
--- | --- | --- | ---
Result | string | Required (exactly 1) | The success/error status

*Example from Dash Core 0.12.2*

Automatically initialize

``` bash
dash-cli -testnet keepass init
```

Result (wrapped):
``` bash
Association successful. Id: testwalletassociation - \
Key: MSb+JLygqz7ZH40SyJ1QR62i00IXoa3tmT85MGGI2K0=
```  

**Command Mode - `setpassphrase`**

*Parameter #2---Passphrase*

Name | Type | Presence | Description
--- | --- | --- | ---
Passphrase | string | Required (exactly 1) | The passphrase to set

*Result---status*

Name | Type | Presence | Description
--- | --- | --- | ---
Result | string | Required (exactly 1) | The success/error status

*Example from Dash Core 0.12.2*

Set KeePass passphrase

``` bash
dash-cli -testnet keepass setpassphrase 1BWi20Xyk76uWumxJQy4
```

Result:
``` bash
setlogin: Updated credentials.
```  

*See also: none*

# KeyPoolRefill

*Requires wallet support.  Requires an unlocked wallet or an unencrypted
wallet.*

The [`keypoolrefill` RPC](core-api-ref-remote-procedure-calls-wallet#section-keypoolrefill) fills the cache of unused pre-generated keys (the keypool).

*Parameter #1---the new keypool size*

Name | Type | Presence | Description
--- | --- | --- | ---
Key Pool Size | number (int) | Optional<br>(0 or 1) | The new size of the keypool; if the number of keys in the keypool is less than this number, new keys will be generated.  Default is `1000`.  The value `0` also equals the default.  The value specified is for this call only---the default keypool size is not changed

*Result---`null` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | If the keypool is successfully filled, JSON `null` will be returned

*Example from Dash Core 0.12.2*

Generate one extra key than the default:

``` bash
dash-cli -testnet keypoolrefill 1001
```

(No result shown: success.)

*See also*

* [GetNewAddress](/docs/core-api-ref-remote-procedure-calls-wallet#section-getnewaddress): returns a new Dash address for receiving payments. If an account is specified, payments received with the address will be credited to that account.
* [GetAccountAddress](/docs/core-api-ref-remote-procedure-calls-wallet#section-getaccountaddress): returns the current Dash address for receiving payments to this account. If the account doesn't exist, it creates both the account and a new address for receiving payment.  Once a payment has been received to an address, future calls to this RPC for the same account will return a different address.
* [GetWalletInfo](/docs/core-api-ref-remote-procedure-calls-wallet#section-getwalletinfo): provides information about the wallet.

# ListAccounts

*Requires wallet support.*

The [`listaccounts` RPC](core-api-ref-remote-procedure-calls-wallet#section-listaccounts) lists accounts and their balances.

![Warning icon](https://dash-docs.github.io/img/icons/icon_warning.svg) **Warning:** `listaccounts` will be removed in a later version of Dash
Core.  Use the RPCs listed in the See Also subsection below instead.

*Parameter #1---the minimum number of confirmations a transaction must have*

Name | Type | Presence | Description
--- | --- | --- | ---
Confirmations | number (int) | Optional<br>(0 or 1) | The minimum number of confirmations an externally-generated transaction must have before it is counted towards the balance.  Transactions generated by this node are counted immediately.  Typically, externally-generated transactions are payments to this wallet and transactions generated by this node are payments to other wallets.  Use `0` to count unconfirmed transactions.  Default is `1`

*Parameter #2--- whether to include transactions locked via InstantSend*

Name | Type | Presence | Description
--- | --- | --- | ---
addlocked | bool | Optional<br>(exactly 1) | Add the balance from InstantSend locked transactions

*Parameter #3---whether to include watch-only addresses in results*

Name | Type | Presence | Description
--- | --- | --- | ---
Include Watch-Only | bool | Optional<br>(0 or 1) | If set to `true`, include watch-only addresses in details and calculations as if they were regular addresses belonging to the wallet.  If set to `false` (the default), treat watch-only addresses as if they didn't belong to this wallet

*Result---a list of accounts and their balances*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | A JSON array containing key/value pairs with account names and values.  Must include, at the very least, the default account (\\")"
→<br>Account : Balance | string : number (dash) | Required<br>(1 or more) | The name of an account as a string paired with the balance of the account as a number of dash.  The number of dash may be negative if the account has spent more dash than it received.  Accounts with zero balances and zero transactions will be displayed

*Example from Dash Core 0.13.0*

Display account balances with one confirmation and watch-only addresses
included. Add the balance of InstantSend locked transactions also.

``` bash
dash-cli -testnet listaccounts 1 true true
```

Result:

``` json
{
  "": -2941.30029732,
  "Watching": 8.50000000,
  "MN": 2000.25442744,
  "PS": 37.02970000,
  "Recv1": 3843.48167912,
}
```

*See also*

* [GetAccount](/docs/core-api-ref-remote-procedure-calls-wallet#section-getaccount): returns the name of the account associated with the given address.
* [GetAddressesByAccount](/docs/core-api-ref-remote-procedure-calls-wallet#section-getaddressesbyaccount): returns a list of every address assigned to a particular account.
* [ListReceivedByAccount](/docs/core-api-ref-remote-procedure-calls-wallet#section-listreceivedbyaccount): lists the total number of dash received by each account.

# ListAddressBalances

The [`listaddressbalances` RPC](core-api-ref-remote-procedure-calls-wallet#section-listaddressbalances) lists addresses of this wallet and their balances

*Parameter #1---Minimum Amount*

Name | Type | Presence | Description
--- | --- | --- | ---
Minimum Amount | numeric (int) | Optional<br>(0 or 1) | Minimum balance in DASH an address should have to be shown in the list (default=0)

*Result---an object containing the addresses and their balances*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | An object containing key/value pairs corresponding to the addresses with balances > the minimum amount.  May be empty
→<br>Address/Amount | string (base58):<br>number (DASH) | Optional<br>(1 or more) | A key/value pair with a base58check-encoded string containing the address as the key, and an amount of DASH as the value

*Example from Dash Core 0.12.3*

``` bash
dash-cli -testnet listaddressbalances 25
```

Result:
``` json
{
  "yMQtQkcMBXrAZyqTGZeg7tQHzmbypGEP4w": 299.99990000,
  "yRyfgrHm4f5A8GGvqpqTFvbCrCQHJm1L4V": 99.13570000,
  "ybePwhPzUbiWzFhkgxPgP6iHnTLTyFH6sU": 123.45600000,
  "ycCdPQnjNEVRgrQY8j6mxEx9h7oaQpo5Ge": 500.00000000
}
```

*See also:*

* [ListAddressGroupings](/docs/core-api-ref-remote-procedure-calls-wallet#section-listaddressgroupings): lists groups of addresses that may have had their common ownership made public by common use as inputs in the same transaction or from being used as change from a previous transaction.

# ListAddressGroupings

*Requires wallet support.*

The [`listaddressgroupings` RPC](core-api-ref-remote-procedure-calls-wallet#section-listaddressgroupings) lists groups of addresses that may have had their common ownership made public by common use as inputs in the same transaction or from being used as change from a previous transaction.

*Parameters: none*

*Result---an array of arrays describing the groupings*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | An array containing the groupings.  May be empty
→<br>Groupings | array | Optional<br>(0 or more) | An array containing arrays of addresses which can be associated with each other
→ →<br>Address Details | array | Required<br>(1 or more) | An array containing information about a particular address
→ → →<br>Address | string (base58) | Required<br>(exactly 1) | The address in base58check format
→ → →<br>Balance | number (bitcoins) | Required<br>(exactly 1) | The current spendable balance of the address, not counting unconfirmed transactions
→ → →<br>Account | string | Optional<br>(0 or 1) | *Deprecated: will be removed in a later version of Dash Core*<br><br>The account the address belongs to, if any.  This field will not be returned for change addresses.  The default account is an empty string (\\")"

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet listaddressgroupings
```

Result (edited to only three results):

``` json
[
  [
    [
      "yNpezfFDfoikDuT1f4iK75AiLp2YLPsGAb",
      0.00000000
    ]
  ],
  [
    [
      "yX7SvurfpwSD7QDA3pZNYNxt6kPPiZmRAk",
      27.02970000,
      "Test1"
    ]
  ],
  [
    [
      "ygMuVDN2raRBma86GpwyQeJV18kR1261d1",
      11.00000000,
      "Test2"
    ]
  ]
]
```

*See also*

* [GetAddressesByAccount](/docs/core-api-ref-remote-procedure-calls-wallet#section-getaddressesbyaccount): returns a list of every address assigned to a particular account.
* [GetTransaction](/docs/core-api-ref-remote-procedure-calls-wallet#section-gettransaction): gets detailed information about an in-wallet transaction.
* [ListAddressBalances](/docs/core-api-ref-remote-procedure-calls-wallet#section-listaddressbalances): lists addresses of this wallet and their balances

# ListLockUnspent

*Requires wallet support.*

The [`listlockunspent` RPC](core-api-ref-remote-procedure-calls-wallet#section-listlockunspent) returns a list of temporarily unspendable (locked) outputs.

*Parameters: none*

*Result---an array of locked outputs*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | An array containing all locked outputs.  May be empty
→<br>Output | object | Optional<br>(1 or more) | An object describing a particular locked output
→ →<br>`txid` | string (hex) | Required<br>(exactly 1) | The TXID of the transaction containing the locked output, encoded as hex in RPC byte order
→ →<br>`vout` | number (int) | Required<br>(exactly 1) | The output index number (vout) of the locked output within the transaction.  Output index `0` is the first output within the transaction

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet listlockunspent
```

Result:

``` json
[
  {
    "txid": "d3d57ec5e4168b7145e911d019e9713563c1f2db5b2d6885739ea887feca4c87",
    "vout": 0
  }
]
```

*See also*

* [LockUnspent](/docs/core-api-ref-remote-procedure-calls-wallet#section-lockunspent): temporarily locks or unlocks specified transaction outputs. A locked transaction output will not be chosen by automatic coin selection when spending dash. Locks are stored in memory only, so nodes start with zero locked outputs and the locked output list is always cleared when a node stops or fails.

# ListReceivedByAccount

*Requires wallet support.*

The [`listreceivedbyaccount` RPC](core-api-ref-remote-procedure-calls-wallet#section-listreceivedbyaccount) lists the total number of dash received by each account.

![Warning icon](https://dash-docs.github.io/img/icons/icon_warning.svg) **Warning:** `listreceivedbyaccount` will be removed in a later version of Dash
Core.  Use the RPCs listed in the See Also subsection below instead.

*Parameter #1---the minimum number of confirmations a transaction must have to be counted*

Name | Type | Presence | Description
--- | --- | --- | ---
Confirmations | number (int) | Optional<br>(0 or 1) | The minimum number of confirmations an externally-generated transaction must have before it is counted towards the balance.  Transactions generated by this node are counted immediately.  Typically, externally-generated transactions are payments to this wallet and transactions generated by this node are payments to other wallets.  Use `0` to count unconfirmed transactions.  Default is `1`

*Parameter #2---whether to include transactions locked via InstantSend*

Name | Type | Presence | Description
--- | --- | --- | ---
addlocked | bool | Optional<br>(exactly 1) | Add the balance from InstantSend locked transactions

*Parameter #3---whether to include empty accounts*

Name | Type | Presence | Description
--- | --- | --- | ---
Include Empty | bool | Optional<br>(0 or 1) | Set to `true` to display accounts which have never received a payment.  Set to `false` (the default) to only include accounts which have received a payment.  Any account which has received a payment will be displayed even if its current balance is `0`

*Parameter #4---whether to include watch-only addresses in results*

Name | Type | Presence | Description
--- | --- | --- | ---
Include Watch-Only | bool | Optional<br>(0 or 1) | If set to `true`, include watch-only addresses in details and calculations as if they were regular addresses belonging to the wallet.  If set to `false` (the default), treat watch-only addresses as if they didn't belong to this wallet

*Result---account names, balances, and minimum confirmations*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | An array containing objects each describing an account.  At the very least, the default account (\\") will be included"
→<br>Account | object | Required<br>(1 or more) | An object describing an account
→ →<br>`involvesWatchonly` | bool | Optional<br>(0 or 1) | Set to `true` if the balance of this account includes a watch-only address which has received a spendable payment (that is, a payment with at least the specified number of confirmations and which is not an immature coinbase).  Otherwise not returned
→ →<br>`account` | string | Required<br>(exactly 1) | The name of the account
→ →<br>`amount` | number (dash) | Required<br>(exactly 1) | The total amount received by this account in dash
→ →<br>`confirmations` | number (int) | Required<br>(exactly 1) | The number of confirmations received by the last transaction received by this account.  May be `0`
→ →<br>`label` | string | Optional<br>(0 or 1) | A comment for the address/transaction

*Example from Dash Core 0.13.0*

Get the balances for all non-empty accounts, including transactions
which have been confirmed at least six times and InstantSend locked transactions:

``` bash
dash-cli -testnet listreceivedbyaccount 6 true false true
```

Result (edited to only show the first two results):

``` json
[
    {
        "account" : "",
        "amount" : 0.19960000,
        "confirmations" : 53601
    },
    {
        "account" : "doc test",
        "amount" : 0.30000000,
        "confirmations" : 8991
    }
]
```

*See also*

* [ListReceivedByAddress](/docs/core-api-ref-remote-procedure-calls-wallet#section-listreceivedbyaddress): lists the total number of dash received by each address.
* [GetReceivedByAccount](/docs/core-api-ref-remote-procedure-calls-wallet#section-getreceivedbyaccount): returns the total amount received by addresses in a particular account from transactions with the specified number of confirmations.  It does not count coinbase transactions.
* [GetReceivedByAddress](/docs/core-api-ref-remote-procedure-calls-wallet#section-getreceivedbyaddress): returns the total amount received by the specified address in transactions with the specified number of confirmations. It does not count coinbase transactions.

# ListReceivedByAddress

*Requires wallet support.*

The [`listreceivedbyaddress` RPC](core-api-ref-remote-procedure-calls-wallet#section-listreceivedbyaddress) lists the total number of dash received by each address.

*Parameter #1---the minimum number of confirmations a transaction must have to be counted*

Name | Type | Presence | Description
--- | --- | --- | ---
Confirmations | number (int) | Optional<br>(0 or 1) | The minimum number of confirmations an externally-generated transaction must have before it is counted towards the balance.  Transactions generated by this node are counted immediately.  Typically, externally-generated transactions are payments to this wallet and transactions generated by this node are payments to other wallets.  Use `0` to count unconfirmed transactions.  Default is `1`

*Parameter #2---whether to include transactions locked via InstantSend*

Name | Type | Presence | Description
--- | --- | --- | ---
addlocked | bool | Optional<br>(exactly 1) | Add the balance from InstantSend locked transactions

*Parameter #3---whether to include empty accounts*

Name | Type | Presence | Description
--- | --- | --- | ---
Include Empty | bool | Optional<br>(0 or 1) | Set to `true` to display accounts which have never received a payment.  Set to `false` (the default) to only include accounts which have received a payment.  Any account which has received a payment will be displayed even if its current balance is `0`

*Parameter #4---whether to include watch-only addresses in results*

Name | Type | Presence | Description
--- | --- | --- | ---
Include Watch-Only | bool | Optional<br>(0 or 1) | If set to `true`, include watch-only addresses in details and calculations as if they were regular addresses belonging to the wallet.  If set to `false` (the default), treat watch-only addresses as if they didn't belong to this wallet

*Result---addresses, account names, balances, and minimum confirmations*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | An array containing objects each describing a particular address
→<br>Address | object | Optional<br>(0 or more) | An object describing an address
→ →<br>`involvesWatchonly` | bool | Optional<br>(0 or 1) | Set to `true` if this address is a watch-only address which has received a spendable payment (that is, a payment with at least the specified number of confirmations and which is not an immature coinbase).  Otherwise not returned
→ →<br>`address` | string (base58) | Required<br>(exactly 1) | The address being described encoded in base58check
→ →<br>`account` | string | Required<br>(exactly 1) | *Deprecated: will be removed in a later version of Dash Core*<br><br>The account the address belongs to.  May be the default account, an empty string (\\")"
→ →<br>`amount` | number (dash) | Required<br>(exactly 1) | The total amount the address has received in dash
→ →<br>`confirmations` | number (int) | Required<br>(exactly 1) | The number of confirmations of the latest transaction to the address.  May be `0` for unconfirmed
→ →<br>`label` | string | Required<br>(exactly 1) | The account the address belongs to.  May be the default account, an empty string (\\")"    
→ →<br>`txids` | array | Required<br>(exactly 1) | An array of TXIDs belonging to transactions that pay the address
→ → →<br>TXID | string | Optional<br>(0 or more) | The TXID of a transaction paying the address, encoded as hex in RPC byte order

*Example from Dash Core 0.13.0*

List addresses with balances confirmed by at least six blocks, including
watch-only addresses. Also include the balance from InstantSend locked transactions:

``` bash
dash-cli -testnet listreceivedbyaddress 6 true false true
```

Result (edit to show only two entries):

``` json
[
  {
    "address": "yV3ZTfwyfUmpspncMSitiwzh7EvqSGrqZA",
    "account": "",
    "amount": 1000.00000000,
    "confirmations": 26779,
    "label": "",
    "txids": [
      "0456aaf51a8df21dd47c2a06ede046a5bf7403bcb95d14d1d71b178c189fb933"
    ]
  },
  {
    "involvesWatchonly" : true,
    "address": "yfoR9uM3rcDfUc7AEfUNm5BjVYGFw7uQ9w",
    "account": "Watching",
    "amount": 1877.78476068,
    "confirmations": 26876,
    "label": "Watching",
    "txids": [
      "cd64114c803a2a243cb6ce4eb5c98e60cd2c688be8e900b3b957fe520cf42601",
      "83d3f7f31926908962e336341b1895d5f734f9d7149bdb35f0381cc78019bd83"
    ]
  }
]
```

*See also*

* [ListReceivedByAccount](/docs/core-api-ref-remote-procedure-calls-wallet#section-listreceivedbyaccount): lists the total number of dash received by each account.
* [GetReceivedByAddress](/docs/core-api-ref-remote-procedure-calls-wallet#section-getreceivedbyaddress): returns the total amount received by the specified address in transactions with the specified number of confirmations. It does not count coinbase transactions.
* [GetReceivedByAccount](/docs/core-api-ref-remote-procedure-calls-wallet#section-getreceivedbyaccount): returns the total amount received by addresses in a particular account from transactions with the specified number of confirmations.  It does not count coinbase transactions.

# ListSinceBlock

*Requires wallet support.*

The [`listsinceblock` RPC](core-api-ref-remote-procedure-calls-wallet#section-listsinceblock) gets all transactions affecting the wallet which have occurred since a particular block, plus the header hash of a block at a particular depth.

*Parameter #1---a block header hash*

Name | Type | Presence | Description
--- | --- | --- | ---
Header Hash | string (hex) | Optional<br>(0 or 1) | The hash of a block header encoded as hex in RPC byte order.  All transactions affecting the wallet which are not in that block or any earlier block will be returned, including unconfirmed transactions.  Default is the hash of the genesis block, so all transactions affecting the wallet are returned by default

*Parameter #2---the target confirmations for the lastblock field*

Name | Type | Presence | Description
--- | --- | --- | ---
Target Confirmations | number (int) | Optional<br>(0 or 1) | Sets the lastblock field of the results to the header hash of a block with this many confirmations.  This does not affect which transactions are returned.  Default is `1`, so the hash of the most recent block on the local best block chain is returned

*Parameter #3---whether to include watch-only addresses in details and calculations*

Name | Type | Presence | Description
--- | --- | --- | ---
Include Watch-Only | bool | Optional<br>(0 or 1) | If set to `true`, include watch-only addresses in details and calculations as if they were regular addresses belonging to the wallet.  If set to `false` (the default), treat watch-only addresses as if they didn't belong to this wallet

*Parameter #4---include_removed*

Name | Type | Presence | Description
--- | --- | --- | ---
include_removed | bool | Optional<br>Default=`true` | Show transactions that were removed due to a reorg in the \removed\" array (not guaranteed to work on pruned nodes)"

**Result**

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | An object containing an array of transactions and the lastblock field
→<br>`transactions` | array | Required<br>(exactly 1) | An array of objects each describing a particular **payment** to or from this wallet.  The objects in this array do not describe an actual transactions, so more than one object in this array may come from the same transaction.  This array may be empty
→ →<br>Payment | object | Optional<br>(0 or more) | An payment which did not appear in the specified block or an earlier block
→ <br>`involvesWatchonly` | bool | Optional<br>(0 or 1) | Set to `true` if the payment involves a watch-only address.  Otherwise not returned
→ <br>`account` | string | Required<br>(exactly 1) | *Deprecated: will be removed in a later version of Bitcoin Core*<br><br>The account which the payment was credited to or debited from.  May be an empty string (\\") for the default account"
→ <br>`address` | string (base58) | Optional<br>(0 or 1) | The address paid in this payment, which may be someone else's address not belonging to this wallet.  May be empty if the address is unknown, such as when paying to a non-standard pubkey script
→ <br>`category` | string | Required<br>(exactly 1) | Set to one of the following values:<br>• `send` if sending payment normally<br>• `privatesend` if sending PrivateSend payment<br>• `receive` if this wallet received payment in a regular transaction<br>• `generate` if a matured and spendable coinbase<br>• `immature` if a coinbase that is not spendable yet<br>• `orphan` if a coinbase from a block that's not in the local best block chain
→ <br>`amount` | number (dash) | Required<br>(exactly 1) | A negative dash amount if sending payment; a positive dash amount if receiving payment (including coinbases)
→ <br>`vout` | number (int) | Required<br>(exactly 1) | For an output, the output index (vout) for this output in this transaction.  For an input, the output index for the output being spent in its transaction.  Because inputs list the output indexes from previous transactions, more than one entry in the details array may have the same output index
→ <br>`fee` | number (dash) | Optional<br>(0 or 1) | If sending payment, the fee paid as a negative dash value.  May be `0`. Not returned if receiving payment
→ <br>`confirmations` | number (int) | Required<br>(exactly 1) | The number of confirmations the transaction has received.  Will be `0` for unconfirmed and `-1` for conflicted
→ <br>`instantlock` | bool | Required<br>(exactly 1) | Current transaction lock state (InstantSend and/or ChainLock)
→ <br>`instantlock_internal` | bool | Required<br>(exactly 1) | Current InstantSend transaction lock state
→ <br>`chainlock` | bool | Required<br>(exactly 1) | *Added in Dash Core 0.14.0*<br><br>If set to `true`, this transaction is in a block that is locked (not susceptible to a chain re-org)  
→ <br>`generated` | bool | Optional<br>(0 or 1) | Set to `true` if the transaction is a coinbase.  Not returned for regular transactions
→ <br>`blockhash` | string (hex) | Optional<br>(0 or 1) | The hash of the block on the local best block chain which includes this transaction, encoded as hex in RPC byte order.  Only returned for confirmed transactions
→ <br>`blockindex` | number (int) | Optional<br>(0 or 1) | The index of the transaction in the block that includes it.  Only returned for confirmed transactions
→ <br>`blocktime` | number (int) | Optional<br>(0 or 1) | The block header time (Unix epoch time) of the block on the local best block chain which includes this transaction.  Only returned for confirmed transactions
→ <br>`txid` | string (hex) | Required<br>(exactly 1) | The TXID of the transaction, encoded as hex in RPC byte order
→ <br>`walletconflicts` | array | Required<br>(exactly 1) | An array containing the TXIDs of other transactions that spend the same inputs (UTXOs) as this transaction.  Array may be empty
→ →<br>TXID | string (hex) | Optional<br>(0 or more) | The TXID of a conflicting transaction, encoded as hex in RPC byte order
→ <br>`time` | number (int) | Required<br>(exactly 1) | A Unix epoch time when the transaction was added to the wallet
→ <br>`timereceived` | number (int) | Required<br>(exactly 1) | A Unix epoch time when the transaction was detected by the local node, or the time of the block on the local best block chain that included the transaction
→ <br>`abandoned` | bool | Optional<br>(0 or 1) | `true` if the transaction has been abandoned (inputs are respendable). Only available for the 'send' category of transactions.
→ <br>`comment` | string | Optional<br>(0 or 1) | For transaction originating with this wallet, a locally-stored comment added to the transaction.  Only returned if a comment was added
→ <br>`to` | string | Optional<br>(0 or 1) | For transaction originating with this wallet, a locally-stored comment added to the transaction identifying who the transaction was sent to.  Only returned if a comment-to was added
→<br>`removed` | array | Optional<br>(0 or 1) | Structure is the same as `transactions`. Only present if `include_removed` is `true`.<br>_Note_: transactions that were re-added in the active chain will appear as-is in this array, and may thus have a positive confirmation count.
→<br>`lastblock` | string (hex) | Required<br>(exactly 1) | The header hash of the block with the number of confirmations specified in the *target confirmations* parameter, encoded as hex in RPC byte order

*Example from Dash Core 0.15.0*

Get all transactions since a particular block (including watch-only
transactions) and the header hash of the sixth most recent block.

``` bash
dash-cli -testnet listsinceblock \
              0000000001fc119ea77e0c67783227fb9d55386125179ea5597109d311af2337 \
              6 true true
```

Result (edited to show only two payments):

``` json
{
  "transactions": [
    {
      "account": "",
      "address": "yMaodAgCofB2gmHEtATAiV3w5NkzTpmkgS",
      "category": "send",
      "amount": -2365.65209808,
      "label": "Mining Consolidation",
      "vout": 0,
      "fee": -0.00031420,
      "confirmations": 5,
      "instantlock": true,
      "instantlock_internal": false,
      "chainlock": true,
      "blockhash": "00000000001c4e142c6deaa273206706d37a7aa792887d9bd81ae787d4259137",
      "blockindex": 1,
      "blocktime": 1566399553,
      "txid": "bb8a2789c3166181cc190e0fd7675770217b69c9aeafe0d8207baf1ebeb05845",
      "walletconflicts": [
      ],
      "time": 1566399271,
      "timereceived": 1566399271,
      "abandoned": false
    },
    {
      "account": "Mining Consolidation",
      "address": "yMaodAgCofB2gmHEtATAiV3w5NkzTpmkgS",
      "category": "receive",
      "amount": 2365.65209808,
      "label": "Mining Consolidation",
      "vout": 0,
      "confirmations": 5,
      "instantlock": true,
      "instantlock_internal": false,
      "chainlock": true,
      "blockhash": "00000000001c4e142c6deaa273206706d37a7aa792887d9bd81ae787d4259137",
      "blockindex": 1,
      "blocktime": 1566399553,
      "txid": "bb8a2789c3166181cc190e0fd7675770217b69c9aeafe0d8207baf1ebeb05845",
      "walletconflicts": [
      ],
      "time": 1566399271,
      "timereceived": 1566399271
    }
  ],
  "removed": [
  ],
  "lastblock": "000000000158ad1e4eab53044e18aaf76e605a27252862d4f1d78cfd373f1686"
}
```

*See also*

* [ListReceivedByAccount](/docs/core-api-ref-remote-procedure-calls-wallet#section-listreceivedbyaccount): lists the total number of dash received by each account.
* [ListReceivedByAddress](/docs/core-api-ref-remote-procedure-calls-wallet#section-listreceivedbyaddress): lists the total number of dash received by each address.

# ListTransactions

*Requires wallet support.*

The [`listtransactions` RPC](core-api-ref-remote-procedure-calls-wallet#section-listtransactions) returns the most recent transactions that affect the wallet.

*Parameter #1---an account name to get transactions from*

Name | Type | Presence | Description
--- | --- | --- | ---
Account | string | Optional<br>(0 or 1) | *Deprecated: will be removed in a later version of Dash Core*<br><br>The name of an account to get transactions from.  Use an empty string (\\") to get transactions for the default account.  Default is `*` to get transactions for all accounts."

*Parameter #2---the number of transactions to get*

Name | Type | Presence | Description
--- | --- | --- | ---
Count | number (int) | Optional<br>(0 or 1) | The number of the most recent transactions to list.  Default is `10`

*Parameter #3---the number of transactions to skip*

Name | Type | Presence | Description
--- | --- | --- | ---
Skip | number (int) | Optional<br>(0 or 1) | The number of the most recent transactions which should not be returned.  Allows for pagination of results.  Default is `0`

*Parameter #4---whether to include watch-only addresses in details and calculations*

Name | Type | Presence | Description
--- | --- | --- | ---
Include Watch-Only | bool | Optional<br>(0 or 1) | If set to `true`, include watch-only addresses in details and calculations as if they were regular addresses belonging to the wallet.  If set to `false` (the default), treat watch-only addresses as if they didn't belong to this wallet

*Result---payment details*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | An array containing objects, with each object describing a **payment** or internal accounting entry (not a transaction).  More than one object in this array may come from a single transaction.  Array may be empty
→<br>Payment | object | Optional<br>(0 or more) | A payment or internal accounting entry
→ →<br>`account` | string | Required<br>(exactly 1) | *Deprecated: will be removed in a later version of Dash Core*<br><br>The account which the payment was credited to or debited from.  May be an empty string (\\") for the default account"
→ →<br>`address` | string (base58) | Optional<br>(0 or 1) | The address paid in this payment, which may be someone else's address not belonging to this wallet.  May be empty if the address is unknown, such as when paying to a non-standard pubkey script or if this is in the *move* category
→ →<br>`category` | string | Required<br>(exactly 1) | Set to one of the following values:<br>• `send` if sending payment<br>• `receive` if this wallet received payment in a regular transaction<br>• `generate` if a matured and spendable coinbase<br>• `immature` if a coinbase that is not spendable yet<br>• `orphan` if a coinbase from a block that's not in the local best block chain<br>• `move` if an off-block-chain move made with the [`move` RPC](core-api-ref-remote-procedure-calls-wallet#section-move)
→ →<br>`amount` | number (dash) | Required<br>(exactly 1) | A negative dash amount if sending payment; a positive dash amount if receiving payment (including coinbases)
→ →<br>`label` | string | Optional<br>(0 or 1) | A comment for the address/transaction  
→ →<br>`vout` | number (int) | Optional<br>(0 or 1) | For an output, the output index (vout) for this output in this transaction.  For an input, the output index for the output being spent in its transaction.  Because inputs list the output indexes from previous transactions, more than one entry in the details array may have the same output index.  Not returned for *move* category payments
→ →<br>`fee` | number (dash) | Optional<br>(0 or 1) | If sending payment, the fee paid as a negative dash value.  May be `0`. Not returned if receiving payment or for *move* category payments
→ →<br>`confirmations` | number (int) | Optional<br>(0 or 1) | The number of confirmations the transaction has received.  Will be `0` for unconfirmed and `-1` for conflicted.  Not returned for *move* category payments
→<br>`instantlock` | bool | Required<br>(exactly 1) | Current transaction lock state (InstantSend and/or ChainLock)  
→<br>`instantlock_internal` | bool | Required<br>(exactly 1) | Current InstantSend transaction lock state
<br>`chainlock` | bool | Required<br>(exactly 1) | *Added in Dash Core 0.14.0*<br><br>If set to `true`, this transaction is in a block that is locked (not susceptible to a chain re-org)
→ →<br>`generated` | bool | Optional<br>(0 or 1) | Set to `true` if the transaction is a coinbase.  Not returned for regular transactions or *move* category payments
→ →<br>`trusted` | bool | Optional<br>(0 or 1) | Indicates whether we consider the outputs of this unconfirmed transaction safe to spend.  Only returned for unconfirmed transactions
→ →<br>`blockhash` | string (hex) | Optional<br>(0 or 1) | The hash of the block on the local best block chain which includes this transaction, encoded as hex in RPC byte order.  Only returned for confirmed transactions
→ →<br>`blockindex` | number (int) | Optional<br>(0 or 1) | The index of the transaction in the block that includes it.  Only returned for confirmed transactions
→ →<br>`blocktime` | number (int) | Optional<br>(0 or 1) | The block header time (Unix epoch time) of the block on the local best block chain which includes this transaction.  Only returned for confirmed transactions
→ →<br>`txid` | string (hex) | Optional<br>(0 or 1) | The TXID of the transaction, encoded as hex in RPC byte order.  Not returned for *move* category payments
→ →<br>`walletconflicts` | array | Optional<br>(0 or 1) | An array containing the TXIDs of other transactions that spend the same inputs (UTXOs) as this transaction.  Array may be empty.  Not returned for *move* category payments
→ → →<br>TXID | string (hex) | Optional<br>(0 or more) | The TXID of a conflicting transaction, encoded as hex in RPC byte order
→ →<br>`time` | number (int) | Required<br>(exactly 1) | A Unix epoch time when the transaction was added to the wallet
→ →<br>`timereceived` | number (int) | Optional<br>(0 or 1) | A Unix epoch time when the transaction was detected by the local node, or the time of the block on the local best block chain that included the transaction.  Not returned for *move* category payments
→ →<br>`comment` | string | Optional<br>(0 or 1) | For transaction originating with this wallet, a locally-stored comment added to the transaction.  Only returned in regular payments if a comment was added.  Always returned in *move* category payments.  May be an empty string
→ →<br>`to` | string | Optional<br>(0 or 1) | For transaction originating with this wallet, a locally-stored comment added to the transaction identifying who the transaction was sent to.  Only returned if a comment-to was added.  Never returned by *move* category payments.  May be an empty string
→ →<br>`otheraccount` | string | Optional<br>(0 or 1) | This is the account the dash were moved from or moved to, as indicated by a negative or positive *amount* field in this payment.  Only returned by *move* category payments
→ →<br>`abandoned` | bool | Optional<br>(0 or 1) | *Added in Bitcoin Core 0.12.1*<br><br>Indicates if a transaction is was abandoned:<br>• `true` if it was abandoned (inputs are respendable)<br>• `false`  if it was not abandoned<br>Only returned by *send* category payments

*Example from Dash Core 0.14.0*

List the most recent transaction from the main account including watch-only addresses.

``` bash
dash-cli listtransactions "" 1 0 true
```

Result:

``` json
[
  {
    "account": "",
    "address": "ySGKtDZ3qBHRqk7mHsdofShQkqMcAS7SYJ",
    "category": "send",
    "amount": -0.50000000,
    "label": "",
    "vout": 1,
    "fee": -0.00040000,
    "confirmations": 3,
    "instantlock": true,
    "instantlock_internal": true,
    "chainlock": false,
    "blockhash": "000000000327ff7785d799dde99949457ac231ef1d956a2287c2f7bb84d9738c",
    "blockindex": 2,
    "blocktime": 1553798971,
    "txid": "048aae3ad194f5398b67fc7029b26bf50d66ecc7d185fd6d26f8c6ec5a4ed1f9",
    "walletconflicts": [
    ],
    "time": 1553798920,
    "timereceived": 1553798920,
    "abandoned": false
  }
]
```

*See also*

* [GetTransaction](/docs/core-api-ref-remote-procedure-calls-wallet#section-gettransaction): gets detailed information about an in-wallet transaction.
* [ListSinceBlock](/docs/core-api-ref-remote-procedure-calls-wallet#section-listsinceblock): gets all transactions affecting the wallet which have occurred since a particular block, plus the header hash of a block at a particular depth.

# ListUnspent

*Requires wallet support.*

The [`listunspent` RPC](core-api-ref-remote-procedure-calls-wallet#section-listunspent) returns an array of unspent transaction outputs belonging to this wallet. **Note:** as of Bitcoin
Core 0.10.0, outputs affecting watch-only addresses will be returned; see
the *spendable* field in the results described below.

*Parameter #1---the minimum number of confirmations an output must have*

Name | Type | Presence | Description
--- | --- | --- | ---
Minimum Confirmations | number (int) | Optional<br>(0 or 1) | The minimum number of confirmations the transaction containing an output must have in order to be returned.  Use `0` to return outputs from unconfirmed transactions. Default is `1`

*Parameter #2---the maximum number of confirmations an output may have*

Name | Type | Presence | Description
--- | --- | --- | ---
Maximum Confirmations | number (int) | Optional<br>(0 or 1) | The maximum number of confirmations the transaction containing an output may have in order to be returned.  Default is `9999999` (~10 million)

*Parameter #3---the addresses an output must pay*

Name | Type | Presence | Description
--- | --- | --- | ---
Addresses | array | Optional<br>(0 or 1) | If present, only outputs which pay an address in this array will be returned
→<br>Address | string (base58) | Required<br>(1 or more) | A P2PKH or P2SH address

*Parameter #4---include unsafe outputs*

Name | Type | Presence | Description
--- | --- | --- | ---
Include Unsafe | bool | Optional<br>(false or true) | Include outputs that are not safe to spend . See description of `safe` attribute below.  Default is `true`

*Parameter #5---query options*

Name | Type | Presence | Description
--- | --- | --- | ---
Query Options | json | Optional | JSON with query options. Available options:<br> - `minimumAmount`: Minimum value of each UTXO in DASH<br> - `maximumAmount`: Maximum value of each UTXO in DASH<br> - `maximumCount`: Maximum number of UTXOs<br> - `minimumSumAmount`: Minimum sum value of all UTXOs in DASH

*Result---the list of unspent outputs*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | An array of objects each describing an unspent output.  May be empty
→<br>Unspent Output | object | Optional<br>(0 or more) | An object describing a particular unspent output belonging to this wallet
→ →<br>`txid` | string (hex) | Required<br>(exactly 1) | The TXID of the transaction containing the output, encoded as hex in RPC byte order
→ →<br>`vout` | number (int) | Required<br>(exactly 1) | The output index number (vout) of the output within its containing transaction
→ →<br>`address` | string (base58) | Optional<br>(0 or 1) | The P2PKH or P2SH address the output paid.  Only returned for P2PKH or P2SH output scripts
→ →<br>`account` | string | Optional<br>(0 or 1) | *Deprecated: will be removed in a later version of Dash Core*<br><br>If the address returned belongs to an account, this is the account.  Otherwise not returned
→ →<br>`scriptPubKey` | string (hex) | Required<br>(exactly 1) | The output script paid, encoded as hex
→ →<br>`redeemScript` | string (hex) | Optional<br>(0 or 1) | If the output is a P2SH whose script belongs to this wallet, this is the redeem script
→ →<br>`amount` | number (int) | Required<br>(exactly 1) | The amount paid to the output in dash
→ →<br>`confirmations` | number (int) | Required<br>(exactly 1) | The number of confirmations received for the transaction containing this output
→ →<br>`spendable` | bool | Required<br>(exactly 1) | Set to `true` if the private key or keys needed to spend this output are part of the wallet.  Set to `false` if not (such as for watch-only addresses)
→ →<br>`solvable` | bool | Required<br>(exactly 1) | *Added in Bitcoin Core 0.13.0*<br><br>Set to `true` if the wallet knows how to spend this output.  Set to `false` if the wallet does not know how to spend the output.  It is ignored if the private keys are available
→ →<br>`ps_rounds` | number (int) | Required<br>(exactly 1) | The number of PrivateSend rounds
→ →<br>`safe` | bool | Required<br>(exactly 1) | *Added in Bitcoin Core 0.15.0*<br><br>Whether this output is considered safe to spend. Unconfirmed transactions from outside keys are considered unsafe and are not eligible for spending by `fundrawtransaction` and `sendtoaddress`.

*Example from Dash Core 0.15.0*

Get all outputs confirmed at least 6 times for a particular
address:

``` bash
dash-cli -testnet listunspent 6 99999999 '''
  [
    "yLki4jbxX28JB3TThm1DTgRfbKVhhiMx3d"
  ]
'''
```

Result:

``` json
[
  {
    "txid": "534fe12e360773dddf8aa125a4027d2d8c0073e13ff2f04fd733202b85dbdcf1",
    "vout": 0,
    "address": "yLki4jbxX28JB3TThm1DTgRfbKVhhiMx3d",
    "scriptPubKey": "76a91404c719ccf48d39d3e6253ac98edaf2b5d24f0c0588ac",
    "amount": 1.00001000,
    "confirmations": 85,
    "spendable": true,
    "solvable": true,
    "safe": true,
    "ps_rounds": 13
  }
]
```

Get all outputs for a particular address that have at least 1 confirmation and a maximum value of 10:

``` bash
listunspent 1 9999999 "[\"yQqTPAw1Nk8iFDeDXqe5dQ7A9xD6LVUStD\"]" true "{\"maximumAmount\":\"10\"}"

dash-cli -testnet listunspent 1 9999999 '''
  [
    "yQqTPAw1Nk8iFDeDXqe5dQ7A9xD6LVUStD"
  ]
  ''' true '''
  {
    "maximumAmount": "10"
  }
  '''
```

Result:

``` json
[
  {
    "txid": "42cd5150fd1179b5a194e034685d524e6d5d38703ac794d236495923a29addc5",
    "vout": 1,
    "address": "yQqTPAw1Nk8iFDeDXqe5dQ7A9xD6LVUStD",
    "account": "",
    "scriptPubKey": "76a914318d6d7e26e07a142a425a32ea917a30147d6c9788ac",
    "amount": 5.00000000,
    "confirmations": 100,
    "spendable": true,
    "solvable": true,
    "safe": true,
    "ps_rounds": -2
  }
]
```

*See also*

* [ListTransactions](/docs/core-api-ref-remote-procedure-calls-wallet#section-listtransactions): returns the most recent transactions that affect the wallet.
* [LockUnspent](/docs/core-api-ref-remote-procedure-calls-wallet#section-lockunspent): temporarily locks or unlocks specified transaction outputs. A locked transaction output will not be chosen by automatic coin selection when spending dash. Locks are stored in memory only, so nodes start with zero locked outputs and the locked output list is always cleared when a node stops or fails.

# ListWallets

The [`listwallets` RPC](core-api-ref-remote-procedure-calls-wallet#section-listwallets) returns a list of currently loaded wallets.

For full information on the wallet, use the [`getwalletinfo` RPC](core-api-ref-remote-procedure-calls-wallet#section-getwalletinfo).

*Parameters: none*

*Result*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | An array of strings containing a list of currently loaded wallet files
→<br>Wallet | string | Required<br>(0 or more) | The name of a wallet file

*Example from Dash Core 0.15.0*

``` bash
dash-cli -testnet listwallets
```

Result:
``` json
[
  "wallet.dat"
]
```

*See also: none*

# LockUnspent

*Requires wallet support.*

The [`lockunspent` RPC](core-api-ref-remote-procedure-calls-wallet#section-lockunspent) temporarily locks or unlocks specified transaction outputs. A locked transaction output will not be chosen by automatic coin selection when spending dash. Locks are stored in memory only, so nodes start with zero locked outputs and the locked output list is always cleared when a node stops or fails.

*Parameter #1---whether to lock or unlock the outputs*

Name | Type | Presence | Description
--- | --- | --- | ---
Unlock | bool | Required<br>(exactly 1) | Set to `false` to lock the outputs specified in the following parameter.  Set to `true` to unlock the outputs specified.  If this is the only argument specified and it is set to `true`, all outputs will be unlocked; if it is the only argument and is set to `false`, there will be no change

*Parameter #2---the outputs to lock or unlock*

Name | Type | Presence | Description
--- | --- | --- | ---
Outputs | array | Optional<br>(0 or 1) | An array of outputs to lock or unlock
→<br>Output | object | Required<br>(1 or more) | An object describing a particular output
→ →<br>`txid` | string | Required<br>(exactly 1) | The TXID of the transaction containing the output to lock or unlock, encoded as hex in internal byte order
→ →<br>`vout` | number (int) | Required<br>(exactly 1) | The output index number (vout) of the output to lock or unlock.  The first output in a transaction has an index of `0`

*Result---`true` if successful*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | bool | Required<br>(exactly 1) | Set to `true` if the outputs were successfully locked or unlocked.  If the outputs were already locked or unlocked, it will also return `true`

*Example from Dash Core 0.12.2*

Lock two outputs:

``` bash
dash-cli -testnet lockunspent false '''
  [
    {
      "txid": "d3d57ec5e4168b7145e911d019e9713563c1f2db5b2d6885739ea887feca4c87",
      "vout": 0
    },
    {
      "txid": "607897611b2f7c5b23297b2a352a8d6f4383f8d0782585f93220082d361f8db9",
      "vout": 1
    }
  ]
'''
```

Result:

``` json
true
```

Verify the outputs have been locked:

``` bash
dash-cli -testnet listlockunspent
```

Result

``` json
[
  {
    "txid": "d3d57ec5e4168b7145e911d019e9713563c1f2db5b2d6885739ea887feca4c87",
    "vout": 0
  },
  {
    "txid": "607897611b2f7c5b23297b2a352a8d6f4383f8d0782585f93220082d361f8db9",
    "vout": 1
  }
]
```

Unlock one of the above outputs:

``` bash
dash-cli -testnet lockunspent true '''
[
  {
    "txid": "607897611b2f7c5b23297b2a352a8d6f4383f8d0782585f93220082d361f8db9",
    "vout": 1
  }
]
'''
```

Result:

``` json
true
```

Verify the output has been unlocked:

``` bash
dash-cli -testnet listlockunspent
```

Result:

``` json
[
  {
    "txid": "d3d57ec5e4168b7145e911d019e9713563c1f2db5b2d6885739ea887feca4c87",
    "vout": 0
  }
]
```

*See also*

* [ListLockUnspent](/docs/core-api-ref-remote-procedure-calls-wallet#section-listlockunspent): returns a list of temporarily unspendable (locked) outputs.
* [ListUnspent](/docs/core-api-ref-remote-procedure-calls-wallet#section-listunspent): returns an array of unspent transaction outputs belonging to this wallet.

# Move

*Requires wallet support.*

The [`move` RPC](core-api-ref-remote-procedure-calls-wallet#section-move) moves a specified amount from one account in your wallet to another using an off-block-chain transaction.

![Warning icon](https://dash-docs.github.io/img/icons/icon_warning.svg) **Warning:** `move` will be removed in a later version of Dash
Core.  Use the RPCs listed in the See Also subsection below instead.

![Warning icon](https://dash-docs.github.io/img/icons/icon_warning.svg) **Warning:** it's allowed to move more funds than are in an account,
giving the sending account a negative balance and giving the receiving
account a balance that may exceed the number of dash in the wallet
(or the number of dash in existence).

*Parameter #1---from account*

Name | Type | Presence | Description
--- | --- | --- | ---
From Account | string | Required<br>(exactly 1) | The name of the account to move the funds from

*Parameter #2---to account*

Name | Type | Presence | Description
--- | --- | --- | ---
To Account | string | Required<br>(exactly 1) | The name of the account to move the funds to

*Parameter #3---amount to move*

Name | Type | Presence | Description
--- | --- | --- | ---
Amount | number (dash) | Required<br>(exactly 1) | The amount of dash to move

*Parameter #4---an unused parameter*

Name | Type | Presence | Description
--- | --- | --- | ---
*Unused* | number (int) | Optional<br>(0 or 1) | This parameter is no longer used. If parameter #5 needs to be specified, this can be any integer

*Parameter #5---a comment*

Name | Type | Presence | Description
--- | --- | --- | ---
Comment | string | Optional<br>(0 or 1) | A comment to assign to this move payment

*Result---`true` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | bool | Required<br>(exactly 1) | Set to `true` if the move was successful

*Example from Dash Core 0.12.2*

Move 1 dash from "doc test" to "test1", giving the transaction the
comment "Example move":

``` bash
dash-cli -testnet move "doc test" "test1" 0.1 0 "Example move"
```

Result:

``` json
true
```

*See also*

* [ListAccounts](/docs/core-api-ref-remote-procedure-calls-wallet#section-listaccounts): lists accounts and their balances.
* [SendFrom](/docs/core-api-ref-remote-procedure-calls-wallet#section-sendfrom): spends an amount from a local account to a dash address.
* [SendToAddress](/docs/core-api-ref-remote-procedure-calls-wallet#section-sendtoaddress): spends an amount to a given address.

# RemovePrunedFunds

*Added in Dash Core 0.12.3 / Bitcoin Core 0.13.0*

*Requires wallet support.*

The [`removeprunedfunds` RPC](core-api-ref-remote-procedure-calls-wallet#section-removeprunedfunds) deletes the specified transaction from the wallet. Meant for use with pruned wallets and as a companion to importprunedfunds. This will effect wallet balances.

*Parameter #1---the raw transaction to import*

Name | Type | Presence | Description
--- | --- | --- | ---
TXID | string<br>(hex) | Required<br>(exactly 1) | The hex-encoded id of the transaction you are removing

*Result---`null` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | If the funds are removed from the wallet, JSON `null` will be returned

*Example from Dash Core 0.12.3*

``` bash
dash-cli removeprunedfunds bb7daff525b83fa6a847ab50bf7f8f14d6\
22761a19f69157b362ef3f25bda687
```

(Success: no result displayed.)

*See also*

* [ImportPrivKey](/docs/core-api-ref-remote-procedure-calls-wallet#section-importprivkey): adds a private key to your wallet. The key should be formatted in the wallet import format created by the [`dumpprivkey` RPC](core-api-ref-remote-procedure-calls-wallet#section-dumpprivkey).
* [ImportPrunedFunds](/docs/core-api-ref-remote-procedure-calls-wallet#section-importprunedfunds): imports funds without the need of a rescan. Meant for use with pruned wallets.

# SendFrom

*Requires wallet support. Requires an unlocked wallet or an
unencrypted wallet.*

The [`sendfrom` RPC](core-api-ref-remote-procedure-calls-wallet#section-sendfrom) spends an amount from a local account to a dash address.

![Warning icon](https://dash-docs.github.io/img/icons/icon_warning.svg) **Warning:** `sendfrom` will be removed in a later version of Dash
Core.  Use the RPCs listed in the See Also subsection below instead.

*Parameter #1---from account*

Name | Type | Presence | Description
--- | --- | --- | ---
From Account | string | Required<br>(exactly 1) | The name of the account from which the dash should be spent.  Use an empty string (\\") for the default account"

*Parameter #2---to address*

Name | Type | Presence | Description
--- | --- | --- | ---
To Address | string | Required<br>(exactly 1) | A P2PKH or P2SH address to which the dash should be sent

*Parameter #3---amount to spend*

Name | Type | Presence | Description
--- | --- | --- | ---
Amount | number (dash) | Required<br>(exactly 1) | The amount to spend in dash.  Dash Core will ensure the account has sufficient dash to pay this amount (but the transaction fee paid is not included in the calculation, so an account can spend a total of its balance plus the transaction fee)

*Parameter #4---minimum confirmations*

Name | Type | Presence | Description
--- | --- | --- | ---
Confirmations | number (int) | Optional<br>(0 or 1) | The minimum number of confirmations an incoming transaction must have for its outputs to be credited to this account's balance. Outgoing transactions are always counted, as are move transactions made with the [`move` RPC](core-api-ref-remote-procedure-calls-wallet#section-move). If an account doesn't have a balance high enough to pay for this transaction, the payment will be rejected.  Use `0` to spend unconfirmed incoming payments. Default is `1`

![Warning icon](https://dash-docs.github.io/img/icons/icon_warning.svg)
**Warning:** if account1 receives an unconfirmed payment and transfers
it to account2 with the [`move` RPC](core-api-ref-remote-procedure-calls-wallet#section-move), account2 will be able to spend those
dash even if this parameter is set to `1` or higher.

*Parameter #5---whether to add the balance from transactions locked via InstantSend*

Name | Type | Presence | Description
--- | --- | --- | ---
addlocked | bool | Optional<br>(0 or 1) | If set to `true`, add the balance from InstantSend locked transactions. If set to `false` (the default), InstantSend locked transaction balances are not included.

*Parameter #6---a comment*

Name | Type | Presence | Description
--- | --- | --- | ---
Comment | string | Optional<br>(0 or 1) | A locally-stored (not broadcast) comment assigned to this transaction.  Default is no comment

*Parameter #7---a comment about who the payment was sent to*

Name | Type | Presence | Description
--- | --- | --- | ---
Comment To | string | Optional<br>(0 or 1) | A locally-stored (not broadcast) comment assigned to this transaction.  Meant to be used for describing who the payment was sent to. Default is no comment

*Result---a TXID of the sent transaction*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string | Required<br>(exactly 1) | The TXID of the sent transaction, encoded as hex in RPC byte order

*Example from Dash Core 0.12.2*

Spend 0.1 dash from the account "test" to the address indicated below
using only UTXOs with at least six confirmations, giving the
transaction the comment "Example spend" and labeling the spender
"Example.com":

``` bash
dash-cli -testnet sendfrom "test" \
            yhJays6zGUFKq1KS5V5WLbyk3cwCXyGrKd \
            0.1 \
            6 \
            false \
            "Example spend" \
            "Example.com"
```

Result:

``` text
cd64b9d55c63bf247f2eca32f978e340622107b607a46c422dabcdc20c0571fe
```

*See also*

* [SendToAddress](/docs/core-api-ref-remote-procedure-calls-wallet#section-sendtoaddress): spends an amount to a given address.
* [SendMany](/docs/core-api-ref-remote-procedure-calls-wallet#section-sendmany): creates and broadcasts a transaction which sends outputs to multiple addresses.

# SendMany

*Requires wallet support. Requires an unlocked wallet or an
unencrypted wallet.*

The [`sendmany` RPC](core-api-ref-remote-procedure-calls-wallet#section-sendmany) creates and broadcasts a transaction which sends outputs to multiple addresses.

*Parameter #1---from account*

Name | Type | Presence | Description
--- | --- | --- | ---
From Account | string | Required<br>(exactly 1) | *Deprecated: will be removed in a later version of Dash Core*<br><br>The name of the account from which the dash should be spent.  Use an empty string (\\") for the default account. Dash Core will ensure the account has sufficient dash to pay the total amount in the *outputs* field described below (but the transaction fee paid is not included in the calculation, so an account can spend a total of its balance plus the transaction fee)"

*Parameter #2---the addresses and amounts to pay*

Name | Type | Presence | Description
--- | --- | --- | ---
Outputs | object | Required<br>(exactly 1) | An object containing key/value pairs corresponding to the addresses and amounts to pay
→<br>Address/Amount | string (base58) : number (dash) | Required<br>(1 or more) | A key/value pair with a base58check-encoded string containing the P2PKH or P2SH address to pay as the key, and an amount of dash to pay as the value

*Parameter #3---minimum confirmations*

Name | Type | Presence | Description
--- | --- | --- | ---
Confirmations | number (int) | Optional<br>(0 or 1) | The minimum number of confirmations an incoming transaction must have for its outputs to be credited to this account's balance. Outgoing transactions are always counted, as are move transactions made with the [`move` RPC](core-api-ref-remote-procedure-calls-wallet#section-move). If an account doesn't have a balance high enough to pay for this transaction, the payment will be rejected.  Use `0` to spend unconfirmed incoming payments. Default is `1`

![Warning icon](https://dash-docs.github.io/img/icons/icon_warning.svg)
**Warning:** if account1 receives an unconfirmed payment and transfers
it to account2 with the [`move` RPC](core-api-ref-remote-procedure-calls-wallet#section-move), account2 will be able to spend those
dash even if this parameter is set to `1` or higher.

*Parameter #4--whether to add the balance from transactions locked via InstantSend*
Name | Type | Presence | Description
--- | --- | --- | ---
addlocked | bool | Optional<br>(0 or 1) | If set to `true`, add the balance from InstantSend locked transactions. If set to `false` (the default), InstantSend locked transaction balances are not included.

*Parameter #5---a comment*

Name | Type | Presence | Description
--- | --- | --- | ---
Comment | string | Optional<br>(0 or 1) | A locally-stored (not broadcast) comment assigned to this transaction.  Default is no comment

*Parameter #6---automatic fee subtraction*

Name | Type | Presence | Description
--- | --- | --- | ---
Subtract Fee From Amount | array | Optional<br>(0 or 1) | An array of addresses.  The fee will be equally divided by as many addresses as are entries in this array and subtracted from each address.  If this array is empty or not provided, the fee will be paid by the sender
→<br>Address | string (base58) | Optional (0 or more) | An address previously listed as one of the recipients.

*Parameter #7---use InstantSend*

Name | Type | Presence | Description
--- | --- | --- | ---
Use InstantSend | bool | Optional<br>(0 or 1) | *Deprecated and ignored since Dash Core 0.15.0*

*Parameter #8---use PrivateSend*

Name | Type | Presence | Description
--- | --- | --- | ---
Use PrivateSend | bool | Optional<br>(0 or 1) | If set to `true`, use anonymized funds only (default: false).

*Parameter #9---confirmation target*

Name | Type | Presence | Description
--- | --- | --- | ---
`conf_target` | number (int) | Optional<br>(0 or 1) | Confirmation target (in blocks)

*Parameter #10---fee estimate mode*

Name | Type | Presence | Description
--- | --- | --- | ---
`estimate_mode` | string | Optional<br>(0 or 1) |  The fee estimate mode, must be one of:<br>`UNSET`<br>`ECONOMICAL`<br>`CONSERVATIVE`<br>Default: `UNSET`

*Result---a TXID of the sent transaction*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string | Required<br>(exactly 1) | The TXID of the sent transaction, encoded as hex in RPC byte order

*Example from Dash Core 0.12.2*

From the account *test1*, send 0.1 dash to the first address and 0.2
dash to the second address, with a comment of "Example Transaction".

``` bash
dash-cli -testnet sendmany \
  "test1" \
  '''
    {
      "ySutkc49Khpz1HQN8AfWNitVBLwqtyaxvv": 0.1,
      "yhQrX8CZTTfSjKmaq5h7DgSShyEsumCRBi": 0.2
    } ''' \
  6       \
  false   \
  "Example Transaction"
```

Result:

``` text
a7c0194a005a220b9bfeb5fdd12d5b90979c10f53de4f8a48a1495aa198a6b95
```

*Example from Dash Core 0.12.2 (InstantSend)*

From the account *test1*, send 0.1 dash to the first address and 0.2
dash to the second address using InstantSend, with a comment of "Example Transaction".

``` bash
dash-cli -testnet sendmany \
  "test1" \
  '''
    {
      "ySutkc49Khpz1HQN8AfWNitVBLwqtyaxvv": 0.1,
      "yhQrX8CZTTfSjKmaq5h7DgSShyEsumCRBi": 0.2
    } ''' \
  6       \
  false   \
  "Example Transaction"
  '''
    [""]
  '''     \
  true
```

Result:

``` text
3a5bbaa1a7aa3a8af45e8f1adf79528f99efc61052b0616d41b33fb8fb7af347
```

*Example from Dash Core 0.12.2 (PrivateSend)*

From the account *test1*, send 0.1 dash to the first address and 0.2
dash to the second address using PrivateSend, with a comment of "Example Transaction".

``` bash
dash-cli -testnet sendmany \
  "test1" \
  '''
    {
      "ySutkc49Khpz1HQN8AfWNitVBLwqtyaxvv": 0.1,
      "yhQrX8CZTTfSjKmaq5h7DgSShyEsumCRBi": 0.2
    } ''' \
  6       \
  false   \
  "Example Transaction"
  '''
    [""]
  '''    \
  false  \
  true
```

Result:

``` text
43337c8e4f3b21bedad7765fa851a6e855e4bb04f60d6b3e4c091ed21ffc5753
```

*See also*

* [SendFrom](/docs/core-api-ref-remote-procedure-calls-wallet#section-sendfrom): spends an amount from a local account to a dash address.
* [SendToAddress](/docs/core-api-ref-remote-procedure-calls-wallet#section-sendtoaddress): spends an amount to a given address.
* [Move](/docs/core-api-ref-remote-procedure-calls-wallet#section-move): moves a specified amount from one account in your wallet to another using an off-block-chain transaction.

# SendToAddress

*Requires wallet support. Requires an unlocked wallet or an
unencrypted wallet.*

The [`sendtoaddress` RPC](core-api-ref-remote-procedure-calls-wallet#section-sendtoaddress) spends an amount to a given address.

*Parameter #1---to address*

Name | Type | Presence | Description
--- | --- | --- | ---
To Address | string | Required<br>(exactly 1) | A P2PKH or P2SH address to which the dash should be sent

*Parameter #2---amount to spend*

Name | Type | Presence | Description
--- | --- | --- | ---
Amount | number (dash) | Required<br>(exactly 1) | The amount to spent in dash

*Parameter #3---a comment*

Name | Type | Presence | Description
--- | --- | --- | ---
Comment | string | Optional<br>(0 or 1) | A locally-stored (not broadcast) comment assigned to this transaction.  Default is no comment

*Parameter #4---a comment about who the payment was sent to*

Name | Type | Presence | Description
--- | --- | --- | ---
Comment To | string | Optional<br>(0 or 1) | A locally-stored (not broadcast) comment assigned to this transaction.  Meant to be used for describing who the payment was sent to. Default is no comment

*Parameter #5---automatic fee subtraction*

Name | Type | Presence | Description
--- | --- | --- | ---
Subtract Fee From Amount | boolean | Optional<br>(0 or 1) | The fee will be deducted from the amount being sent. The recipient will receive less dash than you enter in the amount field. Default is `false`

*Parameter #6---use InstantSend*

Name | Type | Presence | Description
--- | --- | --- | ---
Use InstantSend | bool | Optional<br>(0 or 1) | *Deprecated and ignored since Dash Core 0.15.0*

*Parameter #7---use PrivateSend*

Name | Type | Presence | Description
--- | --- | --- | ---
Use PrivateSend | bool | Optional<br>(0 or 1) | If set to `true`, use anonymized funds only (default: false).

*Parameter #8---confirmation target*

Name | Type | Presence | Description
--- | --- | --- | ---
`conf_target` | number (int) | Optional<br>(0 or 1) | Confirmation target (in blocks)

*Parameter #9---fee estimate mode*

Name | Type | Presence | Description
--- | --- | --- | ---
`estimate_mode` | string | Optional<br>(0 or 1) |  The fee estimate mode, must be one of:<br>`UNSET`<br>`ECONOMICAL`<br>`CONSERVATIVE`<br>Default: `UNSET`

*Result---a TXID of the sent transaction*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string | Required<br>(exactly 1) | The TXID of the sent transaction, encoded as hex in RPC byte order

*Example from Dash Core 0.12.2*

Spend 0.1 dash to the address below with the comment "sendtoaddress
example" and the comment-to "Nemo From Example.com":

``` bash
dash-cli -testnet sendtoaddress ySutkc49Khpz1HQN8AfWNitVBLwqtyaxvv \
  1.0 "sendtoaddress example" "Nemo From Example.com"
```

Result:

``` text
70e2029d363f0110fe8a0aa2ba7bd771a579453135568b2aa559b2cb30f875aa
```

*Example from Dash Core 0.12.2 (InstantSend)*

Spend 0.1 dash via InstantSend to the address below with the comment "sendtoaddress
example" and the comment-to "Nemo From Example.com":

``` bash
dash-cli -testnet sendtoaddress ySutkc49Khpz1HQN8AfWNitVBLwqtyaxvv \
  1.0 "sendtoaddress example" "Nemo From Example.com" false true
```

Result:

``` text
af002b9c931b5efb5b2852df3d65efd48c3b9ac2ba0ef8a4cf97b894f3ff08c2
```

*Example from Dash Core 0.12.2 (PrivateSend)*

Spend 0.1 dash via PrivateSend to the address below with the comment "sendtoaddress
example" and the comment-to "Nemo From Example.com":

``` bash
dash-cli -testnet sendtoaddress ySutkc49Khpz1HQN8AfWNitVBLwqtyaxvv \
  1.0 "sendtoaddress example" "Nemo From Example.com" false false true
```

Result:

``` text
949833bc49e0643f63e2afed1704ccccf005a93067a4e46165b06ace42544694
```

*Example from Dash Core 0.12.2 (InstantSend + PrivateSend)*

Spend 0.1 dash via InstantSend and PrivateSend to the address below with the
comment "sendtoaddressexample" and the comment-to "Nemo From Example.com":

``` bash
dash-cli -testnet sendtoaddress ySutkc49Khpz1HQN8AfWNitVBLwqtyaxvv \
  1.008 "sendtoaddress example" "Nemo From Example.com" false true true
```

Result:

``` text
ba4bbe29fa06b67d6f3f3a73e381627e66abe22e217ce329aefad41ea72c3922
```

*See also*

* [SendFrom](/docs/core-api-ref-remote-procedure-calls-wallet#section-sendfrom): spends an amount from a local account to a dash address.
* [SendMany](/docs/core-api-ref-remote-procedure-calls-wallet#section-sendmany): creates and broadcasts a transaction which sends outputs to multiple addresses.
* [Move](/docs/core-api-ref-remote-procedure-calls-wallet#section-move): moves a specified amount from one account in your wallet to another using an off-block-chain transaction.

# SetAccount

*Requires wallet support.*

The [`setaccount` RPC](core-api-ref-remote-procedure-calls-wallet#section-setaccount) puts the specified address in the given account.

![Warning icon](https://dash-docs.github.io/img/icons/icon_warning.svg) **Warning:** `setaccount` will be removed in a later version of Dash
Core.  Use the RPCs listed in the See Also subsection below instead.

*Parameter #1---a dash address*

Name | Type | Presence | Description
--- | --- | --- | ---
Address | string (base58) | Required<br>(exactly 1) | The P2PKH or P2SH address to put in the account.  Must already belong to the wallet

*Parameter #2---an account*

Name | Type | Presence | Description
--- | --- | --- | ---
Account | string | Required<br>(exactly 1) | The name of the account in which the address should be placed.  May be the default account, an empty string (\\")"

*Result---`null` if successful*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | Set to JSON `null` if the address was successfully placed in the account

*Example from Dash Core 0.12.2*

Put the address indicated below in the "doc test" account.

``` bash
dash-cli -testnet setaccount \
    yMTFRnrfJ4NpnYVeidDNHVwT7uuNsVjevq "doc test"
```

(Success: no result displayed.)

*See also*

* [GetAccount](/docs/core-api-ref-remote-procedure-calls-wallet#section-getaccount): returns the name of the account associated with the given address.
* [ListAccounts](/docs/core-api-ref-remote-procedure-calls-wallet#section-listaccounts): lists accounts and their balances.
* [GetAddressesByAccount](/docs/core-api-ref-remote-procedure-calls-wallet#section-getaddressesbyaccount): returns a list of every address assigned to a particular account.

# SetPrivateSendAmount

The [`setprivatesendamount` RPC](core-api-ref-remote-procedure-calls-wallet#section-setprivatesendamount) sets the amount of DASH to be mixed with PrivateSend

*Parameter #1---amount to mix*

Name | Type | Presence | Description
--- | --- | --- | ---
Amount | int | Required<br>(exactly 1) | The number of DASH to mix (minimum: 2, maximum: 21,000,000)

*Result---`null` on success*

*Example from Dash Core 0.13.0*

``` bash
dash-cli -testnet setprivatesendamount 2000
```

(Success: no result displayed.)

*See also:*

* [SetPrivateSendRounds](/docs/core-api-ref-remote-procedure-calls-wallet#section-setprivatesendrounds): sets the number of PrivateSend mixing rounds to use

# SetPrivateSendRounds

The [`setprivatesendrounds` RPC](core-api-ref-remote-procedure-calls-wallet#section-setprivatesendrounds) sets the number of PrivateSend mixing rounds to use

*Parameter #1---number of mixing rounds to use*

Name | Type | Presence | Description
--- | --- | --- | ---
Rounds | int | Required<br>(exactly 1) | The number of mixing rounds to use (minimum: 1, maximum: 16)

*Result---`null` on success*

*Example from Dash Core 0.13.0*

``` bash
dash-cli -testnet setprivatesendrounds 4
```

(Success: no result displayed.)

*See also:*

* [SetPrivateSendAmount](/docs/core-api-ref-remote-procedure-calls-wallet#section-setprivatesendamount): sets the amount of DASH to be mixed with PrivateSend

# SetTxFee

*Requires wallet support.*

The [`settxfee` RPC](core-api-ref-remote-procedure-calls-wallet#section-settxfee) sets the transaction fee per kilobyte paid by transactions created by this wallet.

*Parameter #1---the transaction fee amount per kilobyte*

Name | Type | Presence | Description
--- | --- | --- | ---
Transaction Fee Per Kilobyte | number (dash) | Required<br>(exactly 1) | The transaction fee to pay, in dash, for each kilobyte of transaction data.  Be careful setting the fee too low---your transactions may not be relayed or included in blocks

*Result: `true` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | bool (true) | Required<br>(exactly 1) | Set to `true` if the fee was successfully set

*Example from Dash Core 0.12.2*

Set the transaction fee per kilobyte to 10,000 duffs.

``` bash
dash-cli -testnet settxfee 0.00010000
```

Result:

``` json
true
```

*See also*

* [GetWalletInfo](/docs/core-api-ref-remote-procedure-calls-wallet#section-getwalletinfo): provides information about the wallet.
* [GetNetworkInfo](/docs/core-api-ref-remote-procedure-calls-network#section-getnetworkinfo): returns information about the node's connection to the network.

# SignMessage

*Requires wallet support. Requires an unlocked wallet or an
unencrypted wallet.*

The [`signmessage` RPC](core-api-ref-remote-procedure-calls-wallet#section-signmessage) signs a message with the private key of an address.

*Parameter #1---the address corresponding to the private key to sign with*

Name | Type | Presence | Description
--- | --- | --- | ---
Address | string (base58) | Required<br>(exactly 1) | A P2PKH address whose private key belongs to this wallet

*Parameter #2---the message to sign*

Name | Type | Presence | Description
--- | --- | --- | ---
Message | string | Required<br>(exactly 1) | The message to sign

*Result---the message signature*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string (base64) | Required<br>(exactly 1) | The signature of the message, encoded in base64.

*Example from Dash Core 0.12.2*

Sign a the message "Hello, World!" using the following address:

``` bash
dash-cli -testnet signmessage yNpezfFDfoikDuT1f4iK75AiLp2YLPsGAb "Hello, World!"
```

Result:

``` text
H4XULzfHCf16In2ECk9Ta9QxQPq639zQto2JA3OLlo3JbUdrClvJ89+A1z+Z9POd6l8LJhn1jGpQYF8mX4jkQvE=
```

*See also*

* [VerifyMessage](/docs/core-api-ref-remote-procedure-calls-utility#section-verifymessage): verifies a signed message.

# WalletLock

*Requires wallet support. Requires an unlocked wallet.*

The [`walletlock` RPC](core-api-ref-remote-procedure-calls-wallet#section-walletlock) removes the wallet encryption key from memory, locking the wallet. After calling this method, you will need to call `walletpassphrase` again before being able to call any methods which require the wallet to be unlocked.

*Parameters: none*

*Result---`null` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | Always set to JSON `null`

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet walletlock
```

(Success: nothing printed.)

*See also*

* [EncryptWallet](/docs/core-api-ref-remote-procedure-calls-wallet#section-encryptwallet): encrypts the wallet with a passphrase.  This is only to enable encryption for the first time. After encryption is enabled, you will need to enter the passphrase to use private keys.
* [WalletPassphrase](/docs/core-api-ref-remote-procedure-calls-wallet#section-walletpassphrase): stores the wallet decryption key in memory for the indicated number of seconds. Issuing the `walletpassphrase` command while the wallet is already unlocked will set a new unlock time that overrides the old one.
* [WalletPassphraseChange](/docs/core-api-ref-remote-procedure-calls-wallet#section-walletpassphrasechange): changes the wallet passphrase from 'old passphrase' to 'new passphrase'.

# WalletPassphrase

*Requires wallet support. Requires an encrypted wallet.*

The [`walletpassphrase` RPC](core-api-ref-remote-procedure-calls-wallet#section-walletpassphrase) stores the wallet decryption key in memory for the indicated number of seconds. Issuing the `walletpassphrase` command while the wallet is already unlocked will set a new unlock time that overrides the old one.

![Warning icon](https://dash-docs.github.io/img/icons/icon_warning.svg) **Warning:** if using this RPC on the command line, remember
that your shell probably saves your command lines (including the
value of the passphrase parameter).

*Parameter #1---the passphrase*

Name | Type | Presence | Description
--- | --- | --- | ---
Passphrase | string | Required<br>(exactly 1) | The passphrase that unlocks the wallet

*Parameter #2---the number of seconds to leave the wallet unlocked*

Name | Type | Presence | Description
--- | --- | --- | ---
Seconds | number (int) | Required<br>(exactly 1) | The number of seconds after which the decryption key will be automatically deleted from memory

*Parameter #3---unlock for PrivateSend mixing only*

Name | Type | Presence | Description
--- | --- | --- | ---
Mixing Only | bool | Optional<br>(0 or 1) | If `true`, the wallet will be locked for sending functions but unlocked for mixing transactions (default: false)

*Result---`null` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | Always set to JSON `null`

*Example from Dash Core 0.12.2*

Unlock the wallet for 10 minutes (the passphrase is "test"):

``` bash
dash-cli -testnet walletpassphrase test 600
```

(Success: no result printed.)

Unlock the wallet for mixing transactions only for 10 minutes (the passphrase is "test"):

``` bash
dash-cli -testnet walletpassphrase test 600 true
```

(Success: no result printed.)

*See also*

* [EncryptWallet](/docs/core-api-ref-remote-procedure-calls-wallet#section-encryptwallet): encrypts the wallet with a passphrase.  This is only to enable encryption for the first time. After encryption is enabled, you will need to enter the passphrase to use private keys.
* [WalletPassphraseChange](/docs/core-api-ref-remote-procedure-calls-wallet#section-walletpassphrasechange): changes the wallet passphrase from 'old passphrase' to 'new passphrase'.
* [WalletLock](/docs/core-api-ref-remote-procedure-calls-wallet#section-walletlock): removes the wallet encryption key from memory, locking the wallet. After calling this method, you will need to call `walletpassphrase` again before being able to call any methods which require the wallet to be unlocked.

# WalletPassphraseChange

*Requires wallet support.  Requires an encrypted wallet.*

The [`walletpassphrasechange` RPC](core-api-ref-remote-procedure-calls-wallet#section-walletpassphrasechange) changes the wallet passphrase from 'old passphrase' to 'new passphrase'.

![Warning icon](https://dash-docs.github.io/img/icons/icon_warning.svg) **Warning:** if using this RPC on the command line, remember
that your shell probably saves your command lines (including the
value of the passphrase parameter).

*Parameter #1---the current passphrase*

Name | Type | Presence | Description
--- | --- | --- | ---
Current Passphrase | string | Required<br>(exactly 1) | The current wallet passphrase

*Parameter #2---the new passphrase*

Name | Type | Presence | Description
--- | --- | --- | ---
New Passphrase | string | Required<br>(exactly 1) | The new passphrase for the wallet

*Result---`null` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | Always set to JSON `null`

*Example from Dash Core 0.12.2*

Change the wallet passphrase from "test" to "example":

``` bash
dash-cli -testnet walletpassphrasechange "test" "example"
```

(Success: no result printed.)

*See also*

* [EncryptWallet](/docs/core-api-ref-remote-procedure-calls-wallet#section-encryptwallet): encrypts the wallet with a passphrase.  This is only to enable encryption for the first time. After encryption is enabled, you will need to enter the passphrase to use private keys.
* [WalletPassphrase](/docs/core-api-ref-remote-procedure-calls-wallet#section-walletpassphrase): stores the wallet decryption key in memory for the indicated number of seconds. Issuing the `walletpassphrase` command while the wallet is already unlocked will set a new unlock time that overrides the old one.
* [WalletLock](/docs/core-api-ref-remote-procedure-calls-wallet#section-walletlock): removes the wallet encryption key from memory, locking the wallet. After calling this method, you will need to call `walletpassphrase` again before being able to call any methods which require the wallet to be unlocked.