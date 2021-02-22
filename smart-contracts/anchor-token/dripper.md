# Faucet

## Config

| Key | Type | Description |
| :--- | :--- | :--- |
| `gov_contract` | CanonicalAddr | Contract address of Gov |
| `anchor_token` | CanonicalAddr | Contract address of ANC token |
| `whitelist` | Vec&lt;CanonicalAddr&gt; | List of addresses permissioned to spend ANC in Faucet |
| `spend_limit` | Uint128 | Maximum amount of ANC spendable per spend event |

## InitMsg

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct InitMsg {
    pub gov_contract: HumanAddr,   // anchor gov contract
    pub anchor_token: HumanAddr,   // anchor token address
    pub whitelist: Vec<HumanAddr>, // whitelisted contract addresses to spend faucet
    pub spend_limit: Uint128,      // spend limit per each `spend` request
}
```
{% endtab %}

{% tab title="JSON" %}
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
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `gov_contract` | HumanAddr | Contract address of Gov |
| `anchor_token` | HumanAddr | Contract address of ANC token |
| `whitelist` | Vec&lt;HumanAddr&gt; | List of addresses permissioned to spend ANC in Faucet |
| `spend_limit` | Uint128 | Maximum amount of ANC spendable per spend event |

## HandleMsg

### `UpdateConfig`

Updates the Faucet contract configuration. Can only be issued by the Gov contract.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg { 
    UpdateConfig {
        spend_limit: Option<Uint128>, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "update_config": {
    "spend_limit": "100000000" 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `spend_limit`\* | Uint128 | Maximum amount of ANC spendable per spend event  |

\* = optional

### `Spend`

Spends ANC in Faucet. Can only be issued by whitelisted addresses.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg { 
    Spend {
        recipient: HumanAddr, 
        amount: Uint128, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "spend": {
    "recipient": "terra1...", 
    "amount": "100000000" 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `recipient` | HumanAddr | Recipient address of ANC spend |
| `amount` | Uint128 | ANC amount to receive |

### `AddDistributor`

Adds a new ANC distribution contract to the whitelist. Can only be issued by the Gov contract.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg { 
    AddDistributor {
        distributor: HumanAddr, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "add_distributor": {
    "distributor": "terra1..." 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `distributor` | HumanAddr | Contract address of ANC distribution contract to add |

### `RemoveDistributor`

Removes a ANC distribution contract from the whitelist. Can only be issued by the Gov contract.

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg { 
    RemoveDistributor {
        distributor: HumanAddr, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "remove_distributor": {
    "distributor": "terra1..." 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `distributor` | HumanAddr | Contract address of ANC distribution contract to remove |

## QueryMsg

### `Config`

Gets the Faucet contract configuration.

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
    pub gov_contract: HumanAddr,
    pub anchor_token: HumanAddr,
    pub whitelist: Vec<HumanAddr>,
    pub spend_limit: Uint128,
}
```
{% endtab %}

{% tab title="JSON" %}
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
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `gov_contract` | HumanAddr | Contract address of Gov |
| `anchor_token` | HumanAddr | Contract address of ANC Token |
| `whitelist` | Vec&lt;HumanAddr&gt; | List of addresses permissioned to spend ANC in Faucet |
| `spend_limit` | Uint128 | Maximum amount of ANC spendable per spend event |

