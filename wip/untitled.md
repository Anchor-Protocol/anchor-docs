# Token

## Config

| Key | Type | Description |
| :--- | :--- | :--- |
|  |  |  |

## InitMsg

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, JsonSchema)]
pub struct TokenInitMsg { 
    pub name: String, 
    pub symbol: String, 
    pub decimals: u8, 
    pub initial_balances: Vec<Cw20CoinHuman>, 
    pub mint: Option<MinterResponse>, 
    pub init_hook: Option<TokenInitHook>, 
    pub owner: CanonicalAddr, 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "name": "bluna", 
  "symbol": "BLUNA", 
  "decimals": 6, 
  "initial_balances": , 
  "mint": "", 
  "init_hook": "", 
  "owner": "" 
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `name` | String |  |
| `symbol` | String |  |
| `decimals` | u8 |  |
| `initial_balances` | Cw20CoinHuman |  |
| `mint` | MinterResponse |  |
| `init_hook` | TokenInitHook |  |
| `owner` | CanonicalAddr |  |



## HandleMsg

### `Transfer`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg { 
    Transfer { 
        recipient: HumanAddr, 
        amount: Uint128, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "tranfer": {
    "recipient": "terra1...", 
    "amount": "23986423" 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `recipient` | HumanAddr |  |
| `amount` | Uint128 |  |

### `Burn`



{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg { 
    Burn { 
        amount: Uint128, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "burn": {
    "amount": "23089745" 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `amount` | Uint128 |  |

### `Send`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg { 
    Send { 
        contract: HumanAddr, 
        amount: Uint128, 
        msg: Option<Binary>, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "send": {
    "contract": "terra1...", 
    "amount": "98242134", 
    "msg": "1234erwfaffaesfaef=" 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `contract` | HumanAddr |  |
| `amount` | Uint128 |  |
| `msg` | Binary |  |

\* = optional

### `Mint`

{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg { 
    Mint { 
        recipient: HumanAddr, 
        amount: Uint128, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "mint": {
    "recipient": "terra1...", 
    "amount": "987231204" 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `recipient` | HumanAddr |  |
| `amount` | Uint128 |  |

### `IncreaseAllowance`



{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg { 
    IncreaseAllowance { 
        spender: HumanAddr, 
        amount: Uint128, 
        expires: Option<Expiration>, 
    }
}

pub enum Expiration {
    AtHeight {
        height: u64, 
    }, 
    AtTime {
        time: u64, 
    }, 
    Never {}, 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "increase_allowance": {
    "spender": "terra1...", 
    "amount": "78420524", 
    "expires": {
      "at_height": "82976541" 
    }
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `spender` | HumanAddr |  |
| `amount` | Uint128 |  |
| `expires`\* | Expiration |  |

\* = optional

### `DecreaseAllowance`



{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg { 
    DecreaseAllowance { 
        spender: HumanAddr, 
        amount: Uint128, 
        expires: Option<Expiration>, 
    }
}

pub enum Expiration {
    AtHeight {
        height: u64, 
    }, 
    AtTime {
        time: u64, 
    }, 
    Never {}, 
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "decrease_allowance": {
    "spender": "terra1...", 
    "amount": "78420524", 
    "expires": {
      "at_height": "82976541" 
    }
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `spender` | HumanAddr |  |
| `amount` | Uint128 |  |
| `expires`\* | Expiration |  |

\* = optional

### `TransferFrom`



{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg { 
    TransferFrom { 
        owner: HumanAddr, 
        recipient: HumanAddr, 
        amount: Uint128, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "transfer_from": {
    "owner": "terra1...", 
    "recipient": "terra1...", 
    "amount": "81249236" 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner` | HumanAddr |  |
| `recipient` | HumanAddr |  |
| `amount` | Uint128 |  |

### `SendFrom`



{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg { 
    SendFrom { 
        owner: HumanAddr, 
        contract: HumanAddr, 
        amount: Uint128, 
        msg: Option<Binary>, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "send_from": {
    "owner": "terra1...", 
    "contract": "terra1...", 
    "amount": "13845792", 
    "msg": "1234erwfaffaesfaef="
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner` | HumanAddr |  |
| `contract` | HumanAddr |  |
| `amount` | Uint128 |  |
| `msg`\* | Binary |  |

\* = optional

### `BurnFrom`



{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum HandleMsg { 
    BurnFrom { 
        owner: HumanAddr, 
        amount: Uint128, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "burn_from": {
    "owner": "terra1...", 
    "amount": "129075232", 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner` | HumanAddr |  |
| `amount` | Uint128 |  |

## QueryMsg

### `Balance`



{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg { 
    Balance { 
        owner: HumanAddr, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "balance": {
    "owner": "terra1..." 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner` | HumanAddr |  |

### `TokenInfo`



{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg { 
    TokenInfo {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "token_info": {}
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
|  |  |  |

### `Minter`



{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg { 
    Minter {}
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "minter": {}
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
|  |  |  |

### `Allowance`



{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg { 
    Allowance { 
        owner: HumanAddr, 
        spender: HumanAddr, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "allowance": {
    "owner": "terra1...", 
    "spender": "terra1...", 
  }
{
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner` | HumanAddr |  |
| `spender` | HumanAddr |  |

### `AllAllowances`



{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg { 
    AllAllowances { 
        owner: HumanAddr, 
        start_after: Option<HumanAddr>, 
        limit: Option<u32>, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "all_allowances": {
    "owner": "terra1...", 
    "start_after": "terra1...", 
    "limit": 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `owner` | HumanAddr |  |
| `start_after`\* | HumanAddr |  |
| `limit`\* | u32 |  |

\* = optional

### `AllAccounts`



{% tabs %}
{% tab title="Rust" %}
```rust
#[derive(Serialize, Deserialize, Clone, Debug, PartialEq, JsonSchema)]
#[serde(rename_all = "snake_case")]
pub enum QueryMsg { 
    AllAccounts { 
        start_after: Option<HumanAddr>, 
        limit: Option<u32>, 
    }
}
```
{% endtab %}

{% tab title="JSON" %}
```javascript
{
  "all_accounts": {
    "start_after": "terra1...", 
    "limit": 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `start_after`\* | HumanAddr |  |
| `limit`\* | u32 |  |

\* = optional

