# Gov

The Gov Contract contains logic for holding governance polls and handling Anchor Token \(ANC\) staking, and allows Anchor Protocol to be governed by its users in a decentralized manner. After the initial bootstrapping of Anchor Protocol's contracts, the Gov Contract is assigned to be the owner of all contracts in Anchor Protocol.

New proposals for change are submitted as polls, and are voted on by ANC stakers through the [voting procedure](../../governance/anchor-governance/). Polls can contain messages that can be executed directly without changing the Anchor Protocol code.

The Gov Contract keeps a balance of ANC tokens, which it uses to reward stakers with funds it receives from trading fees sent by the [Anchor Collector](collector.md) and user deposits from creating new governance polls. This balance is separate from the [Community Pool](../../governance/anchor-governance/spend-community-pool.md), which is held by the [Community](community.md) contract \(owned by the Gov contract\).

## Config

| Name | Type | Description |
| :--- | :--- | :--- |
| `owner` | CanonicalAddr | Address of contract owner |
| `anchor_token` | CanonicalAddr | Contract address of Anchor Token \(ANC\) |
| `quorum` | Decimal | Minimum percentage of participation required for a poll to pass |
| `threshold` | Decimal | Minimum percentage of `yes` votes required for a poll to pass |
| `voting_period` | u64 | Number of blocks during which votes can be cast |
| `effective_delay` | u64 | Number of blocks required after a poll pass before executing changes |
| `expiration_period` | u64 | Number of blocks after a poll's voting period during which the poll can be executed |
| `proposal_deposit` | Uint128 | Minimum ANC deposit required for submitting a new poll |

## InitMsg

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct InitMsg {
    pub anchor_token: HumanAddr,
    pub quorum: Decimal,
    pub threshold: Decimal,
    pub voting_period: u64,
    pub effective_delay: u64,
    pub expiration_period: u64,
    pub proposal_deposit: Uint128,
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "anchor_token": "terra1...", 
  "quorum": "0.1", 
  "threshold": "0.5", 
  "voting_period": 123456, 
  "effective_delay": 123456, 
  "expiration_period": 123456, 
  "proposal_deposit": "100000000" 
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
| `anchor_token` | HumanAddr | Contract address of Anchor Token \(ANC\) |
| `quorum` | Decimal | Minimum percentage of participation required for a poll to pass |
| `threshold` | Decimal | Minimum percentage of `yes` votes required for a poll to pass |
| `voting_period` | u64 | Number of blocks during which votes can be cast |
| `effective_delay` | u64 | Number of blocks required after a poll pass before executing changes |
| `expiration_period` | u64 | Number of blocks after a poll's voting period during which the poll can be executed |
| `proposal_deposit` | Uint128 | Minimum ANC deposit required for submitting a new poll |

## HandleMsg

### `Receive`

Can be called during a CW20 token transfer when the Gov contract is the recipient. Allows the token transfer to execute a [Receive Hook](gov.md#receive-hooks) as a subsequent action within the same transaction.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    Receive {
        amount: Uint128,
        sender: HumanAddr,
        msg: Option<Binary>,
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "receive": {
    "amount": "10000000",
    "sender": "terra1...",
    "msg": "eyAiZXhlY3V0ZV9tc2ciOiAiYmluYXJ5IiB9"
  }
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
| `amount` | Uint128 | Amount of tokens received |
| `sender` | HumanAddr | Sender of token transfer |
| `msg`\* | Binary | Base64-encoded JSON of [Receive Hook](gov.md#receive-hooks) |

\* = optional

### `UpdateConfig`

Updates the configuration of the Gov contract.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    UpdateConfig {
        owner: Option<HumanAddr>, 
        quorum: Option<Decimal>, 
        threshold: Option<Decimal>, 
        voting_period: Option<u64>, 
        effective_delay: Option<u64>, 
        expiration_period: Option<u64>, 
        proposal_deposit: Option<Uint128>, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "update_config": {
    "owner": "terra1...", 
    "quorum": "0.1", 
    "threshold": "0.1", 
    "voting_period": 123456, 
    "effective_delay": 123456, 
    "expiration_period": 123456, 
    "proposal_deposit": "100000000" 
  }
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
| `owner`\* | HumanAddr | New address of contract owner |
| `quorum`\* | Decimal | New percentage of participation \(of total staked ANC\) required for a poll to pass |
| `threshold`\* | Decimal | New percentage of `yes` votes required for a poll to pass |
| `voting_period`\* | u64 | New number of blocks during which votes for a poll can be cast after it has finished its deposit |
| `effective_delay`\* | u64 | New number of blocks required after a poll pass before executing changes |
| `expiration_period`\* | u64 | New number of blocks after a poll's voting period during which the poll can be executed |
| `proposal_deposit`\* | Uint128 | New minimum ANC deposit required for a poll to enter voting |

\* = optional

### `CastVote`

Submits a user's vote for an active poll. Once a user has voted, they cannot change their vote with subsequent messages \(increasing voting power, changing vote option, cancelling vote, etc.\)

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    CastVote {
        poll_id: u64, 
        vote: VoteOption, 
        amount: Uint128, 
    }
}

#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum VoteOption {
    Yes,
    No,
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "cast_vote": {
    "amount": "10000000",
    "poll_id": 8,
    "vote": "yes"
  }
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
| `poll_id` | u64 | Amount of voting power \(staked ANC\) to allocate |
| `vote` | VoteOption | Poll ID |
| `amount` | Uint128 | Can be `yes` or `no` |

### `WithdrawVotingTokens`

Removes specified amount of staked ANC tokens from a staking position and returns them to a user's balance. Withdraws all staked ANC tokens if `amount` is empty.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    WithdrawVotingTokens {
        amount: Option<Uint128>, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "withdraw_voting_tokens": {
    "amount": "100000000" 
  }
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
| `amount`\* | Uint128 | Amount of ANC tokens to withdraw |

\* = optional

### `EndPoll`

Can be issued by anyone to end the voting for an active poll. Triggers tally the results to determine whether the poll has passed. The current block height must exceed the end height of voting phase.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    EndPoll {
        poll_id: u64, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "end_poll": {
    "poll_id": 8 
  }
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
| `poll_id` | u64 | Poll ID |

### `ExecutePoll`

Can be issued by anyone to implement into action the contents of a passed poll. The current block height must exceed the end height of the poll's effective delay.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    ExecutePoll {
        poll_id: u64, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "execute_poll": {
    "poll_id": 8 
  }
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
| `poll_id` | u64 | Poll ID |

### `ExpirePoll`

Can be issued by anyone to expire a poll. Requires the poll to be neither be a text proposal nor passed. The current block height must be more than the expiration period from the poll's end height.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    ExpirePoll {
        poll_id: u64, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "expire_poll": {
    "poll_id": 8 
  }
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
| `poll_id` | u64 | Poll ID |

## Receive Hooks

### `StakeVotingTokens`

{% hint style="danger" %}
**WARNING**  
  
Sending ANC tokens to the Gov contract without issuing this hook will lead to **PERMANENT LOSS OF FUNDS** and will be irrevocably donated to the reward pool for stakers.
{% endhint %}

Issued when sending ANC tokens to the Gov contract to add them to their ANC staking position.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum Cw20HookMsg {
    StakeVotingTokens {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "stake_voting_tokens": {}
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
|  |  |  |

### `CreatePoll`

Issued when sending ANC tokens to the Gov contract to create a new poll. Will only succeed if the amount of tokens sent meets the configured `proposal_deposit` amount. Can contain a list of generic messages to be issued by the Gov contract if it passes \(can invoke messages in other contracts it owns\).

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum Cw20HookMsg {
    CreatePoll {
        title: String, 
        description: String, 
        link: Option<String>, 
        execute_msgs: Option<Vec<ExecuteMsg>>, 
    }
}

#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub struct ExecuteMsg {
    pub contract: HumanAddr,
    pub msg: Binary,
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "create_poll": {
    "title": "...", 
    "description": "...", 
    "link": "https://...", 
    "execute_msgs": [
      {
        "contract": "terra1...", 
        "msg": "eyAiZXhlY3V0ZV9tc2ciOiAiYmluYXJ5IiB9" 
      }, 
      {
        "contract": "terra1...", 
        "msg": "eyAiZXhlY3V0ZV9tc2ciOiAiYmluYXJ5IiB9" 
      } 
    ]
  }
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
| `title` | String | Poll title |
| `description` | String | Poll description |
| `link`\* | String | URL to external post about poll \(forum, PDF, etc.\) |
| `execute_msgs`\* | `Vec<ExecuteMsg>` | List of governance messages to be issued by Gov contract upon poll execution |

| Name | Type | Description |
| :--- | :--- | :--- |
| `contract` | HumanAddr | Contract address of governance message recipient |
| `msg` | Binary | Base64-encoded JSON of governance message |

\* = optional

## QueryMsg

### `Config`

Gets the configuration for the Gov contract.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    Config {} 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "config": {} 
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
|  |  |  |

### `ConfigResponse`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, PartialEq, JsonSchema)]
pub struct ConfigResponse {
    pub owner: HumanAddr,
    pub anchor_token: HumanAddr,
    pub quorum: Decimal,
    pub threshold: Decimal,
    pub voting_period: u64,
    pub effective_delay: u64,
    pub expiration_period: u64,
    pub proposal_deposit: Uint128,
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "anchor_token": "terra1...", 
  "quorum": "0.1", 
  "threshold": "0.5", 
  "voting_period": 123456, 
  "effective_delay": 123456, 
  "expiration_period": 123456, 
  "proposal_deposit": "100000000" 
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
| `owner` | CanonicalAddr | Address of contract owner |
| `anchor_token` | CanonicalAddr | Contract address of Anchor Token \(ANC\) |
| `quorum` | Decimal | Minimum percentage of participation required for a poll to pass |
| `threshold` | Decimal | Minimum percentage of `yes` votes required for a poll to pass |
| `voting_period` | u64 | Number of blocks during which votes can be cast |
| `effective_delay` | u64 | Number of blocks required after a poll pass before executing changes |
| `expiration_period` | u64 | Number of blocks after a poll's voting period during which the poll can be executed |
| `proposal_deposit` | Uint128 | Minimum ANC deposit required for submitting a new poll |

### `State`

Gets state information for the Gov contract.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    State {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "state": {}
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
|  |  |  |

### `StateResponse`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, PartialEq, JsonSchema)]
pub struct StateResponse {
    pub poll_count: u64,
    pub total_share: Uint128,
    pub total_deposit: Uint128,
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "poll_count": 8, 
  "total_share": "100000000", 
  "total_deposit": "100000000" 
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
| `poll_count` | u64 | Total number of created polls |
| `total_share` | Uint128 | Current total number of voting shares |
| `total_deposit` | Uint128 | Total amount of ANC currently deposited for poll creation |

### `Staker`

Gets information for the specified ANC staker.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    Staker {
        address: HumanAddr, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "staker": {
    "address": "terra1..." 
  }
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
| `address` | HumanAddr | Address of staker |

### `StakerResponse`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Debug, Clone, PartialEq, JsonSchema)]
pub struct StakerResponse {
    pub balance: Uint128,
    pub share: Uint128,
    pub locked_balance: Vec<(u64, VoterInfo)>, // (Voted Poll's ID, VoterInfo)
}

#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct VoterInfo {
    pub vote: VoteOption,
    pub balance: Uint128,
}

#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum VoteOption {
    Yes,
    No,
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "balance": "100000000", 
  "share": "100000000", 
  "locked_balance": [
    [
      7, 
      {
        "vote": "yes", 
        "balance": "100000000" 
      }
    ], 
    [
      8, 
      {
        "vote": "no", 
        "balance": "100000000" 
      }
    ] 
  ]
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
| `balance` | Uint128 | Amount of ANC staked by staker |
| `share` | Uint128 | Total voting shares owned by staker |
| `locked_balance` | `Vec<(u64, VoterInfo)>` | List of \(voted poll's ID, voter's vote information\) |

| Name | Type | Description |
| :--- | :--- | :--- |
| `vote` | `VoteOption` | Vote type made by staker |
| `balance` | Uint128 | Amount of staked ANC locked to vote this poll |

| Name | Description |
| :--- | :--- |
| `yes` | Staker has voted for the proposal |
| `no` | Staker has voted against the proposal |

### `Poll`

Gets information for the specified poll.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    Poll {
        poll_id: u64, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "poll": {
    "poll_id": 8 
  }
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
| `poll_id` | u64 | Poll ID |

### `PollResponse`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Debug, Clone, PartialEq, JsonSchema)]
pub struct PollResponse {
    pub id: u64,
    pub creator: HumanAddr,
    pub status: PollStatus,
    pub end_height: u64,
    pub title: String,
    pub description: String,
    pub link: Option<String>,
    pub deposit_amount: Uint128,
    pub execute_data: Option<ExecuteMsg>,
    pub yes_votes: Uint128, // balance
    pub no_votes: Uint128,  // balance
    pub total_balance_at_end_poll: Option<Uint128>,
}

#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum PollStatus {
    InProgress,
    Passed,
    Rejected,
    Executed,
    Expired,
}

#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub struct ExecuteMsg {
    pub contract: HumanAddr,
    pub msg: Binary,
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "id": 8, 
  "creator": "terra1...", 
  "status": "executed", 
  "end_height": 123456, 
  "title": "...", 
  "description": "...", 
  "link": "https://...", 
  "deposit_amount": "100000000", 
  "execute_data": [
    {
      "contract": "terra1...", 
      "msg": "eyAiZXhlY3V0ZV9tc2ciOiAiYmluYXJ5IiB9" 
    }, 
    {
      "contract": "terra1...", 
      "msg": "eyAiZXhlY3V0ZV9tc2ciOiAiYmluYXJ5IiB9" 
    }
  ], 
  "yes_votes": "100000000", 
  "no_votes": "100000000", 
  "total_balance_at_end_poll": "100000000" 
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
| `id` | u64 | Poll ID |
| `creator` | HumanAddr | Poll creator |
| `status` | `PollStatus` | Current poll status |
| `end_height` | u64 | Block number when voting for this poll closes |
| `title` | String | Poll title |
| `description` | String | Poll description |
| `link`\* | String | URL to external post about poll \(forum, PDF, etc.\) |
| `deposit_amount` | Uint128 | ANC deposit used to submit poll |
| `execute_data`\* | `Vec<ExecuteMsg>` | List of governance messages to be issued upon poll execution |
| `yes_votes` | Uint128 | Total yes votes \(staked ANC amount\) for this poll |
| `no_votes` | Uint128 | Total no votes \(staked ANC amount\) for this poll |
| `total_balance_at_end_poll`\* | Uint128 | Poll's total votes \(staked ANC amount\) at the end of the poll |

| Name | Description |
| :--- | :--- |
| `InProgress` | Voting for this poll is currently in progress |
| `Passed` | This poll has been passed by governance |
| `Rejected` | This poll has been rejected by governance |
| `Executed` | This poll has been passed by governance and executed |
| `Expired` | This poll has been expired after rejection / execution |

| Name | Type | Description |
| :--- | :--- | :--- |
| `contract` | HumanAddr | Contract address of governance message recipient |
| `msg` | Binary | Base64-encoded JSON governance message |

\* = optional

### `Polls`

Gets information for all polls.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    Polls {
        filter: Option<PollStatus>, 
        start_after: Option<u64>, 
        limit: Option<u32>, 
        order_by: Option<OrderBy>, 
    }
}

#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum PollStatus {
    InProgress,
    Passed,
    Rejected,
    Executed,
    Expired,
}

#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum OrderBy {
    Asc,
    Desc,
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "polls": {
    "filter": "passed", 
    "start_after": 8, 
    "limit": 8, 
    "order_by": "asc" 
  }
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
| `filter`\* | PollStatus | Poll statuses to search for |
| `start_after`\* | u64 | Poll ID to start query at |
| `limit`\* | u32 | Maximum number of query entries |
| `order_by`\* | OrderBy | Order to make query |

| Name | Description |
| :--- | :--- |
| `InProgress` | Poll is currently in voting period |
| `Passed` | Poll has been passed by governance |
| `Rejected` | Poll has been rejected by governance |
| `Executed` | Poll has been passed and executed by governance |
| `Expired` | Poll has expired |

| Name | Description |
| :--- | :--- |
| `Asc` | Make query in ascending order |
| `Desc` | Make query in descending order |

\* = optional

### `PollsResponse`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Debug, Clone, PartialEq, JsonSchema)]
pub struct PollsResponse {
    pub polls: Vec<PollResponse>,
}

#[derive(Serialize, Deserialize, Debug, Clone, PartialEq, JsonSchema)]
pub struct PollResponse {
    pub id: u64,
    pub creator: HumanAddr,
    pub status: PollStatus,
    pub end_height: u64,
    pub title: String,
    pub description: String,
    pub link: Option<String>,
    pub deposit_amount: Uint128,
    pub execute_data: Option<ExecuteMsg>,
    pub yes_votes: Uint128, // balance
    pub no_votes: Uint128,  // balance
    pub total_balance_at_end_poll: Option<Uint128>,
}

#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum PollStatus {
    InProgress,
    Passed,
    Rejected,
    Executed,
    Expired,
}

#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub struct ExecuteMsg {
    pub contract: HumanAddr,
    pub msg: Binary,
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "polls": [
    {
      "id": 7, 
      "creator": "terra1...", 
      "status": "passed", 
      "end_height": 123456, 
      "title": "...", 
      "description": "...", 
      "link": "https://...", 
      "deposit_amount": "100000000", 
      "execute_data": [
        {
          "contract": "terra1...", 
          "msg": "eyAiZXhlY3V0ZV9tc2ciOiAiYmluYXJ5IiB9" 
        }, 
        {
          "contract": "terra1...", 
          "msg": "eyAiZXhlY3V0ZV9tc2ciOiAiYmluYXJ5IiB9" 
        }
      ], 
      "yes_votes": "100000000", 
      "no_votes": "100000000", 
      "total_balance_at_end_poll": "100000000" 
    }, 
    {
      "id": 8, 
      "creator": "terra1...", 
      "status": "executed", 
      "end_height": 123456, 
      "title": "...", 
      "description": "...", 
      "link": "https://...", 
      "deposit_amount": "100000000", 
      "execute_data": [
        {
          "contract": "terra1...", 
          "msg": "eyAiZXhlY3V0ZV9tc2ciOiAiYmluYXJ5IiB9" 
        }, 
        {
          "contract": "terra1...", 
          "msg": "eyAiZXhlY3V0ZV9tc2ciOiAiYmluYXJ5IiB9" 
        }
      ], 
      "yes_votes": "100000000", 
      "no_votes": "100000000", 
      "total_balance_at_end_poll": "100000000" 
    }
  ]
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
| `polls` | `Vec<PollResponse>` | List of poll information |

| Name | Type | Description |
| :--- | :--- | :--- |
| `id` | u64 | Poll ID |
| `creator` | HumanAddr | Poll creator |
| `status` | `PollStatus` | Current poll status |
| `end_height` | u64 | Block number when voting for this poll closes |
| `title` | String | Poll title |
| `description` | String | Poll description |
| `link`\* | String | URL to external post about poll \(forum, PDF, etc.\) |
| `deposit_amount` | Uint128 | ANC deposit used to submit poll |
| `execute_data`\* | `Vec<ExecuteMsg>` | List of governance messages to be issued upon poll execution |
| `yes_votes` | Uint128 | Total yes votes \(staked ANC amount\) for this poll |
| `no_votes` | Uint128 | Total no votes \(staked ANC amount\) for this poll |
| `total_balance_at_end_poll`\* | Uint128 | Poll's total votes \(staked ANC amount\) at the end of the poll |

| Name | Description |
| :--- | :--- |
| `InProgress` | Voting for this poll is currently in progress |
| `Passed` | This poll has been passed by governance |
| `Rejected` | This poll has been rejected by governance |
| `Executed` | This poll has been passed by governance and executed |
| `Expired` | This poll has been expired after rejection / execution |

| Name | Type | Description |
| :--- | :--- | :--- |
| `contract` | HumanAddr | Contract address of governance message recipient |
| `msg` | Binary | Base64-encoded JSON governance message |

\* = optional

### `Voters`

Gets voter information of the poll with the specified ID.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    Voters {
        poll_id: u64, 
        start_after: Option<HumanAddr>, 
        limit: Option<u32>, 
        order_by: Option<OrderBy>, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "voters": {
    "poll_id": 8, 
    "start_after": "terra1..", 
    "limit": 8, 
    "order_by": "asc" 
  }
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
| `poll_id` | u64 | ID of poll to query voters |
| `start_after`\* | HumanAddr | Address of voter to start query |
| `limit`\* | u32 | Maximum number of query entries |
| `order_by`\* | OrderBy | Order to make query |

| Name | Description |
| :--- | :--- |
| `Asc` | Make query in ascending order |
| `Desc` | Make query in descending order |

\* = optional

### `VotersResponse`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Debug, Clone, PartialEq, JsonSchema)]
pub struct VotersResponse {
    pub voters: Vec<VotersResponseItem>,
}

#[derive(Serialize, Deserialize, Debug, Clone, PartialEq, JsonSchema)]
pub struct VotersResponseItem {
    pub voter: HumanAddr,
    pub vote: VoteOption,
    pub balance: Uint128,
}

#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum VoteOption {
    Yes,
    No,
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "voters": [
    {
      "voter": "terra1...", 
      "vote": "yes", 
      "balance": "100000000" 
    }, 
    {
      "voter": "terra1...", 
      "vote": "no", 
      "balance": "100000000" 
    } 
  ]
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
| `voters` | `Vec<VotersResponseItem>` | List of voter information |

| Name | Type | Description |
| :--- | :--- | :--- |
| `voter` | HumanAddr | Address of voter |
| `vote` | `VoteOption` | Vote type made by voter |
| `balance` | Uint128 | Amount of staked ANC locked to vote this poll |

| Name | Description |
| :--- | :--- |
| `yes` | Voter has voted for the proposal |
| `no` | Voter has voted against the proposal |

