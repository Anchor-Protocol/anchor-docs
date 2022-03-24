# Community

The Community Contract holds the funds of the [Community Pool](../../protocol/anchor-governance/README.md), which can be spent through a governance poll.&#x20;

## Config

| Name           | Type          | Description                            |
| -------------- | ------------- | -------------------------------------- |
| `gov_contract` | CanonicalAddr | Contract address of Gov                |
| `anchor_token` | CanonicalAddr | Contract address of Anchor Token (ANC) |
| `spend_limit`  | Uint128       | Upper cap on community grant size      |

## InstantiateMsg

:::::{tab-set}
::::{tab-item} Rust
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct InstantiateMsg {
    pub gov_contract: String, 
    pub anchor_token: String, 
    pub spend_limit: Uint128, 
}
```
::::

::::{tab-item} JSON
```javascript
{
  "gov_contract": "terra1...", 
  "anchor_token": "terra1...", 
  "spend_limit": "100000000000" 
}
```
::::
:::::

| Name           | Type    | Description                            |
| -------------- | ------- | -------------------------------------- |
| `gov_contract` | String  | Contract address of Gov                |
| `anchor_token` | String  | Contract address of Anchor Token (ANC) |
| `spend_limit`  | Uint128 | Upper cap on community grant size      |

## ExecuteMsg

### `UpdateConfig`

Updates the Collector contract configuration.

:::::{tab-set}
::::{tab-item} Rust
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum ExecuteMsg {
    UpdateConfig {
        spend_limit: Option<Uint128>, 
    }
}
```
::::

::::{tab-item} JSON
```javascript
{
  "update_config": {
    "spend_limit": "100000000000" 
  }
}
```
::::
:::::

| Name            | Type    | Description                           |
| --------------- | ------- | ------------------------------------- |
| `spend_limit`\* | Uint128 | New upper cap on community grant size |

\* = optional

### `Spend`

Transfers ANC tokens to the grant recipient. Can only be issued by the Gov contract.

:::::{tab-set}
::::{tab-item} Rust
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum ExecuteMsg {
    Spend {
        recipient: String, 
        amount: Uint128, 
    }
}
```
::::

::::{tab-item} JSON
```javascript
{
  "spend": {
    "recipient": "terra1...", 
    "amount": "100000000" 
  }
}
```
::::
:::::

| Name        | Type    | Description                  |
| ----------- | ------- | ---------------------------- |
| `recipient` | String  | Recipient of community grant |
| `amount`    | Uint128 | Community grant amount       |

## QueryMsg

### `Config`

Gets the Collector contract configuration.

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
    pub gov_contract: String,
    pub anchor_token: String,
    pub spend_limit: Uint128,
}
```
::::

::::{tab-item} JSON
```javascript
{
  "gov_contract": "terra1...", 
  "anchor_token": "terra1...", 
  "spend_limit": "100000000000" 
}
```
::::
:::::

| Name           | Type    | Description                            |
| -------------- | ------- | -------------------------------------- |
| `gov_contract` | String  | Contract address of Gov                |
| `anchor_token` | String  | Contract address of Anchor Token (ANC) |
| `spend_limit`  | Uint128 | Upper cap on community grant size      |
