# Debug

The [`debug` RPC](core-api-ref-remote-procedure-calls-control#section-debug) changes the debug category from the console.

*Parameter #1---debug category*

Name | Type | Presence | Description
--- | --- | --- | ---
Debug category | string | Required<br>(1 or more) | The debug category to activate. Use a `+` to specify multiple categories. Categories will be one of the following:<br>• `0` - Disables all categories <br>• `1` or `all` - Enables all categories <br>• `addrman` <br>• `bench` <br>• `cmpctblock` <br>• `coindb` <br>• `db` <br>• `estimatefee` <br>• `http` <br>• `leveldb` <br>• `libevent` <br>• `mempool` <br>• `mempoolrej` <br>• `net` <br>• `proxy` <br>• `prune` <br>• `qt` <br>• `rand` <br>• `reindex` <br>• `rpc` <br>• `selectcoins` <br>• `tor` <br>• `zmq` <br>• `dash` (all subcategories)<br><br>The `dash` sub-categories can be enabled individually:<br>• `chainlocks` <br>• `gobject` <br>• `instantsend` <br>• `keepass` <br>• `llmq` <br>• `llmq-dkg` <br>• `llmq-sigs` <br>• `mnpayments` <br>• `mnsync` <br>• `privatesend` <br>• `spork` <br><br><br>Note: No error will be thrown even if the specified category doesn't match any of the above

*Example from Dash Core 0.15.0*

``` bash
dash-cli -testnet debug "net+mempool"
```

Result:

``` text
Debug mode: net+mempool
```

*See also*

* [Logging](/docs/core-api-ref-remote-procedure-calls-control#section-logging): gets and sets the logging configuration

# GetInfo

The [`getinfo` RPC](core-api-ref-remote-procedure-calls-control#section-getinfo) prints various information about the node and the network.
[block:callout]
{
  "type": "warning",
  "body": "**Warning:** `getinfo` will be removed in a later version of Dash Core.  Use the RPCs listed in the See Also subsection below instead.",
  "title": "Deprecation Warning"
}
[/block]
*Parameters: none*

*Result---information about the node and network*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | Information about this node and the network
→<br>`deprecation-warning` | string | Required<br>(exactly 1) | Warning that the getinfo command is deprecated and will be removed in a future version
→<br>`version` | number (int) | Required<br>(exactly 1) | This node's version of Bitcoin Core in its internal integer format.  For example, Dash Core 0.12.2 has the integer version number 120200
→<br>`protocolversion` | number (int) | Required<br>(exactly 1) | The protocol version number used by this node.  See the [protocol versions section](core-ref-p2p-network-protocol-versions) for more information
→<br>`walletversion` | number (int) | Optional<br>(0 or 1) | The version number of the wallet.  Only returned if wallet support is enabled
→<br>`balance` | number (duffs) | Optional<br>(0 or 1) | The total balance of the wallet in duffs.  Only returned if wallet support is enabled
→<br>`privatesend_balance` | number (duffs) | Optional<br>(0 or 1) | The PrivateSend balance of the wallet in duffs.  Only returned if wallet support is enabled  (Added in Dash Core 0.11.0)
→<br>`blocks` | number (int) | Required<br>(exactly 1) | The number of blocks in the local best block chain.  A new node with only the hardcoded genesis block will return `0`
→<br>`timeoffset` | number (int) | Required<br>(exactly 1) | The offset of the node's clock from the computer's clock (both in UTC) in seconds.  The offset may be up to 4200 seconds (70 minutes)
→<br>`connections` | number (int) | Required<br>(exactly 1) | The total number of open connections (both outgoing and incoming) between this node and other nodes
→<br>`proxy` | string | Required<br>(exactly 1) | The hostname/IP address and port number of the proxy, if set, or an empty string if unset
→<br>`difficulty` | number (real) | Required<br>(exactly 1) | The difficulty of the highest-height block in the local best block chain
→<br>`testnet` | bool | Required<br>(exactly 1) | Set to `true` if this node is on testnet; set to `false` if this node is on mainnet or a regtest
→<br>`keypoololdest` | number (int) | Optional<br>(0 or 1) | The date as Unix epoch time when the oldest key in the wallet key pool was created; useful for only scanning blocks created since this date for transactions.  Only returned if wallet support is enabled
→<br>`keypoolsize` | number (int) | Optional<br>(0 or 1) | The number of keys in the wallet keypool.  Only returned if wallet support is enabled
→<br>`unlocked_until` | number (int) | Optional<br>(0 or 1) | The Unix epoch time when the wallet will automatically re-lock.  Only displayed if wallet encryption is enabled.  Set to `0` if wallet is currently locked
→<br>`paytxfee` | number (duffs) | Optional<br>(0 or 1) | The minimum fee to pay per kilobyte of transaction; may be `0`.  Only returned if wallet support is enabled
→<br>`relayfee` | number (duffs) | Required<br>(exactly 1) | The minimum fee per kilobyte a transaction must pay in order for this node to accept it into its memory pool
→<br>`errors` | string | Required<br>(exactly 1) | A plain-text description of any errors this node has encountered or detected.  If there are no errors, an empty string will be returned.  This is not related to the JSON-RPC `error` field

*Example from Dash Core 0.15.0 with wallet support enabled*

``` bash
dash-cli -testnet getinfo
```

Result:

``` json
{
  "deprecation-warning": "WARNING: getinfo is deprecated and will be fully removed in a future version. Projects should transition to using getblockchaininfo, getnetworkinfo, and getwalletinfo.",
  "version": 140100,
  "protocolversion": 70215,
  "walletversion": 61000,
  "balance": 0.00000000,
  "privatesend_balance": 0.00000000,
  "blocks": 0,
  "timeoffset": 0,
  "connections": 0,
  "proxy": "",
  "difficulty": 0.000244140625,
  "testnet": true,
  "keypoololdest": 1507579068,
  "keypoolsize": 617,
  "unlocked_until": 0,
  "paytxfee": 0.00000000,
  "relayfee": 0.00010000,
  "errors": ""
}
```

*See also*

* [GetBlockChainInfo](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getblockchaininfo): provides information about the current state of the block chain.
* [GetMemPoolInfo](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getmempoolinfo): returns information about the node's current transaction memory pool.
* [GetMiningInfo](/docs/core-api-ref-remote-procedure-calls-mining#section-getmininginfo): returns various mining-related information.
* [GetNetworkInfo](/docs/core-api-ref-remote-procedure-calls-network#section-getnetworkinfo): returns information about the node's connection to the network.
* [GetWalletInfo](/docs/core-api-ref-remote-procedure-calls-wallet#section-getwalletinfo): provides information about the wallet.

# GetMemoryInfo

*Added in Dash Core 0.12.3 / Bitcoin Core 0.14.0*

The [`getmemoryinfo` RPC](core-api-ref-remote-procedure-calls-control#section-getmemoryinfo) returns information about memory usage.

*Parameter #1---mode*

Name | Type | Presence | Description
--- | --- | --- | ---
mode| string | Optional<br>Default: `stats` | *Added in Dash Core 0.15.0*<br><br>Determines what kind of information is returned.<br>- `stats` returns general statistics about memory usage in the daemon.<br>- `mallocinfo` returns an XML string describing low-level heap state (only available if compiled with glibc 2.10+).

*Result---information about memory usage*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | An object containing information about memory usage
→<br>`locked` | string : object | Required<br>(exactly 1) | An object containing information about locked memory manager
→→<br>`used` | number (int) | Required<br>(exactly 1) | Number of bytes used
→→<br>`free` | number (int) | Required<br>(exactly 1) | Number of bytes available in current arenas
→→<br>`total` | number (int) | Required<br>(exactly 1) | Total number of bytes managed
→→<br>`locked` | number (int) | Required<br>(exactly 1) | Amount of bytes that succeeded locking
→→<br>`chunks_used` | number (int) | Required<br>(exactly 1) | Number allocated chunks
→→<br>`chunks_free` | number (int) | Required<br>(exactly 1) | Number unused chunks

*Example from Dash Core 0.12.3*

``` bash
dash-cli getmemoryinfo
```

Result:

``` json
{
  "locked": {
    "used": 1146240,
    "free": 426624,
    "total": 1572864,
    "locked": 1572864,
    "chunks_used": 16368,
    "chunks_free": 7
  }
}
```

*See also*

* [GetMemPoolInfo](/docs/core-api-ref-remote-procedure-calls-blockchain#section-getmempoolinfo): returns information about the node's current transaction memory pool.

# Help

The [`help` RPC](core-api-ref-remote-procedure-calls-control#section-help) lists all available public RPC commands, or gets help for the specified RPC.  Commands which are unavailable will not be listed, such as wallet RPCs if wallet support is disabled.

*Parameter---the name of the RPC to get help for*

Name | Type | Presence | Description
--- | --- | --- | ---
RPC | string | Optional<br>(0 or 1) | The name of the RPC to get help for.  If omitted, Dash Core 0.10x will display an alphabetical list of commands; Dash Core 0.11.0 will display a categorized list of commands

*Result---a list of RPCs or detailed help for a specific RPC*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string | Required<br>(exactly 1) | The help text for the specified RPC or the list of commands.  The `dash-cli` command will parse this text and format it as human-readable text

*Example from Dash Core 0.12.2*

Command to get help about the [`help` RPC](core-api-ref-remote-procedure-calls-control#section-help):

``` bash
dash-cli -testnet help help
```

Result:

``` text
help ( "command" )

List all commands, or get help for a specified command.

Arguments:
1. "command"     (string, optional) The command to get help on

Result:
"text"     (string) The help text

```

*See also*

* The [RPC Quick Reference](core-api-ref-remote-procedure-call-quick-reference)

# Logging

The [`logging` RPC](core-api-ref-remote-procedure-calls-control#section-logging) gets and sets the logging configuration

*Parameter #1---include categories*

Name | Type | Presence | Description
--- | --- | --- | ---
`include` | array of strings | Optional<br>(0 or 1) | Enable debugging for these categories

*Parameter #2---exclude categories*

Name | Type | Presence | Description
--- | --- | --- | ---
`exclude` | array of strings | Optional<br>(0 or 1) | Enable debugging for these categories

The categories are:

| Type | Category |
| - | - |
| Special | • `0` - Disables all categories <br>• `1` or `all` - Enables all categories <br>• `dash` - Enables/disables all Dash categories |
| Standard | `addrman`, `bench` <br>`cmpctblock`, `coindb`, `db`, `estimatefee`, `http`, `leveldb`, `libevent`, `mempool`, `mempoolrej`, `net`, `proxy`, `prune`, `qt`, `rand`, `reindex`, `rpc`, `selectcoins`, `tor`, `zmq`|
| Dash | <br>`chainlocks`, `gobject`, `instantsend`, `keepass`, `llmq`, `llmq-dkg`, `llmq-sigs`, `mnpayments`, `mnsync`, `privatesend`, `spork` |

*Result---a list of the logging categories that are active*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | A JSON object show a list of the logging categories that are active

*Example from Dash Core 0.15.0*

Include a category in logging

``` bash
dash-cli -testnet logging '["llmq", "spork"]'
```

Result:
``` json
{
  "net": 0,
  "tor": 0,
  "mempool": 0,
  "http": 0,
  "bench": 0,
  "zmq": 0,
  "db": 0,
  "rpc": 0,
  "estimatefee": 0,
  "addrman": 0,
  "selectcoins": 0,
  "reindex": 0,
  "cmpctblock": 0,
  "rand": 0,
  "prune": 0,
  "proxy": 0,
  "mempoolrej": 0,
  "libevent": 0,
  "coindb": 0,
  "qt": 0,
  "leveldb": 0,
  "chainlocks": 0,
  "gobject": 0,
  "instantsend": 0,
  "keepass": 0,
  "llmq": 1,
  "llmq-dkg": 0,
  "llmq-sigs": 0,
  "mnpayments": 0,
  "mnsync": 0,
  "privatesend": 0,
  "spork": 1
}
```

Excluding a previously included category (without including any new ones):

``` bash
dash-cli -testnet logging '[]' '["spork"]'
```

Result:
``` json
{
  "net": 0,
  "tor": 0,
  "mempool": 0,
  "http": 0,
  "bench": 0,
  "zmq": 0,
  "db": 0,
  "rpc": 0,
  "estimatefee": 0,
  "addrman": 0,
  "selectcoins": 0,
  "reindex": 0,
  "cmpctblock": 0,
  "rand": 0,
  "prune": 0,
  "proxy": 0,
  "mempoolrej": 0,
  "libevent": 0,
  "coindb": 0,
  "qt": 0,
  "leveldb": 0,
  "chainlocks": 0,
  "gobject": 0,
  "instantsend": 0,
  "keepass": 0,
  "llmq": 1,
  "llmq-dkg": 0,
  "llmq-sigs": 0,
  "mnpayments": 0,
  "mnsync": 0,
  "privatesend": 0,
  "spork": 0
}
```

*See also*

* [Debug](/docs/core-api-ref-remote-procedure-calls-control#section-debug): changes the debug category from the console.

# Stop

The [`stop` RPC](core-api-ref-remote-procedure-calls-control#section-stop) safely shuts down the Dash Core server.

*Parameters: none*

*Result---the server is safely shut down*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string | Required<br>(exactly 1) | The string \Dash Core server stopping\""

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet stop
```

Result:

``` text
Dash Core server stopping
```

*See also: none*

# Uptime

The [`uptime` RPC](core-api-ref-remote-procedure-calls-control#section-uptime) returns the total uptime of the server.

*Parameters: none*

*Result*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | number (int) | Required<br>(exactly 1) | The number of seconds that the server has been running

*Example from Dash Core 0.15.0*

``` bash
dash-cli -testnet uptime
```

Result:
``` text
5500
```

*See also: none*