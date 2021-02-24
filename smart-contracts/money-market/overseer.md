# Overseer

The Overseer contract is responsible for storing key protocol parameters and the whitelisting of new bAsset collaterals. The borrow limit of users are calculated here, as the Overseer keeps track of locked collateral amounts for all users.

This contract is the recipient for collected bAsset rewards claimed by Custody contracts. The Overseer calculates the amount of depositor subsidies that has to be distributed, and the resulting amount is sent to the Market contract.

The Overseer halts borrow-related operations if the Oracle's price data is older than 60 seconds \(`price_timeframe`\). Operations are resumed when new price data is fed-in.

## Config

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner_addr` | CanonicalAddr | Address of contract owner that can update config |
| `oracle_contract` | CanonicalAddr | Contract address of Oracle |
| `market_contract` | CanonicalAddr | Contract address of Market |
| `liquidation_contract` | CanonicalAddr | Contract address of Liquidation Contract |
| `collector_contract` | CanonicalAddr | Contract address of Collector |
| `stable_denom` | String | Native token denomination for stablecoin |
| `epoch_period` | u64 | Minimum time delay between epoch operations |
| `threshold_deposit_rate` | Decimal256 | Threshold per-block deposit rate before triggering interest buffer distribution |
| `target_deposit_rate` | Decimal256 | Maximum per-block deposit rate before a portion of rewards are set aside as interest buffer |
| `buffer_distribution_factor` | Decimal256 | Maximum portion of interest buffer that can be distributed in an epoch |
| `anc_purchase_factor` | Decimal256 | Portion of bAsset rewards used to purchase ANC |
| `price_timeframe` | u64 | Window of time before price data is considered outdated |

## InitMsg

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub struct InitMsg {
    pub owner_addr: HumanAddr, 
    pub oracle_contract: HumanAddr, 
    pub market_contract: HumanAddr, 
    pub liquidation_contract: HumanAddr, 
    pub collector_contract: HumanAddr, 
    pub stable_denom: String, 
    pub epoch_period: u64, 
    pub threshold_deposit_rate: Decimal256, 
    pub target_deposit_rate: Decimal256, 
    pub buffer_distribution_factor: Decimal256, 
    pub anc_purchase_factor: Decimal256, 
    pub price_timeframe: u64, 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "owner_addr": "terra1...", 
  "oracle_contract": "terra1...", 
  "market_contract": "terra1...", 
  "liquidation_contract": "terra1...", 
  "collector_contract": "terra1...", 
  "stable_denom": "uusd", 
  "epoch_period": 86400, 
  "threshold_deposit_rate": "0.1", 
  "target_deposit_rate": "0.15", 
  "buffer_distribution_factor": "0.1", 
  "anc_purchase_factor": "0.5", 
  "price_timeframe": 60 
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner_addr` | HumanAddr | Address of contract owner that can update config |
| `oracle_contract` | HumanAddr | Contract address of Oracle |
| `market_contract` | HumanAddr | Contract address of Market |
| `liquidation_contract` | HumanAddr | Contract address of Liquidation Contract |
| `collector_contract` | HumanAddr | Contract address of Collector |
| `stable_denom` | String | Native token denomination for stablecoin |
| `epoch_period` | u64 | Minimum time delay between epoch operations |
| `threshold_deposit_rate` | Decimal256 | Threshold per-block deposit rate to trigger interest buffer distribution |
| `target_deposit_rate` | Decimal256 | Maximum per-block deposit rate before a portion of rewards are set aside as interest buffer |
| `buffer_distribution_factor` | Decimal256 | Maximum portion of interest buffer that can be distributed in an epoch |
| `anc_purchase_factor` | Decimal256 | Portion of bAsset rewards used to purchase ANC |
| `price_timeframe` | u64 | Window of time before price data is considered outdated |

## HandleMsg

### `UpdateConfig`

Updates the configuration of the contract. Can only be issued by the owner.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    UpdateConfig {
        owner_addr: Option<HumanAddr>, 
        oracle_contract: Option<HumanAddr>, 
        liquidation_contract: Option<HumanAddr>, 
        threshold_deposit_rate: Option<Decimal256>, 
        target_deposit_rate: Option<Decimal256>, 
        buffer_distribution_factor: Option<Decimal256>, 
        anc_purchase_factor: Option<Decimal256>, 
        epoch_period: Option<u64>, 
        price_timeframe: Option<u64>, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "update_config": { 
    "owner_addr": "terra1...", 
    "oracle_contract": "terra1...", 
    "liquidation_contract": "terra1...", 
    "threshold_deposit_rate": "0.1", 
    "target_deposit_rate": "0.15", 
    "buffer_distribution_factor": "0.1", 
    "anc_purchase_factor": "0.5", 
    "epoch_period": 86400, 
    "price_timeframe": 60 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner_addr`\* | HumanAddr | Address of new contract owner |
| `oracle_contract`\* | HumanAddr | Contract address of new Oracle |
| `liquidation_contract`\* | HumanAddr | Contract address of new Liquidation Contract |
| `threshold_deposit_rate`\* | Decimal256 | New threshold per-block deposit rate to trigger interest buffer distribution |
| `target_deposit_rate`\* | Decimal256 | New maximum per-block deposit rate before a portion of rewards are set aside as interest buffer |
| `buffer_distribution_factor`\* | Decimal256 | New maximum portion of interest buffer that can be distributed in an epoch |
| `anc_purchase_factor`\* | Decimal256 | New portion of bAsset rewards used to purchase ANC |
| `epoch_period`\* | u64 | New minimum time delay between epoch operations |
| `price_timeframe`\* | u64 | New window of time before price data is considered outdated |

\* = optional

### `Whitelist`

Whitelists a new collateral accepted in the money market. Can only be issued by the owner.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    Whitelist {
        name: String, 
        symbol: String, 
        collateral_token: HumanAddr, 
        custody_contract: HumanAddr, 
        max_ltv: Decimal256,  
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "whitelist": { 
    "name": "bonded luna", 
    "symbol": "ubluna", 
    "collateral_token": "terra1...", 
    "custody_contract": "terra1...", 
    "max_ltv": "0.75" 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `name` | String | Name of collateral bAsset |
| `symbol` | String | Token symbol of collateral bAsset |
| `collateral_token` | HumanAddr | Cw20 token contract address of collateral |
| `custody_contract` | HumanAddr | Custody contract address of collateral |
| `max_ltv` | Decimal256 | Maximum loan-to-value ratio allowed for collateral |

### `UpdateWhitelist`

Updates information for an already whitelisted collateral. Can only be issued by the owner.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    UpdateWhitelist {
        collateral_token: HumanAddr, 
        custody_contract: Option<HumanAddr>, 
        max_ltv: Option<Decimal256>,  
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "update_whitelist": {
    "collateral_token": "terra1...", 
    "custody_contract": "terra1...", 
    "max_ltv": "0.75" 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `collateral_token` | HumanAddr | Cw20 token contract address of collateral |
| `custody_contract`\* | HumanAddr | New Custody contract address of collateral |
| `max_ltv`\* | Decimal256 | New maximum loan-to-value ratio allowed for collateral |

\* = optional

### `ExecuteEpochOperations`

Executes epoch operations. Distributes interest buffers if necessary, and requests Custody contracts to claim bAsset rewards and distribute depositor subsidies.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    ExecuteEpochOperations {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "execute_epoch_operations": {}
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
|  |  |  |

### `[Internal] UpdateEpochState`

Updates state related to epoch operations. Can only be issued by `Overseer`.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    UpdateEpochState {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "update_epoch_state": {}
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
|  |  |  |

### `LockCollateral`

Locks specified amount of collateral deposited by message sender. Requests Custody contracts to reduce spendable collateral balance.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    LockCollateral {
        collaterals: TokensHuman, 
    }
}

pub type TokensHuman = Vec<(HumanAddr, Uint256)>;
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "lock_collateral": { 
    "collaterals": [
      ["terra1...", "100000000"], // (CW20 contract address, Amount to lock)
      ["terra1...", "100000000"] 
    ]
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `collaterals` | TokensHuman | List of collaterals and their lock amounts |

| Key | Type | Description |
| :--- | :--- | :--- |
| `TokensHuman` | Vec&lt;\(HumanAddr, Uint256\)&gt; | Vector of \(Collateral token address, Amount to lock\) |

### `UnlockCollateral`

Unlocks specified amount of collateral unlocked by message sender. Requests Custody contracts to increase spendable collateral balance.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    UnlockCollateral {
        collaterals: TokensHuman, 
    }
}

pub type TokensHuman = Vec<(HumanAddr, Uint256)>;
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "unlock_collateral": {
    "collaterals": [
      ["terra1...", "100000000"], // (CW20 contract address, Amount to unlock)
      ["terra1...", "100000000"] 
    ]
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `collaterals` | TokensHuman | List of collaterals and their unlock amounts |

| Key | Type | Description |
| :--- | :--- | :--- |
| `TokensHuman` | Vec&lt;\(HumanAddr, Uint256\)&gt; | Vector of \(Collateral token address, Amount to lock\) |

### `LiquidateCollateral`

Liquidates loan position of the specified borrower. Requests Custody contracts to process collateral liquidation.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    LiquidateCollateral {
        borrower: HumanAddr, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "liquidate_collateral": {
    "borrower": "terra1..."
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `borrower` | HumanAddr | Address of borrower to liquidate loan position |

## QueryMsg

### `Config`

Gets the configuration of the Overseer contract.

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

| Key | Type | Description |
| :--- | :--- | :--- |
|  |  |  |

### `ConfigResponse`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct ConfigResponse {
    pub owner_addr: HumanAddr, 
    pub oracle_contract: HumanAddr, 
    pub market_contract: HumanAddr, 
    pub liquidation_contract: HumanAddr, 
    pub collector_contract: HumanAddr, 
    pub threshold_deposit_rate: Decimal256, 
    pub target_deposit_rate: Decimal256, 
    pub buffer_distribution_factor: Decimal256, 
    pub anc_purchase_factor: Decimal256, 
    pub stable_denom: String, 
    pub epoch_period: u64, 
    pub price_timeframe: u64, 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "owner_addr": "terra1...", 
  "oracle_contract": "terra1...", 
  "market_contract": "terra1...", 
  "liquidation_contract": "terra1...", 
  "collector_contract": "terra1...", 
  "distribution_threshold": "0.1", 
  "target_deposit_rate": "0.15", 
  "buffer_distribution_rate": "0.1", 
  "anc_purchase_factor": "0.5", 
  "stable_denom": "uusd", 
  "epoch_period": 86400, 
  "price_timeframe": 60 
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner_addr` | HumanAddr | Address of contract owner |
| `oracle_contract` | HumanAddr | Contract address of Oracle |
| `market_contract` | HumanAddr | Contract address of Market |
| `liquidation_contract` | HumanAddr | Contract address of Liquidation Contract |
| `collector_contract` | HumanAddr | Contract address of Collector |
| `threshold_deposit_rate` | Decimal256 | Threshold per-block deposit rate before triggering interest buffer distribution |
| `target_deposit_rate` | Decimal256 | Maximum per-block deposit rate before a portion of rewards are set aside as interest buffer |
| `buffer_distribution_factor` | Decimal256 | Maximum portion of interest buffer that can be distributed in an epoch |
| `anc_purchase_factor` | Decimal256 | Portion of bAsset rewards used to purchase ANC |
| `stable_denom` | String | Native token denomination for stablecoin |
| `epoch_period` | u64 | Minimum time delay between epoch operations |
| `price_timeframe` | u64 | Window of time before price data is considered outdated |

### `EpochState`

Gets information related to the current epoch.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    EpochState {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "epoch_state": {}
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
|  |  |  |

### `EpochStateResponse`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct EpochState {
    pub deposit_rate: Decimal256, 
    pub prev_aterra_supply: Uint256, 
    pub prev_exchange_rate: Decimal256, 
    pub last_executed_height: u64, 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "deposit_rate": "0.13", 
  "prev_aterra_supply": "100000000", 
  "prev_exchange_rate": "1.2", 
  "last_executed_height": 123456 
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `deposit_rate` | Decimal256 | Average per-block deposit rate during the last epoch |
| `prev_aterra_supply` | Uint256 | Total aTerra supply at when epoch operations were last executed |
| `prev_exchange_rate` | Decimal256 | aTerra exchange rate when epoch operations were last executed |
| `last_executed_height` | u64 | Block number when epoch operations were last executed |

### `Whitelist`

Gets information about the specified collateral if the `collateral_token` field is filled. Gets information about all collaterals if the `collateral_token` field is not filled.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    Whitelist {
        collateral_token: Option<HumanAddr>, 
        start_after: Option<HumanAddr>, 
        limit: Option<u32>, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "whitelist": { 
    "collateral_token": null, 
    "start_after": "terra1...", 
    "limit": 3 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `collateral_token`\* | HumanAddr | Cw20 Token address of collateral to query information |
| `start_after`\* | HumanAddr | Collateral Cw20 Token address to start query |
| `limit`\* | u32 | Maximum number of query entries |

\* = optional

### `WhitelistResponse`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct WhitelistResponse {
    pub elems: Vec<WhitelistResponseElem>, 
}

#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct WhitelistResponseElem {
    pub name: String, 
    pub symbol: String, 
    pub max_ltv: Decimal256, 
    pub custody_contract: HumanAddr, 
    pub collateral_token: HumanAddr, 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "elems": [
    {
      "name": "bonded luna", 
      "symbol": "ubluna", 
      "max_ltv": "0.5", 
      "custody_contract": "terra1...", 
      "collateral_token": "terra1..." 
    }, 
    {
      "name": "bonded atom", 
      "symbol": "ubatom", 
      "max_ltv": "0.4", 
      "custody_contract": "terra1...", 
      "collateral_token": "terra1..." 
    }
  ]
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `elems` | Vec&lt;WhitelistResponseElem&gt; | Vector of whitelisted collateral information |

| Key | Type | Description |
| :--- | :--- | :--- |
| `name` | String | Name of bAsset collateral |
| `symbol` | String | Token symbol of bAsset collateral |
| `max_ltv` | Decimal256 | Loan-to-value ratio allowed for collateral |
| `custody_contract` | HumanAddr | Custody contract address of this collateral |
| `collateral_token` | HumanAddr | Cw20 Token contract address of this collateral |

### `Collaterals`

Gets locked collateral information for the specified borrower.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    Collaterals {
        borrower: HumanAddr, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "collaterals": {
    "borrower": "terra1..." 
  }
{
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `borrower` | HumanAddr | Address of borrower that locked collateral |

### `CollateralsResponse`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct CollateralsResponse {
    pub borrower: HumanAddr, 
    pub collaterals: TokensHuman, 
}

pub type TokensHuman = Vec<(HumanAddr, Uint256)>;
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "borrower": "terra1...", 
  "collaterals": [
    ["terra1...", "100000000"],  // (CW20 contract address, Locked amount)
    ["terra1...", "100000000"]
  ] 
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `borrower` | HumanAddr | Address of borrower that locked collateral |
| `collaterals` | TokensHuman | List of collaterals and locked amounts |

| Key | Type | Description |
| :--- | :--- | :--- |
| `TokensHuman` | Vec&lt;\(HumanAddr, Uint256\)&gt; | Vector of \(Collateral token address, Amount locked\) |

### `AllCollaterals`

Gets locked collateral information for all borrowers.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    AllCollaterals {
        start_after: Option<HumanAddr>, 
        limit: Option<u32>, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "all_collaterals": {
    "start_after": "terra1...", 
    "limit": 8 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `start_after`\* | HumanAddr | Borrower address of start query |
| `limit`\* | u32 | Maximum number of query entries |

\* = optional

### `AllCollateralsResponse`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct AllCollateralsResponse {
    pub all_collaterals: Vec<CollateralsResponse>, 
}

pub struct CollateralsResponse {
    pub borrower: HumanAddr, 
    pub collaterals: TokensHuman, 
}

pub type TokensHuman = Vec<(HumanAddr, Uint256)>;
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "all_collaterals": [
    {
      "borrower": "terra1...", 
      "collaterals": [
        ["terra1...", "100000000"], // (CW20 contract address, Locked amount)
        ["terra1...", "100000000"] 
      ]
    }, 
    {
      "borrower": "terra1...", 
      "collaterals": [
        ["terra1...", "100000000"], // (CW20 contract address, Locked amount)
        ["terra1...", "100000000"] 
      ]
    }
  ]
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `all_collaterals` | Vec&lt;CollateralsResponse&gt; | List of collaterals and locked amounts |

| Key | Type | Description |
| :--- | :--- | :--- |
| `borrower` | HumanAddr | Address of user that locked collateral |
| `collaterals` | TokensHuman | List of collaterals and locked amounts |

| Key | Type | Description |
| :--- | :--- | :--- |
| `TokensHuman` | Vec&lt;\(HumanAddr, Uint256\)&gt; | Vector of \(Contract address of collateral token, Locked amount\) |

### `DistributionParams`

Gets parameter information related to reward distribution.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    DistributionParams {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "distribution_params": {}
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
|  |  |  |

### `DistributionParamsResponse`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct DistributionParamsResponse {
    pub deposit_rate: Decimal256, 
    pub target_deposit_rate: Decimal256, 
    pub threshold_deposit_rate: Decimal256, 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "deposit_rate": "0.0000000013", 
  "target_deposit_rate": "0.0000000015", 
  "threshold_deposit_rate": "0.000000001" 
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `deposit_rate` | Decimal256 | Average per-block deposit rate during the last epoch |
| `target_deposit_rate` | Decimal256 | Maximum per-block deposit rate before a portion of rewards are set aside as interest buffer |
| `threshold_deposit_rate` | Decimal256 | Threshold per-block deposit rate before triggering interest buffer distribution |

### `BorrowLimit`

Gets the borrow limit for the specified borrower. Fails if the oracle price is expired.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    BorrowLimit {
        borrower: HumanAddr, 
        block_time: Option<u64>, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "borrow_limit": { 
    "borrower": "terra1...", 
    "block_time": 123456 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `borrower` | HumanAddr | Address of borrower |
| `block_time`\* | u64 | Current block timestamp |

\* = optional

### `BorrowLimitResponse`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct BorrowLimitResponse {
    pub borrower: HumanAddr, 
    pub borrow_limit: Uint128, 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "borrower": "terra1...", 
  "borrow_limit": "100000000" 
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `borrower` | HumanAddr | Address of borrower |
| `borrow_limit` | Uint256 | Borrow limit of borrower |

