# Hub

The Hub contract acts as the central hub for all minted bLuna. Native Luna tokens received from users are delegated from here, and undelegations from bLuna unbond requests are also handled from this contract. Rewards generated from delegations are withdrawn to the Reward contract, later distributed to bLuna holders.

## Config

| Key | Type | Description |
| :--- | :--- | :--- |
| `creator` | CanonicalAddr | Address of contract creator that is allowed to change config and parameters |
| `reward_contract`\* | CanonicalAddr | Contract address of bLuna Reward |
| `token_contract`\* | CanonicalAddr | Contract address of bLuna's Cw20 token contract |

\* = Set as `None` until an address is registered

## Parameters

| Key | Type | Description |
| :--- | :--- | :--- |
| `epoch_period` | u64 | Minimum time delay between undelegation batches |
| `underlying_coin_denom` | String | Underlying asset denomination of bAsset \(Luna\) |
| `unbonding_period` | u64 | Time required for the Hub contract to consider an undelegation batch to be fully undelegated \(past the unbonding period\) |
| `peg_recovery_fee` | Decimal | Fee applied to bLuna generation and redemption |
| `er_threshold` | Decimal | Minimum bLuna exchange rate before the peg recovery fee is applied |
| `reward_denom` | String | Native token denomination for distributed bLuna rewards |

## InitMsg

Instantiates the bLuna Hub contract. Adds specified validator to whitelist and bonds the creator's initial Luna deposit. The creator's initial Luna deposit ensures the bLuna supply to always be a high enough value to prevent rounding errors in the bLuna exchange rate calculation. 

{% tabs %}
{% tab title="Rust" %}
```rust
pub struct InitMsg {
    pub epoch_period: u64, 
    pub underlying_coin_denom: String, 
    pub unbonding_period: u64, 
    pub peg_recovery_fee: Decimal, 
    pub er_threshold: Decimal, 
    pub reward_denom: String, 
    pub validator: HumanAddr, 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "epoch_period": 1000000, 
  "underlying_coin_denom": "uluna", 
  "unbonding_period": 7000000, 
  "peg_recovery_fee": "0.001", 
  "er_threshold": "1.0", 
  "reward_denom": "uusd", 
  "validator": "terravaloper1..." 
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description`epoch_poer` |
| :--- | :--- | :--- |
| `epoch_period` | u64 | Minimum time delay between undelegation batches |
| `underlying_coin_denom` | String | Underlying asset denomination of bAsset \(Luna\) |
| `unbonding_period` | u64 | Time required for the Hub contract to consider an undelegation batch to be fully undelegated \(past the unbonding period\) |
| `peg_recovery_fee` | Decimal | Fee applied to bLuna generation and redemption |
| `er_threshold` | Decimal | Minimum bLuna exchange rate before the peg recovery fee is applied |
| `reward_denom` | String | Native token denomination for distributed bLuna rewards |
| `validator` | HumanAddr | Address of validator for initial whitelisting |

## HandleMsg

### `Receive`

Can be called during a CW20 token transfer when the Hub contract is the recipient. Allows the token transfer to execute a [Receive Hook](hub.md#receive-hooks) as a subsequent action within the same transaction.

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
| `msg`\* | Binary | Base64-encoded string of JSON of [Receive Hook](hub.md#receive-hooks) |

\* = optional

### `Bond`

Bonds luna to the specified validator and mints bLuna tokens to the message sender. Requires native Luna tokens to be sent to `Hub` beforehand.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    Bond {
        validator: HumanAddr, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "bond": { 
    "validator": "terravaloper1..." 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `validator` | HumanAddr | Address of validator to bond Luna |

### `UpdateGlobalIndex`

Distributes Luna delegation rewards to bLuna holders. Withdraws all accrued delegation rewards to the `Reward` contract and requests the `Reward` contract to update the global reward index value. Can be issued by anyone without restrictions.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    UpdateGlobalIndex {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "update_global_index": {}
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
|  |  |  |

### `WithdrawUnbonded`

Withdraws unbonded Luna. Requires an unbonding entry to have been made prior to the unbonding period.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg { 
    WithdrawUnbonded {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "withdraw_unbonded": {}
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
|  |  | ~~~~ |

### `[Internal] RegisterSubcontracts`

Registers the address of `Reward` and `Token` contract to the `Hub` contract. Can only be issued by the creator.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    RegisterSubcontracts {
        contract: Registration, 
        contract_address: HumanAddr, 
    }
}

pub enum Registration {
    Token, 
    Reward, 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "register_subcontracts": { 
    "contract": "Token", 
    "contract_address": "terra1..." 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `contract` | Registration | Contract to register |
| `contract_address` | HumanAddr | Contract address of contract being registered |

### `RegisterValidator`

Registers a new validator to the validator whitelist. Can only be issued by the creator.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    RegisterValidator {
        validator: HumanAddr, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "register_validator": { 
    "validator": "terravaloper1..." 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `validator` | HumanAddr | Address of validator to add to whitelist |

### `DeregisterValidator`

Deregisters a validator from the validator whitelist and redelegates all delegations to a randomly selected validator. Can only be issued by the creator.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    DeregisterValidator {
        validator: HumanAddr, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "deregister_validator": { 
    "validator": "terravaloper1..." 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `validator` | HumanAddr | Address of validator to remove from whitelist |

### `CheckSlashing`

Checks whether a slashing event occurred and updates state accordingly.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    CheckSlashing {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "check_slashing": {}
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
|  |  |  |

### `UpdateParams`

Updates parameters of the Hub contract. Can only be issued by the creator.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    UpdateParams {
        epoch_period: Option<u64>, 
        underlying_coin_denom: Option<String>, 
        unbonding_period: Option<u64>, 
        peg_recovery_fee: Option<Decimal>, 
        er_threshold: Option<Decimal>, 
        reward_denom: Option<String>, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "update_params": {
    "epoch_period": 260000, 
    "underlying_coin_denom": "uluna", 
    "unbonding_period": 1000000, 
    "peg_recovery_fee": "0.001", 
    "er_threshold": "1.0", 
    "reward_denom": "uusd" 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `epoch_period`\* | u64 | New minimum time delay between undelegation batches |
| `underlying_coin_denom`\* | String | New underlying asset denomination of bAsset \(Luna\) |
| `unbonding_period`\* | u64 | New time period required for the Hub contract to consider an undelegation batch to be fully undelegated \(past the unbonding period\) |
| `peg_recovery_fee`\* | Decimal | New fee applied to bLuna generation and redemption |
| `er_threshold`\* | Decimal | New minimum bLuna exchange rate before the peg recovery fee is applied |
| `reward_denom`\* | String | New native token denomination for distributed bLuna rewards |

\* = optional

### `UpdateConfig`

Updates the `Hub` contract configuration. Can only be issued by the creator.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    UpdateConfig {
        owner: HumanAddr, 
        reward_contract: HumanAddr, 
        token_contract: HumanAddr, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "update_config": {
    "owner": "terra1...", 
    "reward_contract": "terra1...", 
    "token_contract": "terra1..." 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner` | HumanAddr | Address of new owner |
| `reward_contract` | HumanAddr | New contract address of bLuna Reward |
| `token_contract` | HumanAddr | New contract address of bLuna Cw20 token |

## Receive Hooks

### `Unbond`

Burns received bLuna and unbonds a corresponding amount of Luna from a randomly chosen validator.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum Cw20HookMsg {
    Unbond {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "unbond": {}
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
|  |  |  |

## QueryMsg

### `Config`

Gets the `Hub` contract's configuration.

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
    pub owner: HumanAddr,
    pub reward_contract: Option<HumanAddr>,
    pub token_contract: Option<HumanAddr>,
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "owner": "terra1...", 
  "reward_contract": "terra1...", 
  "token_contract": "terra1..." 
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner` | HumanAddr | Address of contract owner |
| `reward_contract`\* | HumanAddr | Contract address of bLuna Reward |
| `token_contract`\* | HumanAddr | Contract address of bLuna's Cw20 token contract |

\* = Not returned if address not registered yet

### `State`

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
pub struct StateResponse {
    pub exchange_rate: Decimal, 
    pub total_bond_amount: Uint128, 
    pub last_index_modification: u64, 
    pub prev_hub_balance: Uint128, 
    pub actual_unbonded_amount: Uint128, 
    pub last_unbonded_time: u64, 
    pub last_processed_batch: u64, 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "exchange_rate": "0.99", 
  "total_bond_amount": "100000000", 
  "last_index_modification": 10000, 
  "prev_hub_balance": "100000000", 
  "actual_unbonded_amount": "10000000", 
  "last_unbonded_time": 10000, 
  "last_processed_batch": 10 
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `exchange_rate` | Decimal | Current bLuna &lt;&gt; Luna exchange rate |
| `total_bond_amount` | Uint128 | Total amount of Luna currently bonded by `Hub` |
| `last_index_modification` | u64 | Block timestamp when the global reward index was last updated |
| `prev_hub_balance` | Uint128 | `Hub`'s Luna balance when `WithdrawUnbonded` was lasted executed. Used to calculate the actual amount of unbonded Luna |
| `actual_unbonded_amount` | Uint128 | Amount of Luna released from undelegation since last undelegation batch release |
| `last_unbonded_time` | u64 | Block timestamp when a batch was last undelegated |
| `last_processed_batch` | u64 | Batch id of the most recently released batch |

### `WhitelistedValidators`

Gets the list of whitelisted validators.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    WhitelistedValidators {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "whitelisted_validators": {}
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
|  |  |  |

### `WhitelistedValidatorsResponse`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct WhitelistedvalidatorsResponse { 
    pub validators: Vec<HumanAddr>, 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "validators": [
    "terravaloper1...", 
    "terravaloper1...", 
    "terravaloper1..." 
  ]
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `validators` | Vec&lt;HumanAddr&gt; | Vector of whitelisted validator addresses |

### `CurrentBatch`

Gets information about the current undelegation batch.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    CurrentBatch {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "current_batch": {}
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
|  |  |  |

### `CurrentBatchResponse`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct CurrentBatchResponse { 
    pub id: u64, 
    pub requested_with_fee: Uint128, 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "id": 10, 
  "requested_with_fee": "100000" 
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `id` | u64 | Batch id of the current undelegation batch |
| `requested_with_fee` | Uint128 | Amount of \(fee-applied\) bLuna requested for undelegation in this batch |

### `WithdrawableUnbonded`

Gets the amount of undelegated Luna that is available for withdrawal \(unbonding requests past the unbonding period\).

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    WithdrawableUnbonded {
        address: HumanAddr, 
        block_time: u64, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "withdrawable_unbonded": {
    "address": "terra1...", 
    "block_time": 123456 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `address` | HumanAddr | Address of user that previously unbonded Luna via redeeming bLuna |
| `block_time` | u64 | Current block timestamp |

### `WithdrawableUnbondedResponse`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct WithdrawablUnbondedResponse {
    pub withdrawable: Uint128, 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "withdrawable": "100000000" 
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `withdrawable` | Uint128 | Amount of undelegated Luna available for withdrawal |

### `Parameters`

Gets parameter information.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    Parameters {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "parameters": {}
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
|  |  |  |

### `ParametersResponse`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct Parameters {
    pub epoch_period: u64, 
    pub underlying_coin_denom: String, 
    pub unbonding_period: u64, 
    pub peg_recovery_fee: Decimal, 
    pub er_threshold: Decimal, 
    pub reward_denom: String, 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "epoch_period": 260000, 
  "underlying_coin_denom": "uluna", 
  "unbonding_period": 1820000, 
  "peg_recovery_fee": "0.001", 
  "er_threshold": "1.0", 
  "reward_denom": "uusd" 
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `epoch_period` | u64 | Minimum time delay between undelegations |
| `underlying_coin_denom` | String | Underlying asset denomination of bAsset \(Luna\) |
| `unbonding_period` | u64 | Time required for the Hub contract to consider an undelegation batch to be fully undelegated |
| `peg_recovery_fee` | Decimal | Fee applied to bLuna generation and redemption |
| `er_threshold` | Decimal | Minimum bLuna exchange rate before the peg recovery fee is applied |
| `reward_denom` | String | Native token denomination for distributed bLuna rewards |

### `UnbondRequests`

Gets the list of Luna unbonding amounts being unbonded for the specified user.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    UnbondRequests {
        address: HumanAddr, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "unbond_requests": {
    "address": "terra1..." 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `address` | HumanAddr | Address of user that previously unbonded Luna by redeeming bLuna |

### `UnbondRequestsResponse`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct UnbondRequestsResponse {
    pub address: HumanAddr, 
    pub requests: UnbondRequest, 
}

pub type UnbondRequest = Vec<(u64, Uint128)>;
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "address": "terra1...", 
  "requests": [
    [7, "1000000"], 
    [8, "2000000"], 
    [9, "3000000"] 
  ]
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `address` | HumanAddr | Address of user that requested to unbond bLuna |
| `requests` | UnbondRequest | List of unbonding requests made by user |

| Key | Type | Description |
| :--- | :--- | :--- |
| `UnbondRequest` | Vec&lt;\(u64, Uint128\)&gt; | Vector of \(batch id, bLuna unbond amount\) |

### `AllHistory`

Gets the historical list of undelegation batch entries.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    AllHistory {
        start_from: Option<u64>, 
        limit: Option<u32>, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "all_history": {
    "start_from": 10, 
    "limit": 10 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `start_from`\* | u64 | Batch ID to start query |
| `limit`\* | u32 | Maximum number of query entries |

\* = optional

### `AllHistoryResponse`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct AllHistoryResponse {
    pub history: Vec<UnbondHistory>, 
}

pub struct UnbondHistory {
    pub batch_id: u64, 
    pub time: u64, 
    pub amount: Uint128, 
    pub withdraw_rate: Decimal, 
    pub released: bool, 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "history": [
    {
      "batch_id": 7, 
      "time": 100000, 
      "amount": "100000000", 
      "withdraw_rate": "0.99", 
      "released": true 
    }, 
    {
      "batch_id": 8, 
      "time": 150000, 
      "amount": "300000000", 
      "withdraw_rate": "0.98", 
      "released": false 
    } 
  ]
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `history` | Vec&lt;UnbondHistory&gt; | Vector of batch information |

| Key | Type | Description |
| :--- | :--- | :--- |
| `batch_id` | u64 | Batch ID |
| `time` | u64 | Block timestamp when this batch was undelegated |
| `amount` | Uint128 | \(Fee-applied\) amount of bLuna unbonded in this batch |
| `withdraw_rate` | Decimal | Conversion rate applied when users later withdraw from this batch |
| `released` | bool | Indicator on whether is batch is released \(processed as fully undelegated by the contract\) |

