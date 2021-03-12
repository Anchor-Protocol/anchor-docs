# Modify Market Parameters

The mechanism behind Anchor's money market is controlled by a set of carefully determined parameters, which are `Target Deposit Rate`, `Threshold Deposit Rate`, `Buffer Distribution Factor`, `Max Borrow Factor`, and `Valid Price Timeframe`. The **Modify Market Parameters** poll allows users to submit governance polls that adjust parameters that effectively control the Anchor money market.

Modifying the `Target Deposit Rate` adjusts Anchor's target deposit APY, which the protocol attempts to achieve by constantly controlling the ANC emission rate as borrower incentives.

The `Threshold Deposit Rate` value is the minimum deposit APY that Anchor tries to ensure by making direct deposit rate subsidizations from the interest buffer if the current deposit rate is observed to be below this value. Interest buffer usage from direct subsidization events are limited to a `Buffer Distribution Factor` portion of the interest buffer's balance per subsidization event.

In cases of excessive and uncontrollable borrow demand, the `Max Borrow Factor`, which limits the amount of stablecoin liquidity available to be borrowed, can be adjusted to allow aTerra redemptions to occur.

The money market is configured to be somewhat resilient to price oracle downtimes, where price values are considered invalid if they are older than `Valid Price Timeframe`.

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
| `Target Deposit Rate` | 20% APY |
| `Threshold Deposit Rate` | 15% APY |
| `Buffer Distribution Factor` | 10% |
| `Max Borrow Factor` | 95% |
| `Valid Price Timeframe` | 60 seconds |

## Poll Format

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

