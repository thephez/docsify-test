Dash Core provides testing tools designed to let developers test their applications with reduced risks and limitations.

# Testnet

When run with no arguments, all Dash Core programs default to Dash's main network (<<glossary:mainnet>>). However, for development, it's safer and cheaper to use Dash's test network (<<glossary:testnet>>) where the <<glossary:duffs>> spent have no real-world value. Testnet also relaxes some restrictions (such as standard transaction checks) so you can test functions which might currently be disabled by default on <<glossary:mainnet>>.

To use testnet, use the argument `-testnet` with `dash-cli`, `dashd` or `dash-qt` or add `testnet=1` to your `dash.conf` file as [described earlier](core-examples-configuration-file).  To get free duffs for testing, check the faucets listed below. They are community supported and due to potentially frequent Testnet changes, one or more of them may be unavailable at a given time:

* [Testnet Faucet - Crowdnode.io](http://faucet.test.dash.crowdnode.io/)
* [Testnet Faucet - Masternode.io](http://test.faucet.masternode.io/)

Testnet is a public resource provided for free by members of the community, so please don't abuse it.

# Regtest Mode

For situations where interaction with random <<glossary:peers>> and <<glossary:blocks>> is unnecessary or unwanted, Dash Core's <<glossary:regression test mode>> (regtest mode) lets you instantly create a brand-new private <<glossary:block chain>> with the same basic rules as testnet---but one major difference: you choose when to create new blocks, so you have complete control over the environment.

Many developers consider regtest mode the preferred way to develop new applications. The following example will let you create a regtest environment after you first [configure dashd](core-examples-configuration-file).

``` bash
> dashd -regtest -daemon
Dash Core server starting
```

Start `dashd` in regtest mode to create a private block chain.

``` text
## Dash Core
dash-cli -regtest generate 101
```

Generate 101 blocks using a special RPC which is only available in regtest mode. This takes less than a second on a generic PC. Because this is a new block chain using Dash's default rules, the first blocks pay a <<glossary:block reward>> of 500 dash.  Unlike <<glossary:mainnet>>, in regtest mode only the first 150 blocks pay a reward of 500 dash. However, a block must have 100 <<glossary:confirmations>> before that reward can be spent, so we generate 101 blocks to get access to the <<glossary:coinbase transaction>> from block #1.

``` bash
dash-cli -regtest getbalance
500.00000000
```

Verify that we now have 500 dash available to spend.

You can now use Dash Core RPCs prefixed with `dash-cli -regtest`.

Regtest wallets and block chain state (chainstate) are saved in the `regtest` subdirectory of the Dash Core configuration directory. You can safely delete the `regtest` subdirectory and restart Dash Core to start a new regtest. (See the [Developer Examples Introduction](core-examples-introduction) for default configuration directory locations on various operating systems. **Always back up mainnet wallets before performing dangerous operations such as deleting**.)

# Devnet Mode

Developer networks (devnets) have some aspects of testnet and some aspects of regtest. Unlike testnet, multiple independent devnets can be created and coexist without interference. Each one is identified by a name which is hardened into a "devnet genesis" block, which is automatically positioned at height 1. Validation rules will ensure that a <<glossary:node>> from `devnet=test1` never be able to accept blocks from `devnet=test2`. This is done by checking the expected devnet <<glossary:genesis block>>.

The genesis block of the devnet is the same as the one from regtest. This starts the devnet with a very low <<glossary:difficulty>>, allowing quick generation of a sufficient balance to create a <<glossary:masternode>>.

The devnet name is put into the sub-version of the [`version` message](core-ref-p2p-network-control-messages#section-version). If a node connects to the wrong <<glossary:network>>, it will immediately be disconnected.

To use devnet, use the argument `-devnet=<name>` with `dash-cli`, `dashd`or `dash-qt` or add `devnet=<name>` to your `dash.conf` file as [described earlier](core-examples-configuration-file).

Devnets must be assigned both `-port` and `-rpcport` unless they are not listening (`-listen=0`). It is possible to run a devnet on a private (RFC1918) network by using the `-allowprivatenet=1` argument.

Example devnet start command:

``` bash
> dashd -devnet=mydevnet -rpcport=18998 -port=18999 -daemon
Dash Core server starting
```

You can now use Dash Core RPCs prefixed with `dash-cli -devnet=<name>`.

Devnet wallets and block chain state (chainstate) are saved in the `devnet-<name>` subdirectory of the Dash Core configuration directory. You can safely delete the `devnet-<name>` subdirectory and restart Dash Core to start a new devnet. (See the [Developer Examples Introduction](core-examples-introduction) for default configuration directory locations on various operating systems. **Always back up mainnet wallets before performing dangerous operations such as deleting.**)

Eventually, there may be many public and/or private devnets that vary in size and function. Providing the correct devnet name and the seed node of the network will be all that is required to join.

An old devnet can be easily dropped and a new one started just by destroying all nodes and recreating them with a new devnet name. This works best in combination with an automated deployment using something like Ansible and Terraform. The [Dash Network Deploy](https://github.com/dashpay/dash-cluster-ansible) tool provides a way to do this (currently a work-in-progress at an early development stage).