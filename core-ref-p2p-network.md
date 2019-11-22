This section describes the Dash P2P network protocol (but it is not a specification). It does not describe the <<glossary:BIP70 payment protocol>>, the [GetBlockTemplate mining protocol](core-guide-mining-block-prototypes#section-getblocktemplate-rpc), or any network protocol never implemented in an official version of Dash Core.

All peer-to-peer communication occurs entirely over TCP.
[block:callout]
{
  "type": "warning",
  "body": "Note: unless their description says otherwise, all multi-byte integers mentioned in this section are transmitted in little-endian order."
}
[/block]