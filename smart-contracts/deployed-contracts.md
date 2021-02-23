# Deployed Contracts

## Contract Addresses

The core smart contracts of Anchor are deployed on the [Terra blockchain](https://terra.money/), and can be found at the below networks:

| Network Classification | Chain ID |
| :--- | :--- |
| Mainnet | `columbus-4` |
| Testnet | `tequila-0004` |

For money market and liquidations, a separate set of contracts are to be deployed for each Terra stablecoin denomination. Each set, called Markets, will use different Terra denominations as their base currency. Only UST-supporting contracts have been deployed on initial launch.

### bLuna Smart Contracts

{% tabs %}
{% tab title="Mainnet" %}
#### Core Contracts

| Contract | Address |
| :--- | :--- |
| Hub | [terra1...](https://finder.terra.money/) |
| Reward | [terra1...](https://finder.terra.money/) |
| Airdrop Registry | [terra1...](https://finder.terra.money/) |

#### Cw20-Compliant Token Contracts

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
| Airdrop Registry | [terra1...](https://finder.terra.money/) |

#### Cw20-Compliant Token Contracts

| Contract | Address |
| :--- | :--- |
| Bonded LUNA \(bLUNA\) | [terra1...](https://finder.terra.money/) |
{% endtab %}
{% endtabs %}

### ANC-related Smart Contracts

{% tabs %}
{% tab title="Mainnet" %}
#### Core Contracts

| Contract | Address |
| :--- | :--- |
| Gov | [terra1...](https://finder.terra.money/) |
| Staking | [terra1...](https://finder.terra.money/) |
| Community | [terra1...](https://finder.terra.money/) |
| Collector | [terra1...](https://finder.terra.money/) |
| Faucet | [terra1...](https://finder.terra.money/) |

#### Cw20-Compliant Token Contracts

| Contract | Address |
| :--- | :--- |
| Anchor Token \(ANC\) | [terra1...](https://finder.terra.money/) |
{% endtab %}

{% tab title="Testnet" %}
#### Core Contracts

| Contract | Address |
| :--- | :--- |
| Gov | [terra1...](https://finder.terra.money/) |
| Staking | [terra1...](https://finder.terra.money/) |
| Community | [terra1...](https://finder.terra.money/) |
| Collector | [terra1...](https://finder.terra.money/) |
| Faucet | [terra1...](https://finder.terra.money/) |

#### Cw20-Compliant Token Contracts

| Contract | Address |
| :--- | :--- |
| Anchor Token \(ANC\) | [terra1...](https://finder.terra.money/) |
{% endtab %}
{% endtabs %}

### Terra USD Market Smart Contracts

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
| Distribution Model | [terra1...](https://finder.terra.money/) |
| Oracle | [terra1...](https://finder.terra.money/) |

#### Cw20-Compliant Token Contracts

| Contract | Address |
| :--- | :--- |
| Anchor Token \(ANC\) | [terra1...](https://finder.terra.money/) |
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
| Distribution Model | [terra1...](https://finder.terra.money/) |
| Oracle | [terra1...](https://finder.terra.money/) |

#### Cw20-Compliant Token Contracts

| Contract | Address |
| :--- | :--- |
| Anchor Token \(ANC\) | [terra1...](https://finder.terra.money/) |
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

