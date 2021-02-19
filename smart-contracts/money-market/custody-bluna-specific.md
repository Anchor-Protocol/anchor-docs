# Custody \[bLUNA\]

The Custody contract is where supplied bAsset collaterals are managed. Users can make collateral deposits and withdrawals to and from this contract. The Custody contract is also responsible for claiming bAsset rewards and converting them to Terra stablecoins, which is then sent to the Overseer contract for eventual distribution.

## Config

| Key | Type | Description |
| :--- | :--- | :--- |
| `collateral_token` | CanonicalAddr | Contract address of bLuna Token |
| `overseer_contract` | CanonicalAddr | Contract address of Overseer |
| `market_contract` | CanonicalAddr | Contract address of Market |
| `reward_contract` | CanonicalAddr | Contract address of bLuna Reward |
| `liquidation_contract` | CanonicalAddr | Contract address of Liquidation Contract |
| `stable_denom` | String | Native token denomination for stablecoin |
| `basset_info` | BAssetInfo | bAsset token information |

## InitMsg

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub struct InitMsg {
    pub collateral_token: HumanAddr,
    pub overseer_contract: HumanAddr,
    pub market_contract: HumanAddr,
    pub reward_contract: HumanAddr,
    pub liquidation_contract: HumanAddr,
    pub stable_denom: String, 
    pub basset_info: BAssetInfo
}

#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct BAssetInfo {
    pub name: String,
    pub symbol: String,
    pub decimals: u8,
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "collateral_token": "terra1...",
  "overseer_contract": "terra1...",
  "market_contract": "terra1...",
  "reward_contract": "terra1...",
  "liquidation_contract": "terra1...", 
  "stable_denom": "uusd", 
  "basset_info": {
    "name": "bonded luna", 
    "symbol": "ubluna", 
    "decimals": 6 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `collateral_token` | HumanAddr | Contract address of bLuna Token |
| `overseer_contract` | HumanAddr | Contract address of Overseer |
| `market_contract` | HumanAddr | Contract address of Market |
| `reward_contract` | HumanAddr | Contract address of bLuna Reward |
| `liquidation_contract` | HumanAddr | Contract address of Liquidation Contract |
| `stable_denom` | String | Native token denomination for stablecoin |
| `basset_info` | BAssetInfo | bAsset token information |

| Key | Type | Description |
| :--- | :--- | :--- |
| `name` | String | Name of bAsset |
| `symbol` | String | Symbol of bAsset |
| `decimals` | u8 | Number of decimals of bAsset token |

## HandleMsg

### `Receive`

Can be called during a Cw20 token transfer when the Mint contract is the recipient. Allows the token transfer to execute a [Receive Hook](custody-bluna-specific.md#receive-hooks) as a subsequent action within the same transaction.

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

| Key | Type | Description |
| :--- | :--- | :--- |
| `amount` | Uint128 | Amount of tokens received |
| `sender` | HumanAddr | Sender of the token transfer |
| `msg`\* | Binary | Base64-encoded string of JSON of [Receive Hook](custody-bluna-specific.md#receive-hooks) |

\* = optional

### `UpdateConfig`

Updates the configuration of the Custody contract.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    UpdateConfig {
        liquidation_contract: Option<HumanAddr>, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "update_config": {
    "liquidation_contract": "terra1..." 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `liquidation_contract`\* | HumanAddr | New contract address of Liquidation Contract |

\* = optional

### `[Internal] LockCollateral`

Locks borrower's collateral to be used in their loan position, decreasing the amount of spendable collateral. Can only be issued by `Overseer`.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    LockCollateral {
        borrower: HumanAddr, 
        amount: Uint256, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "lock_collateral": { 
    "borrower": "terra1...", 
    "amount": "10000000"
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `borrower` | HumanAddr | Address of borrower locking collateral |
| `amount` | Uint256 | Amount of collateral to lock |

### `[Internal] UnlockCollateral`

Unlocks borrower's collateral from their loan position, increasing the amount of spendable collateral. Can only be issued by `Overseer`.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    UnlockCollateral {
        borrower: HumanAddr, 
        amount: Uint256, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "unlock_collateral": { 
    "borrower": "terra1...", 
    "amount": "10000000"
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `borrower` | HumanAddr | Address of borrower unlocking collateral |
| `amount` | Uint256 | Amount of collateral to unlock |

### `[Internal] DistributeRewards`

Withdraws accrued rewards from the bLuna Contract, swaps rewards to the appropriate stablecoin denomination. Can only be issued by `Overseer`.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    DistributeRewards {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "distribute_rewards": {} 
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
|  |  |  |

### `[Internal] DistributeHook`

Distributes swapped rewards to depositors by sending swapped rewards to `Market`. If the deposit rate during the last epoch is above the target deposit rate, then a portion of the rewards are set aside as an interest buffer, which are sent to `Overseer`. Can only be issued by `Custody`.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    DistributeHook {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "distribute_hook": {} 
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
|  |  |  |

### `[Internal] SwapToStableDenom`

Swaps claimed bAsset rewards to `stable_denom`. Can only be issued by `Custody`.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    SwapToStableDenom {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "swap_to_stable_denom": {} 
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
|  |  |  |

### `[Internal] LiquidateCollateral`

Liquidates specified amount of locked collateral. Can only be issued by `Overseer`.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    LiquidateCollateral {
        liquidator: HumanAddr, 
        borrower: HumanAddr, 
        amount: Uint256, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "liquidate_collateral": {
    "liquidator": "terra1...", 
    "borrower": "terra1...", 
    "amount": "100000000" 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `liquidator` | HumanAddr | Address of user that triggered liquidations |
| `borrower` | HumanAddr | Address of borrower being liquidated |
| `amount` | Uint256 | Amount of collateral to liquidate |

### `WithdrawCollateral // Not Updated`

{% hint style="info" %}
Collaterals have to be first unlocked in the [Overseer](overseer.md) before they can be withdrawn by the user.
{% endhint %}

Withdraws specified amount of spendable collateral. Withdraws all spendable collateral if the `amount` field is not filled.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    WithdrawCollateral {
        amount: Option<Uint256>, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "withdraw_collateral": { 
    "amount": "10000000"
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `amount`\* | Uint256 | Amount of collateral to withdraw |

\* = optional

## Receive Hooks

### `DepositCollateral`

{% hint style="info" %}
Deposited collaterals have to be locked in the [Overseer](overseer.md) before they can be utilized in a loan position.
{% endhint %}

Deposits collateral. Issued when a user sends bAsset tokens to the Custody contract.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum Cw20HookMsg {
    DepositCollateral {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "deposit_collateral": {}
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |


## QueryMsg

### `Config`

Gets the contract configuration of the Custody contract.

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
    pub collateral_token: HumanAddr, 
    pub overseer_contract: HumanAddr, 
    pub market_contract: HumanAddr, 
    pub reward_contract: HumanAddr, 
    pub liquidation_contract: HumanAddr, 
    pub stable_denom: String, 
    pub basset_info: BAssetInfo, 
}

#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct BAssetInfo {
    pub name: String,
    pub symbol: String,
    pub decimals: u8,
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "collateral_token": "terra1...", 
  "overseer_contract": "terra1...", 
  "market_contract": "terra1...", 
  "reward_contract": "terra1...", 
  "liquidation_contract": "terra1...", 
  "stable_denom": "uusd" 
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `collateral_token` | HumanAddr | Contract address of bLuna Token |
| `overseer_contract` | HumanAddr | Contract address of Overseer |
| `market_contract` | HumanAddr | Contract address of Market |
| `reward_contract` | HumanAddr | Contract address bLuna Reward |
| `liquidation_contract` | HumanAddr | Contract address of Liquidation Contract |
| `stable_denom` | String | Native token denomination for stablecoin |
| `basset_info` | BAssetInfo | bAsset token information |

| Key | Type | Description |
| :--- | :--- | :--- |
| `name` | String | Name of bAsset token |
| `symbol` | String | Symbol of bAsset token |
| `decimals` | u8 | Number of decimals of bAsset token |

### `Borrower`

Gets the collateral balance of the specified borrower.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    Borrower {
        address: HumanAddr, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "borrower": { 
    "address": "terra1..." 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `address` | HumanAddr | Address of borrower that deposited collateral |

### `BorrowerResponse`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct BorrowerResponse {
    pub borrower: HumanAddr, 
    pub balance: Uint256, 
    pub spendable: Uint256, 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "borrower": "terra1...", 
  "balance": "1000000000", 
  "spendable": "100000000" 
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `borrower` | HumanAddr | Address of borrower that deposited collateral |
| `balance` | Uint256 | Total amount of deposited collateral |
| `spendable` | Uint256 | Amount of spendable collateral |

### `Borrowers`

Get the collateral balance of all borrowers.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    Borrowers {
        start_after: Option<HumanAddr>, 
        limit: Option<u32>, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "borrowers": { 
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
| `limit`\* | u32 | Maximum number of query entries |

\* = optional

### `BorrowersResponse`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct BorrowersResponse {
    pub borrowers: Vec<BorrowerResponse>, 
}

pub struct BorrowerResponse {
    pub borrower: HumanAddr, 
    pub balance: Uint256, 
    pub spendable: Uint256, 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "borrowers": [
    {
      "borrower": "terra1...", 
      "balance": "2389476982", 
      "spendable": "2837492" 
    }, 
    {
      "borrower": "terra1...", 
      "balance": "2389476982", 
      "spendable": "2837492" 
    }
  ]
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `borrowers` | Vec&lt;BorrowerResponse&gt; | Collateral balance information of borrowers |

| Key | Type | Description |
| :--- | :--- | :--- |
| `borrower` | HumanAddr | Address of borrower that deposited collateral |
| `balance` | Uint256 | Total amount of deposited collateral |
| `spendable` | Uint256 | Amount of spendable collateral |

