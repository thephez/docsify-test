# AddNode

The [`addnode` RPC](core-api-ref-remote-procedure-calls-network#section-addnode) attempts to add or remove a node from the addnode list, or to try a connection to a node once.

*Parameter #1---hostname/IP address and port of node to add or remove*

Name | Type | Presence | Description
--- | --- | --- | ---
`node` | string | Required<br>(exactly 1) | The node to add as a string in the form of `<IP address>:<port>`.

*Parameter #2---whether to add or remove the node, or to try only once to connect*

Name | Type | Presence | Description
--- | --- | --- | ---
`command` | string | Required<br>(exactly 1) | What to do with the IP address above.  Options are:<br>• `add` to add a node to the addnode list.  Up to 8 nodes can be added additional to the default 8 nodes. Not limited by `-maxconnections`<br>• `remove` to remove a node from the list.  If currently connected, this will disconnect immediately<br>• `onetry` to immediately attempt connection to the node even if the outgoing connection slots are full; this will only attempt the connection once

*Result---`null` plus error on failed remove*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | Always JSON `null` whether the node was added, removed, tried-and-connected, or tried-and-not-connected.  The JSON-RPC error field will be set only if you try adding a node that was already added or removing a node that is not on the addnodes list

*Example from Dash Core 0.12.2*

Try connecting to the following node.

``` bash
dash-cli -testnet addnode 192.0.2.113:19999 onetry
```

Result (no output from `dash-cli` because result is set to `null`).

*See also*

* [GetAddedNodeInfo](/docs/core-api-ref-remote-procedure-calls-network#section-getaddednodeinfo): returns information about the given added node, or all added nodes (except onetry nodes). Only nodes which have been manually added using the [`addnode` RPC](core-api-ref-remote-procedure-calls-network#section-addnode) will have their information displayed.

# ClearBanned

*Added in Bitcoin Core 0.12.0*

The [`clearbanned` RPC](core-api-ref-remote-procedure-calls-network#section-clearbanned) clears list of banned nodes.

*Parameters: none*

*Result---`null` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | JSON `null` when the list was cleared

*Example from Dash Core 0.12.2*

Clears the ban list.

``` bash
dash-cli clearbanned
```

Result (no output from `dash-cli` because result is set to `null`).

*See also*

* [ListBanned](/docs/core-api-ref-remote-procedure-calls-network#section-listbanned): lists all banned IPs/Subnets.
* [SetBan](/docs/core-api-ref-remote-procedure-calls-network#section-setban): attempts add or remove a IP/Subnet from the banned list.

# DisconnectNode

*Added in Bitcoin Core 0.12.0*

The [`disconnectnode` RPC](core-api-ref-remote-procedure-calls-network#section-disconnectnode) immediately disconnects from a specified node.

*Parameter #1---hostname/IP address and port of node to disconnect*

Name | Type | Presence | Description
--- | --- | --- | ---
`address` | string | Required<br>(exactly 1) | The node you want to disconnect from as a string in the form of `<IP address>:<port>`.<br><br>*Updated in Bitcoin Core 0.14.1*

*Parameter #2---nodeid*

Name | Type | Presence | Description
--- | --- | --- | ---
nodeid | number | Optional | The node ID (see `getpeerinfo` for node IDs)

*Result---`null` on success or error on failed disconnect*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | JSON `null` when the node was disconnected

*Example from Dash Core 0.15.0*

Disconnects following node by address.

``` bash
dash-cli -testnet disconnectnode 192.0.2.113:19999
```

Result (no output from `dash-cli` because result is set to `null`).

Disconnects following node by id.

``` bash
dash-cli -testnet disconnectnode "" 3
```

Result (no output from `dash-cli` because result is set to `null`).

*See also*

* [AddNode](/docs/core-api-ref-remote-procedure-calls-network#section-addnode): attempts to add or remove a node from the addnode list, or to try a connection to a node once.
* [GetAddedNodeInfo](/docs/core-api-ref-remote-procedure-calls-network#section-getaddednodeinfo): returns information about the given added node, or all added nodes (except onetry nodes). Only nodes which have been manually added using the [`addnode` RPC](core-api-ref-remote-procedure-calls-network#section-addnode) will have their information displayed.

# GetAddedNodeInfo

The [`getaddednodeinfo` RPC](core-api-ref-remote-procedure-calls-network#section-getaddednodeinfo) returns information about the given added node, or all added nodes (except onetry nodes). Only nodes which have been manually added using the [`addnode` RPC](core-api-ref-remote-procedure-calls-network#section-addnode) will have their information displayed.

Prior to Dash Core 0.12.3, this dummy parameter was required for historical purposes but not used:

*DEPRECATED Parameter #1---whether to display connection information*

Name | Type | Presence | Description
--- | --- | --- | ---
_Dummy_ | _bool_ | _Required<br>(exactly 1)_ | _Removed in Dash Core 0.12.3_

Beginning with Dash Core 0.12.3, this is the single (optional) parameter:

*Parameter #1---what node to display information about*

Name | Type | Presence | Description
--- | --- | --- | ---
`node` | string | Optional<br>(0 or 1) | The node to get information about in the same `<IP address>:<port>` format as the [`addnode` RPC](core-api-ref-remote-procedure-calls-network#section-addnode).  If this parameter is not provided, information about all added nodes will be returned

*Result---a list of added nodes*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | An array containing objects describing each added node.  If no added nodes are present, the array will be empty.  Nodes added with `onetry` will not be returned
→<br>Added Node | object | Optional<br>(0 or more) | An object containing details about a single added node
→ →<br>`addednode` | string | Required<br>(exactly 1) | An added node in the same `<IP address>:<port>` format as used in the [`addnode` RPC](core-api-ref-remote-procedure-calls-network#section-addnode).
→ →<br>`connected` | bool | Optional<br>(0 or 1) | This will be set to `true` if the node is currently connected and `false` if it is not
→ →<br>`addresses` | array | Required<br>(exactly 1) | This will be an array of addresses belonging to the added node
→ → →<br>Address | object | Optional<br>(0 or more) | An object describing one of this node's addresses
→ → → →<br>`address` | string | Required<br>(exactly 1) | An IP address and port number of the node.  If the node was added using a DNS address, this will be the resolved IP address
→ → → →<br>`connected` | string | Required<br>(exactly 1) | Whether or not the local node is connected to this addnode using this IP address.  Valid values are:<br>• `false` for not connected<br>• `inbound` if the addnode connected to us<br>• `outbound` if we connected to the addnode

*Example from Dash Core 0.12.3*

``` bash
dash-cli getaddednodeinfo
```

Result (real hostname and IP address replaced with [RFC5737](http://tools.ietf.org/html/rfc5737) reserved address):

``` json
[
  {
    "addednode": "192.0.2.113:19999",
    "connected": true,
    "addresses": [
      {
        "address": "192.0.2.113:19999",
        "connected": "outbound"
      }
    ]
  }
]
```

*See also*

* [AddNode](/docs/core-api-ref-remote-procedure-calls-network#section-addnode): attempts to add or remove a node from the addnode list, or to try a connection to a node once.
* [GetPeerInfo](/docs/core-api-ref-remote-procedure-calls-network#section-getpeerinfo): returns data about each connected network node.

# GetConnectionCount

The [`getconnectioncount` RPC](core-api-ref-remote-procedure-calls-network#section-getconnectioncount) returns the number of connections to other nodes.

*Parameters: none*

*Result---the number of connections to other nodes*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | number (int) | Required<br>(exactly 1) | The total number of connections to other nodes (both inbound and outbound)

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet getconnectioncount
```

Result:

``` text
14
```

*See also*

* [GetNetTotals](/docs/core-api-ref-remote-procedure-calls-network#section-getnettotals): returns information about network traffic, including bytes in, bytes out, and the current time.
* [GetPeerInfo](/docs/core-api-ref-remote-procedure-calls-network#section-getpeerinfo): returns data about each connected network node.
* [GetNetworkInfo](/docs/core-api-ref-remote-procedure-calls-network#section-getnetworkinfo): returns information about the node's connection to the network.

# GetNetTotals

The [`getnettotals` RPC](core-api-ref-remote-procedure-calls-network#section-getnettotals) returns information about network traffic, including bytes in, bytes out, and the current time.

*Parameters: none*

*Result---the current bytes in, bytes out, and current time*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | An object containing information about the node's network totals
→<br>`totalbytesrecv` | number (int) | Required<br>(exactly 1) | The total number of bytes received since the node was last restarted
→<br>`totalbytessent` | number (int) | Required<br>(exactly 1) | The total number of bytes sent since the node was last restarted
→<br>`timemillis` | number (int) | Required<br>(exactly 1) | Unix epoch time in milliseconds according to the operating system's clock (not the node adjusted time)
→<br>`uploadtarget` | string : <br>object | Required<br>(exactly 1) | The upload target information
→ →<br>`timeframe` | number (int) | Required<br>(exactly 1) | Length of the measuring timeframe in seconds (currently set to `24` hours)
→ →<br>`target` | number (int) | Required<br>(exactly 1) | The maximum allowed outbound traffic in bytes (default is `0`).  Can be changed with `-maxuploadtarget`
→ →<br>`target_reached` | bool | Required<br>(exactly 1) | Indicates if the target is reached.  If the target is reached the node won't serve SPV and historical block requests anymore  
→ →<br>`serve_historical_blocks` | bool | Required<br>(exactly 1) | Indicates if historical blocks are served  
→ →<br>`bytes_left_in_cycle` | number (int) | Required<br>(exactly 1) | Amount of bytes left in current time cycle.  `0` is displayed if no upload target is set  
→ →<br>`time_left_in_cycle` | number (int) | Required<br>(exactly 1) | Seconds left in current time cycle.  `0` is displayed if no upload target is set    

*Example from Dash Core 0.12.2*

``` bash
dash-cli getnettotals
```

Result:

``` json
{
  "totalbytesrecv": 4661588,
  "totalbytessent": 2899423,
  "timemillis": 1507815162756,
  "uploadtarget": {
    "timeframe": 86400,
    "target": 0,
    "target_reached": false,
    "serve_historical_blocks": true,
    "bytes_left_in_cycle": 0,
    "time_left_in_cycle": 0
  }
}
```

*See also*

* [GetNetworkInfo](/docs/core-api-ref-remote-procedure-calls-network#section-getnetworkinfo): returns information about the node's connection to the network.
* [GetPeerInfo](/docs/core-api-ref-remote-procedure-calls-network#section-getpeerinfo): returns data about each connected network node.

# GetNetworkInfo

The [`getnetworkinfo` RPC](core-api-ref-remote-procedure-calls-network#section-getnetworkinfo) returns information about the node's connection to the network.

*Parameters: none*

*Result---information about the node's connection to the network*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | Information about this node's connection to the network
→<br>`version` | number | Required<br>(exactly 1) | This node's version of Dash Core in its internal integer format.  For example, Dash Core 0.12.2 has the integer version number 120200
→<br>`subversion` | string | Required<br>(exactly 1) | The user agent this node sends in its [`version` message](core-ref-p2p-network-control-messages#section-version)
→<br>`protocolversion` | number (int) | Required<br>(exactly 1) | The protocol version number used by this node.  See the [protocol versions section](core-ref-p2p-network-protocol-versions) for more information
→<br>`localservices` | string (hex) | Required<br>(exactly 1) | The services supported by this node as advertised in its [`version` message](core-ref-p2p-network-control-messages#section-version)
→<br>`localrelay` | bool | Required<br>(exactly 1) | *Added in Bitcoin Core 0.13.0*<br><br>The services supported by this node as advertised in its [`version` message](core-ref-p2p-network-control-messages#section-version)
→<br>`timeoffset` | number (int) | Required<br>(exactly 1) | The offset of the node's clock from the computer's clock (both in UTC) in seconds.  The offset may be up to 4200 seconds (70 minutes)
→<br>`networkactive` | bool | Required<br>(exactly 1) | Set to `true` if P2P networking is enabled.  Set to `false` if P2P networking is disabled. Enabling/disabling done via [SetNetworkActive](/docs/core-api-ref-remote-procedure-calls-network#section-setnetworkactive)
→<br>`connections` | number (int) | Required<br>(exactly 1) | The total number of open connections (both outgoing and incoming) between this node and other nodes
→<br>`networks` | array | Required<br>(exactly 1) | An array with three objects: one describing the IPv4 connection, one describing the IPv6 connection, and one describing the Tor hidden service (onion) connection
→ →<br>Network | object | Optional<br>(0 to 3) | An object describing a network.  If the network is unroutable, it will not be returned
→ → →<br>`name` | string | Required<br>(exactly 1) | The name of the network.  Either `ipv4`, `ipv6`, or `onion`
→ → →<br>`limited` | bool | Required<br>(exactly 1) | Set to `true` if only connections to this network are allowed according to the `-onlynet` Dash Core command-line/configuration-file parameter.  Otherwise set to `false`
→ → →<br>`reachable` | bool | Required<br>(exactly 1) | Set to `true` if connections can be made to or from this network.  Otherwise set to `false`
→ → →<br>`proxy` | string | Required<br>(exactly 1) | The hostname and port of any proxy being used for this network.  If a proxy is not in use, an empty string
→ → →<br>`proxy_randomize_credentials` | bool | Required<br>(exactly 1) | *Added in Bitcoin Core 0.11.0*<br><br>Set to `true` if randomized credentials are set for this proxy. Otherwise set to `false`
→<br>`relayfee` | number (DASH) | Required<br>(exactly 1) | The minimum relay fee per kilobyte for transactions in order for this node to accept it into its memory pool
→<br>`incrementalfee` | number (DASH) | Required<br>(exactly 1) | *Added in Dash Core 0.12.3*<br><br>The minimum fee increment for mempool limiting or BIP 125 replacement in DASH/kB
→<br>`localaddresses` | array | Required<br>(exactly 1) | An array of objects each describing the local addresses this node believes it listens on
→ →<br>Address | object | Optional<br>(0 or more) | An object describing a particular address this node believes it listens on
→ → →<br>`address` | string | Required<br>(exactly 1) | An IP address or .onion address this node believes it listens on.  This may be manually configured, auto detected, or based on [`version` messages](core-ref-p2p-network-control-messages#section-version) this node received from its peers
→ → →<br>`port` | number (int) | Required<br>(exactly 1) | The port number this node believes it listens on for the associated `address`.  This may be manually configured, auto detected, or based on [`version` messages](core-ref-p2p-network-control-messages#section-version) this node received from its peers
→ → →<br>`score` | number (int) | Required<br>(exactly 1) | The number of incoming connections during the uptime of this node that have used this `address` in their [`version` message](core-ref-p2p-network-control-messages#section-version)
→<br>`warnings` | string | Required<br>(exactly 1) | *Added in Bitcoin Core 0.11.0*<br><br>A plain-text description of any network warnings. If there are no warnings, an empty string will be returned.

*Example from Dash Core 0.12.3*

``` bash
dash-cli getnetworkinfo
```

Result (actual addresses have been replaced with [RFC5737](http://tools.ietf.org/html/rfc5737) reserved addresses):

``` json
{
  "version": 120300,
  "subversion": "/Dash Core:0.12.3/",
  "protocolversion": 70208,
  "localservices": "0000000000000005",
  "localrelay": true,
  "timeoffset": 0,
  "networkactive": true,
  "connections": 9,
  "networks": [
    {
      "name": "ipv4",
      "limited": false,
      "reachable": true,
      "proxy": "",
      "proxy_randomize_credentials": false
    },
    {
      "name": "ipv6",
      "limited": false,
      "reachable": true,
      "proxy": "",
      "proxy_randomize_credentials": false
    },
    {
      "name": "onion",
      "limited": true,
      "reachable": false,
      "proxy": "",
      "proxy_randomize_credentials": false
    }
  ],
  "relayfee": 0.00001000,
  "incrementalfee": 0.00001000,
  "localaddresses": [
    {
      "address": "192.0.2.113",
      "port": 19999,
      "score": 4
    }
  ],
  "warnings": ""
}

```

*See also*

* [GetPeerInfo](/docs/core-api-ref-remote-procedure-calls-network#section-getpeerinfo): returns data about each connected network node.
* [GetNetTotals](/docs/core-api-ref-remote-procedure-calls-network#section-getnettotals): returns information about network traffic, including bytes in, bytes out, and the current time.
* [SetNetworkActive](/docs/core-api-ref-remote-procedure-calls-network#section-setnetworkactive): disables/enables all P2P network activity.

# GetPeerInfo

The [`getpeerinfo` RPC](core-api-ref-remote-procedure-calls-network#section-getpeerinfo) returns data about each connected network node.

*Parameters: none*

*Result---information about each currently-connected network node*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | An array of objects each describing one connected node.  If there are no connections, the array will be empty
→<br>Node | object | Optional<br>(0 or more) | An object describing a particular connected node
→ →<br>`id` | number (int) | Required<br>(exactly 1) | The node's index number in the local node address database
→ →<br>`addr` | string | Required<br>(exactly 1) | The IP address and port number used for the connection to the remote node
→ →<br>`addrlocal` | string | Optional<br>(0 or 1) | Our IP address and port number according to the remote node.  May be incorrect due to error or lying.  Most SPV nodes set this to `127.0.0.1:9999`
→ →<br>`addrbind` | string | Optional<br>(0 or 1) | Bind address of the connection to the peer
→ →<br>`services` | string (hex) | Required<br>(exactly 1) | The services advertised by the remote node in its [`version` message](core-ref-p2p-network-control-messages#section-version)
→ →<br>`lastsend` | number (int) | Required<br>(exactly 1) | The Unix epoch time when we last successfully sent data to the TCP socket for this node
→ →<br>`lastrecv` | number (int) | Required<br>(exactly 1) | The Unix epoch time when we last received data from this node
→ →<br>`bytessent` | number (int) | Required<br>(exactly 1) | The total number of bytes we've sent to this node
→ →<br>`bytesrecv` | number (int) | Required<br>(exactly 1) | The total number of bytes we've received from this node
→ →<br>`conntime` | number (int) | Required<br>(exactly 1) | The Unix epoch time when we connected to this node
→ →<br>`timeoffset` | number (int) | Required<br>(exactly 1) | *Added in Bitcoin Core 0.12.0*<br><br>The time offset in seconds
→ →<br>`pingtime` | number (real) | Required<br>(exactly 1) | The number of seconds this node took to respond to our last P2P [`ping` message](core-ref-p2p-network-control-messages#section-ping)
→ →<br>`minping` | number (real) | Optional<br>(0 or 1) | *Updated in Bitcoin Core 0.13.0*<br><br>The minimum observed ping time (if any at all)
→ →<br>`pingwait` | number (real) | Optional<br>(0 or 1) | The number of seconds we've been waiting for this node to respond to a P2P [`ping` message](core-ref-p2p-network-control-messages#section-ping).  Only shown if there's an outstanding [`ping` message](core-ref-p2p-network-control-messages#section-ping)
→ →<br>`version` | number (int) | Required<br>(exactly 1) | The protocol version number used by this node.  See the [protocol versions section](core-ref-p2p-network-protocol-versions) for more information
→ →<br>`subver` | string | Required<br>(exactly 1) | The user agent this node sends in its [`version` message](core-ref-p2p-network-control-messages#section-version).  This string will have been sanitized to prevent corrupting the JSON results.  May be an empty string
→ →<br>`inbound` | bool | Required<br>(exactly 1) | Set to `true` if this node connected to us (inbound); set to `false` if we connected to this node (outbound)
→ →<br>`addnode` | bool | Required<br>(exactly 1) | Set to `true` if this node was added via the [`addnode` RPC](core-api-ref-remote-procedure-calls-network#section-addnode).
→ →<br>`startingheight` | number (int) | Required<br>(exactly 1) | The height of the remote node's block chain when it connected to us as reported in its [`version` message](core-ref-p2p-network-control-messages#section-version)
→ →<br>`banscore` | number (int) | Required<br>(exactly 1) | The ban score we've assigned the node based on any misbehavior it's made.  By default, Dash Core disconnects when the ban score reaches `100`
→ →<br>`synced_headers` | number (int) | Required<br>(exactly 1) | The highest-height header we have in common with this node based the last P2P [`headers` message](core-ref-p2p-network-data-messages#section-headers) it sent us.  If a [`headers` message](core-ref-p2p-network-data-messages#section-headers) has not been received, this will be set to `-1`
→ →<br>`synced_blocks` | number (int) | Required<br>(exactly 1) | The highest-height block we have in common with this node based on P2P [`inv` messages](core-ref-p2p-network-data-messages#section-inv) this node sent us.  If no block [`inv` messages](core-ref-p2p-network-data-messages#section-inv) have been received from this node, this will be set to `-1`
→ →<br>`inflight` | array | Required<br>(exactly 1) | An array of blocks which have been requested from this peer.  May be empty
→ → →<br>Blocks | number (int) | Optional<br>(0 or more) | The height of a block being requested from the remote peer
→ →<br>`whitelisted` | bool | Required<br>(exactly 1) | Set to `true` if the remote peer has been whitelisted; otherwise, set to `false`.  Whitelisted peers will not be banned if their ban score exceeds the maximum (100 by default).  By default, peers connecting from localhost are whitelisted
→ →<br>`bytessent_per_msg` | string : <br>object | Required<br>(exactly 1) | *Added in Bitcoin Core 0.13.0*<br><br>Information about total sent bytes aggregated by message type
→ → →<br>Message Type | number (int) | Required<br>(1 or more) | Total sent bytes aggregated by message type. One field for every used message type
→ →<br>`bytesrecv_per_msg` | string : <br>object | Required<br>(exactly 1) | *Added in Bitcoin Core 0.13.0*<br><br>Information about total received bytes aggregated by message type
→ → →<br>Message Type | number (int) | Required<br>(1 or more) | Total received bytes aggregated by message type. One field for every used message type

*Example from Dash Core 0.12.2*

``` bash
dash-cli getpeerinfo
```

Result (edited to show only a single entry, with IP addresses changed to
[RFC5737](http://tools.ietf.org/html/rfc5737) reserved IP addresses):

``` json
[
  {
    "id": 3,
    "addr": "192.0.2.113:19999",
    "addrlocal": "127.0.0.1:56332",
    "addrbind": "192.168.10.111:56332",
    "services": "0000000000000005",
    "relaytxes": true,
    "lastsend": 1507818327,
    "lastrecv": 1507818327,
    "bytessent": 844135,
    "bytesrecv": 887651,
    "conntime": 1507808575,
    "timeoffset": 0,
    "pingtime": 0.113646,
    "minping": 0.106826,
    "version": 70215,
    "subver": "/Dash Core:0.14.0.3/",
    "inbound": false,
    "addnode": false,
    "startingheight": 159292,
    "banscore": 0,
    "synced_headers": 159350,
    "synced_blocks": 159350,
    "inflight": [
    ],
    "whitelisted": false,
    "bytessent_per_msg": {
      "addr": 55,
      "dsq": 69530,
      "getaddr": 24,
      "getdata": 9224,
      "getheaders": 989,
      "getsporks": 24,
      "govsync": 66,
      "headers": 6148,
      "inv": 837,
      "ping": 1856,
      "pong": 1856,
      "qsendrecsigs": 25,
      "sendcmpct": 33,
      "senddsq": 25,
      "sendheaders": 24,
      "tx": 5808,
      "verack": 24,
      "version": 160
    },
    "bytesrecv_per_msg": {
      "*other*": 25,
      "addr": 17362,
      "clsig": 3276,
      "getdata": 122,
      "getheaders": 989,
      "headers": 5300,
      "inv": 24611,
      "islock": 3249,
      "mnauth": 152,
      "ping": 1856,
      "pong": 1856,
      "qsigrec": 16492,
      "sendcmpct": 33,
      "senddsq": 25,
      "sendheaders": 24,
      "spork": 2090,
      "ssc": 32,
      "tx": 11523,
      "verack": 24,
      "version": 162
    }
  }
]
```

*See also*

* [GetAddedNodeInfo](/docs/core-api-ref-remote-procedure-calls-network#section-getaddednodeinfo): returns information about the given added node, or all added nodes (except onetry nodes). Only nodes which have been manually added using the [`addnode` RPC](core-api-ref-remote-procedure-calls-network#section-addnode) will have their information displayed.
* [GetNetTotals](/docs/core-api-ref-remote-procedure-calls-network#section-getnettotals): returns information about network traffic, including bytes in, bytes out, and the current time.
* [GetNetworkInfo](/docs/core-api-ref-remote-procedure-calls-network#section-getnetworkinfo): returns information about the node's connection to the network.

# ListBanned

*Added in Bitcoin Core 0.12.0*

The [`listbanned` RPC](core-api-ref-remote-procedure-calls-network#section-listbanned) lists all banned IPs/Subnets.

*Parameters: none*

*Result---information about each banned IP/Subnet*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | An array of objects each describing one entry. If there are no entries in the ban list, the array will be empty
→<br>Node | object | Optional<br>(0 or more) | A ban list entry
→ →<br>`address` | string | Required<br>(exactly 1) | The IP/Subnet of the entry
→ →<br>`banned_until` | number<br>(int) | Required<br>(exactly 1) | The Unix epoch time when the entry was added to the ban list
→ →<br>`ban_created` | number<br>(int) | Required<br>(exactly 1) | The Unix epoch time until the IP/Subnet is banned
→ →<br>`ban_reason` | string | Required<br>(exactly 1) | Set to one of the following reasons:<br>• `node misbehaving` if the node was banned by the client because of DoS violations<br>• `manually added` if the node was manually banned by the user

*Examples from Dash Core 0.12.2*

The default (`false`):

``` bash
dash-cli listbanned
```

Result:

``` json
[
  {
    "address": "192.0.2.201/32",
    "banned_until": 1507906175,
    "ban_created": 1507819775,
    "ban_reason": "node misbehaving"
  },
  {
    "address": "192.0.2.101/32",
    "banned_until": 1507906199,
    "ban_created": 1507819799,
    "ban_reason": "manually added"
  }
]
```

*See also*

* [SetBan](/docs/core-api-ref-remote-procedure-calls-network#section-setban): attempts add or remove a IP/Subnet from the banned list.
* [ClearBanned](/docs/core-api-ref-remote-procedure-calls-network#section-clearbanned): clears list of banned nodes.

# Ping

The [`ping` RPC](core-api-ref-remote-procedure-calls-network#section-ping) sends a P2P ping message to all connected nodes to measure ping time. Results are provided by the [`getpeerinfo` RPC](core-api-ref-remote-procedure-calls-network#section-getpeerinfo) pingtime and pingwait fields as decimal seconds. The P2P [`ping` message](core-ref-p2p-network-control-messages#section-ping) is handled in a queue with all other commands, so it measures processing backlog, not just network ping.

*Parameters: none*

*Result---`null`*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required | Always JSON `null`

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet ping
```

(Success: no result printed.)

Get the results using the [`getpeerinfo` RPC](core-api-ref-remote-procedure-calls-network#section-getpeerinfo):

``` bash
dash-cli -testnet getpeerinfo | grep ping
```

Results:

``` json
        "pingtime" : 0.11790800,
        "pingtime" : 0.22673400,
        "pingtime" : 0.16451900,
        "pingtime" : 0.12465200,
        "pingtime" : 0.13267900,
        "pingtime" : 0.23983300,
        "pingtime" : 0.16764700,
        "pingtime" : 0.11337300,
```

*See also*

* [GetPeerInfo](/docs/core-api-ref-remote-procedure-calls-network#section-getpeerinfo): returns data about each connected network node.
* [P2P Ping Message](core-ref-p2p-network-control-messages#section-ping)

# SetBan

*Added in Bitcoin Core 0.12.0*

The [`setban` RPC](core-api-ref-remote-procedure-calls-network#section-setban) attempts add or remove a IP/Subnet from the banned list.

*Parameter #1---IP/Subnet of the node*

Name | Type | Presence | Description
--- | --- | --- | ---
IP(/Netmask) | string | Required<br>(exactly 1) | The node to add or remove as a string in the form of `<IP address>`.  The IP address may be a hostname resolvable through DNS, an IPv4 address, an IPv4-as-IPv6 address, or an IPv6 address

*Parameter #2---whether to add or remove the node*

Name | Type | Presence | Description
--- | --- | --- | ---
Command | string | Required<br>(exactly 1) | What to do with the IP/Subnet address above.  Options are:<br>• `add` to add a node to the addnode list<br>• `remove` to remove a node from the list.  If currently connected, this will disconnect immediately

*Parameter #3---time how long the ip is banned*

Name | Type | Presence | Description
--- | --- | --- | ---
Bantime | numeric<br>(int) | Optional<br>(0 or 1) | Time in seconds how long (or until when if `absolute` is set) the entry is banned. The default is 24h which can also be overwritten by the -bantime startup argument

*Parameter #4---whether a relative or absolute timestamp*

Name | Type | Presence | Description
--- | --- | --- | ---
Absolute | bool | Optional<br>(0 or 1) | If set, the bantime must be a absolute timestamp in seconds since epoch (Jan 1 1970 GMT)

*Result---`null` on success*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | Always JSON `null`

*Example from Dash Core 0.12.2*

Ban the following node.

``` bash
dash-cli -testnet setban 192.0.2.113 add 2592000
```

Result (no output from `dash-cli` because result is set to `null`).

*See also*

* [ListBanned](/docs/core-api-ref-remote-procedure-calls-network#section-listbanned): lists all banned IPs/Subnets.
* [ClearBanned](/docs/core-api-ref-remote-procedure-calls-network#section-clearbanned): clears list of banned nodes.

# SetNetworkActive

*Added in Bitcoin Core 0.14.0*

The [`setnetworkactive` RPC](core-api-ref-remote-procedure-calls-network#section-setnetworkactive) disables/enables all P2P network activity.

*Parameter #1---whether to disable or enable all P2P network activity*

Name | Type | Presence | Description
--- | --- | --- | ---
Activate | bool | Required<br>(exactly 1) | Set to `true` to enable all P2P network activity. Set to `false` to disable all P2P network activity

*Result---`null` or error on failure*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | null | Required<br>(exactly 1) | JSON `null`.  The JSON-RPC error field will be set only if you entered an invalid parameter

*Example from Dash Core 0.12.2*

``` bash
dash-cli setnetworkactive true
```

Result (no output from `dash-cli` because result is set to `null`).

*See also*

* [GetNetworkInfo](/docs/core-api-ref-remote-procedure-calls-network#section-getnetworkinfo): returns information about the node's connection to the network.