# Modify Collateral Attributes

The attributes of a whitelisted bAsset collateral is determined by its `Max LTV` parameter, defined as the maximum portion of collateral value that can be borrowed. This value signals bAsset's the level of usability as Anchor collateral, where bAssets that make poor collateral \(e.g. low exchange liquidity, high price volatility\) are assigned low `Max LTV` values and vice versa. For example, providing 100 UST worth of collaterals with a `Max LTV` of 0.5 \(50%\) enables the user to borrow up to 50 UST.

Through the **Modify Collateral Attributes** poll, the Anchor community is able to adjust a bAsset collateral's `Max LTV` value as its properties change over time.

| Parameter Name | Description |
| :--- | :--- |
| `Max LTV` | Maximum portion of collateral value that can be borrowed |

## Genesis Values

| Collateral Ticker | Collateral Name | Parameter Name | Value |
| :--- | :--- | :--- | :--- |
| **bLUNA** | Bonded Luna | `Max LTV` | 50% |

## Poll Format

///// Screenshot Image /////

| Field | Description | Optionality |
| :--- | :--- | :--- |
| Title | Poll title | Required |
| Rationale | Short description of poll | Required |
| Information Link | External URL for further information | Optional |
| Collateral | Ticker of collateral to modify parameter | Required |
| Max LTV | Proposed new `Max LTV` value for collateral | Required |

