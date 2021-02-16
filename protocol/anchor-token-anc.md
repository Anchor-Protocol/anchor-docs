# Anchor Token \(ANC\)

The **Anchor Token** \(**ANC**\) is Anchor Protocol's governance token. ANC tokens used as a deposit to create new governance polls, which can be voted by users that have staked ANC.

ANC tokens accrue value from protocol fees, which are distributed pro-rata to ANC stakers. The ANC token is designed to benefit its stakers with increasing adoption of Anchor Protocol -- stakers of ANC are incentivized to vote for proposals that further merit the protocol.

ANC is also used as incentives to bootstrap borrow demand. ANC is distributed every block to stablecoin borrowers, proportional to the amount of their accrued borrow interest.

## Value Accrual

{% hint style="info" %}
ANC rewards can also be earned by staking LP tokens of the ANC &lt;&gt; UST Terraswap Pair.
{% endhint %}

![ANC Token Structure](../.gitbook/assets/anc_incentive_diagram%20%283%29.png)

ANC is designed to generate a buying pressure which increases proportionally with Anchor's assets under management \(AUM\). Terra stablecoins from below sources are used to purchase ANC tokens from Terraswap.

A part of the ANC tokens purchased from protocol fees are recollected as future borrower ANC incentives, enabling borrower incentivization to continue even after full expenditure of their initial ANC allocation.

### Protocol Fees

#### bAsset Rewards

A portion of rewards from deposited bAsset collaterals are used to purchase ANC, with the remainder used to replenish the interest buffer. The ratio of bAsset rewards used for ANC purchases can be adjusted thorough governance if the interest buffer's inventory rises to a sufficient level.

#### Deposit Rate Commission

Anchor applies a 5% commission to accrued deposit interest, set aside as ANC purchase [reserves](money-market/#anchor-tokens-atokens). This allows ANC value to scale with the deposit AUM.

#### Collateral Liquidation Fees

Whenever a loan is liquidated, 1% of the liquidated collateral value is used to purchase ANC. This fee is applied separately to a bid's [premium](liquidations.md#premium-rate).



### Governance Fees

ANC token deposits of Anchor governance polls that have failed to reach the required quorum are redistributed ANC stakers.

## Anchor Token Supply

There are planned to be a total of 1,000,000 ANC tokens to be distributed over a 4-year period. Beyond that, there will be no more new ANC tokens introduced to the supply.

### Cumulative Allocation Schedule

|  | Genesis | Year 1 | Year 2 | Year 3 | Year 4 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Luna staking airdrop |  |  |  |  |  |
|  |  |  |  |  |  |
|  |  |  |  |  |  |
|  |  |  |  |  |  |
|  |  |  |  |  |  |
| **Token Supply** |  |  |  |  |  |
| Annual Inflation \(%\) | nil |  |  |  |  |

### Genesis Token Allocation

///// Diagram of Token Distribution at Genesis ///// 



### Final Token Allocation

///// Diagram of Token Distribution Post-Distribution /////



#### Inflation Rate

Inflation rate of ANC tokens are designed to gradually decrease every year, until it reaches **1M** at the end of year 4. After the end of year 4, no more ANC tokens will be minted through inflation.

## Distribution to Borrowers

ANC tokens allocated for borrower incentives are gradually distributed to borrowers through the [ANC emission control algorithm](money-market/deposit-rate-subsidization.md#anc-emission-feedback-control). This is further distributed to individual borrowers pro-rata to their amount of accrued borrow interest. 

ANC incentives fuel a self-reinforcing adoption cycle, where they incentivize more borrowers to deposit bAsset collaterals, bringing further buying pressure to ANC, further increasing borrow incentives.

