# Loan Liquidation

To prohibit borrowers from defaulting on their loans, Anchor incentivizes liquidators to observe and liquidate loans with an LTV ratio above the allowed maximum. The Liquidation Contract is used to convert collaterals of a liquidating loan to Terra stablecoins, which are then used to repay the loan.

The Liquidation Contract acts as an over-the-counter \(OTC\) exchange between Cw20-compliant collateral tokens and Terra stablecoins. Using Anchor's Oracle Contract as a price feed, conversions between any arbitrary Cw20 token-based assets and Terra stablecoins are facilitated.

In addition to collateral liquidation, the Liquidation Contract handles calculations of collateral liquidation amounts in cases of [partial collateral liquidation](loan-liquidation.md#partial-liquidation).

## Bids

Collateral tokens are liquidated to Terra stablecoins by executing bids submitted to the Liquidation Contract. Bids are purchase offers for Cw20 tokens in exchange for Terra stablecoins, typically submitted by those that hold Terra stablecoins, and are wishing to exchange their stablecoins with a certain Cw20 token.

### Properties

Bids are characterized by four properties: **bidder**, **asset**, **size**, and **premium rate**.

#### Bidder

The **bidder** of a bid is the account that has submitted the bid. When a bid is executed, the purchased Cw20 tokens are sent to this account.

#### Asset

A bid's **asset** refers to the Cw20 token that the bidder is hoping to purchase. Any token that complies with the Cw20 standard is supported as long as a price feed is provided by the Oracle Contract.

#### Size

The **size** of a bid is the amount of Terra stablecoins that were put up upon bid submission. This is the amount of stablecoins that the bidder will use to purchase the specified Cw20 token.

#### Premium Rate

The **premium rate** of a bid is the rate of premium that the bidder is demanding upon bid execution. If set to a non-zero value, the bidder can purchase Cw20 tokens at a price cheaper than the current oracle price. While bidders are able to set premium rates of their own, the Liquidation Contract limits the maximum submittable value to **30%**.

### Bid Submission / Retraction

{% hint style="info" %}
There can exist at most one bid per asset type per bidder -- existing bids should be retracted before submitting a new bid with different properties \(i.e. premium rate\).
{% endhint %}

Any user with a Terra stablecoin balance can submit a bid to a Liquidation Contract with the matching stablecoin denomination. Bidders are required to specify the Cw20 token address of purchasing asset and their preferred premium rate.

A submitted bid can be retracted at any time, provided that the bid has not been fully executed. Users can specify the retract amount, and if not specified, the entire bid is retracted.

### Bid Execution

Bids can be executed when a user desires to convert their Cw20 assets for Terra stablecoins. Bid executors should specify a bidder to execute on, from which the specified bidder's bid is executed. The bidder receives the Cw20 asset sent by the user, and the user receives the bidder's stablecoin, minus the bidder premium. Executors are allowed to designate a different account to receive the converted stablecoins \(optional\).

For external contracts \(e.g. money market\) interacting with the Liquidation Contract, a fee address can be set to receive **1%** of the executed bid's stablecoins.

## Collateral Liquidation

A loan position's **risk ratio**, defined as the liability to borrow limit ratio, characterizes a position's riskiness. Loans are open for anyone to liquidate when its risk ratio is greater than one.

$$
\text{riskRatio} = \frac{\text{liability}}{\text{borrowLimit}}
$$

### Partial Liquidation

Loans with a total collateral value of above **500 UST** are partially liquidated, with only a portion of collateral liquidated instead of liquidating the full amount. Locked collaterals are fully liquidated for loans with a total collateral value below 500 UST.

Partially liquidating loan positions are liquidated until the position reaches below the **safe risk ratio** of **0.8**; loan positions with a risk ratio of 0.8 or below are considered safe from undercollateralization. Collaterals are liquidated proportionally to their locked amounts and the position's liquidation factor:

$$
\text{liquidationAmount}_{\text{collateral}} = \text{liquidationFactor} \cdot \text{amountLocked}_{\text{collateral}}
$$

Where a loan position's liquidation factor is determined as a function of the loan's total collateral value, borrow limit, and liability.

$$
\text{liquidationFactor} = \frac{\text{liability} - \text{safeRiskRatio} \cdot \text{borrowLimit}}{\text{collateralValue} \cdot \text{feeDeductor} - \text{safeRiskRatio} \cdot \text{borrowLimit}}
$$

The liquidation factor accounts for fees lost during bid execution\( $$\text{feeDeductor}$$ \), such as the premium rate of bids, fees applied on bid execution, and taxes charged on native Terra transfers:

$$
\text{feeDeductor} = (1-\text{maxPremiumRate}) \cdot(1-\text{executionFee})\cdot(1-\text{terraTax})
$$

Note that $$\text{feeDeductor}$$ uses the maximum rate of fees that can be applied during liquidation, liquidating slightly more collateral than the minimum required \(to reach the safe risk ratio\).

### Multicollateral Liquidation

Liquidation also applies for multicollateral loans, which are loans that are backed with two or more collaterals. When liquidated, all of the locked collateral types are liquidated accordingly until the multicollateral loan meets the safe risk ratio \(if partial liquidation is applicable\).

During liquidation, collaterals in a multicollateral loan are liquidated proportional to each collateral's collateral value.

$$
\text{liquidationAmount}_\text{collateral}\propto \frac{\text{collateralValue}_\text{collateral}}{\Sigma\, \text{collateralValue}}
$$

### Bid Execution

Collaterals locked to a liquidated loan position are converted to Terra stablecoins through bid execution. The money market executes bids that were submitted by the loan liquidator, and stablecoins received from execution are used to repay the liquidated borrower's loan.

The money market sets the execution fee address to its yield reserve, transferring **1%** of the bid value to the yield reserve.

