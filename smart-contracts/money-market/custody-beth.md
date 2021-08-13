# Custody \[bETH\]

The bETH Custody contract is where supplied bETH collaterals are managed. Users can make collateral deposits and withdrawals to and from this contract. The Custody contract is also responsible for claiming bETH rewards and converting them to Terra stablecoins, which is then sent to the Overseer contract for eventual distribution.

## Contract State

### Config

Stores information about the bETH Custody contract's config.

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner` | CanonicalAddr | Address of contract owner |
| `collateral_token` | CanonicalAddr | Contract address of bETH Token |
| `overseer_contract` | CanonicalAddr | Contract address of Overseer |
| `market_contract` | CanonicalAddr | Contract address of Market |
| `reward_contract` | CanonicalAddr | Contract address of bETH Reward |
| `liquidation_contract` | CanonicalAddr | Contract address of Liquidation Contract |
| `stable_denom` | String | Native token denomination for stablecoin |
| `basset_info` | BAssetInfo | bAsset token information |

### BorrowerInfo

Stores information about a borrower.

| Key | Type | Description |
| :--- | :--- | :--- |
| `balance` | Uint256 | Amount of bETH deposited as collateral |
| `spendable` | Uint256 | Amount of bETH that can be withdrawn \(not locked in loan\) |

## InitMsg

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub struct InitMsg {
    pub owner: HumanAddr, 
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
  "owner": "terra1...", 
  "collateral_token": "terra1...",
  "overseer_contract": "terra1...",
  "market_contract": "terra1...",
  "reward_contract": "terra1...",
  "liquidation_contract": "terra1...", 
  "stable_denom": "uusd", 
  "basset_info": {
    "name": "Bonded ETH", 
    "symbol": "BETH", 
    "decimals": 6 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner` | HumanAddr | Address of contract owner |
| `collateral_token` | HumanAddr | Contract address of bETH Token |
| `overseer_contract` | HumanAddr | Contract address of Overseer |
| `market_contract` | HumanAddr | Contract address of Market |
| `reward_contract` | HumanAddr | Contract address of bETH Reward |
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

Can be called during a CW20 token transfer when the Mint contract is the recipient. Allows the token transfer to execute a [Receive Hook](custody-bluna-specific.md#receive-hooks) as a subsequent action within the same transaction.

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
        owner: Option<HumanAddr>, 
        liquidation_contract: Option<HumanAddr>, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "update_config": {
    "owner": "terra1...", 
    "liquidation_contract": "terra1..." 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner`\* | HumanAddr | New address of contract owner |
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

Withdraws accrued rewards from the bETH Reward contract, swaps rewards to the appropriate stablecoin denomination. Can only be issued by `Overseer`.

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

### `WithdrawCollateral`

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
#### Request

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    Config {}
}
```

| Key | Type | Description |
| :--- | :--- | :--- |
|  |  |  |

#### Response

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct ConfigResponse {
    pub owner: HumanAddr, 
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

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner` | HumanAddr | Address of contract owner |
| `collateral_token` | HumanAddr | Contract address of bETH Token |
| `overseer_contract` | HumanAddr | Contract address of Overseer |
| `market_contract` | HumanAddr | Contract address of Market |
| `reward_contract` | HumanAddr | Contract address bETH Reward |
| `liquidation_contract` | HumanAddr | Contract address of Liquidation Contract |
| `stable_denom` | String | Native token denomination for stablecoin |
| `basset_info` | BAssetInfo | bAsset token information |

| Key | Type | Description |
| :--- | :--- | :--- |
| `name` | String | Name of bAsset token |
| `symbol` | String | Symbol of bAsset token |
| `decimals` | u8 | Number of decimals of bAsset Token |
{% endtab %}

{% tab title="JSON" %}
#### Request

```rust
{
  "config": {}
}
```

| Key | Type | Description |
| :--- | :--- | :--- |
|  |  |  |

#### Response

```rust
{
  "owner": "terra1...", 
  "collateral_token": "terra1...", 
  "overseer_contract": "terra1...", 
  "market_contract": "terra1...", 
  "reward_contract": "terra1...", 
  "liquidation_contract": "terra1...", 
  "stable_denom": "uusd", 
  "basset_info": {
    "name": "Bonded ETH", 
    "symbol": "BETH", 
    "decimals": 6 
  }
}
```

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner` | HumanAddr | Address of contract owner |
| `collateral_token` | HumanAddr | Contract address of bETH Token |
| `overseer_contract` | HumanAddr | Contract address of Overseer |
| `market_contract` | HumanAddr | Contract address of Market |
| `reward_contract` | HumanAddr | Contract address bETH Reward |
| `liquidation_contract` | HumanAddr | Contract address of Liquidation Contract |
| `stable_denom` | String | Native token denomination for stablecoin |
| `basset_info` | BAssetInfo | bAsset token information |

| Key | Type | Description |
| :--- | :--- | :--- |
| `name` | String | Name of bAsset token |
| `symbol` | String | Symbol of bAsset token |
| `decimals` | u8 | Number of decimals of bAsset Token |
{% endtab %}
{% endtabs %}

### `Borrower`

Gets the collateral balance of the specified borrower.

{% tabs %}
{% tab title="Rust" %}
#### Request

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    Borrower {
        address: HumanAddr, 
    }
}
```

| Key | Type | Description |
| :--- | :--- | :--- |
| `address` | HumanAddr | Address of borrower that deposited collateral |

#### Response

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct BorrowerResponse {
    pub borrower: HumanAddr, 
    pub balance: Uint256, 
    pub spendable: Uint256, 
}
```

| Key | Type | Description |
| :--- | :--- | :--- |
| `borrower` | HumanAddr | Address of borrower that deposited collateral |
| `balance` | Uint256 | Total amount of deposited collateral |
| `spendable` | Uint256 | Amount of spendable collateral |
{% endtab %}

{% tab title="JSON" %}
#### Request

```rust
{
  "borrower": { 
    "address": "terra1..." 
  }
}
```

| Key | Type | Description |
| :--- | :--- | :--- |
| `address` | HumanAddr | Address of borrower that deposited collateral |

#### Response

```rust
{
  "borrower": "terra1...", 
  "balance": "1000000000", 
  "spendable": "100000000" 
}
```

| Key | Type | Description |
| :--- | :--- | :--- |
| `borrower` | HumanAddr | Address of borrower that deposited collateral |
| `balance` | Uint256 | Total amount of deposited collateral |
| `spendable` | Uint256 | Amount of spendable collateral |
{% endtab %}
{% endtabs %}

### `Borrowers`

Get the collateral balance of all borrowers.

{% tabs %}
{% tab title="Rust" %}
#### Request

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

| Key | Type | Description |
| :--- | :--- | :--- |
| `start_after`\* | HumanAddr | Borrower address to start query |
| `limit`\* | u32 | Maximum number of query entries |

\* = optional

#### Response

```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct BorrowersResponse {
    pub borrowers: Vec<BorrowerResponse>, 
}

#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct BorrowerResponse {
    pub borrower: HumanAddr, 
    pub balance: Uint256, 
    pub spendable: Uint256, 
}
```

| Key | Type | Description |
| :--- | :--- | :--- |
| `borrowers` | Vec&lt;BorrowerResponse&gt; | Collateral balance information of borrowers |

| Key | Type | Description |
| :--- | :--- | :--- |
| `borrower` | HumanAddr | Address of borrower that deposited collateral |
| `balance` | Uint256 | Total amount of deposited collateral |
| `spendable` | Uint256 | Amount of spendable collateral |
{% endtab %}
{% endtabs %}

