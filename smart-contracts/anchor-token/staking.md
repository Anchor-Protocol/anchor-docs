# Staking

The Staking Contract contains the logic for LP Token staking and reward distribution. ANC tokens allocated for as liquidity incentives are distributed pro-rata to stakers of the ANC &lt;&gt; UST Terraswap pair LP token. 

## Config

| Name | Type | Description |
| :--- | :--- | :--- |
| `anchor_token` | CanonicalAddr | Contract address of Anchor Token \(ANC\) |
| `staking_token` | CanonicalAddr | Contract address of ANC &lt;&gt; UST Terraswap pair LP token |

## InitMsg

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct InitMsg {
    pub anchor_token: HumanAddr,
    pub staking_token: HumanAddr,
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "anchor_token": "terra1...", 
  "staking_token": "terra1..." 
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
| `anchor_token` | HumanAddr | Contract address of Anchor Token \(ANC\) |
| `staking_token` | HumanAddr | Contract address of ANC &lt;&gt; UST Terraswap pair LP token |

## HandleMsg

### `Receive`

Can be called during a CW20 token transfer when the Staking contract is the recipient. Allows the token transfer to execute a [Receive Hook](staking.md#receive-hooks) as a subsequent action within the same transaction.

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
| `sender` | HumanAddr | Sender of the token transfer |
| `msg`\* | Binary | Base64-encoded string of JSON of [Receive Hook](staking.md#receive-hooks) |

\* = optional

### `Unbond`

Unbonds specified amount of ANC &lt;&gt; UST Terraswap LP tokens and transfers them to the message sender.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    Unbond {
        amount: Uint128, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "unbond": {
    "amount": "100000000"
  }
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
| `amount` | Uint128 | Amount of LP tokens to unbond |

### `Withdraw`

Withdraws user's accrued LP token staking rewards.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    Withdraw {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "withdraw": {}
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
|  |  |  |

## Receive Hooks

### `Bond`

{% hint style="danger" %}
**WARNING**

Sending LP tokens to the Staking contract without issuing this hook will lead to **PERMANENT LOSS OF FUNDS**.
{% endhint %}

Bonds LP tokens of the ANC &lt;&gt; UST Terraswap pair.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum Cw20HookMsg {
    Bond {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "bond": {}
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
|  |  |  |

### `DepositReward`

Deposits and distributes ANC rewards to LP token stakers.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum Cw20HookMsg {
    DepositReward {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "deposit_reward": {}
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
|  |  |  |

## QueryMsg

### `Config`

Gets the Staking contract configuration.

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
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct ConfigResponse {
    pub anchor_token: HumanAddr,
    pub staking_token: HumanAddr,
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "anchor_token": "terra1...", 
  "staking_token": "terra1..." 
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
| `anchor_token` | HumanAddr | Contract address of Anchor Token \(ANC\) |
| `staking_token` | HumanAddr | Contract address of ANC &lt;&gt; UST Terraswap pair LP token |

### `PoolInfo`

Gets LP token staking information.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    PoolInfo {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "pool_info": {}
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
|  |  |  |

### `PoolInfoResponse`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct PoolInfoResponse {
    pub total_bond_amount: Uint128,
    pub reward_index: Decimal,
    pub pending_reward: Uint128,
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "total_bond_amount": "100000000", 
  "reward_index": "123.456", 
  "pending_reward": "100000000" 
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
| `total_bond_amount` | Uint128 | Total amount of bonded LP tokens by all stakers |
| `reward_index` | Decimal | Global reward index for LP staking rewards |
| `pending_reward` | Uint128 | Total amount of deposited rewards yet to be distributed |

### `RewardInfo`

Gets reward information for the specified LP token staker.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    RewardInfo {
        staker: HumanAddr, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "reward_info": {
    "staker": "terra1..." 
  }
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
| `staker` | HumanAddr | Address of LP token staker |

### `RewardInfoResponse`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct RewardInfoResponse {
    pub staker: HumanAddr,
    pub index: Decimal,
    pub bond_amount: Uint128,
    pub pending_reward: Uint128,
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "staker": "terra1...", 
  "index": "123.456", 
  "bond_amount": "100000000", 
  "pending_rewards": "100000000" 
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
| `staker` | HumanAddr | Address of LP token staker |
| `index` | Decimal | Reward index of staker |
| `bond_amount` | Uint128 | Amount of LP tokens bonded by staker |
| `pending_rewards` | Uint128 | Amount of pending rewards of staker |

