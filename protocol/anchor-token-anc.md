# Anchor Token \(ANC\)

The **Anchor Token** \(**ANC**\) is Anchor Protocol's governance token. ANC tokens used as a deposit to create new governance proposals, which can be voted by users that have staked ANC.

ANC tokens are also used to bootstrap the initial borrow demand for the Anchor money market. ANC is distributed to borrowers, with the rate of distribution carefully controlled to ensure deposit rate stability.

ANC is designed to benefit its stakers with increasing adoption of Anchor Protocol; stakers of ANC are incentivized to vote for proposals that further merit the protocol.

## Value Accrual to ANC

{% hint style="info" %}
ANC rewards can also be earned by staking LP tokens of the ANC &lt;&gt; UST Terraswap Pair.
{% endhint %}

ANC is designed to generate a buying pressure that increases proportionally with Anchor Protocol's assets under management \(AUM\).

### bAsset Rewards

The protocol uses a portion of rewards from deposited bAsset collaterals to purchase ANC from Terraswap, with the remainder used to replenish the interest buffer. The portion of bAsset rewards used for ANC purchases \($$portion_{ANC}$$\) increases with the inventory level of the interest buffer pool, displayed as: 

$$
portion_{ANC} = (cap - balance) \cdot multiplier
$$

$$portion_{ANC}$$ increases as the interest buffer balance \( $$balance$$ \) converges to the set interest buffer cap \($$cap$$\), allowing more value accrual to ANC as the interest buffer reaches a sufficient amount.

### Deposit Rate Commission

The protocol also applies a commission to the deposit interest, used to purchase ANC from Terraswap.

### Collateral Liquidation Fees

Whenever a loan is liquidated, 1% of the liquidated collateral value is used to purchase ANC.

### Proposal Creation Fees

ANC token deposits of Anchor governance proposals that have failed to reach the required quorum are redistributed ANC stakers.

## Anchor Token Supply

There are planned to be a total of 1,000,000 ANC tokens to be distributed over a 4-year period. Beyond that, there will be no more new ANC tokens introduced to the supply.

### Cumulative Distribution Schedule

|  | Genesis | Year 1 | Year 2 | Year 3 | Year 4 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Luna staking airdrop |  |  |  |  |  |
|  |  |  |  |  |  |
|  |  |  |  |  |  |
|  |  |  |  |  |  |
|  |  |  |  |  |  |
| **Token Supply** |  |  |  |  |  |
| Annual Inflation \(%\) | nil |  |  |  |  |

### Genesis Token Distribution

///// Diagram of Token Distribution at Genesis ///// 



### Final Token Distribution

///// Diagram of Token Distribution Post-Distribution /////



#### Inflation Rate





