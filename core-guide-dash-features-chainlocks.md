Dash's <<glossary:ChainLock>> feature leverages <<glossary:LLMQ>> Signing Requests/Sessions to reduce uncertainty when receiving funds and remove the possibility of 51% mining attacks.

For each block, an LLMQ of a few hundred <<glossary:masternodes>> (300-400) is selected and each participating member signs the first <<glossary:block>> that it sees extending the active chain at the current <<glossary:block height>>. If enough members (at least 240) see the same block as the first block, they will be able to create a [`clsig` message](core-ref-p2p-network-instantsend-messages#section-clsig) and propagate it to all <<glossary:nodes>> in the <<glossary:network>>.

If a valid [`clsig` message](core-ref-p2p-network-instantsend-messages#section-clsig) is received by a node, it must reject all blocks (and any descendants) at the same height that do not match the block specified in the [`clsig` message](core-ref-p2p-network-instantsend-messages#section-clsig). This makes the decision on the active chain quick, easy and unambiguous. It also makes reorganizations below this block impossible.

When LLMQ-based <<glossary:InstantSend>> is enabled, a ChainLock is only attempted once all <<glossary:transactions>> in the block are locked via InstantSend. If a block contains unlocked transactions, retroactive InstantSend locks are established prior to a ChainLock.

Please read [DIP8 ChainLocks](https://github.com/dashpay/dips/blob/master/dip-0008.md) for additional details.