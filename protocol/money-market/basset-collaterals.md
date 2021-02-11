# bAsset Collaterals

## Whitelisting Criteria

New bAssets can be added as an accepted collateral in Anchor Protocol, which involves several preparations. After providing the criteria listed below, whitelisting applications can be submitted to whitelisting@anchorprotocol.com for review.

### Transferability to Terra

Anchor's core smart contracts reside on the Terra blockchain. bAssets must be transferred from the origin chain to the Terra blockchain before they can be used to collateralize an Anchor loan.

Aside from simple cross-chain token transfers, rewards generated from 

bAssets transferred to the Terra blockchain, named wrapped bAssets, should



The Terra blockchain currently integrates the below cross-chain solutions:

| Name | Status | Supported Chains | Development Team |
| :--- | :--- | :--- | :--- |
| [Shuttle](https://github.com/terra-project/shuttle) | Live | Terra &lt;&gt; Ethereum | Terraform Labs |
| [Wormhole](https://github.com/certusone/wormhole) | Planned | Terra &lt;&gt; Solana &lt;&gt; Ethereum | Certus One |





The Terra-side wrapper smart contract should provide: 

* Cross-chain transfer of bAsset tokens to the Terra blockchain
* Transfer of accrued bAsset rewards to Terra



### Custody Smart Contract

Custody contract are bAsset-specifically deployed smart contracts that handle:

* Deposits and withdrawals of bAsset collaterals
* Reward claims of deposited bAsset collaterals

As reward-claiming mechanisms may be different amongst various collateral types, 

Custody contracts must follow the interfaces provided by the Overseer contract:

| Message | Description |
| :--- | :--- |
| LockCollateral |  |
| UnlockCollateral |  |
| DistributeRewards |  |
| LiquidateCollateral |  |

#### `LockCollateral`

Decreases `borrower`'s deposited collateral by `amount`.

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
    "amount": "100000000" 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `borrower` | HumanAddr | Address of borrower locking collateral |
| `amount` | Uint256 | Amount of collateral to Lock |

#### `UnlockCollateral`

Increases `borrower`'s deposited collateral by `amount`.

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
    "amount": "100000000" 
  }
}
```
{% endtab %}
{% endtabs %}

| Key | Type | Description |
| :--- | :--- | :--- |
| `borrower` | HumanAddr | Address of borrower unlocking collateral |
| `amount` | Uint256 | Amount of collateral to unlock |

#### `LiquidateCollateral`

Liquidates specified amount of collateral locked by `borrower`.

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
| `liquidator` | HumanAddr | Address of user that triggered liquidation |
| `borrower` | HumanAddr | Address of borrower to liquidate |
| `amount` | Uint256 | Amount of collateral to liquidate |

#### `DistributeRewards`

Sends accrued bAsset rewards to the interest buffer by: 

* Claiming bAsset rewards accrued to Custody contract
* Swapping claimed rewards for Terra stablecoins
* Sending swapped rewards to interest buffer

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

#### \`\`





### Maximum Loan-To-Value \(LTV\) Ratio

On whitelisting, a maximum LTV ratio is set for each bAsset type, which determines the maximum amount of liability allowed per unit of locked collateral. The maximum LTV ratio, a value between 0 and 1, is set according to the bAsset's degree of usability as loan collateral. bAssets with high liquidity and low volatility have values set closer to 1, while bAssets with low liquidity and high volatility have lower set values.

### Oracle Feeder



#### bAsset Pricing

As novel financial derivatives, bAssets are likely to have lower trading liquidity than their underlying asset. In those cases, using the market price of such bAssets cause lower capital efficiency as their LTV ratios must be set low to account for the possibility of short-term price swings. The value of an bAsset is instead derived from the price of the underlying asset and the conversion rate used by the bAsset protocol, with the price becoming:

$$
\text{bAssetPrice} = \text{assetPrice} * \text{conversionRate}
$$

where assetPrice refers to the underlying asset price, while conversionRate is the exchange rate employed within the bAsset implementation. For a typical bAsset implementation, conversionRate could be rewritten as: 

$$
\text{conversionRate} = \frac {\text{assetsBonded}} {\text{bAssetSupply}}
$$

though calculation may vary with implementation.

A bAsset price value resistant to short-term trading volatility can be created this way as conversionRate is determined independent of external trading activities. Price data fed to Anchor's price oracle should be calculated using this method.

### 

