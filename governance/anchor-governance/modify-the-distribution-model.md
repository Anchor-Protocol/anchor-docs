# Modify ANC Distribution

The **Modify ANC Distribution** poll can be used to submit polls that adjust parameters related to borrower ANC distribution. ANC distribution to borrowers are controlled by 3 parameter values, `Borrower Emission Cap`, `Increment Multiplier`, and `Decrement Multiplier`.

Calibration of the borrower ANC emission rate is used to control stablecoin borrow demand, which in turn affects the stablecoin deposit rate. The emission rate is adjusted based on the relation between the current deposit rate and the target deposit rate.

If the current deposit rate is calculated to be below the target, the borrower ANC emission rate is increased by a factor of `Increment Multiplier`. Otherwise, if the current deposit rate is above the target, the emission rate decreases by a factor of `Decrement Multiplier`.

To prevent the emission rate from spiking and having an excessively high rate, the emission rate is capped to the `Borrower Emission Cap`.

## Genesis Values

| Parameter Name | Value |
| :--- | :--- |
| `Borrower Emission Cap` | Maximum per-block ANC emission rate to borrowers |
| `Increment Multiplier` | Emission rate multiplier when increasing ANC emission |
| `Decrement Multiplier` | Emission rate multiplier when decreasing ANC emission |

## Poll Format

///// Screenshot Image ///// 

| Field | Description | Optionality |
| :--- | :--- | :--- |
| Title | Poll title | Required |
| Rationale | Short description of poll | Required |
| External Link | External URL for further information | Optional |
| Borrower Emission Cap | Proposed maximum per-block ANC emission rate to borrowers | Optional |
| Increment Multiplier | Proposed emission rate multiplier when increasing emission | Optional |
| Decrement Multiplier | Proposed emission rate multiplier when decreasing emission | Optional |

