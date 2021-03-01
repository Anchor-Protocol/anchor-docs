# Modify Liquidation Parameters





| Parameter Name | Description |
| :--- | :--- |
| `Safe Ratio` | Targeted risk ratio of a liquidated loan |
| `Bid Fee` | Protocol commission on executed bids used to purchase ANC |
| `Max Premium Rate` | Maximum bid premium rate value submittable by bidders |
| `Partial Liquidation Threshold` | Threshold total collateral value to trigger partial collateral liquidations |
| `Valid Price Timeframe` | Time window before the Liquidation Contract considers oracle price data invalid |



## Genesis Values

| Parameter Name | Value |
| :--- | :--- |
| `Safe Ratio` | 80% |
| `Bid Fee` | 1% |
| `Max Premium Rate` | 10% |
| `Partial Liquidation Threshold` | 200 UST |
| `Valid Price Timeframe` | 60 seconds |

## Poll Format

| Field | Description | Optionality |
| :--- | :--- | :--- |
| Title | Poll title | Required |
| Rationale | Short description of poll | Required |
| Information Link | External URL for further information | Optional |
| Safe Ratio | Proposed risk ratio target for a liquidated loan | Optional |
| Bid Fee | Proposed protocol commission on executed bids used to purchase ANC | Optional |
| Max Premium Rate | Proposed maximum bid premium rate value submittable by bidders | Optional |
| Partial Liquidation Threshold | Proposed threshold total collateral value to trigger partial collateral liquidations | Optional |
| Valid Price Timeframe | Proposed time window before the Liquidation Contract considers oracle price data invalid | Optional |

