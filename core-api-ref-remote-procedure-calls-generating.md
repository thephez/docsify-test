# Generate

*Requires wallet support.*

The [`generate` RPC](core-api-ref-remote-procedure-calls-generating#section-generate) mines blocks immediately (before the RPC call returns).

*Parameter #1---the number of blocks to generate*

Name | Type | Presence | Description
--- | --- | --- | ---
`numblocks` | number (int) | Required<br>(exactly 1) | The number of blocks to generate.  The RPC call will not return until all blocks have been generated.

*Parameter #2---the number of iterations*

Name | Type | Presence | Description
--- | --- | --- | ---
`maxtries` | number (int) | Required<br>(exactly 1) | The number of iterations to try (default = 1000000).

*Result---the generated block header hashes*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | An array containing the block header hashes of the generated blocks (may be empty if used with `generate 0`)
→<br>Header Hashes | string (hex) | Required<br>(1 or more) | The hashes of the headers of the blocks generated in regtest mode, as hex in RPC byte order

*Example from Dash Core 0.12.2*

Using regtest mode, generate 2 blocks:

``` bash
dash-cli -regtest generate 2
```

Result:

``` json
[
  "55a4c47da8151c0823eec22c41ebc6d690a0288302179625bae9eb6f36808266",
  "3f07b9aa4e3bcd5518610945c4a6b32699acac71b1762605ff79ba553111fc79"
]
```

*See also*

* [GenerateToAddress](/docs/core-api-ref-remote-procedure-calls-generating#section-generatetoaddress): mines blocks immediately to a specified address.
* [GetBlockTemplate](/docs/core-api-ref-remote-procedure-calls-mining#section-getblocktemplate): gets a block template or proposal for use with mining software.
* [GetGenerate](/docs/core-api-ref-remote-procedure-calls-removed#section-getgenerate): was removed in Dash Core 0.12.3.
* [GetMiningInfo](/docs/core-api-ref-remote-procedure-calls-mining#section-getmininginfo): returns various mining-related information.
* [SetGenerate](/docs/core-api-ref-remote-procedure-calls-removed#section-setgenerate): was removed in Dash Core 0.12.3.

# GenerateToAddress

*Added in Dash Core 0.12.3 / Bitcoin Core 0.13.0*

*Requires wallet support.*

The [`generatetoaddress` RPC](core-api-ref-remote-procedure-calls-generating#section-generatetoaddress) mines blocks immediately to a specified address.

*Parameter #1---the number of blocks to generate*

Name | Type | Presence | Description
--- | --- | --- | ---
Blocks | number (int) | Required<br>(exactly 1) | The number of blocks to generate.  The RPC call will not return until all blocks have been generated or the maximum number of iterations has been reached

*Parameter #2---a transaction identifier (TXID)*

Name | Type | Presence | Description
--- | --- | --- | ---
Address | string (base58) | Required<br>(exactly 1) | The address that will receive the newly generated Dash

*Parameter #3---the maximum number of iterations to try*

Name | Type | Presence | Description
--- | --- | --- | ---
Maxtries | number (int) | Optional<br>(0 or 1) | The maximum number of iterations that are tried to create the requested number of blocks.  Default is `1000000`

*Result---the generated block header hashes*

Name | Type | Presence | Description
--- | --- | --- | ---
`result` | array | Required<br>(exactly 1) | An array containing the block header hashes of the generated blocks (may be empty if used with `generate 0`)
→<br>Header Hashes | string (hex) | Required<br>(1 or more) | The hashes of the headers of the blocks generated, as hex in RPC byte order

*Example from Dash Core 0.12.3*

Using regtest mode, generate 2 blocks with maximal 500000 iterations:

``` bash
dash-cli -regtest generatetoaddress 2 "yaQzdWrDVYGncLKSKG4bHQ\
ML9UdAe726QN" 500000
```

Result:

``` json
[
  "34726c518d1688a9c56b3399e892089d3a639b43de194517c07da2b168a3a89c",
  "1f030abe2bb323b8895542e3a85ed8386bd92c67af9d19fe9c163a4c5f5ef149"
]
```

*See also*

* [Generate](/docs/core-api-ref-remote-procedure-calls-generating#section-generate): mines blocks immediately (before the RPC call returns).
* [GetMiningInfo](/docs/core-api-ref-remote-procedure-calls-mining#section-getmininginfo): returns various mining-related information.
* [GetBlockTemplate](/docs/core-api-ref-remote-procedure-calls-mining#section-getblocktemplate): gets a block template or proposal for use with mining software.