# Modify Governance Parameters

The modify



| Parameter Name | Description |
| :--- | :--- |
| `Quorum` | Minimum voter turnout required for a poll to pass |
| `Vote Threshold` | Minimum percentage of yes votes required for a poll to pass |
| `Voting Period` | Poll voting period |
| `Snapshot Period` | Window of time \(number of blocks\) allowed for poll snapshot before a poll's end |
| `Expiration Period` | Number of blocks after a poll's voting period during which the poll can be executed |
| `Timelock Period` | Number of blocks required after a poll pass before executing changes |
| `Proposal Deposit` | Minimum ANC deposit required for submitting a new poll |

## Genesis Values

| Parameter Name | Value |
| :--- | :--- |
| `Quorum` | 10% |
| `Vote Threshold` | 50% |
| `Voting Period` | 7 days |
| `Snapshot Period` | 1 day |
| `Expiration Period` |  |
| `Timelock Period` | 3 days |
| `Proposal Deposit` | 100 ANC |

## Poll Format



| Field | Description | Optionality |
| :--- | :--- | :--- |
| Title | Poll title | Required |
| Rationale | Short description of poll | Required |
| External Link | External URL for further information | Optional |
| Quorum | Proposed minimum voter turnout required for a poll to pass | Optional |
| Vote Threshold | Proposed minimum percentage of yes votes required for a poll to pass | Optional |
| Voting Period | Proposed voting period for polls | Optional |
| Snapshot Period | Proposed time window before poll end in which  | Optional |
| Expiration Period | Proposed time window in | Optional |
| Timelock Period |  | Optional |
| Proposal Deposit |  | Optional |









