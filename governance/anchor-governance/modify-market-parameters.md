# Modify Market Parameters

The mechanism behind Anchor's money market is controlled by a set of carefully determined parameters.



| Parameter Name | Description |
| :--- | :--- |
| `Target Deposit Rate` | Target stablecoin deposit APY of Anchor Protocol |
| `Threshold Deposit Rate` | Threshold deposit APY to trigger interest buffer distribution |
| `Buffer Distribution Factor` | Maximum portion of interest buffer that can be distributed per deposit rate subsidization event |
| `Max Borrow Factor` | Maximum portion of money market's stablecoin liquidity available for borrows |
| `Valid Price Timeframe` | Time window before the money market considers oracle price data invalid |

## Genesis Values

| Parameter Name | Value |
| :--- | :--- |
| `Target Deposit Rate` | 15% APY |
| `Threshold Deposit Rate` | 10% APY |
| `Buffer Distribution Factor` | 10% |
| `Max Borrow Factor` | 95% |
| `Valid Price Timeframe` | 60 seconds |

## Poll Format

///// Screenshot Image /////

| Field | Description | Optionality |
| :--- | :--- | :--- |
| Title | Poll title | Required |
| Rationale | Short description of poll | Required |
| Information Link | External URL for further information | Optional |
| Target Deposit Rate | Proposed target deposit APY of Anchor Protocol | Optional |
| Threshold Deposit Rate | Proposed threshold deposit APY of Anchor Protocol | Optional |
| Buffer Distribution Factor | Proposed maximum portion of interest buffer distributable per direct subsidization event | Optional |
| Max Borrow Factor | Proposed maximum portion of money market's stablecoin liquidity available for borrows | Optional |
| Valid Price Timeframe | Proposed time window before the money market considers oracle price data invalid | Optional |

