The Proof of Service (PoSe) scoring system helps incentivize <<glossary:masternodes>> to provide <<glossary:network>> services. Masternodes that neglect to participate receive an increased PoSe score which eventually results in them being excluded from masternode payment eligibility.

The current PoSe scoring system is based on participation in the <<glossary:LLMQ>> DKG process. This scoring system will expand over time to incorporate additional service requirements in support of the future Dash Platform (Evolution) functionality.

| Service | Percent of Score | Requirement |
| ----------- | ---- | ------------------- |
| LLMQ DKG    | 100% | Participate in the DKG process used to establish LLMQs. Requires exchanging messages with other quorum members. |

# **PoSe Score Calculation**

As shown in the following table, the PoSe Score always decreases by 1 per <<glossary:block>> as long as a masternode has not been banned. Once banned, the masternode can only be restored by sending a Provider Update Service ([ProUpServTx](core-ref-transactions-special-transactions#section-proupservtx)) special transaction.

| PoSe Parameter | Value | Example Value |
| --- | --- | --- |
| Maximum PoSe Score | Number of registered masternodes | 5000 |
| PoSe Score Increases | Maximum PoSe Score * 2/3 | 3333 |
| PoSe Score Decreases | 1 (per block) | Always `1` |

The current PoSe scoring algorithm increases the PoSe score by 66% of the maximum score for each failed DKG session. Depending on timing, this allows for no more than 2 failures for a masternode within a payment cycle (i.e a number of blocks equal to the number of registered masternodes).

For example, using the values from above with 5000 masternodes:

- In the first 5000 block cycle, two DKG failures occur without the PoSe score exceeding the maximum. This happens since a sufficient number of blocks are mined prior to the second failure to drop the PoSe score below the threshold (`< 5000 - 3333`) that would result in banning.

- In the second 5000 block cycle, the second DKG failure occurs too close to the first and results in the PoSe score exceeding the maximum limit. This results in the masternode receiving a PoSe Ban.

| Payment Cycle | Block Number | Event | Score Change | PoSe Score | MN Status |
| :---: | :--- | --- | :---: | :---: | :---: |
| 1 | 1 | DKG Failure (1) | +3333 | 3333 | Valid |
| 1 | 1734 | 1733 Blocks Mined | -1733 | 1600 | Valid |
| 1 | 1734 | DKG Failure (2) | +3333 | 4933 | Valid |
| 1 | 5000 | 3266 Blocks Mined | -3266 | 1667 | Valid |
| | | End of Payment Cycle 1| | | |
| 2 | 5500 | 500 Blocks Mined | -500 | 1167 | Valid |
| 2 | 5500 | DKG Failure (3) | +3333 | 4500 | Valid |
| 2 | 7000 | 1500 Blocks Mined | -1500 | 3000 | Valid |
| 2 | 7000 | DKG Failure (4) | +3333 | 6333 | PoSe Banned |
| 2 | 10000 | End of Payment Cycle 2 | - | 6333 | PoSe Banned |