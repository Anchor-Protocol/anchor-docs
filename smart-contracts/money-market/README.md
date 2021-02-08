# Money Market

This section describes provides a high-level overview of Anchor Protocol's Money Market contracts.

{% hint style="warning" %}
Even with a thorough understanding of Anchor Protocol, it is highly recommended to interact with Anchor through client channels such as the Anchor Web App or [Anchor.js](../../developers/anchor.js.md).
{% endhint %}

## Smart Contracts

| Contract | Function |
| :--- | :--- |
| [Overseer](overseer.md) | Manages money market overalls, stores borrower information |
| [Market](market.md) | Handles Terra stablecoin deposits and borrows |
| [Custody \[bLuna\]](custody-bluna-specific.md) | Handles bLuna collateral deposits and withdrawals |
| [Oracle](oracle.md) | Provides a price feed for bAsset collaterals |
| [Interest Model](interest-model.md) | Calculates the current borrow interest rate based on the market situation |

