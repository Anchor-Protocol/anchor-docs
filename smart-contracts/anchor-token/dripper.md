# Faucet

## Config

| Key | Type | Description |
| :--- | :--- | :--- |
| `gov_contract` | CanonicalAddr | Contract address of Gov |
| `anchor_token` | CanonicalAddr | Contract address of ANC token |
| `whitelist` | Vec&lt;CanonicalAddr&gt; |  |
| `spend_limit` | Uint128 |  |

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
{% endtabs %}



## HandleMsg





## QueryMsg

