# Vesting

The vesting contract handles the ANC lockup and vesting for the Investor and Team ANC allocations.

## Config

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner` | CanonicalAddr | Address of contract owner |
| `anchor_token` | CanonicalAddr | Contract address of Anchor Token \(ANC\) |
| `genesis_time` | u64 | Block timestamp when Anchor Protocol launched |

## InitMsg

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct InitMsg {
    pub owner: HumanAddr,
    pub anchor_token: HumanAddr,
    pub genesis_time: u64,
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "owner": "terra1...", 
  "anchor_token": "terra1...", 
  "genesis_time": 123456, 
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner` | HumanAddr | Address of contract owner |
| `anchor_token` | HumanAddr | Contract address of Anchor Token \(ANC\) |
| `genesis_time` | u64 | Block timestamp when Anchor Protocol launched |

## HandleMsg

### `UpdateConfig`

Updates the Vesting contract configuration. Can only be issued by contract owner. 

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    UpdateConfig {
        owner: Option<HumanAddr>, 
        anchor_token: Option<HumanAddr>, 
        genesis_time: Option<u64> 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "update_config": {
    "owner": "terra1...", 
    "anchor_token": "terra1...", 
    "genesis_time": 123456 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner`\* | HumanAddr | Address of new contract owner |
| `anchor_token`\* | HumanAddr | New contract address of Anchor Token \(ANC\) |
| `genesis_time`\* | u64 | New block timestamp when Anchor Protocol launched |

\* = optional

### `RegisterVestingAccounts`

Registers a new vesting account to the Vesting contract.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    RegisterVestingAccounts {
        vesting_accounts: Vec<VestingAccount>, 
    }
}

#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct VestingAccount {
    pub address: HumanAddr,
    pub schedules: Vec<(u64, u64, Uint128)>,
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "register_vesting_accounts": {
    "vesting_accounts": [
      {
        "address": "terra1...", 
        "schedules": [123456, 234567, "10000000"]
      }, 
      {
        "address": "terra1...", 
        "schedules": [123456, 234567, "10000000"]
      }
    ]
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `vesting_accounts` | Vec&lt;VestingAccount&gt; | List of accounts with ANC vesting and their vesting schedules |

| Key | Type | Description |
| :--- | :--- | :--- |
| `address` | HumanAddr | Address with vested ANC |
| `schedules` | Vec&lt;\(u64, u64, Uint128\)&gt; | Vesting schedule of \(ANC claimable start time, end time, total vested amount\) |

### `Claim`

Claims vested ANC tokens. Can only be issued by an account with vested ANC.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg {
    Claim {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "claim": {} 
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
|  |  |  |

## QueryMsg

### `Config`

Gets the Vesting contract configuration.

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
    pub anchor_token: HumanAddr,
    pub genesis_time: u64,
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "owner": "terra1...", 
  "anchor_token": "terra1...", 
  "genesis_time": 123456 
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner` | HumanAddr | Address of contract owner |
| `anchor_token` | HumanAddr | Contract address of Anchor Token \(ANC\) |
| `genesis_time` | u64 | Block timestamp when Anchor Protocol launched |

### `VestingAccount`

Gets information for an account with vested ANC.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    VestingAccount {
        address: HumanAddr, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "vesting_account": {
    "address": "terra1..." 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `address` | HumanAddr | Address with ANC vested |

### `VestingAccountResponse`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct VestingAccountResponse {
    pub address: HumanAddr,
    pub info: VestingInfo,
}

#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct VestingInfo {
    pub schedules: Vec<(u64, u64, Uint128)>,
    pub last_claim_time: u64,
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "address": "terra1...", 
  "info": {
    "schedules": [123456, 234567, "10000000"], 
    "last_claim_time": 123456, 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `address` | HumanAddr | Address with ANC vested |
| `info` | VestingInfo | Vesting information |

| Key | Type | Description |
| :--- | :--- | :--- |
| `schedules` | Vec&lt;\(u64, u64, Uint128\)&gt; | Vesting schedule of \(ANC claimable start time, end time, total vested amount\) |
| `last_claim_time` | u64 | Block timestamp when this account last claimed ANC |

### `VestingAccounts`

Gets information for accounts with vested ANC.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg {
    VestingAccounts {
        start_after: Option<HumanAddr>, 
        limit: Option<u32>, 
        order_by: Option<OrderBy>, 
    }
}

#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum OrderBy {
    Asc,
    Desc,
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "vesting_accounts": {
    "start_after": "terra1...", 
    "limit": 8, 
    "order_by": "asc"
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `start_after`\* | HumanAddr | Address with ANC vested to start query |
| `limit`\* | u32 | Maximum number of query entries |
| `order_by`\* | OrderBy | Order to make query |

| Key | Description |
| :--- | :--- |
| `Asc` | Make query in ascending order |
| `Desc` | Make query in descending order |

\* = optional

### `VestingAccountsResponse`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct VestingAccountsResponse {
    pub vesting_accounts: Vec<VestingAccountResponse>,
}

#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct VestingAccountResponse {
    pub address: HumanAddr,
    pub info: VestingInfo,
}

#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct VestingInfo {
    pub schedules: Vec<(u64, u64, Uint128)>,
    pub last_claim_time: u64,
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "vesting_accounts": [
    {
      "address": "terra1...", 
      "info": {
        "schedules": [123456, 234567, "10000000"], 
        "last_claim_time": 123456 
      }
    }, 
    {
      "address": "terra1...", 
      "info": {
        "schedules": [123456, 234567, "10000000"], 
        "last_claim_time": 123456 
      }
    }
  ]
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `vesting_accounts` | Vec&lt;VestingAccountResponse&gt; | List of addresses with ANC vested |

| Key | Type | Description |
| :--- | :--- | :--- |
| `address` | HumanAddr | Address with ANC vested |
| `info` | VestingInfo | Vesting information |

| Key | Type | Description |
| :--- | :--- | :--- |
| `schedules` | Vec&lt;\(u64, u64, Uint128\)&gt; | Vesting schedule of \(ANC claimable start time, end time, total vested amount\) |
| `last_claim_time` | u64 | Block timestamp when this account last claimed ANC |

