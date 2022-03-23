# Staking

The Staking Contract contains the logic for LP Token staking and reward distribution. ANC tokens allocated for as liquidity incentives are distributed pro-rata to stakers of the ANC-UST Terraswap pair LP token.&#x20;

## Config

| Name                    | Type                     | Description                                                                                                            |
| ----------------------- | ------------------------ | ---------------------------------------------------------------------------------------------------------------------- |
| `anchor_token`          | CanonicalAddr            | Contract address of Anchor Token (ANC)                                                                                 |
| `staking_token`         | CanonicalAddr            | Contract address of ANC-UST Terraswap pair LP token                                                                    |
| `distribution_schedule` | Vec<(u64, u64, Uint128)> | ANC distribution schedule for LP token stakers (start block **\[block]**, end block **\[block]**, distribution amount) |

## InstantiateMsg

:::::{tab-set}
::::{tab-item} Rust
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct InstantiateMsg {
    pub anchor_token: String,
    pub staking_token: String,
    pub distribution_schedule: Vec<(u64, u64, Uint128)>, 
}
```
::::

::::{tab-item} JSON
```javascript
{
  "anchor_token": "terra1...", 
  "staking_token": "terra1...", 
  "distribution_schedule": [
    [123456, 234567, "100000000"], 
    [234567, 345678, "200000000"]
  ]
}
```
::::
:::::

| Name                    | Type                     | Description                                                                                                            |
| ----------------------- | ------------------------ | ---------------------------------------------------------------------------------------------------------------------- |
| `anchor_token`          | String                   | Contract address of Anchor Token (ANC)                                                                                 |
| `staking_token`         | String                   | Contract address of ANC-UST Terraswap pair LP token                                                                    |
| `distribution_schedule` | Vec<(u64, u64, Uint128)> | ANC distribution schedule for LP token stakers (start block **\[block]**, end block **\[block]**, distribution amount) |

## ExecuteMsg

### `Receive`

Can be called during a CW20 token transfer when the Staking contract is the recipient. Allows the token transfer to execute a [Receive Hook](staking.md#receive-hooks) as a subsequent action within the same transaction.

:::::{tab-set}
::::{tab-item} Rust
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum ExecuteMsg {
    Receive {
        sender: String, 
        amount: Uint128, 
        msg: Binary, 
    }
}
```
::::

::::{tab-item} JSON
```javascript
{
  "receive": {
    "amount": "10000000",
    "sender": "terra1...",
    "msg": "eyAiZXhlY3V0ZV9tc2ciOiAiYmluYXJ5IiB9"
  }
}
```
::::
:::::

| Name     | Type    | Description                                                               |
| -------- | ------- | ------------------------------------------------------------------------- |
| `sender` | String  | Sender of the token transfer                                              |
| `amount` | Uint128 | Amount of tokens received                                                 |
| `msg`    | Binary  | Base64-encoded string of JSON of [Receive Hook](staking.md#receive-hooks) |

### `Unbond`

Unbonds specified amount of ANC-UST Terraswap LP tokens and transfers them to the message sender.

:::::{tab-set}
::::{tab-item} Rust
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum ExecuteMsg {
    Unbond {
        amount: Uint128, 
    }
}
```
::::

::::{tab-item} JSON
```javascript
{
  "unbond": {
    "amount": "100000000"
  }
}
```
::::
:::::

| Name     | Type    | Description                   |
| -------- | ------- | ----------------------------- |
| `amount` | Uint128 | Amount of LP tokens to unbond |

### `Withdraw`

Withdraws user's accrued LP token staking rewards.

:::::{tab-set}
::::{tab-item} Rust
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum ExecuteMsg {
    Withdraw {}
}
```
::::

::::{tab-item} JSON
```javascript
{
  "withdraw": {}
}
```
::::
:::::

| Name | Type | Description |
| ---- | ---- | ----------- |
|      |      |             |

### `MigrateStaking`

Migrates ANC LP incentives to a new LP token staking contract.

:::::{tab-set}
::::{tab-item} Rust
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum ExecuteMsg {
    MigrateStaking {
        new_staking_contract: String, 
    }
}
```
::::

::::{tab-item} JSON
```javascript
{
  "migrate_staking": {
    "new_staking_contract": "terra1...", 
  }
}
```
::::
:::::

| Key                    | Type   | Description                                 |
| ---------------------- | ------ | ------------------------------------------- |
| `new_staking_contract` | String | Contract address of new LP staking contract |

## Receive Hooks

### `Bond`

::: {danger}
**WARNING**

Sending LP tokens to the Staking contract without issuing this hook will lead to **PERMANENT LOSS OF FUNDS**.
:::

Bonds LP tokens of the ANC-UST Terraswap pair.

:::::{tab-set}
::::{tab-item} Rust
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum Cw20HookMsg {
    Bond {}
}
```
::::

::::{tab-item} JSON
```javascript
{
  "bond": {}
}
```
::::
:::::

| Name | Type | Description |
| ---- | ---- | ----------- |
|      |      |             |

## QueryMsg

### `Config`

Gets the Staking contract configuration.

:::::{tab-set}
::::{tab-item} Rust
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    Config {}
}
```
::::

::::{tab-item} JSON
```javascript
{
  "config": {}
}
```
::::
:::::

| Name | Type | Description |
| ---- | ---- | ----------- |
|      |      |             |

### `ConfigResponse`

:::::{tab-set}
::::{tab-item} Rust
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct ConfigResponse {
    pub anchor_token: String,
    pub staking_token: String,
    pub distribution_schedule: Vec<(u64, u64, Uin128)>, 
}
```
::::

::::{tab-item} JSON
```javascript
{
  "anchor_token": "terra1...", 
  "staking_token": "terra1...", 
  "distribution_schedule": [
    [123456, 234567, "100000000"], 
    [234567, 345678, "200000000"]
  ]
}
```
::::
:::::

| Name                    | Type                     | Description                                                                                               |
| ----------------------- | ------------------------ | --------------------------------------------------------------------------------------------------------- |
| `anchor_token`          | String                   | Contract address of Anchor Token (ANC)                                                                    |
| `staking_token`         | String                   | Contract address of ANC-UST Terraswap pair LP token                                                       |
| `distribution_schedule` | Vec<(u64, u64, Uint128)> | ANC distribution schedule for LP token stakers (start block **\[block]**, end block **\[block]**, amount) |

### `State`

Gets state information for the specified block number.

:::::{tab-set}
::::{tab-item} Rust
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    State {
        block_height: Option<u64>, 
    }
}
```
::::

::::{tab-item} JSON
```javascript
{
  "state": {
    "block_height": 123456 
  }
}
```
::::
:::::

| Name             | Type | Description                       |
| ---------------- | ---- | --------------------------------- |
| `block_height`\* | u64  | Current block number **\[block]** |

\* = optional

### `StateResponse`

:::::{tab-set}
::::{tab-item} Rust
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct StateResponse {
    pub last_distributed: u64, 
    pub total_bond_amount: Uint128, 
    pub global_reward_index: Decimal,
}
```
::::

::::{tab-item} JSON
```javascript
{
  "last_distributed": 123456, 
  "total_bond_amount": "100000000", 
  "global_reward_index": "123.456" 
}
```
::::
:::::

| Name                  | Type    | Description                                                   |
| --------------------- | ------- | ------------------------------------------------------------- |
| `last_distributed`    | u64     | Block number when rewards where last distributed **\[block]** |
| `total_bond_amount`   | Uint128 | Total amount of bonded LP tokens by all stakers               |
| `global_reward_index` | Decimal | Global reward index for LP staking rewards                    |

### `StakerInfo`

Gets reward information for the specified LP token staker.

:::::{tab-set}
::::{tab-item} Rust
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    StakerInfo {
        staker: String, 
        block_height: Option<u64>, 
    }
}
```
::::

::::{tab-item} JSON
```javascript
{
  "staker_info": {
    "staker": "terra1...", 
    "block_height": 123456 
  }
}
```
::::
:::::

| Name             | Type   | Description                       |
| ---------------- | ------ | --------------------------------- |
| `staker`         | String | Address of LP token staker        |
| `block_height`\* | u64    | Current block number **\[block]** |

\* = optional

### `StakerInfoResponse`

:::::{tab-set}
::::{tab-item} Rust
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct StakerInfoResponse {
    pub staker: String,
    pub reward_index: Decimal,
    pub bond_amount: Uint128,
    pub pending_reward: Uint128,
}
```
::::

::::{tab-item} JSON
```javascript
{
  "staker": "terra1...", 
  "reward_index": "123.456", 
  "bond_amount": "100000000", 
  "pending_rewards": "100000000" 
}
```
::::
:::::

| Name              | Type    | Description                          |
| ----------------- | ------- | ------------------------------------ |
| `staker`          | String  | Address of LP token staker           |
| `reward_index`    | Decimal | Reward index of staker               |
| `bond_amount`     | Uint128 | Amount of LP tokens bonded by staker |
| `pending_rewards` | Uint128 | Amount of pending rewards of staker  |
