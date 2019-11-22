# GetGovernanceInfo

The [`getgovernanceinfo` RPC](core-api-ref-remote-procedure-calls-dash#section-getgovernanceinfo) returns an object containing governance parameters.

*Parameters: none*

*Result---information about the governance system*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | Information about the governance system
→<br>`governanceminquorum` | number (int) | Required<br>(exactly 1) | The absolute minimum number of votes needed to trigger a governance action
→<br>`proposalfee` | number (int) | Required<br>(exactly 1) | The collateral transaction fee which must be paid to create a proposal in Dash
→<br>`superblockcycle` | number (int) | Required<br>(exactly 1) | The number of blocks between superblocks
→<br>`lastsuperblock` | number (int) | Required<br>(exactly 1) | The block number of the last superblock
→<br>`nextsuperblock` | number (int) | Required<br>(exactly 1) | The block number of the next superblock

*Example from Dash Core 0.14.0*

``` bash
dash-cli -testnet getgovernanceinfo
```

Result:
``` json
{
  "governanceminquorum": 1,
  "proposalfee": 5.00000000,
  "superblockcycle": 24,
  "lastsuperblock": 250824,
  "nextsuperblock": 250848
}
```

*See also:*

* [GObject](/docs/core-api-ref-remote-procedure-calls-dash#section-gobject): provides a set of commands for managing governance objects and displaying information about them.

# GetPoolInfo

The [`getpoolinfo` RPC](core-api-ref-remote-procedure-calls-dash#section-getpoolinfo) returns an object containing mixing pool related information.

*Parameters: none*

*Result---information about the mixing pool*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | Information about the mixing pool
→<br>`state` | string | Required<br>(exactly 1) | Mixing pool state.  Will be one of the following:<br>• `IDLE` <br>• `QUEUE` <br>• `ACCEPTING_ENTRIES` <br>• `SIGNING` <br>• `ERROR` <br>• `SUCCESS` <br>• `UNKNOWN` <br>
→<br>`mixing_mode` | string | Required<br>(exactly 1) | Mixing mode - will be one of the following:<br>• `normal` <br>• `multi-session` <br>
→<br>`queue` | number (int) | Required<br>(exactly 1) | Queue size
→<br>`entries` | number (int) | Required<br>(exactly 1) | The number of entries
→<br>`status` | string | Required<br>(exactly 1) | A more detailed description of the current state
→<br>`outpoint` | string (hex) | Optional<br>(exactly 1) | Previous output
→<br>`addr` | string | Optional<br>(exactly 1) | Address
→<br>`keys_left` | number (int) | Optional<br>(exactly 1) | The number of keys left in the local wallet
→<br>`warnings` | number (int) | Optional<br>(exactly 1) | Warnings related to local wallet

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet getpoolinfo
```

Result:
``` json
{
  "state": "IDLE",
  "mixing_mode": "normal",
  "queue": 0,
  "entries": 0,
  "status": "PrivateSend is idle.",
  "keys_left": 617,
  "warnings": ""
}
```

``` json
{
  "state": "QUEUE",
  "mixing_mode": "normal",
  "queue": 1,
  "entries": 0,
  "status": "Submitted to masternode, waiting in queue .",
  "outpoint": "e3a6b7878a7e9413898bb379b323c521676f9d460db17ec3bf42d9ac0c9a432f-1",
  "addr": "217.182.229.146:19999",
  "keys_left": 571,
  "warnings": ""
}
```

``` json
{
  "state": "ERROR",
  "mixing_mode": "normal",
  "queue": 0,
  "entries": 0,
  "status": "PrivateSend request incomplete: Session timed out. Will retry...",
  "keys_left": 571,
  "warnings": ""
}
```

*See also:*

# GetPrivateSendInfo

The [`getprivatesendinfo` RPC](core-api-ref-remote-procedure-calls-dash#section-getprivatesendinfo) returns an object containing an information about PrivateSend settings and state.

*Parameters: none*

*Result---(for regular nodes) information about the mixing pool*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | Information about the mixing pool
→<br>`enabled` | bool | Required<br>(exactly 1) | Whether mixing functionality is enabled
→<br>`running` | bool | Required<br>(exactly 1) | Whether mixing is currently running
→<br>`multisession` | bool | Required<br>(exactly 1) | Whether PrivateSend Multisession option is enabled
→<br>`max_sessions` | number (int) | Required<br>(exactly 1) | How many parallel mixing sessions can there be at once
→<br>`max_rounds` | number (int) | Required<br>(exactly 1) | How many rounds to mix
→<br>`max_amount` | number (int) | Required<br>(exactly 1) | How many DASH to keep anonymized
→<br>`max_denoms` | number (int) | Required<br>(exactly 1) | How many inputs of each denominated amount to create
→<br>`queue_size` | number (int) | Required<br>(exactly 1) | How many queues there are currently on the network
→<br>`sessions` | array of json objects | Required<br>(exactly 1) | Information about session(s)
→ →<br>Session | object | Optional<br>(1 or more) | Information for a session
→ → →<br>`protxhash` | string | Required<br>(exactly 1) | The ProTxHash of the masternode
→ → →<br>`outpoint` | string (txid-index) | Required<br>(exactly 1) | The outpoint of the masternode
→ → →<br>`service` | string (host:port) | Required<br>(exactly 1) | The IP address and port of the masternode
→ → →<br>`denomination` | number (int) | Required<br>(exactly 1) | The denomination of the mixing session in DASH
→ → →<br>`state` | string | Required<br>(exactly 1) | Current state of the mixing session
→ → →<br>`entries_count` | number (int) | Required<br>(exactly 1) | The number of entries in the mixing session
→<br>`keys_left` | number (int) | Required<br>(exactly 1) | How many new keys are left since last automatic backup
→<br>`warnings` | string | Optional<br>(exactly 1) | Any warnings

*Result---(for masternodes) information about the mixing pool*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | Information about the mixing pool
→<br>`queue_size` | number (int) | Required<br>(exactly 1) | How many queues there are currently on the network
→<br>`denomination` | number (int) | Required<br>(exactly 1) | The denomination of the mixing session in DASH
→<br>`state` | string | Required<br>(exactly 1) | Current state of the mixing session
→<br>`entries_count` | number (int) | Required<br>(exactly 1) | The number of entries in the mixing session

*Example from Dash Core 0.15.0 (regular node)*

``` bash
dash-cli -testnet getprivatesendinfo
```

Result:
``` json
{
  "enabled": true,
  "running": true,
  "multisession": true,
  "max_sessions": 4,
  "max_rounds": 4,
  "max_amount": 2000,
  "max_denoms": 300,
  "queue_size": 2,
  "sessions": [
    {
      "denomination": 0.00000000,
      "state": "ERROR",
      "entries_count": 0
    },
    {
      "protxhash": "7d336336b7e8910f518b2b270c6d72a2d7fc05aec3c6720108da80805ffc3aab",
      "outpoint": "7d336336b7e8910f518b2b270c6d72a2d7fc05aec3c6720108da80805ffc3aab-1",
      "service": "34.241.93.160:26039",
      "denomination": 0.10000100,
      "state": "QUEUE",
      "entries_count": 0
    },
    {
      "protxhash": "11eabc1e72394af02bbe86815975d054816fe69006fdc64c6d7a06b585e5c311",
      "outpoint": "ee7741bac62cb468c09c00e7a78148064db9da781d183a8f23c7beef9ed569d6-0",
      "service": "95.183.53.17:10004",
      "denomination": 10.00010000,
      "state": "QUEUE",
      "entries_count": 0
    }
  ],
  "keys_left": 996,
  "warnings": ""
}
```

*See also: none*

# GetSuperblockBudget

The [`getsuperblockbudget` RPC](core-api-ref-remote-procedure-calls-dash#section-getsuperblockbudget) returns the absolute maximum sum of superblock payments allowed.

*Parameter #1---block index*

Name | Type | Presence | Description
--- | --- | --- | ---
index | number (int) | Required<br>(exactly 1) | The superblock index

*Result---maximum sum of superblock payments*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | number (int) | Required<br>(exactly 1) | The absolute maximum sum of superblock payments allowed, in DASH

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet getsuperblockbudget 7392
```

Result:
``` text
367.20
```

*See also:*

* [GetGovernanceInfo](/docs/core-api-ref-remote-procedure-calls-dash#section-getgovernanceinfo): returns an object containing governance parameters.

# GObject

The [`gobject` RPC](core-api-ref-remote-procedure-calls-dash#section-gobject) provides a set of commands for managing governance objects and displaying information about them.

## GObject Check

The `gobject check` RPC validates governance object data (_proposals only_).

*Parameter #1---object data (hex)*

Name | Type | Presence | Description
--- | --- | --- | ---
`data-hex` | string (hex) | Required<br>(exactly 1) | The data (hex) of a governance proposal object

*Result---governance object status*

Name | Type | Presence | Description
--- | --- | --- | ---
Result | object | Required<br>(exactly 1) | Object containing status
→<br>`Object Status` | string | Required<br>(exactly 1) | Status of the governance object

*Example from Dash Core 0.14.0*

``` bash
dash-cli -testnet gobject check 7b22656e645f65706f6368223a3135363034353730\
35352c226e616d65223a2274657374222c227061796d656e745f61646472657373223a22796\
4354b4d52457333474c4d65366d544a597233597248316a75774e777246436642222c227061\
796d656e745f616d6f756e74223a352c2273746172745f65706f6368223a313536303435333\
439302c2274797065223a312c2275726c223a22687474703a2f2f746573742e636f6d227d
```

Result:
``` json
{
  "Object status": "OK"
}
```

## GObject Prepare

The `gobject prepare` RPC prepares a governance object by signing and creating a collateral transaction.

*Parameter #1---parent hash*

Name | Type | Presence | Description
--- | --- | --- | ---
`parent-hash` | string (hex) | Required<br>(exactly 1) | Hash of the parent object. Usually the root node which has a hash of 0

*Parameter #2---revision*

Name | Type | Presence | Description
--- | --- | --- | ---
`revision` | int | Required<br>(exactly 1) | Object revision number

*Parameter #3---time*

Name | Type | Presence | Description
--- | --- | --- | ---
`time` | int64_t | Required<br>(exactly 1) | Create time (Unix epoch time)

*Parameter #4---data*

Name | Type | Presence | Description
--- | --- | --- | ---
`data-hex` | string (hex) | Required<br>(exactly 1) | **Updated in Dash Core 0.14.0 to require all new proposals to use JSON serialization.**<br><br>Object data (JSON object with governance details). Additional details regarding this are provided in an example below.

*Parameter #5---use-IS*

Name | Type | Presence | Description
--- | --- | --- | ---
`use-IS` | boolean | Optional<br>(0 or 1) | *Deprecated and ignored since Dash Core 0.15.0*

*Parameter #6---outputHash*

Name | Type | Presence | Description
--- | --- | --- | ---
`outputHash` | string (hex) | Optional<br>(0 or 1) | *Added in Dash Core 0.13.0*<br><br>The single output to submit the proposal fee from

*Parameter #7---outputIndex*

Name | Type | Presence | Description
--- | --- | --- | ---
`outputIndex` | numeric | Optional<br>(0 or 1) | *Added in Dash Core 0.13.0*<br><br>The output index (required if the `outputHash` parameter is provided)

*Result---collateral transaction ID*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string (hex) | Required<br>(exactly 1) | Transaction ID for the collateral transaction

**Details of the `data-hex` field:**

The `data-hex` field is comprised of a JSON object as described in [GObject
Deserialize](#gobject-deserialize) which is serialized to hex.

An example of a proposal JSON object is shown here:

``` json
{
  "end_epoch": 1560457055,
  "name": "test",
  "payment_address": "yd5KMREs3GLMe6mTJYr3YrH1juwNwrFCfB",
  "payment_amount": 5,
  "start_epoch": 1560453490,
  "type": 1,
  "url": "http://test.com"
}
```

To serialize the object, first remove all spaces from the JSON object as shown below:

``` json
{"end_epoch":1560457055,"name":"test","payment_address":"yd5KMREs3GLMe6mTJYr3YrH1juwNwrFCfB","payment_amount":5,"start_epoch":1560453490,"type":1,"url":"http://test.com"}
```

Then convert the string to its hex equivalent as shown below. This is what will
be used for the `data-hex` field of the `gobject prepare` command:

``` bash
7b22656e645f65706f6368223a313536303435373035352c226e616d65223a2274657374222c\
227061796d656e745f61646472657373223a227964354b4d52457333474c4d65366d544a5972\
33597248316a75774e777246436642222c227061796d656e745f616d6f756e74223a352c2273\
746172745f65706f6368223a313536303435333439302c2274797065223a312c2275726c223a\
22687474703a2f2f746573742e636f6d227d
```

*Example from Dash Core 0.14.0*

``` bash
gobject prepare 0 1 1560449223 7b22656e645f65706f6368223a3135363034353730353\
52c226e616d65223a2274657374222c227061796d656e745f61646472657373223a227964354\
b4d52457333474c4d65366d544a597233597248316a75774e777246436642222c227061796d6\
56e745f616d6f756e74223a352c2273746172745f65706f6368223a313536303435333439302\
c2274797065223a312c2275726c223a22687474703a2f2f746573742e636f6d227d
```

Result (Collateral Transaction ID):
``` bash
3fd758e7a5761bb07b2850b8ba432ef42c1ea80f0921d2eab0682697dda78262
```

## GObject Submit

The `gobject submit` RPC submits a governance object to network (objects must
first be prepared via `gobject prepare`).

Note: Parameters 1-4 should be the same values as the ones used for `gobject
prepare`.

*Parameter #1---parent hash*

Name | Type | Presence | Description
--- | --- | --- | ---
`parent-hash` | string (hex) | Required<br>(exactly 1) | Hash of the parent object. Usually the root node which has a hash of 0

*Parameter #2---revision*

Name | Type | Presence | Description
--- | --- | --- | ---
`revision` | int | Required<br>(exactly 1) | Object revision number

*Parameter #3---time*

Name | Type | Presence | Description
--- | --- | --- | ---
`time` | int64_t | Required<br>(exactly 1) | Create time

*Parameter #4---data*

Name | Type | Presence | Description
--- | --- | --- | ---
`data-hex` | string (hex) | Required<br>(exactly 1) | **Updated in Dash Core 0.14.0 to require all new proposals to use JSON serialization.**<br><br>Object data (JSON object with governance details). See [GObject Prepare](#gobject-prepare) for additional details about this field.

*Parameter #5---fee transaction ID*

Name | Type | Presence | Description
--- | --- | --- | ---
`data` | string (hex) | Required<br>(exactly 1) | Fee transaction ID - required for all objects except triggers

*Result---governance object hash*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string (hex) | Required<br>(exactly 1) | Governance object hash

*Example from Dash Core 0.14.0*

``` bash
dash-cli -testnet gobject submit 0 1 1560449223 7b22656e645f65706f6368223a3\
13536303435373035352c226e616d65223a2274657374222c227061796d656e745f61646472\
657373223a227964354b4d52457333474c4d65366d544a597233597248316a75774e7772464\
36642222c227061796d656e745f616d6f756e74223a352c2273746172745f65706f6368223a\
313536303435333439302c2274797065223a312c2275726c223a22687474703a2f2f7465737\
42e636f6d227d \
3fd758e7a5761bb07b2850b8ba432ef42c1ea80f0921d2eab0682697dda78262
```

Result (Governance Object Hash):
``` bash
e353b2ab5f7e7cb24b95e00e153ec2a6339249672f18b8e8e144aa711678710d
```

## GObject Deserialize

The `gobject deserialize` RPC deserializes a governance object from a hex string to JSON.

*Parameter #1---object data (hex)*

Name | Type | Presence | Description
--- | --- | --- | ---
`hex_data` | string (hex) | Required<br>(exactly 1) | The data (hex) of a governance object

**Results**

The result output varies depending on the type of governance object being
deserialized. Examples are shown below for both proposal and trigger object types.

**Result - Proposal**

*Result---governance proposal object deserialized to JSON*

Name | Type | Presence | Description
--- | --- | --- | ---
Result | object | Required<br>(exactly 1) | Array of governance objects
→ →<br>`end_epoch` | string | Required<br>(exactly 1) | Governance object info as string
→ →<br>`name` | string (hex) | Required<br>(exactly 1) | Proposal name
→ →<br>`payment_address` | string (hex) | Required<br>(exactly 1) | Proposal payment address
→ →<br>`payment_amount` | string | Required<br>(exactly 1) | Proposal payment amount
→ →<br>`start_epoch` | string (hex) | Required<br>(exactly 1) | Proposal start
→ →<br>`type` | int | Required<br>(exactly 1) | Object type
→ →<br>`url` | string | Required<br>(exactly 1) | Proposal URL

*Example from Dash Core 0.14.0*

``` bash
dash-cli -testnet gobject deserialize 7b22656e645f65706f6368223a313536303435\
373035352c226e616d65223a2274657374222c227061796d656e745f61646472657373223a22\
7964354b4d52457333474c4d65366d544a597233597248316a75774e777246436642222c2270\
61796d656e745f616d6f756e74223a352c2273746172745f65706f6368223a31353630343533\
3439302c2274797065223a312c2275726c223a22687474703a2f2f746573742e636f6d227d
```

Result:
``` json
{
  "end_epoch": 1560457055,
  "name": "test",
  "payment_address": "yd5KMREs3GLMe6mTJYr3YrH1juwNwrFCfB",
  "payment_amount": 5,
  "start_epoch": 1560453490,
  "type": 1,
  "url": "http://test.com"
}
```

**Result - Trigger**

*Result---governance trigger object deserialized to JSON*

Name | Type | Presence | Description
--- | --- | --- | ---
Result | object | Required<br>(exactly 1) | Array of governance objects
→ →<br>`event_block_height` | int | Required<br>(exactly 1) | Block height to activate trigger
→ →<br>`payment_addresses` | string (hex) | Required<br>(exactly 1) | Proposal payment address
→ →<br>`payment_amounts` | string | Required<br>(exactly 1) | Proposal payment amount
→ →<br>`proposal_hashes` | string (hex) | Required<br>(exactly 1) | Proposal hashes
→ →<br>`type` | int | Required<br>(exactly 1) | Object type

*Example from Dash Core 0.14.0*

``` bash
dash-cli -testnet gobject deserialize 7b226576656e745f626c6f636b5f68656967687\
4223a203131393539322c20227061796d656e745f616464726573736573223a20227954686d6e\
75565a316765516e79776f456147627079333362695435473573587a62222c20227061796d656\
e745f616d6f756e7473223a2022312e3335393631393331222c202270726f706f73616c5f6861\
73686573223a20223836333966636464653131626432373032373663396330333564366435346\
3653962393138323465366466373532636164376464646331616532663734386435222c202274\
797065223a20327d
```

Result (wrapped):
``` json
{
  "event_block_height": 119592,
  "payment_addresses": "yThmnuVZ1geQnywoEaGbpy33biT5G5sXzb",
  "payment_amounts": "1.35961931",
  "proposal_hashes": "8639fcdde11bd270276c9c035d6d54ce9b91824e6df752cad7dddc1ae2f748d5",
  "type": 2
}
```

## GObject Count

The `gobject count` RPC returns the count of governance objects and votes.

*Parameter #1---mode*

Name | Type | Presence | Description
--- | --- | --- | ---
`mode` | string | Optional<br>(exactly 1) | Result return format:<br>`json` (default)<br>`all` - Default before Dash Core 0.12.3 (for backwards compatibility)

**Command Mode - `json`**

*Result---count of governance objects and votes*

Name | Type | Presence | Description
--- | --- | --- | ---
Result | object | Required<br>(exactly 1) | Information about the governance object
→<br>`objects_total` | int | Required<br>(exactly 1) | Total object count
→<br>`proposals` | int | Required<br>(exactly 1) | Proposal count
→<br>`triggers` | int | Required<br>(exactly 1) | Trigger count
→<br>`other` | int | Required<br>(exactly 1) | Non-proposal/trigger count
→<br>`erased` | int | Required<br>(exactly 1) | Erased count
→<br>`votes` | int | Required<br>(exactly 1) | Vote count

*Example from Dash Core 0.14.0 (mode: `json`/default)*

``` bash
dash-cli -testnet gobject count
```

Result (wrapped):
``` json
{
  "objects_total": 3,
  "proposals": 3,
  "triggers": 0,
  "other": 0,
  "erased": 4,
  "votes": 18
}
```

**Command Mode - `all`**

*Result---count of governance objects and votes*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string | Required<br>(exactly 1) | The count of governance objects and votes

*Example from Dash Core 0.14.0 (mode: `all`)*

``` bash
dash-cli -testnet gobject count all
```

Result (wrapped):
``` text
Governance Objects: 177 (Proposals: 177, Triggers: 0, Other: 0; Erased: 5), \
Votes: 9680
```

## GObject Get

The `gobject get` RPC returns a governance object by hash.

*Parameter #1---object hash*

Name | Type | Presence | Description
--- | --- | --- | ---
`governance-hash` | string (hex) | Required<br>(exactly 1) | The hash of a governance object

*Result---governance object details*

Name | Type | Presence | Description
--- | --- | --- | ---
Result | object | Required<br>(exactly 1) | Information about the governance object
→<br>`DataHex` | string (hex) | Required<br>(exactly 1) | Governance object info as hex string
→<br>`DataString` | string | Required<br>(exactly 1) | Governance object info as string
→<br>`Hash` | string (hex) | Required<br>(exactly 1) | Hash of this governance object
→<br>`CollateralHash` | string (hex) | Required<br>(exactly 1) | Hash of the collateral payment transaction
→<br>`ObjectType` | number | Required<br>(exactly 1) | Object types:<br>`1` - Unknown<br>`2` - Proposal<br>`3` - Trigger
→<br>`CreationTime` | number | Required<br>(exactly 1) | Object creation time as Unix epoch time
→<br>`FundingResult` | object | Required<br>(exactly 1) | Funding vote details
→ →<br>`AbsoluteYesCount` | number | Required<br>(exactly 1) | Number of `Yes` votes minus number of `No` votes
→ →<br>`YesCount` | number | Required<br>(exactly 1) | Number of `Yes` votes
→ →<br>`NoCount` | number | Required<br>(exactly 1) | Number of `No` votes
→ →<br>`AbstainCount` | number | Required<br>(exactly 1) | Number of `Abstain` votes
→<br>`ValidResult` | object | Required<br>(exactly 1) | Object validity vote details
→ →<br>`AbsoluteYesCount` | number | Required<br>(exactly 1) | Number of `Yes` votes minus number of `No` votes
→ →<br>`YesCount` | number | Required<br>(exactly 1) | Number of `Yes` votes
→ →<br>`NoCount` | number | Required<br>(exactly 1) | Number of `No` votes
→ →<br>`AbstainCount` | number | Required<br>(exactly 1) | Number of `Abstain` votes
→<br>`DeleteResult` | object | Required<br>(exactly 1) | Delete vote details
→ →<br>`AbsoluteYesCount` | number | Required<br>(exactly 1) | Number of `Yes` votes minus number of `No` votes
→ →<br>`YesCount` | number | Required<br>(exactly 1) | Number of `Yes` votes
→ →<br>`NoCount` | number | Required<br>(exactly 1) | Number of `No` votes
→ →<br>`AbstainCount` | number | Required<br>(exactly 1) | Number of `Abstain` votes
→<br>`EndorsedResult` | object | Required<br>(exactly 1) | Endorsed vote details
→ →<br>`AbsoluteYesCount` | number | Required<br>(exactly 1) | Number of `Yes` votes minus number of `No` votes
→ →<br>`YesCount` | number | Required<br>(exactly 1) | Number of `Yes` votes
→ →<br>`NoCount` | number | Required<br>(exactly 1) | Number of `No` votes
→ →<br>`AbstainCount` | number | Required<br>(exactly 1) | Number of `Abstain` votes
→<br>`fLocalValidity` | boolean | Required<br>(exactly 1) | Valid by the blockchain
→<br>`IsValidReason` | string | Required<br>(exactly 1) | `fLocalValidity` error result. Empty if no error returned.
→<br>`fCachedValid` | boolean | Required<br>(exactly 1) | Minimum network support has been reached flagging this object as a valid and understood governance object (e.g, the serialized data is correct format, etc)
→<br>`fCachedFunding` | boolean | Required<br>(exactly 1) | Minimum network support has been reached for this object to be funded (doesn't mean it will be for sure though)
→<br>`fCachedDelete` | boolean | Required<br>(exactly 1) | Minimum network support has been reached saying this object should be deleted from the system entirely
→<br>`fCachedEndorsed` | boolean | Required<br>(exactly 1) | Minimum network support has been reached flagging this object as endorsed

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet gobject get \
	42253a7bec554b97a65d2889e6cb9a1cf308b3d47a778c704bf9cdc1fe1bf6ff
```

Result (wrapped):
``` json
{
  "DataHex": "5b5b2270726f706f73616c222c7b22656e645f65706f6368223a2231353037343339353130222c226e616d65223a227465737470726f706f73616c5f2d5f6162636465666768696a6b6c6d6e6f707172737475767778797a3031323334353637383931353037323530343338222c227061796d656e745f61646472657373223a22795668577955345933756456784d5234464b3333556741534a41436831436835516a222c227061796d656e745f616d6f756e74223a2232222c2273746172745f65706f6368223a2231353037323530343338222c2274797065223a312c2275726c223a2268747470733a2f2f7777772e6461736863656e7472616c2e6f72672f702f746573745f70726f706f73616c5f31353037323530343338227d5d5d",
  "DataString": "[[\"proposal\",{\"end_epoch\":\"1507439510\",\"name\":\"testproposal_-_abcdefghijklmnopqrstuvwxyz01234567891507250438\",\"payment_address\":\"yVhWyU4Y3udVxMR4FK33UgASJACh1Ch5Qj\",\"payment_amount\":\"2\",\"start_epoch\":\"1507250438\",\"type\":1,\"url\":\"https://www.dashcentral.org/p/test_proposal_1507250438\"}]]",
  "Hash": "42253a7bec554b97a65d2889e6cb9a1cf308b3d47a778c704bf9cdc1fe1bf6ff",
  "CollateralHash": "cb09bd0310c0a67cde9387ad4d8908a7ad9f5d89c5afd58e9332b8bd26a646c7",
  "ObjectType": 1,
  "CreationTime": 1507246694,
  "FundingResult": {
    "AbsoluteYesCount": 0,
    "YesCount": 0,
    "NoCount": 0,
    "AbstainCount": 0
  },
  "ValidResult": {
    "AbsoluteYesCount": 0,
    "YesCount": 0,
    "NoCount": 0,
    "AbstainCount": 0
  },
  "DeleteResult": {
    "AbsoluteYesCount": 31,
    "YesCount": 31,
    "NoCount": 0,
    "AbstainCount": 0
  },
  "EndorsedResult": {
    "AbsoluteYesCount": 0,
    "YesCount": 0,
    "NoCount": 0,
    "AbstainCount": 0
  },
  "fLocalValidity": true,
  "IsValidReason": "",
  "fCachedValid": true,
  "fCachedFunding": false,
  "fCachedDelete": false,
  "fCachedEndorsed": false
}
```

## GObject Getvotes
[block:callout]
{
  "type": "danger",
  "body": "**Warning:** **_Removed in Dash Core 0.14.0._**",
  "title": "Deprecation Warning"
}
[/block]

## GObject Getcurrentvotes

The `gobject getcurrentvotes` RPC gets only current (tallying) votes for a governance object hash (does not include old votes).

*Parameter #1---object hash*

Name | Type | Presence | Description
--- | --- | --- | ---
`governance-hash` | string (hex) | Required<br>(exactly 1) | The hash of a governance object

*Result---votes for specified governance*

Name | Type | Presence | Description
--- | --- | --- | ---
Result | object | Required<br>(exactly 1) | The governance object votes
→<br>Vote Info | string | Required<br>(1 or more) | Key: vote-hash<br><br>Value: vinMasternode, time, outcome, and signal of the vote

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet gobject getcurrentvotes 78941af577f639ac94440e4855a1ed8f\
  696f1506d1c0bed4f4b68f05be26d3ca
```

Result (truncated):
``` json
{
  "174aaba65982d25a23f437e2a66ec3836146ba7b7ce5b3fe2d5476907f7079d9": "2eab488e3a7b030303de0d18e357ce17a9fc6b8876705d61076bbe923b2e5fc8-1:1509354047:YES:DELETE",
  "444d4d871ec35479804f060c733f516908382642ec2dfce6044a59fcadfdcd60": "18e496fe85b61ac9a5fcaec1ef683c7e3fc9bce4a83c883608427ecfb1002fca-1:1508866932:YES:FUNDING",
  "d49a472c62e9d8105931829fc50ef6c6ce04a230507646ee0eaa615e863ef3a0": "18e496fe85b61ac9a5fcaec1ef683c7e3fc9bce4a83c883608427ecfb1002fca-1:1509117071:YES:DELETE",
  "78442507441d4524d2493b8568d130415c1eb394adb2fe38d6ffeb199115bc5d": "3df7fb192e21c34da99bdd10c34e58ecaf3f3c37d6b2289f0ffedba5050188cc-1:1509312524:YES:DELETE",
  "aa4dc9d3b9e74e8c1ffc725b737d07f8a32e43c64907e4bea19e64a86135f08a": "af9f5646ace92f76b3a01b0abe08716a0a7ded64074c2d2e712c93174b9013d1-1:1508866932:YES:FUNDING",
}
```

## GObject List

The `gobject list` RPC Lists governance objects (can be filtered by signal and/or object type).

*Parameter #1---signal*

Name | Type | Presence | Description
--- | --- | --- | ---
`signal` | string (hex) | Optional<br>(exactly 1) | Type of governance object signal: <br>• `valid`<br>• `funding`<br>• `delete`<br>• `endorsed`<br>• `all` (_DEFAULT_)

*Parameter #2---type*

Name | Type | Presence | Description
--- | --- | --- | ---
`type` | string (hex) | Optional<br>(exactly 1) | Type of governance object signal: <br>• `proposals`<br>• `triggers`<br>• `all` (_DEFAULT_)

*Result---governance objects*

Name | Type | Presence | Description
--- | --- | --- | ---
Result | object | Required<br>(exactly 1) | Information about the governance object
→<br>Governance Object(s) | object | Required<br>(1 or more) | Key: Governance object hash<br>Values: Governance object details
→ →<br>`DataHex` | string (hex) | Required<br>(exactly 1) | Governance object info as hex string
→ →<br>`DataString` | string | Required<br>(exactly 1) | Governance object info as string
→ →<br>`Hash` | string (hex) | Required<br>(exactly 1) | Hash of this governance object
→ →<br>`CollateralHash` | string (hex) | Required<br>(exactly 1) | Hash of the collateral payment transaction
→ →<br>`ObjectType` | number | Required<br>(exactly 1) | Object types:<br>`1` - Unknown<br>`2` - Proposal<br>`3` - Trigger
→ →<br>`CreationTime` | number | Required<br>(exactly 1) | Object creation time as Unix epoch time
→ →<br>`SigningMasternode` | string (hex) | Optional<br>(0 or 1) | Signing masternode's vin (only present in triggers)
→ →<br>`AbsoluteYesCount` | number | Required<br>(exactly 1) | Number of `Yes` votes minus number of `No` votes
→ →<br>`YesCount` | number | Required<br>(exactly 1) | Number of `Yes` votes
→ →<br>`NoCount` | number | Required<br>(exactly 1) | Number of `No` votes
→ →<br>`AbstainCount` | number | Required<br>(exactly 1) | Number of `Abstain` votes
→<br>`fLocalValidity` | boolean | Required<br>(exactly 1) | Valid by the blockchain
→<br>`IsValidReason` | string | Required<br>(exactly 1) | `fLocalValidity` error result. Empty if no error returned.
→<br>`fCachedValid` | boolean | Required<br>(exactly 1) | Minimum network support has been reached flagging this object as a valid and understood governance object (e.g, the serialized data is correct format, etc)
→<br>`fCachedFunding` | boolean | Required<br>(exactly 1) | Minimum network support has been reached for this object to be funded (doesn't mean it will be for sure though)
→<br>`fCachedDelete` | boolean | Required<br>(exactly 1) | Minimum network support has been reached saying this object should be deleted from the system entirely
→<br>`fCachedEndorsed` | boolean | Required<br>(exactly 1) | Minimum network support has been reached flagging this object as endorsed

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet gobject list all proposals
```

Result (truncated):
``` json
{
  "b370fa1afd61aca9aa879abea3087e29656a670478f281d4196efb4e7e893ffe": {
    "DataHex": "5b5b2270726f706f73616c222c7b22656e645f65706f6368223a2231353037343430303338222c226e616d65223a227465737470726f706f73616c5f2d5f6162636465666768696a6b6c6d6e6f707172737475767778797a3031323334353637383931353037323530393636222c227061796d656e745f61646472657373223a2279544c636f506d4e315963654432534345474d6b6e34395753565a4277626f646e6e222c227061796d656e745f616d6f756e74223a2232222c2273746172745f65706f6368223a2231353037323530393636222c2274797065223a312c2275726c223a2268747470733a2f2f7777772e6461736863656e7472616c2e6f72672f702f746573745f70726f706f73616c5f31353037323530393636227d5d5d",
    "DataString": "[[\"proposal\",{\"end_epoch\":\"1507440038\",\"name\":\"testproposal_-_abcdefghijklmnopqrstuvwxyz01234567891507250966\",\"payment_address\":\"yTLcoPmN1YceD2SCEGMkn49WSVZBwbodnn\",\"payment_amount\":\"2\",\"start_epoch\":\"1507250966\",\"type\":1,\"url\":\"https://www.dashcentral.org/p/test_proposal_1507250966\"}]]",
    "Hash": "b370fa1afd61aca9aa879abea3087e29656a670478f281d4196efb4e7e893ffe",
    "CollateralHash": "a51ea89c14735f8b5df37cd846b3561494cc616d4a741e4ef83b368d45c960ba",
    "ObjectType": 1,
    "CreationTime": 1507250966,
    "AbsoluteYesCount": 0,
    "YesCount": 0,
    "NoCount": 0,
    "AbstainCount": 0,
    "fBlockchainValidity": true,
    "IsValidReason": "",
    "fCachedValid": true,
    "fCachedFunding": false,
    "fCachedDelete": false,
    "fCachedEndorsed": false
  },
  "906ae4dbd285e1025832ac9b3160073ecbfeef094d34cf81b3d797a349c720ff": {
    "DataHex": "5b5b2270726f706f73616c222c7b22656e645f65706f6368223a2231353037343534383935222c226e616d65223a227465737470726f706f73616c5f2d5f6162636465666768696a6b6c6d6e6f707172737475767778797a3031323334353637383931353037323635383233222c227061796d656e745f61646472657373223a2279664e68484c4c695936577a5a646a51766137324a64395134313468516578514c68222c227061796d656e745f616d6f756e74223a2232222c2273746172745f65706f6368223a2231353037323635383233222c2274797065223a312c2275726c223a2268747470733a2f2f7777772e6461736863656e7472616c2e6f72672f702f746573745f70726f706f73616c5f31353037323635383233227d5d5d",
    "DataString": "[[\"proposal\",{\"end_epoch\":\"1507454895\",\"name\":\"testproposal_-_abcdefghijklmnopqrstuvwxyz01234567891507265823\",\"payment_address\":\"yfNhHLLiY6WzZdjQva72Jd9Q414hQexQLh\",\"payment_amount\":\"2\",\"start_epoch\":\"1507265823\",\"type\":1,\"url\":\"https://www.dashcentral.org/p/test_proposal_1507265823\"}]]",
    "Hash": "906ae4dbd285e1025832ac9b3160073ecbfeef094d34cf81b3d797a349c720ff",
    "CollateralHash": "1707470c4372ba048b72945365b4bb71afc8a986e0755c1f1e8a37bba21fde83",
    "ObjectType": 1,
    "CreationTime": 1507265823,
    "AbsoluteYesCount": 0,
    "YesCount": 0,
    "NoCount": 0,
    "AbstainCount": 0,
    "fBlockchainValidity": true,
    "IsValidReason": "",
    "fCachedValid": true,
    "fCachedFunding": false,
    "fCachedDelete": false,
    "fCachedEndorsed": false
  }
}
```

## GObject Diff

The `gobject diff` RPC Lists governance objects differences since last diff.

*Parameter #1---signal*

Name | Type | Presence | Description
--- | --- | --- | ---
`signal` | string (hex) | Optional<br>(exactly 1) | Type of governance object signal: <br>• `valid`<br>• `funding`<br>• `delete`<br>• `endorsed`<br>• `all` (_DEFAULT_)

*Parameter #2---type*

Name | Type | Presence | Description
--- | --- | --- | ---
`type` | string (hex) | Optional<br>(exactly 1) | Type of governance object signal: <br>• `proposals`<br>• `triggers`<br>• `all` (_DEFAULT_)

*Result---governance objects*

Name | Type | Presence | Description
--- | --- | --- | ---
Result | object | Required<br>(exactly 1) | Information about the governance object
→<br>Governance Object(s) | object | Required<br>(1 or more) | Key: Governance object hash<br>Values: Governance object details
→ →<br>`DataHex` | string (hex) | Required<br>(exactly 1) | Governance object info as hex string
→ →<br>`DataString` | string | Required<br>(exactly 1) | Governance object info as string
→ →<br>`Hash` | string (hex) | Required<br>(exactly 1) | Hash of this governance object
→ →<br>`CollateralHash` | string (hex) | Required<br>(exactly 1) | Hash of the collateral payment transaction
→ →<br>`ObjectType` | number | Required<br>(exactly 1) | Object types:<br>`1` - Unknown<br>`2` - Proposal<br>`3` - Trigger
→ →<br>`CreationTime` | number | Required<br>(exactly 1) | Object creation time as Unix epoch time
→ →<br>`SigningMasternode` | string (hex) | Optional<br>(0 or 1) | Signing masternode's vin (only present in triggers)
→ →<br>`AbsoluteYesCount` | number | Required<br>(exactly 1) | Number of `Yes` votes minus number of `No` votes
→ →<br>`YesCount` | number | Required<br>(exactly 1) | Number of `Yes` votes
→ →<br>`NoCount` | number | Required<br>(exactly 1) | Number of `No` votes
→ →<br>`AbstainCount` | number | Required<br>(exactly 1) | Number of `Abstain` votes
→<br>`fLocalValidity` | boolean | Required<br>(exactly 1) | Valid by the blockchain
→<br>`IsValidReason` | string | Required<br>(exactly 1) | `fLocalValidity` error result. Empty if no error returned.
→<br>`fCachedValid` | boolean | Required<br>(exactly 1) | Minimum network support has been reached flagging this object as a valid and understood governance object (e.g, the serialized data is correct format, etc)
→<br>`fCachedFunding` | boolean | Required<br>(exactly 1) | Minimum network support has been reached for this object to be funded (doesn't mean it will be for sure though)
→<br>`fCachedDelete` | boolean | Required<br>(exactly 1) | Minimum network support has been reached saying this object should be deleted from the system entirely
→<br>`fCachedEndorsed` | boolean | Required<br>(exactly 1) | Minimum network support has been reached flagging this object as endorsed

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet gobject diff all all
```

Result (truncated):
``` json
{
  "17c2bd32005c5168a52f9b5caa74d875ee8a6867a6109f36923887ef6c36b301": {
    "DataHex": "5b5b2270726f706f73616c222c7b22656e645f65706f6368223a2231353037343533353731222c226e616d65223a227465737470726f706f73616c5f2d5f6162636465666768696a6b6c6d6e6f707172737475767778797a3031323334353637383931353037323634343939222c227061796d656e745f61646472657373223a2279697355653636445352487048504233514245426764574746637068435933626234222c227061796d656e745f616d6f756e74223a2232222c2273746172745f65706f6368223a2231353037323634343939222c2274797065223a312c2275726c223a2268747470733a2f2f7777772e6461736863656e7472616c2e6f72672f702f746573745f70726f706f73616c5f31353037323634343939227d5d5d",
    "DataString": "[[\"proposal\",{\"end_epoch\":\"1507453571\",\"name\":\"testproposal\",\"payment_address\":\"yisUe66DSRHpHPB3QBEBgdWGFcphCY3bb4\",\"payment_amount\":\"2\",\"start_epoch\":\"1507264499\",\"type\":1,\"url\":\"https://www.dashcentral.org/p/test_proposal_1507264499\"}]]",
    "Hash": "17c2bd32005c5168a52f9b5caa74d875ee8a6867a6109f36923887ef6c36b301",
    "CollateralHash": "a25c44b57931afd74530ce39741f91456446a8fd794d2f1c58c42d6f492647ad",
    "ObjectType": 1,
    "CreationTime": 1507264499,
    "AbsoluteYesCount": 0,
    "YesCount": 0,
    "NoCount": 0,
    "AbstainCount": 0,
    "fBlockchainValidity": true,
    "IsValidReason": "",
    "fCachedValid": true,
    "fCachedFunding": false,
    "fCachedDelete": false,
    "fCachedEndorsed": false
  }
}
```

## GObject Vote-alias

The `gobject vote-alias` RPC votes on a governance object by masternode alias (using masternode.conf setup).

*Parameter #1---governance hash*

Name | Type | Presence | Description
--- | --- | --- | ---
`governance-hash` | string (hex) | Required<br>(exactly 1) | Hash of the governance object

*Parameter #2---vote signal*

Name | Type | Presence | Description
--- | --- | --- | ---
`signal` | string | Required<br>(exactly 1) | Vote signal: `funding`, `valid`, or `delete`

*Parameter #3---vote outcome*

Name | Type | Presence | Description
--- | --- | --- | ---
`outcome` | string | Required<br>(exactly 1) | Vote outcome: `yes`, `no`, or `abstain`

*Parameter #4---masternode alias*

Name | Type | Presence | Description
--- | --- | --- | ---
`alias` | string | Required<br>(exactly 1) | Alias of voting masternode

*Result---votes for specified governance*

Name | Type | Presence | Description
--- | --- | --- | ---
Result | object | Required<br>(exactly 1) | The governance object votes
→<br>`overall` | string | Required<br>(1 or more) | Reports number of vote successes/failures
→<br>`detail` | object | Required<br>(exactly 1) | Vote details
→ →<br>Masternode Alias | object | Required<br>(1 or more) | Name of the masternode alias
→ → →<br>`result` | string | Required<br>(exactly 1) | Vote result

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet gobject vote-alias \
0bf97bce78b3b642c36d4ca8e9265f8f66de8774c220221f57739c1956413e2b \
funding yes MN01
```

Result:
``` json
{
  "overall": "Voted successfully 1 time(s) and failed 0 time(s).",
  "detail": {
    "MN01": {
      "result": "success"
    }
  }
}
```

## GObject Vote-conf

The `gobject vote-conf` RPC votes on a governance object by masternode configured in dash.conf.

*Parameter #1---governance hash*

Name | Type | Presence | Description
--- | --- | --- | ---
`governance-hash` | string (hex) | Required<br>(exactly 1) | Hash of the governance object

*Parameter #2---vote signal*

Name | Type | Presence | Description
--- | --- | --- | ---
`signal` | string | Required<br>(exactly 1) | Vote signal: `funding`, `valid`, or `delete`

*Parameter #3---vote outcome*

Name | Type | Presence | Description
--- | --- | --- | ---
`outcome` | string | Required<br>(exactly 1) | Vote outcome: `yes`, `no`, or `abstain`

*Result---votes for specified governance*

Name | Type | Presence | Description
--- | --- | --- | ---
Result | object | Required<br>(exactly 1) | The governance object votes
→<br>`overall` | string | Required<br>(1 or more) | Reports number of vote successes/failures
→<br>`detail` | object | Required<br>(exactly 1) | Vote details
→ →<br>`dash.conf` | object | Required<br>(1 or more) |
→ → →<br>`result` | string | Required<br>(exactly 1) | Vote result

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet gobject vote-conf \
0bf97bce78b3b642c36d4ca8e9265f8f66de8774c220221f57739c1956413e2b funding yes
```

``` json
{
  "overall": "Voted successfully 1 time(s) and failed 0 time(s).",
  "detail": {
    "dash.conf": {
      "result": "success"
    }
  }
}
```

## GObject Vote-many

The `gobject vote-many` RPC votes on a governance object by all masternodes (using masternode.conf setup).

*Parameter #1---governance hash*

Name | Type | Presence | Description
--- | --- | --- | ---
`governance-hash` | string (hex) | Required<br>(exactly 1) | Hash of the governance object

*Parameter #2---vote signal*

Name | Type | Presence | Description
--- | --- | --- | ---
`signal` | string | Required<br>(exactly 1) | Vote signal: `funding`, `valid`, or `delete`

*Parameter #3---vote outcome*

Name | Type | Presence | Description
--- | --- | --- | ---
`outcome` | string | Required<br>(exactly 1) | Vote outcome: `yes`, `no`, or `abstain`

*Parameter #4---masternode alias*

Name | Type | Presence | Description
--- | --- | --- | ---
`alias` | string | Required<br>(exactly 1) | Alias of voting masternode

*Result---votes for specified governance*

Name | Type | Presence | Description
--- | --- | --- | ---
Result | object | Required<br>(exactly 1) | The governance object votes
→<br>`overall` | string | Required<br>(1 or more) | Reports number of vote successes/failures
→<br>`detail` | object | Required<br>(exactly 1) | Vote details
→ →<br>Masternode Alias | object | Required<br>(1 or more) | Name of the masternode alias
→ → →<br>`result` | string | Required<br>(exactly 1) | Vote result

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet gobject vote-many \
0bf97bce78b3b642c36d4ca8e9265f8f66de8774c220221f57739c1956413e2b funding yes
```

``` json
{
  "overall": "Voted successfully 1 time(s) and failed 0 time(s).",
  "detail": {
    "MN01": {
      "result": "success"
    }
  }
}
```

*See also:*

* [GetGovernanceInfo](/docs/core-api-ref-remote-procedure-calls-dash#section-getgovernanceinfo): returns an object containing governance parameters.
* [GetSuperblockBudget](/docs/core-api-ref-remote-procedure-calls-dash#section-getsuperblockbudget): returns the absolute maximum sum of superblock payments allowed.

# Masternode

The [`masternode` RPC](core-api-ref-remote-procedure-calls-dash#section-masternode) provides a set of commands for managing masternodes and displaying information about them.

## Masternode Count

The `masternode count` RPC prints the number of all known masternodes.

*Parameter #1---mode*

Name | Type | Presence | Description
--- | --- | --- | ---
Mode | string (hex) | Deprecated | Which masternodes to count:<br>`total` - Pre-0.12.3 default result,<br>`ps` - PrivateSend capable,<br>`enabled` - Enabled,<br>`all` - All,<br>`qualify` - Eligible for payment

*Result---number of known masternodes*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | Masternode count by mode
→<br>`total` | int | Required<br>(exactly 1) | Count of all masternodes
→<br>`enabled` | int | Required<br>(exactly 1) | Count of enabled masternodes

*Example from Dash Core 0.14.0*

``` bash
dash-cli -testnet masternode count
```

Result:
``` bash
{
  "total": 185,
  "enabled": 130
}
```

**Get summarized count of all masternodes**

*Result---summary of known masternodes*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string | Required<br>(exactly 1) | Summary of masternodes in each state

*Example from Dash Core 0.14.0*

``` bash
dash-cli -testnet masternode count all
```

Result:
``` bash
Total: 185 (Enabled: 130)
```

**Get total count of all masternodes (default output of `masternode count` pre-0.12.3)**

*Result---number of known masternodes in mode*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | int | Required<br>(exactly 1) | Number of masternodes

*Example from Dash Core 0.12.3*

``` bash
dash-cli -testnet masternode count total
```

Result:
``` bash
142
```

## Masternode Current

The `masternode current` RPC prints info on current masternode winner to be paid the next block (calculated locally).

*Parameters: none*

*Result---current winning masternode info*

Name | Type | Presence | Description
--- | --- | --- | ---
Result | object | Required<br>(exactly 1) | Winning masternode info
→<br>`height` | int | Required<br>(exactly 1) | Block height
→<br>`IP:port` | string | Required<br>(exactly 1) | The IP address/port of the masternode
→<br>`proTxHash` | string | Required<br>(exactly 1) | The masternode's Provider Registration transaction hash
→<br>`outpoint` | string | Required<br>(exactly 1) | The masternode's outpoint
→<br>`payee` | string | Required<br>(exactly 1) | Payee address

*Example from Dash Core 0.14.0*

``` bash
dash-cli -testnet masternode current
```

Result:
``` json
{
  "height": 76179,
  "IP:port": "34.242.53.163:26155",
  "proTxHash": "9de76b8291d00026ab0af86306023c7b90f8e9229dc04916fe1335bf5e11f15d",
  "outpoint": "9de76b8291d00026ab0af86306023c7b90f8e9229dc04916fe1335bf5e11f15d-1",
  "payee": "yZnU7YJJgGQKvKPQFqXJ4k4DGSsRMhgLXx"
}
```

## Masternode Outputs

The `masternode outputs` RPC prints masternode compatible outputs.

*Parameters: none*

*Result---masternode outputs*

Name | Type | Presence | Description
--- | --- | --- | ---
Result | object | Required<br>(exactly 1) | Masternode compatible outputs
→<br>Output | string | Required<br>(1 or more) | Masternode compatible output (TXID:Index)

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet masternode outputs
```

Result:
``` json
{
  "f6c83fd96bfaa47887c4587cceadeb9af6238a2c86fe36b883c4d7a6867eab0f": "1"
}
```

## Masternode Status

The `masternode status` RPC prints masternode status information.

*Parameters: none*

*Result---masternode status info*

Name | Type | Presence | Description
--- | --- | --- | ---
Result | object | Required<br>(exactly 1) | Masternode status info
→<br>`outpoint` | string | Required<br>(exactly 1) | The masternode's outpoint
→<br>`service` | string | Required<br>(exactly 1) | The IP address/port of the masternode
→<br>`proTxHash` | string (hex) | Required<br>(exactly 1) | The masternode's ProRegTx hash
→<br>`collateralHash` | string (hex) | Required<br>(exactly 1) | The masternode's collateral hash
→<br>`collateralIndex` | int | Required<br>(exactly 1) | Index of the collateral
→<br>`dmnState` | object | Required<br>(exactly 1) | Deterministic Masternode State
→ →<br>`service` | string | Required<br>(exactly 1) | The IP address/port of the masternode
→ →<br>`registeredHeight` | int | Required<br>(exactly 1) | Block height at which the masternode was registered
→ →<br>`lastPaidHeight` | int | Required<br>(exactly 1) | Block height at which the masternode was last paid
→ →<br>`PoSePenalty` | int | Required<br>(exactly 1) | Current proof-of-service penalty
→ →<br>`PoSeRevivedHeight` | int | Required<br>(exactly 1) | Block height at which the masternode was last revived from a PoSe ban
→ →<br>`PoSeBanHeight` | int | Required<br>(exactly 1) | Block height at which the masternode was last PoSe banned
→ →<br>`revocationReason` | int | Required<br>(exactly 1) | Reason code for of masternode operator key revocation
→ →<br>`ownerAddress` | string | Required<br>(exactly 1) | The owner address
→ →<br>`votingAddress` | string | Required<br>(exactly 1) | The voting address
→ →<br>`payoutAddress` | string | Required<br>(exactly 1) | The payout address
→ →<br>`pubKeyOperator` | string | Required<br>(exactly 1) | The operator public key
→ →<br>`operatorPayoutAddress` | string | Optional<br>(0 or 1) | The operator payout address
→<br>`status` | string | Required<br>(1 or more) | The masternode's status

*Example from Dash Core 0.13.2*

``` bash
dash-cli -testnet masternode status
```

Result:
``` json
{
  "outpoint": "d1be3a1aa0b9516d06ed180607c168724c21d8ccf6c5a3f5983769830724c357-0",
  "service": "45.32.237.76:19999",
  "proTxHash": "04d06d16b3eca2f104ef9749d0c1c17d183eb1b4fe3a16808fd70464f03bcd63",
  "collateralHash": "d1be3a1aa0b9516d06ed180607c168724c21d8ccf6c5a3f5983769830724c357",
  "collateralIndex": 0,
  "dmnState": {
    "service": "45.32.237.76:19999",
    "registeredHeight": 7402,
    "lastPaidHeight": 59721,
    "PoSePenalty": 0,
    "PoSeRevivedHeight": 61915,
    "PoSeBanHeight": -1,
    "revocationReason": 0,
    "ownerAddress": "yT8DDY5NkX4ZtBkUVz7y1RgzbakCnMPogh",
    "votingAddress": "yMLrhooXyJtpV3R2ncsxvkrh6wRennNPoG",
    "payoutAddress": "yTsGq4wV8WF5GKLaYV2C43zrkr2sfTtysT",
    "pubKeyOperator": "02a2e2673109a5e204f8a82baf628bb5f09a8dfc671859e84d2661cae03e6c6e198a037e968253e94cd099d07b98e94e"
  },
  "state": "READY",
  "status": "Ready"
}
```

## Masternode List

The `masternode list` prints a list of all known masternodes.

This RPC uses the same parameters and returns the same data as
[masternodelist](/docs/core-api-ref-remote-procedure-calls-dash#section-masternodelist). Please reference it for full details.

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet masternode list \
	rank f6c83fd96bfaa47887c4587cceadeb9af6238a2c86fe36b883c4d7a6867eab0f
```

Result:
``` json
{
  "f6c83fd96bfaa47887c4587cceadeb9af6238a2c86fe36b883c4d7a6867eab0f-1": 11
}
```

## Masternode Winner

The `masternode winner` RPC prints info on the next masternode winner to vote for.

*Parameters: none*

*Result---next masternode winner info*

Name | Type | Presence | Description
--- | --- | --- | ---
Result | object | Required<br>(exactly 1) | Winning masternode info
→<br>`height` | int | Required<br>(exactly 1) | Block height
→<br>`IP:port` | string | Required<br>(exactly 1) | The IP address/port of the masternode
→<br>`proTxHash` | string | Required<br>(exactly 1) | The masternode's Provider Registration transaction hash
→<br>`outpoint` | string | Required<br>(exactly 1) | The masternode's outpoint
→<br>`payee` | string | Required<br>(exactly 1) | Payee address

*Example from Dash Core 0.14.0*

``` bash
dash-cli -testnet masternode winner
```

Result:
``` json
{
  "height": 76191,
  "IP:port": "34.242.53.163:26173",
  "proTxHash": "024608d03beb6a6065f14a29a837c68ae449ac1e17056819366ca0b72b6dd81f",
  "outpoint": "024608d03beb6a6065f14a29a837c68ae449ac1e17056819366ca0b72b6dd81f-1",
  "payee": "yhp182AnF7gUAyHiWgSbDrKqHKt2dzhoyW"
}
```

## Masternode Winners

The `masternode winners` RPC prints the list of masternode winners.

By default, the 10 previous block winners, the current block winner, and the
next 20 block winners are displayed. More past block winners can be requested
via the optional `count` parameter.

*Parameter #1---count*

Name | Type | Presence | Description
--- | --- | --- | ---
Count | string (hex) | Optional<br>(exactly 1) | Number of previous block winners to display (default: 10)

*Parameter #2---filter*

Name | Type | Presence | Description
--- | --- | --- | ---
Filter | string | Optional<br>(exactly 1) | Payment address to filter by

*Result---masternode winners*

Name | Type | Presence | Description
--- | --- | --- | ---
Result | object | Required<br>(exactly 1) | Winning masternode info
→<br>Masternode Winner | int | Required<br>(exactly 1) | Key: Block height<br>Value: payee address

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet masternode winners
```

Result (current block - 37458):
``` json
{
  "37448": "ygSWwhyzU61FNEta8gDh8gfoH5EZZUvc5m:8",
  "37449": "yjGZLzSSoFfTFgLDJrgniXfYxu3xF9xKQg:5",
  "37450": "yRTo1wXWoNnPFWcQVepKGXuLsoypnPkGWj:7",
  "37451": "yYMFRAYZ25XspHZ1EXC39wUMx9FhoC5VT2:9",
  "37452": "yX5y3otE4LitGYiSfZhVH4LdbwHShdzQ8v:7",
  "37453": "yX5y3otE4LitGYiSfZhVH4LdbwHShdzQ8v:4",
  "37454": "yUamtYUFhqUxCMny3JTcZJTyttVt8SYFug:9",
  "37455": "yU35XcdGMnj8Exa2ZZqCg4ongiNqQwpeUZ:9",
  "37456": "yaJc6tADbEjxQBAC69ugWNoTFpzxqkcgWd:7",
  "37457": "yf4WpwRX17p7YRkHJPQpHMXTwzi5s2VDcR:7",
  "37458": "ydbfUYWfLm6xg7Y5aBLjy38DvksrvNcHEc:9",
  "37459": "yYp9k2iuDptT2MB7qVZtVy6ModHtLXFjio:6",
  "37460": "yP1UHNx26ShYLej56SbHiTiPAUv2QppbUv:6",
  "37461": "yaCtZRpiYnVFMyWELHZF74v9ayLKCLPcC9:8",
  "37462": "ygYFnLHoVRyhRoxd6fXQ9nmEafX4eLoWkB:6",
  "37463": "yM5kTThWi8MnAFtZqx98Zipp1BbyypUZGK:7",
  "37464": "yeDY39aiqbBHbJft5F6rokR23EaZca6UTU:9",
  "37465": "yMME1ns1xfpGS2XbFPktsNyp7Cjr1BoJxb:8",
  "37466": "ycn5RWc4Ruo35FTS8bJwugVyCEkfVcrw9a:6",
  "37467": "yUTDkKKhbvDrnwkiaoP8HvqxTNC6rNnUe2:6",
  "37468": "yTstes2nSaSpvu9nTapiCGnjCLvLD5fUqt:5",
  "37469": "Unknown",
  "37470": "Unknown",
  "37471": "Unknown",
  "37472": "Unknown",
  "37473": "Unknown",
  "37474": "Unknown",
  "37475": "Unknown",
  "37476": "Unknown",
  "37477": "Unknown"
}
```

Get a filtered list of masternode winners

``` bash
dash-cli -testnet masternode winners 150 "yTZ99"
```

Result:
``` json
{
  "37338": "yTZ99fCnjNu33RDRtawf81iwJ9uxXFmkgM:9",
  "37339": "yTZ99fCnjNu33RDRtawf81iwJ9uxXFmkgM:8",
  "37432": "yTZ99fCnjNu33RDRtawf81iwJ9uxXFmkgM:6",
  "37433": "yTZ99fCnjNu33RDRtawf81iwJ9uxXFmkgM:9"
}
```

**Deprecated RPCs**
[block:callout]
{
  "type": "danger",
  "title": "Removed RPCs",
  "body": "The following RPCs were deprecated by Dash Core 0.14.0 and have been removed."
}
[/block]
**Masternode Check**

Forces a check of all masternodes and removes invalid ones.

**Masternode Genkey**

Generates a new masternodeprivkey.

**Masternode Start-alias**

Starts a single remote masternode by assigned alias configured in masternode.conf.

**Masternode Start-mode**

Starts remote masternodes configured in masternode.conf. Valid modes are: `all`, `missing`, or `disabled`.

**Masternode List-conf**

Prints masternode.conf in JSON format.

*See also:*

* [MasternodeList](/docs/core-api-ref-remote-procedure-calls-dash#section-masternodelist): returns a list of masternodes in different modes.

# MasternodeList

The [`masternodelist` RPC](core-api-ref-remote-procedure-calls-dash#section-masternodelist) returns a list of masternodes in different modes.

*Parameter #1---List mode*

Name | Type | Presence | Description
--- | --- | --- | ---
`mode` | string | Optional (exactly 1);<br>Required to use `filter` | The mode to run list in

*Mode Options (Default=json)*

Mode | Description
--- | --- | --- |
`addr` | Print IP address associated with a masternode (can be additionally filtered, partial match)
`full` | Print info in format 'status payee lastpaidtime lastpaidblock IP' (can be additionally filtered, partial match)
`info` | Print info in format 'status payee IP' (can be additionally filtered, partial match)
`json` (Default) | Print info in JSON format (can be additionally filtered, partial match)
`lastpaidblock` | Print the last block height a node was paid on the network
`lastpaidtime` | Print the last time a node was paid on the network
`owneraddress` | Print the masternode owner Dash address
`payee` | Print Dash address associated with a masternode (can be additionally filtered, partial match)
`pubKeyOperator` | Print the masternode operator public key
`status` | Print masternode status: ENABLED / POSE_BANNED (can be additionally filtered, partial match)
`votingaddress` | Print the masternode voting Dash address

*Parameter #2---List filter*

Name | Type | Presence | Description
--- | --- | --- | ---
`filter` | string | Optional<br>(exactly 1) | Filter results. Partial match by outpoint by default in all modes, additional matches in some modes are also available.

*Result---the masternode list*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object/null | Required<br>(exactly 1) | Information about the masternode sync status
→<br>Masternode Info | string | Required<br>(1 or more) | The requested masternode info. Output varies based on selected `mode` and `filter` parameters

*Example from Dash Core 0.14.0*

Get unfiltered Masternode list in default mode

``` bash
dash-cli -testnet masternodelist
```

Result:
``` json
{
  "64fbf05880cdbd35a0278ba01a5edf0c20a9c756d15f883d496f5df35b31b542-1": {
    "proTxHash": "ab51b2ba4dca27658e13fea81c0764167c1466aa2d92050c67e4490ce7623da0",
    "address": "167.99.164.60:19999",
    "payee": "ycZgaHNb8AQq7HnJ3rTwx2fXUd7VCWQumX",
    "status": "ENABLED",
    "lastpaidtime": 1556615121,
    "lastpaidblock": 89725,
    "owneraddress": "yisedvAxYga44V9bTABzoQ7KoQMugyfU1E",
    "votingaddress": "yVpKfQgjkRkezFS5SpZvAEVFsbv9zJedf4",
    "collateraladdress": "yeXE94admJeH3oKiaB7UpwWnPZD6Q8srhT",
    "pubkeyoperator": "8072ac9a55d1cf5bf9c4262d49e2ef1ffcd716b8983ffdc62b940fec6cb4179d6275f8b68316f29c6c2ad540db329258"
  },
  "6ed4aa5fa90565c2331bcd22275f684ecdca5da8dd7f83ca943aadc6f44e6746-0": {
    "proTxHash": "8f5d5c7c0d9232f45f3a77eef6541922f827930b1f3bb789ad1771dc4d6275c0",
    "address": "3.209.222.37:19999",
    "payee": "yiVDR2HothEwH2Ss17GntqNp1rBUthnyje",
    "status": "POSE_BANNED",
    "lastpaidtime": 1554219432,
    "lastpaidblock": 72365,
    "owneraddress": "ycAZ9adjpGfZ2WLEpyfyUWAjkF6sXdD5df",
    "votingaddress": "yQrieR9S99hqnPghoj12RszMXYzc6yzyn2",
    "collateraladdress": "ya82BzRBhuFZAPhgXvhkzZgqiVsMdnfan7",
    "pubkeyoperator": "0a7fd01cfd502296cfd523d58ee9f4cff34243abb0dcc543ec237ff4d73938e69d187f0b6838bbaf9d54b89adc0d4c8e"
  },
  "4758b97bbd20024e171767b8baf4290bec1475b254180869cdfe0db75d7faefb-0": {
    "proTxHash": "5cd86ed16f87819dca7b6e4e3d24947b1a6328ed8cc4c9aec7af35fa2b162220",
    "address": "68.183.167.16:19999",
    "payee": "ycZgaHNb8AQq7HnJ3rTwx2fXUd7VCWQumX",
    "status": "ENABLED",
    "lastpaidtime": 1556616437,
    "lastpaidblock": 89734,
    "owneraddress": "yPmESxMJhZYuKDLJ1oYdH6kpE8oADVAPUQ",
    "votingaddress": "yLvTNLDLHa3pDMbFDRBX5mVMjCshzrDD1X",
    "collateraladdress": "ydGCjUEVRHkQZK3ajCsGJGDE9sjrbbS56v",
    "pubkeyoperator": "18af4d035eed23d30eb02808af0c133d9879c0fb82c72329ab2ed208ebc1631641ca42bbf462239d151f4e84d8dcde7b"
  }
}
```

Get a filtered Masternode list

``` bash
dash-cli -testnet masternodelist full "NEW"
```

Result:
``` json
{
  "64fbf05880cdbd35a0278ba01a5edf0c20a9c756d15f883d496f5df35b31b542-1": "           ENABLED ycZgaHNb8AQq7HnJ3rTwx2fXUd7VCWQumX 1553155206  65121 167.99.164.60:19999",
  "809818107c1104bbba6d386567aa231a294219387e591542df599b7ae7d23339-1": "       POSE_BANNED yLriZkwBhftk8VBUqrSykhFhAi4PowZ2Rs 1547488185  24447 45.48.177.222:19999",
  "d9fd715b7d896f5426e90bd3383a67fd3e311e00c021750560c6e5c5f9cdac85-1": "           ENABLED yRbiW3dguCym4fzUGZCf2kWzKUgw97zEqE 1553155396  65122 109.235.71.56:19999",
  "0950cce784fadcc2df4febb19d3a21eab4836ba22ea996ce7e2dde32b6c31431-0": "           ENABLED ycZgaHNb8AQq7HnJ3rTwx2fXUd7VCWQumX 1553154969  65119 165.227.63.223:19999",
  "08b493929f61a3205f09af9290af9034bec6a8355040a82ce4413f294c703e9a-0": "           ENABLED ybCE7m9oPjvCjm8MzPdbMBGgkF7p9wXsFq 1553153959  65110 34.207.45.58:19999",
  "b4f9de65ae676b63f84f2865317b8b512a12516c4459f2f59ca2626c71f7dda3-1": "       POSE_BANNED yYmromZERpc15GTDvgvjmjChPmgHbhWf1r          0      0 1.1.1.1:19999",
  "b7ec36db0c4ece8018183dcb90eed910e38e1c8d3641bbb4facced9a48a283a3-5": "           ENABLED ybFPBD7hm9KVd2Dubj97K5mw2ymR8gWJre 1553164088  65172 18.202.52.170:20028",
  "71fa05269adf3efc9ffa9a9ce33d27320de61c230cdf4a3835ba7f707bd7807a-1": "       POSE_BANNED yVxBZ8JeM5qRbLnUnswZ2APV3rgeZ7C9n9 1552466625  61158 167.99.110.59:19999"
}
```

*See also:*

* [Masternode](/docs/core-api-ref-remote-procedure-calls-dash#section-masternode): provides a set of commands for managing masternodes and displaying information about them.
* [MnSync](/docs/core-api-ref-remote-procedure-calls-dash#section-mnsync): returns the sync status, updates to the next step or resets it entirely.

# MnSync

The [`mnsync` RPC](core-api-ref-remote-procedure-calls-dash#section-mnsync) returns the sync status, updates to the next step or resets it entirely.

*Parameter #1---Command mode*

Name | Type | Presence | Description
--- | --- | --- | ---
`mode` | string | Required<br>(exactly 1) | The command mode to use:<br>`status` - Get masternode sync status<br>`next` - Move to next sync asset<br>`reset` - Reset sync status

**Command Mode - `status`**

*Result---the sync status*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object/null | Required<br>(exactly 1) | Information about the masternode sync status
→<br>`AssetID` | number (int) | Required<br>(exactly 1) | The sync asset ID
→<br>`AssetName` | string | Required<br>(exactly 1) | The sync asset name
→<br>`AssetStartTime` | number (int) | Required<br>(exactly 1) | The sync asset start time
→<br>`Attempt` | number (int) | Required<br>(exactly 1) | The sync attempt number
→<br>`IsBlockchainSynced` | boolean | Required<br>(exactly 1) | Blockchain sync status
→<br>`IsSynced` | boolean | Required<br>(exactly 1) | Masternode sync status
→<br>`IsFailed` | boolean | Required<br>(exactly 1) | Masternode list sync fail status

Sync Assets

AssetID | AssetName
--- | --- | --- |
0 | MASTERNODE_SYNC_INITIAL
1 | MASTERNODE_SYNC_WAITING
_2_ | **Deprecated since 0.14.0**<br>_MASTERNODE_SYNC_LIST_
_3_ | **Deprecated since 0.14.0**<br>_MASTERNODE_SYNC_MNW_
4 | MASTERNODE_SYNC_GOVERNANCE
-1 | MASTERNODE_SYNC_FAILED
999 | MASTERNODE_SYNC_FINISHED

*Example from Dash Core 0.14.0*

Get Masternode sync status

``` bash
dash-cli -testnet mnsync status
```

Result:
``` json
{
  "AssetID": 999,
  "AssetName": "MASTERNODE_SYNC_FINISHED",
  "AssetStartTime": 1507662300,
  "Attempt": 0,
  "IsBlockchainSynced": true,
  "IsSynced": true,
  "IsFailed": false
}
```

**Command Mode - `next`**

*Result---next command return status*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string | Required<br>(exactly 1) | Command return status

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet mnsync next
```

Result:
``` text
sync updated to MASTERNODE_SYNC_GOVERNANCE
```

**Command Mode - `reset`**

*Result---reset command return status*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string | Required<br>(exactly 1) | Command return status:<br>`success` or `failure`

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet mnsync reset
```

Result:
``` text
success
```

*See also:*

* [Masternode](/docs/core-api-ref-remote-procedure-calls-dash#section-masternode): provides a set of commands for managing masternodes and displaying information about them.
* [MasternodeList](/docs/core-api-ref-remote-procedure-calls-dash#section-masternodelist): returns a list of masternodes in different modes.

# PrivateSend

As of Dash Core 0.12.3, client-side mixing is not supported on masternodes.

The [`privatesend` RPC](core-api-ref-remote-procedure-calls-dash#section-privatesend) controls the mixing process.

Name | Type | Presence | Description
--- | --- | --- | ---
`mode` | string | Required<br>(exactly 1) | The command mode to use:<br>`start` - Start mixing<br>`stop` - Stop mixing<br>`reset` - Reset mixing

**Command Mode - `start`**

*Result---start command return status*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string | Required<br>(exactly 1) | Command return status

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet privatesend start
```

Result:
``` text
Mixing started successfully
```

**Command Mode - `stop`**

*Result---stop command return status*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string | Required<br>(exactly 1) | Command return status

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet privatesend stop
```

Result:
``` text
Mixing was stopped
```

**Command Mode - `reset`**

*Result---reset command return status*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | string | Required<br>(exactly 1) | Command return status

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet privatesend reset
```

Result:
``` text
Mixing was reset
```

*See also: none*

# Spork

The [`spork` RPC](core-api-ref-remote-procedure-calls-dash#section-spork) reads or updates spork settings on the network.

To display the status of sporks, use the `show` or `active` syntax.

*Parameter #1---Command mode*

Name | Type | Presence | Description
--- | --- | --- | ---
`mode` | string | Required<br>(exactly 1) | The command mode to use:<br>`show` - Display spork values<br>`active` - Display spork activation status

**Command Mode - `show`**

*Result---spork values*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | Object containing status
→<br>`Spork Value` | int64_t | Required<br>(1 or more) | Spork value (epoch datetime to enable/disable)

*Example from Dash Core 0.15.0*

``` bash
dash-cli -testnet spork show
```

Result:
``` json
{
  "SPORK_2_INSTANTSEND_ENABLED": 0,
  "SPORK_3_INSTANTSEND_BLOCK_FILTERING": 0,
  "SPORK_6_NEW_SIGS": 4000000000,
  "SPORK_9_SUPERBLOCKS_ENABLED": 0,
  "SPORK_15_DETERMINISTIC_MNS_ENABLED": 1047200,
  "SPORK_16_INSTANTSEND_AUTOLOCKS": 0,
  "SPORK_17_QUORUM_DKG_ENABLED": 0,
  "SPORK_19_CHAINLOCKS_ENABLED": 0,
  "SPORK_20_INSTANTSEND_LLMQ_BASED": 0
}
```

**Command Mode - `active`**

*Result---spork active status*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | Object containing status
→<br>`Spork Activation Status` | bool | Required<br>(1 or more) | Spork activation status

*Example from Dash Core 0.14.0*

``` bash
dash-cli -testnet spork active
```

Result:
``` json
{
  "SPORK_2_INSTANTSEND_ENABLED": true,
  "SPORK_3_INSTANTSEND_BLOCK_FILTERING": true,
  "SPORK_6_NEW_SIGS": false,
  "SPORK_9_SUPERBLOCKS_ENABLED": true,
  "SPORK_15_DETERMINISTIC_MNS_ENABLED": true,
  "SPORK_16_INSTANTSEND_AUTOLOCKS": true,
  "SPORK_17_QUORUM_DKG_ENABLED": true,
  "SPORK_19_CHAINLOCKS_ENABLED": true,
  "SPORK_20_INSTANTSEND_LLMQ_BASED": true
}
```

To update the state of a spork activation, use the `<name> [value]` syntax.

**Command Mode - `update`**

*Parameter #1---Spork name*

Name | Type | Presence | Description
--- | --- | --- | ---
`name` | string | Required<br>(exactly 1) | The name of the spork to update

*Parameter #2---Spork value*

Name | Type | Presence | Description
--- | --- | --- | ---
`value` | int | Required<br>(exactly 1) | The value to assign the spork

*Result---spork update status*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | object | Required<br>(exactly 1) | Update status (`success` or `failure`)

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet spork SPORK_2_INSTANTSEND_ENABLED 0
```

Result:
``` bash
failure
```

*See also: none*

# VoteRaw

The [`voteraw` RPC](core-api-ref-remote-procedure-calls-dash#section-voteraw) compiles and relays a governance vote with provided external signature instead of signing vote internally

*Parameter #1---masternode collateral transaction hash*

Name | Type | Presence | Description
--- | --- | --- | ---
`masternode-collateral-tx-hash` | string (hex) | Required<br>(exactly 1) | Hash of the masternode collateral transaction

*Parameter #2---masternode collateral transaction index*

Name | Type | Presence | Description
--- | --- | --- | ---
`masternode-collateral-tx-index` | string | Required<br>(exactly 1) | Index of the masternode collateral transaction

*Parameter #3---governance hash*

Name | Type | Presence | Description
--- | --- | --- | ---
`governance-hash` | string (hex) | Required<br>(exactly 1) | Hash of the governance object

*Parameter #4---vote signal*

Name | Type | Presence | Description
--- | --- | --- | ---
`signal` | string | Required<br>(exactly 1) | Vote signal: `funding`, `valid`, or `delete`

*Parameter #5---vote outcome*

Name | Type | Presence | Description
--- | --- | --- | ---
`outcome` | string | Required<br>(exactly 1) | Vote outcome: `yes`, `no`, or `abstain`

*Parameter #6---time*

Name | Type | Presence | Description
--- | --- | --- | ---
`time` | int64_t | Required<br>(exactly 1) | Create time

*Parameter #7---vote signature*

Name | Type | Presence | Description
--- | --- | --- | ---
`vote-sig` | string (base64) | Required<br>(exactly 1) | The vote signature created by external application (i.e. [Dash Masternode Tool](https://github.com/Bertrand256/dash-masternode-tool) or [dashmnb](https://github.com/chaeplin/dashmnb)).<br><br>Must match the Dash Core ([governance vote signature format](https://github.com/dashpay/dash/blob/48d63ab296f5613c727306ea39524f51d157a04c/src/governance-vote.cpp#L240-#L241)).

*Result---votes for specified governance*

Name | Type | Presence | Description
--- | --- | --- | ---
Result | object | Required<br>(exactly 1) | The vote result

*Example from Dash Core 0.12.2*

``` bash
dash-cli -testnet voteraw \
f6c83fd96bfaa47887c4587cceadeb9af6238a2c86fe36b883c4d7a6867eab0f 1 \
65a358fefaace40fc07053350be23e519178519290f963dab8ba92f6f85f98c3 \
funding yes 1512507255 \
H1jXKZQp1TZWBPW11E665OwmGBYV1038FohEr0au7zp+O5BCKmVDP/3rGq38ZMy3KOpwnBu6ehd6jlas79hsRBY=
```

Result:
``` bash
Voted successfully
```

*See also:*

* [GObject](/docs/core-api-ref-remote-procedure-calls-dash#section-gobject): provides a set of commands for managing governance objects and displaying information about them.