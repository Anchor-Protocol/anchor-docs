# Market

The Market contract acts as the point of interaction for all lending and borrowing related activities. New stablecoin deposits are added to this contract's balance, while borrows are subtracted from the contract balance.

**10%** of accrued borrow interest \(`reserve_factor`\) is set aside as aToken liquidity reserves, for fluid aToken redemptions even in cases of high borrow demand.

## Config

| Key | Type | Description |
| :--- | :--- | :--- |
| `contract_addr` | CanonicalAddr | Address of itself \(Market contract\) |
| `owner_addr` | CanonicalAddr | Address of contract owner that can update config |
| `anchor_token` | CanonicalAddr | Contract address of aToken |
| `interest_model` | CanonicalAddr | Contract address of Interest Model |
| `overseer_contract` | CanonicalAddr | Contract address of Overseer |
| `stable_denom` | String | Native token denomination for stablecoin |
| `reserve_factor` | Decimal256 | Percentage of borrower interest set aside as aToken reserves |

## InitMsg

Instantiates the money market Market contract. Requires the owner to make an initial deposit of 1 Terra stablecoin and mints 1 aToken to the Market contract \(inaccessible\). The creator's initial stablecoin deposit ensures the aToken supply to always be a high enough value to prevent rounding errors in the aToken exchange rate calculation.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub struct InitMsg {
    pub owner_addr: HumanAddr, 
    pub interest_model: HumanAddr, 
    pub stable_denom: String, 
    pub reserve_factor: Decimal256, 
    pub anchor_token_code_id: u64, 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "owner_addr": "terra1...", 
  "interest_model": "terra1...", 
  "stable_denom": "uusd", // Terra USD
  "reserve_factor": "0.1", 
  "anchor_token_code_id": 5 
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner_addr` | HumanAddr | Address of contract owner |
| `interest_model` | HumanAddr | Contract address of Interest Model |
| `stable_denom` | String | Native token denomination for stablecoin |
| `reserve_factor` | Decimal256 | Portion of borrower interest set aside as reserves |
| `anchor_token_code_id` | u64 | Code ID for aToken contract |

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

### `RegisterOverseer`

Registers the contract address of `Overseer`. Can only be issued by the owner.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    RegisterOverseer {
        overseer_contract: HumanAddr, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "register_overseer": {
    "overseer_contract": "terra1..."
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `overseer_contract` | HumanAddr | Contract address of Overseer |

### `[Internal] RegisterAnchorToken`

Registers the contract address of `aToken`. Issued by `aToken` after initialization.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    RegisterAnchorToken {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "register_anchor_token": {}
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
        interest_model: Option<HumanAddr>, 
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
    "interest_model": "terra1..."
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner_addr`\* | HumanAddr | Address of new owner |
| `reserve_factor`\* | Decimal256 | New portion of borrower interest set aside as reserves |
| `interest_model`\* | HumanAddr | New interest model contract address |

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

## Receive Hooks

### `RedeemStable`

Redeems aTokens to their underlying stablecoins.

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
    pub anchor_token: HumanAddr, 
    pub interest_model: HumanAddr, 
    pub overseer_contract: HumanAddr, 
    pub stable_denom: String, 
    pub reserve_factor: Decimal256, 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "owner_addr": "terra1...", 
  "anchor_token": "terra1...", 
  "interest_model": "terra1...", 
  "overseer_contract": "terra1...", 
  "stable_denom": "uusd", 
  "reserve_factor": "0.1" 
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner_addr` | HumanAddr | Address of contract owner |
| `anchor_token` | HumanAddr | Contract address of aToken |
| `interest_model` | HumanAddr | Contract address of Interest Model |
| `overseer_contract` | HumanAddr | Contract address of Overseer |
| `stable_denom` | String | Native token denomination for stablecoin |
| `reserve_factor` | Decimal256 | Portion of borrower interest set aside as reserves |

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
    pub global_interest_index: Decimal256, 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "total_liabilities": "123.456789", 
  "total_reserves": "12.3456789", 
  "last_interest_updated": 123456789, 
  "global_interest_index": "1.23456789" 
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `total_liabilities` | Decimal256 | Total amount of liabilities of all borrowers |
| `total_reserves` | Decimal256 | Total amount of aToken reserves |
| `last_interest_updated` | u64 | Block number when interest was last accrued |
| `global_interest_index` | Decimal256 | Current global interest index |

### `EpochState`

Gets state information related to epoch operations. Returns the interest-accrued `block_height` field is filled. Returns the stored \(no interest accrued\) state if not filled.

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
    pub a_token_supply: Uint256, 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "exchange_rate": "1.23", 
  "a_token_supply": "100000000" 
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `exchange_rate` | Decimal256 | Current aToken exchange rate |
| `a_token_supply` | Uint256 | Current aToken supply |

### `Liability`

Gets liability information for the specified borrower

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    Liability {
        borrower: HumanAddr, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "liability": {
    "borrower": "terra1..."
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `borrower` | HumanAddr | Address of borrower |

### `LiabilityResponse`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct LiabilityResponse {
    pub borrower: HumanAddr, 
    pub interest_index: Decimal256, 
    pub loan_amount: Uint256, 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "borrower": "terra1...", 
  "interest_index": "1.23456789", 
  "loan_amount": "123456789" 
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `borrower` | HumanAddr | Address of borrower |
| `interest_index` | Decimal256 | Interest index of borrower |
| `loan_amount` | Uint256 | Amount of borrower's liability |

### `Liabilities`

Gets liability information for all borrowers.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    Liabilites {
        start_after: Option<HumanAddr>, 
        limit: Option<u32>, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "liabilites": {
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

### `LiabilitiesResponse`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct LiabilitiesResponse {
    pub liabilities: Vec<LiabilityResponse>, 
}

pub struct LiabilityResponse {
    pub borrower: HumanAddr, 
    pub interest_index: Decimal256, 
    pub loan_amount: Uint128, 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "liabilities": [
    {
      "borrower": "terra1...", 
      "interest_index": "1.387", 
      "loan_amount": "2840753" 
    }, 
    {
      "borrower": "terra1...", 
      "interest_index": "1.387", 
      "loan_amount": "2840753" 
    } 
  ]
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `liabilities` | Vec&lt;LiabilityResponse&gt; | Liability information of borrowers |

| Key | Type | Description |
| :--- | :--- | :--- |
| `borrower` | HumanAddr | Address of borrower |
| `interest_index` | Decimal256 | Interest index of borrower |
| `loan_amount` | Uint256 | Amount of borrower's liability |

### `LoanAmount`

Gets the liability amount for the specified borrower at the specified block number.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    LoanAmount {
        borrower: HumanAddr, 
        block_height: u64, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "loan_amount": {
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
| `block_height` | u64 | Block number to apply in calculation |

### `LoanAmountResponse`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct LoanAmountResponse {
    pub borrower: HumanAddr, 
    pub loan_amount: Uint256, 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "borrower": "terra1...", 
  "loan_amount": "100000000" 
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `borrower` | HumanAddr | Address of borrower |
| `loan_amount` | Uint256 | Amount of borrower's liability at the specified block |

