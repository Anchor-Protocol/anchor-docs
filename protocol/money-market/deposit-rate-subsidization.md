# Deposit Rate Subsidization

Anchor Protocol's deposit rate stability is supported by two mechanisms, 

offers deposit rate stability via

Direct subsidization of Anchor's 



## Interest Buffer Pool

Markets periodically distribute subsidies to depositors whenever the deposit APY is below 10%.

Deposit rate subsidization is a multi-step process, requiring:

* Reward collection from bAsset collaterals deposited by borrowers
* Conversion of rewards into Terra stablecoins
* Distribution of subsidies to depositors

The subsidization process can only be triggered at most once in 30 minutes. Markets wait for bAsset rewards to be transferred to the Terra blockchain as reward claims of bAssets \(excluding bLuna\) involve a cross-chain transaction.

Claimed bAsset rewards, which are likely to be in a non-stablecoin denomination, are converted to Terra stablecoins and stockpiled separately in the market's **interest buffer** pool. Besides holding on to future subsidies, the interest buffers are responsible for calculating the amount of stablecoins required to increase the deposit rate to 10%. To prevent excessive drainage of the interest buffer, only up to 5% of its balance can be used per subsidization process.

Distributed subsidies are added to the money market’s liquidity, increasing the aToken exchange rate. Depositors indirectly receive subsidies via value appreciation of their aTokens.



## Borrow Demand Bootstrapping Via ANC Distribution

The subsidization process 



### Multiplicative Increase / Additive Decrease \(MIAD\) Distribution Rate

In order to prevent borrow demand runaways, 





#### When `deposit_rate < distribution_threshold`





#### When `deposit_rate > target_deposit_rate`

















