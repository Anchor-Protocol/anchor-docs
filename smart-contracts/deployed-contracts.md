# Deployed Contracts

## Contract Addresses

The core smart contracts of Anchor are deployed on the [Terra blockchain](https://terra.money/), and can be found at the below networks:

| Network Classification | Chain ID |
| :--- | :--- |
| Mainnet | `columbus-4` |
| Testnet | `tequila-0004` |

For money market and liquidations, a separate set of contracts are to be deployed for each Terra stablecoin denomination. Each set, called Markets, will use different Terra denominations as their base currency. Only UST-supporting contracts have been deployed on initial launch.

### bLuna

{% tabs %}
{% tab title="Mainnet" %}
#### Core Contracts

| Contract | Address |
| :--- | :--- |
| Hub | [terra1...](https://finder.terra.money/) |
| Reward | [terra1...](https://finder.terra.money/) |

#### Cw20 Token Contracts

| Contract | Address |
| :--- | :--- |
| Bonded LUNA \(bLUNA\) | [terra1...](https://finder.terra.money/) |
{% endtab %}

{% tab title="Testnet" %}
#### Core Contracts

| Contract | Address |
| :--- | :--- |
| Hub | [terra1...](https://finder.terra.money/) |
| Reward | [terra1...](https://finder.terra.money/) |

#### Cw20 Token Contracts

| Contract | Address |
| :--- | :--- |
| Bonded LUNA \(bLUNA\) | [terra1...](https://finder.terra.money/) |
{% endtab %}
{% endtabs %}

### Terra USD Market

{% hint style="danger" %}
### **WARNING**

Sending native tokens with a denomination not supported by the recipient contract will lead to **PERMANENT LOSS OF FUNDS**.
{% endhint %}

Below are addresses of money market and liquidation contracts that use **Terra USD** as their base denomination.

#### Money Market

{% tabs %}
{% tab title="Mainnet" %}
#### Core Contracts

| Contract | Address |
| :--- | :--- |
| Overseer | [terra1...](https://finder.terra.money/) |
| Market | [terra1...](https://finder.terra.money/) |
| bLuna Custody | [terra1...](https://finder.terra.money/) |
| Interest Model | [terra1...](https://finder.terra.money/) |
| Oracle | [terra1...](https://finder.terra.money/) |

#### Cw20 Token Contracts

| Contract | Address |
| :--- | :--- |
| Anchor UST \(aUST\) | [terra1...](https://finder.terra.money/) |
{% endtab %}

{% tab title="Testnet" %}
#### Core Contracts

| Contract | Address |
| :--- | :--- |
| Overseer | [terra1...](https://finder.terra.money/) |
| Market | [terra1...](https://finder.terra.money/) |
| bLuna Custody | [terra1...](https://finder.terra.money/) |
| Interest Model | [terra1...](https://finder.terra.money/) |
| Oracle | [terra1...](https://finder.terra.money/) |

#### Cw20 Token Contracts

| Contract | Address |
| :--- | :--- |
| Anchor UST \(aUST\) | [terra1...](https://finder.terra.money/) |
{% endtab %}
{% endtabs %}

#### Liquidation

{% tabs %}
{% tab title="Mainnet" %}
#### Core Contracts

| Contract | Address |
| :--- | :--- |
| Liquidation Contract | [terra1...](https://finder.terra.money/) |
{% endtab %}

{% tab title="Testnet" %}
#### Core Contracts

| Contract | Address |
| :--- | :--- |
| Liquidation Contract | [terra1...](https://finder.terra.money/) |
{% endtab %}
{% endtabs %}

