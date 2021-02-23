# Market

The Market contract acts as the point of interaction for all lending and borrowing related activities. New stablecoin deposits are added to this contract's balance, while borrows are subtracted from the contract balance.

**10%** of accrued borrow interest \(`reserve_factor`\) is set aside as ANC purchase reserves, used to purchase ANC tokens via the Collector.

## Config

| Key | Type | Description |
| :--- | :--- | :--- |
| `contract_addr` | CanonicalAddr | Address of itself \(Market contract\) |
| `owner_addr` | CanonicalAddr | Address of contract owner that can update config |
| `aterra_contract` | CanonicalAddr | Contract address of aTerra |
| `interest_model` | CanonicalAddr | Contract address of Interest Model |
| `distribution_model` | CanonicalAddr | Contract address of Distribution Model |
| `overseer_contract` | CanonicalAddr | Contract address of Overseer |
| `collector_contract` | CanonicalAddr | Contract address of Collector |
| `faucet_contract` | CanonicalAddr | Contract address of Faucet |
| `stable_denom` | String | Native token denomination for stablecoin |
| `reserve_factor` | Decimal256 | Percentage of borrower interest set aside as ANC purchase reserves |
| `max_borrow_factor` | Decimal256 | Maximum portion of stablecoin liquidity available for borrows |

## InitMsg

Instantiates the money market Market contract. Requires the owner to make an initial deposit of 1 Terra stablecoin and mints 1 aTerra to the Market contract \(inaccessible\). The creator's initial stablecoin deposit ensures the aTerra supply to always be a high enough value to prevent rounding errors in the aTerra exchange rate calculation.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub struct InitMsg {
    pub owner_addr: HumanAddr, 
    pub stable_denom: String, 
    pub reserve_factor: Decimal256, 
    pub aterra_code_id: u64, 
    pub anc_emission_rate: Decimal256, 
    pub max_borrow_factor: Decimal256, 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "owner_addr": "terra1...", 
  "stable_denom": "uusd", // Terra USD
  "reserve_factor": "0.1", 
  "aterra_code_id": 5, 
  "anc_emission_rate": "0.05", 
  "max_borrow_factor": "0.95" 
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner_addr` | HumanAddr | Address of contract owner |
| `stable_denom` | String | Native token denomination for stablecoin |
| `reserve_factor` | Decimal256 | Portion of borrower interest set aside as reserves |
| `aterra_code_id` | u64 | Code ID for aTerra contract |
| `anc_emission_rate` | Decimal256 | Initial per-block ANC emission rate to borrowers |
| `max_borrow_factor` | Decimal256 | Maximum portion of stablecoin liquidity available for borrows |

## HandleMsg

### `Receive`

Can be called during a CW20 token transfer when the Mint contract is the recipient. Allows the token transfer to execute a [Receive Hook](market.md#receive-hooks) as a subsequent action within the same transaction.

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
    "msg": "eyAiZXhlY3V0ZV9tc2ciOiAiYmxhaCBibGFoIiB9"
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `amount` | Uint128 | Amount of tokens received |
| `sender` | HumanAddr | Sender of token transfer |
| `msg`\* | Binary | Base64-encoded string of JSON of [Receive Hook](market.md#receive-hooks) |

\* = optional

### `RegisterContracts`

Registers the addresses of other Money Market contracts. Can only be issued by the owner.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    RegisterContracts {
        overseer_contract: HumanAddr, 
        interest_model: HumanAddr, 
        distribution_model: HumanAddr, 
        collector_contract: HumanAddr, 
        faucet_contract: HumanAddr, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "register_contracts": {
    "overseer_contract": "terra1...", 
    "interest_model": "terra1...", 
    "distribution_model": "terra1...", 
    "collector_contract": "terra1...", 
    "faucet_contract": "terra1..." 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `overseer_contract` | HumanAddr | Contract address of Overseer |
| `interest_model` | HumanAddr | Contract address of Interest Model |
| `distribution_model` | HumanAddr | Contract address of Distribution Model |
| `collector_contract` | HumanAddr | Contract address of Collector |
| `faucet_contract` | HumanAddr | Contract address of Faucet |

### `[Internal] RegisterATerra`

Registers the contract address of `aTerra` Cw20 Token contract. Issued by `aTerra` after initialization.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    RegisterATerra {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "register_a_terra": {}
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
|  |  |  |

### `UpdateConfig`

Updates the configuration of the contract. Can be only issued by the owner.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    UpdateConfig {
        owner_addr: Option<HumanAddr>, 
        reserve_factor: Option<Decimal256>, 
        max_borrow_factor: Option<Decimal256>, 
        interest_model: Option<HumanAddr>, 
        distribution_model: Option<HumanAddr>, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "update_config": {
    "owner_addr": "terra1...", 
    "reserve_factor": "0.1", 
    "max_borrow_factor": "0.95", 
    "interest_model": "terra1...", 
    "distribution_model": "terra1..." 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner_addr`\* | HumanAddr | Address of new owner |
| `reserve_factor`\* | Decimal256 | New portion of borrower interest set aside as reserves |
| `max_borrow_factor`\* | Decimal256 | New maximum portion of stablecoin liquidity available for borrows |
| `interest_model`\* | HumanAddr | New interest model contract address |
| `distribution_model`\* | HumanAddr | New contract address of Distribution Model |

\* = optional

### `[Internal] RepayStableFromLiquidation`

Repays a liquidated loan using stablecoins gained from liquidated collaterals. Can only be issued by `Overseer`.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    RepayStableFromLiquidation {
        borrower: HumanAddr, 
        prev_balance: Uint256, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "repay_stable_from_liquidation": {
    "borrower": "terra1...", 
    "prev_balance": "1000000000" 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `borrower` | HumanAddr | Address of loan borrower |
| `prev_balance` | Uint256 | Balance of Market contract prior to collateral liquidation |

### `[Internal] ExecuteEpochOperations`

Adjusts the borrower ANC emission rate and sends accumulated ANC purchase reserves to Collector. Can only be issued by Overseer

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    ExecuteEpochOperations {
        target_deposit_rate: Decimal256, 
        deposit_rate: Decimal256, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "execute_epoch_operations": {
    "target_deposit_rate": "0.000000001", 
    "deposit_rate": "0.000000002" 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `target_deposit_rate` | Decimal256 | Target per-block deposit rate of Anchor |
| `deposit_rate` | Decimal256 | Calculated per-block deposit of the last epoch |

### `DepositStable`

Deposits stablecoins to Anchor. Requires stablecoins to be sent beforehand.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    DepositStable {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "deposit_stable": {}
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
|  |  |  |

### `BorrowStable`

Borrows stablecoins from Anchor.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    BorrowStable {
        borrow_amount: Uint256, 
        to: Option<HumanAddr>, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "borrow_stable": {
    "borrow_amount": "1000000000", 
    "to": "terra1..." 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `borrow_amount` | Uint256 | Amount of stablecoins to borrow |
| `to`\* | HumanAddr | Withdrawal address for borrowed stablecoins |

\* = optional

### `RepayStable`

Repays previous stablecoin liability. Requires stablecoins to be sent beforehand.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    RepayStable {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "repay_stable": {}
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
|  |  |  |

### `ClaimRewards`

Claims accrued ANC rewards. Can designate an optional recipient. Sends rewards to message sender if `to` is not specified.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    ClaimRewards {
        to: Option<HumanAddr>, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "claim_rewards": {
    "to": "terra1..." 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `to`\* | HumanAddr | Optional recipient of accrued ANC rewards |

\* = optional

## Receive Hooks

### `RedeemStable`

Redeems aTerra to their underlying stablecoins.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum Cw20HookMsg {
    RedeemStable {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "redeem_stable": {}
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
|  |  |  |

## QueryMsg

### `Config`

Gets the Market contract configuration.

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
    pub aterra_contract: HumanAddr, 
    pub interest_model: HumanAddr, 
    pub distribution_model: HumanAddr, 
    pub overseer_contract: HumanAddr, 
    pub collector_contract: HumanAddr, 
    pub faucet_contract: HumanAddr, 
    pub stable_denom: String, 
    pub reserve_factor: Decimal256, 
    pub max_borrow_factor: Decimal256, 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "owner_addr": "terra1...", 
  "aterra_contract": "terra1...", 
  "interest_model": "terra1...", 
  "distribution_model": "terra1...", 
  "overseer_contract": "terra1...", 
  "collector_contract": "terra1...", 
  "faucet_contract": "terra1...", 
  "stable_denom": "uusd", 
  "reserve_factor": "0.1", 
  "max_borrow_factor": "1.0" 
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner_addr` | HumanAddr | Address of contract owner |
| `aterra_contract` | HumanAddr | Contract address of aTerra |
| `interest_model` | HumanAddr | Contract address of Interest Model |
| `distribution_model` | HumanAddr | Contract address of Distribution Model |
| `overseer_contract` | HumanAddr | Contract address of Overseer |
| `collector_contract` | HumanAddr | Contract address of Collector |
| `faucet_contract` | HumanAddr | Contract address of Faucet |
| `stable_denom` | String | Native token denomination for stablecoin |
| `reserve_factor` | Decimal256 | Portion of borrower interest set aside as reserves |
| `max_borrow_factor` | Decimal256 | Maximum portion of stablecoin liquidity available for borrows |

### `State`

Gets information related to the overall state of `Market`.

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

| Key | Type | Description |
| :--- | :--- | :--- |
|  |  |  |

### `StateResponse`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct State {
    pub total_liabilites: Decimal256, 
    pub total_reserves: Decimal256, 
    pub last_interest_updated: u64, 
    pub last_reward_updated: u64, 
    pub global_interest_index: Decimal256, 
    pub global_reward_index: Decimal256, 
    pub anc_emission_rate: Decimal256, 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "total_liabilities": "123.456789", 
  "total_reserves": "12.3456789", 
  "last_interest_updated": 123456789, 
  "last_reward_updated": 123456789, 
  "global_interest_index": "1.23456789", 
  "global_reward_index": "123456.789", 
  "anc_emission_rate": "0.05"
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `total_liabilities` | Decimal256 | Total amount of liabilities of all borrowers |
| `total_reserves` | Decimal256 | Total amount of ANC purchase reserves |
| `last_interest_updated` | u64 | Block number when interest was last accrued |
| `last_reward_updated` | u64 | Block number when rewards were last accrued |
| `global_interest_index` | Decimal256 | Current global interest index |
| `global_reward_index` | Decimal256 | Current ANC global reward index |
| `anc_emission_rate` | Decimal256 | Current per-block ANC emission rate to borrowers |

### `EpochState`

Gets state information related to epoch operations. Returns an interest-accrued value if `block_height` field is filled. Returns the stored \(no interest accrued\) state if not filled.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    EpochState {
        block_height: Option<u64>, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "epoch_state": {
    "block_height": 123456 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `block_height`\* | u64 | Current block number |

\* = optional

### `EpochStateResponse`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct EpochStateResponse {
    pub exchange_rate: Decimal256, 
    pub aterra_supply: Uint256, 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "exchange_rate": "1.23", 
  "aterra_supply": "100000000" 
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `exchange_rate` | Decimal256 | Current aTerra exchange rate |
| `aterra_supply` | Uint256 | Current aTerra supply |

### `BorrowerInfo`

Gets information for the specified borrower. Returns an interest-and-reward-accrued value if `block_height` field is filled. Returns the stored \(no interest / reward accrued\) state if not filled.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    BorrowerInfo {
        borrower: HumanAddr, 
        block_height: Option<u64>, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "borrower_info": {
    "borrower": "terra1...", 
    "block_height": 123456 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `borrower` | HumanAddr | Address of borrower |
| `block_height`\* | u64 | Current block number |

\* = optional

### `BorrowerInfoResponse`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct BorrowInfoResponse {
    pub borrower: HumanAddr, 
    pub interest_index: Decimal256, 
    pub reward_index: Decimal256, 
    pub loan_amount: Uint256, 
    pub pending_rewards: Decimal256, 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "borrower": "terra1...", 
  "interest_index": "1.23456789", 
  "reward_index": "123.456789", 
  "loan_amount": "123456789", 
  "pending_rewards": "123456.789" 
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `borrower` | HumanAddr | Address of borrower |
| `interest_index` | Decimal256 | Interest index of borrower |
| `reward_index` | Decimal256 | ANC reward index of borrower |
| `loan_amount` | Uint256 | Amount of borrower's liability |
| `pending_rewards` | Decimal256 | Amount of ANC rewards accrued to borrower |

### `BorrowInfos`

Gets information for all borrowers.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    BorrowInfos {
        start_after: Option<HumanAddr>, 
        limit: Option<u32>, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "borrower_infos": {
    "start_after": "terra1...", 
    "limit": 10 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `start_after`\* | HumanAddr | Borrower address to start query |
| `limit`\* | u32 | Maximum number of entries to query |

\* = optional

### `BorrowerInfosResponse`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct BorrowerInfosResponse {
    pub borrower_infos: Vec<BorrowerInfoResponse>, 
}

#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct BorrowInfoResponse {
    pub borrower: HumanAddr, 
    pub interest_index: Decimal256, 
    pub reward_index: Decimal256, 
    pub loan_amount: Uint256, 
    pub pending_rewards: Decimal256, 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "borrower_infos": [
    {
      "borrower": "terra1...", 
      "interest_index": "1.23456789", 
      "reward_index": "123.456789", 
      "loan_amount": "123456789", 
      "pending_rewards": "123456.789" 
    }, 
    {
      "borrower": "terra1...", 
      "interest_index": "1.23456789", 
      "reward_index": "123.456789", 
      "loan_amount": "123456789", 
      "pending_rewards": "123456.789" 
    }  
  ]
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `borrower_infos` | Vec&lt;BorrowerInfoResponse&gt; | List of borrower information |

| Key | Type | Description |
| :--- | :--- | :--- |
| `borrower` | HumanAddr | Address of borrower |
| `interest_index` | Decimal256 | Interest index of borrower |
| `reward_index` | Decimal256 | ANC reward index of borrower |
| `loan_amount` | Uint256 | Amount of borrower's liability |
| `pending_rewards` | Decimal256 | Amount of ANC rewards accrued to borrower |

