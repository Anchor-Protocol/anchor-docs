# Distributor

## Config

| Key            | Type                | Description                                                |
| -------------- | ------------------- | ---------------------------------------------------------- |
| `gov_contract` | CanonicalAddr       | Contract address of Gov                                    |
| `anchor_token` | CanonicalAddr       | Contract address of ANC token                              |
| `whitelist`    | Vec\<CanonicalAddr> | List of addresses permissioned to spend ANC in Distributor |
| `spend_limit`  | Uint128             | Maximum amount of ANC spendable per spend event            |

## InstantiateMsg

:::::{tab-set}
::::{tab-item} Rust
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct InstantiateMsg {
    pub gov_contract: String,   // anchor gov contract
    pub anchor_token: String,   // anchor token address
    pub whitelist: Vec<String>, // whitelisted contract addresses allowed to spend from distributor 
    pub spend_limit: Uint128,      // spend limit per each `spend` request
}
```
::::

::::{tab-item} JSON
```javascript
{
  "gov_contract": "terra1...", 
  "anchor_token": "terra1...", 
  "whitelist": [
    "terra1...", 
    "terra1...", 
    "terra1..." 
  ], 
  "spend_limit": "100000000"
}
```
::::
:::::

| Key            | Type         | Description                                                |
| -------------- | ------------ | ---------------------------------------------------------- |
| `gov_contract` | String       | Contract address of Gov                                    |
| `anchor_token` | String       | Contract address of ANC token                              |
| `whitelist`    | Vec\<String> | List of addresses permissioned to spend ANC in Distributor |
| `spend_limit`  | Uint128      | Maximum amount of ANC spendable per spend event            |

## ExecuteMsg

### `UpdateConfig`

Updates the Distributor contract configuration. Can only be issued by the Gov contract.

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
    "spend_limit": "100000000" 
  }
}
```
::::
:::::

| Key             | Type    | Description                                      |
| --------------- | ------- | ------------------------------------------------ |
| `spend_limit`\* | Uint128 | Maximum amount of ANC spendable per spend event  |

\* = optional

### `Spend`

Spends ANC in Distributor. Can only be issued by whitelisted addresses.

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

| Key         | Type    | Description                    |
| ----------- | ------- | ------------------------------ |
| `recipient` | String  | Recipient address of ANC spend |
| `amount`    | Uint128 | ANC amount to receive          |

### `AddDistributor`

Adds a new ANC distribution contract to the whitelist. Can only be issued by the Gov contract.

:::::{tab-set}
::::{tab-item} Rust
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum ExecuteMsg { 
    AddDistributor {
        distributor: String, 
    }
}
```
::::

::::{tab-item} JSON
```javascript
{
  "add_distributor": {
    "distributor": "terra1..." 
  }
}
```
::::
:::::

| Key           | Type   | Description                                          |
| ------------- | ------ | ---------------------------------------------------- |
| `distributor` | String | Contract address of ANC distribution contract to add |

### `RemoveDistributor`

Removes a ANC distribution contract from the whitelist. Can only be issued by the Gov contract.

:::::{tab-set}
::::{tab-item} Rust
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum ExecuteMsg { 
    RemoveDistributor {
        distributor: String, 
    }
}
```
::::

::::{tab-item} JSON
```javascript
{
  "remove_distributor": {
    "distributor": "terra1..." 
  }
}
```
::::
:::::

| Key           | Type   | Description                                             |
| ------------- | ------ | ------------------------------------------------------- |
| `distributor` | String | Contract address of ANC distribution contract to remove |

## QueryMsg

### `Config`

Gets the Distributor contract configuration.

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

| Key | Type | Description |
| --- | ---- | ----------- |
|     |      |             |

### `ConfigResponse`

:::::{tab-set}
::::{tab-item} Rust
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct ConfigResponse {
    pub gov_contract: String,
    pub anchor_token: String,
    pub whitelist: Vec<String>,
    pub spend_limit: Uint128,
}
```
::::

::::{tab-item} JSON
```javascript
{
  "gov_contract": "terra1...", 
  "anchor_token": "terra1...", 
  "whitelist": [
    "terra1...", 
    "terra1...", 
    "terra1..." 
  ], 
  "spend_limit": "100000000"
}
```
::::
:::::

| Key            | Type         | Description                                                |
| -------------- | ------------ | ---------------------------------------------------------- |
| `gov_contract` | String       | Contract address of Gov                                    |
| `anchor_token` | String       | Contract address of ANC Token                              |
| `whitelist`    | Vec\<String> | List of addresses permissioned to spend ANC in Distributor |
| `spend_limit`  | Uint128      | Maximum amount of ANC spendable per spend event            |
