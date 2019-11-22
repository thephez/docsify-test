# Dash Protocol Versions
The table below lists some notable versions of the P2P network protocol, with the most recent versions listed first.

As of Dash Core 0.15.0, the most recent protocol version is 70215.

| Version | Initial Release                    | Major Changes
|---------|------------------------------------|--------------
| 70215  | Dash Core 0.14.0.1 <br>(May 2019)  | • None (Governance bugfix only)
| 70214  | Dash Core 0.14.0.0 <br>(May 2019)  | • <<glossary:Long-Living Masternode Quorum>><br>• <<glossary:ChainLocks>><br>• PrivateSend improvements<br>• Experimental LLMQ InstantSend<br>• Bitcoin Core 0.15 backports
| 70213  | Dash Core 0.13.0.x <br>(Jan 2019)  | • <<glossary:Special Transactions>><br>• Deterministic Masternode List<br>• Coinbase Special Transaction<br>• Automatic InstantSend
| 70210  | Dash Core 0.12.3.x <br>(July 2018)  | • Named Devnets<br>• New signature format / Spork 6 addition<br>• Bitcoin Core 0.13/0.14 backports<br>• [BIP90](https://github.com/bitcoin/bips/blob/master/bip-0090.mediawiki): Buried deployments<br>• [BIP147](https://github.com/bitcoin/bips/blob/master/bip-0147.mediawiki): NULLYDUMMY enforcement<br>• [BIP152](https://github.com/bitcoin/bips/blob/master/bip-0152.mediawiki) Compact Blocks<br>• Transaction version increased to 2<br>• Zero fee transactions removed<br>• Pruning in Lite Mode
| 70208  | Dash Core 0.12.2.x <br>(Nov 2017)  | • [DIP1](https://github.com/dashpay/dips/blob/master/dip-0001.md) (2MB blocks)<br>• Fee reduction (10x)<br>• InstantSend fix<br>• PrivateSend improvements<br>• _Experimental_ HD wallet<br>• Local Masternode support removed
| 70206  | Dash Core 0.12.1.x <br>(Mar 2017)  | • Switch to Bitcoin Core 0.12.1<br>• BIP-0065 (CheckLockTimeVerify)<br>• BIP-0112 (CheckSequenceVerify)
| 70103  | Dash Core 0.12.0.x <br>(Aug 2015)  | • Switch to Bitcoin Core 0.10<br>• Decentralized budget system<br>• New IX implementation
| 70076  | Dash Core 0.11.2.x <br>(Mar 2015)  | • Masternode enhancements<br>• Mining/relay policy enhancements<br>• BIP-66 - strict DER encoding for signatures
| 70066  | Dash Core 0.11.1.x <br>(Feb 2015)  | • InstantX fully implemented<br>• <<glossary:Spork>> fully implemented<br>• Masternode payment updates<br>• Rebrand to Dash (0.11.1.26)
| 70052  | Dash Core 0.11.0.x <br>(Jan 2015)  | • Switch from fork of Litecoin 0.8 to Bitcoin 0.9.3<br>• Rebrand to Darkcoin Core
| 70051  | Dash Core 0.10.0.x <br>(Sep 2014)  | • Release of the originally closed source implementation of DarkSend
| 70002  | Dash Core 0.9.0.x <br>(Mar 2014)   | • Masternode implementation<br>• Rebrand to Darkcoin
| 70002  | Dash Core 0.8.7 <br>(Jan 2014)     | Initial release of Dash (branded XCoin) as a fork of Litecoin 0.8

# Bitcoin Protocol Versions

Historical Bitcoin protocol versions for reference shown below since Dash is a <<glossary:fork>> of Bitcoin Core.

| Version | Initial Release                    | Major Changes
|---------|------------------------------------|--------------
| 70012   | Bitcoin Core 0.12.0 <br>(Feb 2016) | [BIP130](https://github.com/bitcoin/bips/blob/master/bip-0130.mediawiki): <br>• Added [`sendheaders` message](core-ref-p2p-network-control-messages#section-sendheaders).
| 70011   | Bitcoin Core 0.12.0 <br>(Feb 2016) | [BIP111](https://github.com/bitcoin/bips/blob/master/bip-0111.mediawiki): <br>• `filter*` messages are disabled without NODE_BLOOM after and including this version.
| 70002   | Bitcoin Core 0.9.0 <br>(Mar 2014)  | • Send multiple [`inv` messages](core-ref-p2p-network-data-messages#section-inv) in response to a [`mempool` message](core-ref-p2p-network-data-messages#section-mempool) if necessary <br><br>[BIP61](https://github.com/bitcoin/bips/blob/master/bip-0061.mediawiki): <br>• Added [`reject` message](core-ref-p2p-network-control-messages#section-reject)
| 70001   | Bitcoin Core 0.8.0 <br>(Feb 2013)  | • Added [`notfound` message](core-ref-p2p-network-data-messages#section-notfound). <br><br>[BIP37](https://github.com/bitcoin/bips/blob/master/bip-0137.mediawiki): <br>• Added [`filterload` message](core-ref-p2p-network-control-messages#section-filterload). <br>• Added [`filteradd` message](core-ref-p2p-network-control-messages#section-filteradd). <br>• Added [`filterclear` message](core-ref-p2p-network-control-messages#section-filterclear). <br>• Added [`merkleblock` message](core-ref-p2p-network-data-messages#section-merkleblock). <br>• Added relay field to [`version` message](core-ref-p2p-network-control-messages#section-version) <br>• Added `MSG_FILTERED_BLOCK` inventory type to [`getdata` message](core-ref-p2p-network-data-messages#section-getdata).
| 60002   | Bitcoin Core 0.7.0 <br>(Sep 2012)  | [BIP35](https://github.com/bitcoin/bips/blob/master/bip-0035.mediawiki): <br>• Added [`mempool` message](core-ref-p2p-network-data-messages#section-mempool). <br>• Extended [`getdata` message](core-ref-p2p-network-data-messages#section-getdata) to allow download of memory pool transactions
| 60001   | Bitcoin Core 0.6.1 <br>(May 2012)  | [BIP31](https://github.com/bitcoin/bips/blob/master/bip-0031.mediawiki): <br>• Added nonce field to [`ping` message](core-ref-p2p-network-control-messages#section-ping) <br>• Added [`pong` message](core-ref-p2p-network-control-messages#section-pong)
| 60000   | Bitcoin Core 0.6.0 <br>(Mar 2012)  | [BIP14](https://github.com/bitcoin/bips/blob/master/bip-0014.mediawiki): <br>• Separated protocol version from Bitcoin Core version
| 31800   | Bitcoin Core 0.3.18 <br>(Dec 2010) | • Added [`getheaders` message](core-ref-p2p-network-data-messages#section-getheaders) and [`headers` message](core-ref-p2p-network-data-messages#section-headers).
| 31402   | Bitcoin Core 0.3.15 <br>(Oct 2010) | • Added time field to [`addr` message](core-ref-p2p-network-control-messages#section-addr).
| 311     | Bitcoin Core 0.3.11 <br>(Aug 2010) | • Added `alert` message.
| 209     | Bitcoin Core 0.2.9 <br>(May 2010)  | • Added checksum field to message headers.
| 106     | Bitcoin Core 0.1.6 <br>(Oct 2009)  | • Added receive IP address fields to [`version` message](core-ref-p2p-network-control-messages#section-version).