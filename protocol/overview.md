# Overview

## Components

Anchor Protocol can be subdivided to the below components:

| Component | Description |
| :--- | :--- |
| [Bonded Assets \(bAssets\)](bonded-assets-bassets/) | Tokenized representations of bonded PoS assets |
| [Money Market](money-market/) | Handles lending and borrowing of Terra stablecoins, with borrows collateralized by bAssets |
| [Liquidation Contract](liquidations.md) | Manages collateral liquidations of loans at risk of undercollateralization |

## Protocol Participants

Three types of users exist in the Anchor ecosystem: **depositors** \(lenders\), **borrowers**, and **liquidators**. Anchor also requires **oracle feeders**, critical for providing the necessary infrastructure.

In Anchor Protocol, depositors are incentivized to lend Terra stablecoins to Anchor's money market, which is borrowed out by borrowers through bAsset collateralized loans. Interest paid by borrowers are given to depositors, along with subsidies generated from rewards of deposited bAsset collaterals. In addition, the protocol prevents borrowers from forming liabilities in excess of collateral value by incentivizing liquidators to observe and liquidate loans at risk of undercollateralization.

### Depositor \(Lender\)

A **Depositor** is a user that lends Terra stablecoins to the Anchor money market. Deposited stablecoins are pooled and lent out to borrowers, with accrued interest proportionally distributed to all depositors.

Depositors receive newly minted [Anchor Tokens \(aTokens\)](money-market/#anchor-tokens-atokens) in exchange for their deposit. aTokens represent a depositor's share in the stablecoin pool and can later be redeemed to claim the initial stablecoin deposit, along with accrued interest and depositor subsidies.

### Borrower

**Borrowers** are entities that create bAsset-collateralized loan positions to borrow Terra stablecoins from the Anchor money market. bAssets that were whitelisted by Anchor can be deposited and locked to create a loan position. Positions are required to maintain a [loan-to-value \(LTV\)](money-market/#borrowing-terra-stablecoins) ratio below the set maximum.

By borrowing, users can gain access to liquidity without losing price exposure to their bAsset collateral. Borrowers are recommended to keep a close eye on their loan position's LTV ratio, as loans with LTV ratios over the set maximum are subject to liquidation.

### Liquidator

A **Liquidator** monitors for the existence of risky loans \(loans with an LTV ratio above the set maximum\) and requests loan collaterals to be liquidated if necessary. Before liquidating a loan, liquidators must submit a bid to the Liquidation Contract, offering to purchase the liquidated collateral in exchange for the liquidator's Terra stablecoins.

Collaterals are liquidated by executing bids in the Liquidation Contract. Only bids that were submitted by the liquidator can be executed; the liquidator triggering liquidations must have a pre-existing bid submitted by them. On execution, the liquidator receives the collateral tokens at a discounted rate, and stablecoins in the liquidator's bid is used to repay the liquidated borrower's loan.

### Oracle Feeder

An **Oracle Feeder** is a Terra account that is responsible for providing an accurate and up-to-date price feed for bAsset collaterals. Fed-in price data is used to calculate the collateral value of a borrower, also used as the reference price in the Liquidation Contract.

As an entity crucial for protocol operation, Anchor's oracle feeder is initially set as the creator of Anchor Protocol. Anchor will extend support for third-party oracle feeders as the protocol further matures.

## Tokens

| Name | Type | Function |
| :--- | :--- | :--- |
| Terra USD \(UST\) | Native Terra Token | Stablecoin |
| [Bonded Assets \(bAssets\)](bonded-assets-bassets/) | Cw20 Token | Loan collateral for Anchor money market |
| [Anchor Tokens \(aTokens\)](money-market/#anchor-tokens-atokens) | Cw20 Token | Deposit receipt for Anchor money market |

