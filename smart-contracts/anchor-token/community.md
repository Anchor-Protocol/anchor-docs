# Community

The Community Contract holds the funds of the [Community Pool](../../governance/anchor-governance/), which can be spent through a governance poll. 

## Config

| Name | Type | Description |
| :--- | :--- | :--- |
| `gov_contract` | CanonicalAddr | Contract address of Gov |
| `anchor_token` | CanonicalAddr | Contract address of Anchor Token \(ANC\) |
| `spend_limit` | Uint128 | Upper cap on community grant size |

## InitMsg

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
pub struct InitMsg {
    pub gov_contract: HumanAddr, 
    pub anchor_token: HumanAddr, 
    pub spend_limit: Uint128, 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "gov_contract": "terra1...", 
  "anchor_token": "terra1...", 
  "spend_limit": "100000000000" 
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
| `gov_contract` | HumanAddr | Contract address of Gov |
| `anchor_token` | HumanAddr | Contract address of Anchor Token \(ANC\) |
| `spend_limit` | HumanAddr | Upper cap on community grant size |

## HandleMsg

### `UpdateConfig`

Updates the Collector contract configuration.

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
    "spend_limit": "100000000000" 
  }
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
| `spend_limit`\* | Uint128 | New upper cap on community grant size |

\* = optional

### `Spend`

Transfers ANC tokens to the grant recipient. Can only be issued by the Gov contract.

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

| Name | Type | Description |
| :--- | :--- | :--- |
| `recipient` | HumanAddr | Recipient of community grant |
| `amount` | Uint128 | Community grant amount |

## QueryMsg

### `Config`

Gets the Collector contract configuration.

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

| Name | Type | Description |
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
    pub spend_limit: Uint128,
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "gov_contract": "terra1...", 
  "anchor_token": "terra1...", 
  "spend_limit": "100000000000" 
}
```
{% endtab %}
{% endtabs %}

| Name | Type | Description |
| :--- | :--- | :--- |
| `gov_contract` | HumanAddr | Contract address of Gov |
| `anchor_token` | HumanAddr | Contract address of Anchor Token \(ANC\) |
| `spend_limit` | Uint128 | Upper cap on community grant size |

