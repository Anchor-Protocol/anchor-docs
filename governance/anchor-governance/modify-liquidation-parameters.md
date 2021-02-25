# Modify Liquidation Parameters





| Parameter Name | Description |
| :--- | :--- |
| `Safe Ratio` | Targeted risk ratio of  |
| `Bid Fee` |  |
| `Max Premium Rate` |  |
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
| Safe Ratio | Proposed  | Optional |
| Bid Fee | Proposed | Optional |
| Max Premium Rate | Proposed | Optional |
| Partial Liquidation Threshold | Proposed | Optional |
| Valid Price Timeframe | Proposed | Optional |

