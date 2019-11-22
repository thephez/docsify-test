# Synchronization

Dash Core synchronizes the governance system via the <<glossary:masternode>> <<glossary:network>> as the last stage of the Masternode sync process (following the sync of sporks, the Masternode list, and Masternode payments).

The [`govsync` message](core-ref-p2p-network-governance-messages#section-govsync) initiates a sync of the governance system. Masternodes ignore this request if they are not fully synced.  

There are two distinct stages of governance sync:

1. Initial request (object sync) - requests the governance objects only via a [`govsync` message](core-ref-p2p-network-governance-messages#section-govsync) sent with a hash of all zeros.  

2. Follow up request(s) (vote sync) - request governance object votes for a specific object via a [`govsync` message](core-ref-p2p-network-governance-messages#section-govsync) containing the hash of the object. One message is required for each object. Dash Core periodically (~ every 6 seconds) sends messages to connected nodes until all the governance objects have been synchronized.
[block:callout]
{
  "type": "info",
  "body": "Dash Core limits how frequently the first type of sync (object sync) can be requested. Frequent requests will result in the node being banned.",
  "title": "Governance Object Sync Limitation"
}
[/block]
Masternodes respond to the [`govsync` message](core-ref-p2p-network-governance-messages#section-govsync) with several items:

For Object Sync:

* First, the Masternode sends a [`ssc` message](core-ref-p2p-network-masternode-messages#section-ssc) (Sync Status Count) for `govobj` objects. This message indicates how many <<glossary:inventory>> items will be sent.

* Second, the Masternode sends an [`inv` message](core-ref-p2p-network-data-messages#section-inv) for the `govobj` and `govobjvote` objects.

For Vote Sync:

* First, the Masternode sends a [`ssc` message](core-ref-p2p-network-masternode-messages#section-ssc) (Sync Status Count) for `govobjvote` objects. This message indicates how many inventory items will be sent.

* Second, the Masternode sends an [`inv` message](core-ref-p2p-network-data-messages#section-inv) for the `govobjvote` object(s).

Once the syncing <<glossary:node>> receives the counts and inventories, it may request any `govobj` and `govobjvote` objects from the masternode via a [`getdata` message](core-ref-p2p-network-data-messages#section-getdata).

## Governance Sync Data Flow

| **Syncing Node Message** | **Direction**  | **Masternode Response**   | **Description** |
| --- | --- | --- | --- |
| **Initial request** | | | **Requests all governance objects (without votes)** |
| [`govsync` message](core-ref-p2p-network-governance-messages#section-govsync)        | →              |                           | Syncing node initiates governance sync (hash set to all zeros)
|                          | ←              | [`ssc` message](core-ref-p2p-network-masternode-messages#section-ssc) (govobj)    | Number of governance objects (0 or more)
|                          | ←              | [`inv` message](core-ref-p2p-network-data-messages#section-inv) (govobj)    | Governance object inventories
| [`getdata` message](core-ref-p2p-network-data-messages#section-getdata) (govobj) | →              |                           | (Optional) Syncing node requests govobj
|                          | ←              | [`govobj` message](core-ref-p2p-network-governance-messages#section-govobj)          | (If requested) Governance object
| | | | |
| **Follow up requests** | | | **Requests governance object (with votes)** |
| [`govsync` message](core-ref-p2p-network-governance-messages#section-govsync)        | →              |                           | Syncing node requests governance sync for a specific governance object
|                          | ←              | [`ssc` message](core-ref-p2p-network-masternode-messages#section-ssc) (govobjvote)| Number of governance object votes (0 or more)
|                          | ←              | [`inv` message](core-ref-p2p-network-data-messages#section-inv) (govobjvote)| Governance object vote inventories
| [`getdata` message](core-ref-p2p-network-data-messages#section-getdata) (govobjvote) | →              |                           | (Optional) Syncing node requests govobjvote
|                          | ←              | [`govobjvote` message](core-ref-p2p-network-governance-messages#section-govobjvote)      | (If requested) Governance object vote

# Sentinel

[Sentinel](https://github.com/dashpay/sentinel/) is a Python application that connects to a masternode's local dashd instance to run as an autonomous agent for persisting, processing, and automating Dash 12.1+ governance objects and tasks. Sentinel abstracts some governance details away from Dash Core for easier extensibility of the governance system in the future. This will allow the integration between Evolution and Dash Core to proceed more smoothly and enable new governance object additions with minimal impact to Dash Core.

Sentinel runs periodically and performs three main tasks as described below:
governance sync, governance object pruning, and superblock management. The governance object data is stored in a SQLite database.

## Sentinel Sync

Sentinel issues a [`gobject list` RPC](core-api-ref-remote-procedure-calls-dash#section-gobject-list) command and updates its database with the results returned from dashd. When objects are removed from the network, they are purged from the Sentinel database.

## Sentinel Prune

Sentinel 1.1.0 introduced proposal pruning which automatically votes to delete expired proposals following approximately half of a <<glossary:superblock>> cycle. This delay period ensures that proposals are not deleted prematurely. Prior to this, proposals remained in memory unless a sufficient number of masternodes manually voted to delete them.

## Sentinel Superblock

Sentinel manages superblock creation, voting, and submission to dashd for network propagation.

Beginning ~3 days (1662 blocks) prior to a superblock, Sentinel selects one masternode per block to rank proposals. This ranking is used to determine what a candidate superblock (or "superblock trigger") should contain. Based on the results, it creates and broadcasts a new superblock trigger if a matching one was not found.

All masternodes vote for existing superblock triggers. Each masternode casts only 1 superblock trigger "Yes" vote per superblock cycle. It will vote "No" for any other triggers it receives.

[block:callout]
{
  "type": "warning",
  "body": "Note: This means that proposal votes submitted _after_ superblock trigger creation begins will **not** be counted by some masternodes (those that have already voted on a superblock trigger).",
  "title": "Single masternode trigger vote"
}
[/block]
At the superblock height, the trigger with the most "Yes" votes is paid out by that block's miner.

## Sentinel Ping
[block:callout]
{
  "type": "danger",
  "body": "Note: Sentinel Pings were deprecated by [Dash Core v0.14](https://github.com/dashpay/sentinel/pull/64)",
  "title": "Sentinel Ping Deprecation"
}
[/block]
In Dash Core 12.2, use of the `watchdog` governance object type was replaced by integrating sentinel information into the masternode ping (`mnp` message) via [Pull Request #1491](https://github.com/dashpay/dash/pull/1491).

From Dash Core 0.12.2 through Dash Core 0.13, Sentinel used the [`sentinelping` RPC](core-api-ref-remote-procedure-calls-removed#section-sentinelping) to update the masternode info and prevent it from entering a `MASTERNODE_WATCHDOG_EXPIRED` state.