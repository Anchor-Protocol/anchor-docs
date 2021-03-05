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
| Hub | [terra1zdu9ph3429dtstv57ve3sfzr2vz2fclvmhn6td](https://finder.terra.money/tequila-0004/address/terra1zdu9ph3429dtstv57ve3sfzr2vz2fclvmhn6td) |
| Reward | [terra16rjk255rjc6vt2qg7h8ntfdykrzfkt0e5wykus](https://finder.terra.money/tequila-0004/address/terra16rjk255rjc6vt2qg7h8ntfdykrzfkt0e5wykus) |
| Airdrop Registry | [terra1093jc6g8gcuxp0vvfuzkk26rvnz38du886c88m](https://finder.terra.money/tequila-0004/address/terra1093jc6g8gcuxp0vvfuzkk26rvnz38du886c88m) |

#### Cw20-Compliant Token Contracts

| Contract | Address |
| :--- | :--- |
| Bonded LUNA \(bLUNA\) | [terra1wq9f8p8f7gldztpdc4v3awngupfkap8wpxhtjr](https://finder.terra.money/tequila-0004/address/terra1wq9f8p8f7gldztpdc4v3awngupfkap8wpxhtjr) |
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
| Distributor | [terra1...](https://finder.terra.money/) |

#### Cw20-Compliant Token Contracts

| Contract | Address |
| :--- | :--- |
| Anchor Token \(ANC\) | [terra1...](https://finder.terra.money/) |
{% endtab %}

{% tab title="Testnet" %}
#### Core Contracts

| Contract | Address |
| :--- | :--- |
| Gov | [terra1dakqt3s8dywea9advxz4duxkuvglz3a34yczw9](https://finder.terra.money/tequila-0004/address/terra1dakqt3s8dywea9advxz4duxkuvglz3a34yczw9) |
| Staking | [terra1tcmhs005clcakqtquk58j3s5z0gkjm4c7wkzhu](https://finder.terra.money/tequila-0004/address/terra1tcmhs005clcakqtquk58j3s5z0gkjm4c7wkzhu) |
| Community | [terra15l0pep3ww9k4aa50jmf2dnj68ak9tc2s30m2d3](https://finder.terra.money/tequila-0004/address/terra15l0pep3ww9k4aa50jmf2dnj68ak9tc2s30m2d3) |
| Collector | [terra1hz6wk7psk5d0sh3u3vwtjrawvrk8hkt6vgnemm](https://finder.terra.money/tequila-0004/address/terra1hz6wk7psk5d0sh3u3vwtjrawvrk8hkt6vgnemm) |
| Distributor | [terra1ytyge2vqtl9kcj8amrx9pxjypmw00244e7l3ye](https://finder.terra.money/tequila-0004/address/terra1ytyge2vqtl9kcj8amrx9pxjypmw00244e7l3ye) |

#### Cw20-Compliant Token Contracts

| Contract | Address |
| :--- | :--- |
| Anchor Token \(ANC\) | [terra1800p00qlxh0nmt0r0u9hv7m4lg042fnafng2t6](https://finder.terra.money/tequila-0004/address/terra1800p00qlxh0nmt0r0u9hv7m4lg042fnafng2t6) |
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
| Anchor UST \(aUST\) | [terra1...](https://finder.terra.money/) |
{% endtab %}

{% tab title="Testnet" %}
#### Core Contracts

| Contract | Address |
| :--- | :--- |
| Overseer | [terra174dcdqlkdwvsxqpkt47f9cy3anlv56ge5c05ex](https://finder.terra.money/tequila-0004/address/terra174dcdqlkdwvsxqpkt47f9cy3anlv56ge5c05ex) |
| Market | [terra1shmnertem9ujjxys2vxy2x92h0jzhctkjdv956](https://finder.terra.money/tequila-0004/address/terra1shmnertem9ujjxys2vxy2x92h0jzhctkjdv956) |
| bLuna Custody | [terra1urn8z5uqukjzr8sqdjdryj6nt5v3qttfta2zwn](https://finder.terra.money/tequila-0004/address/terra1urn8z5uqukjzr8sqdjdryj6nt5v3qttfta2zwn) |
| Interest Model | [terra1rrutuqshjkgfh22n5eau8jac0vn4hsyhcz3ju2](https://finder.terra.money/tequila-0004/address/terra1rrutuqshjkgfh22n5eau8jac0vn4hsyhcz3ju2) |
| Distribution Model | [terra1ajawq49hutlsytxstys2x58464dy06rlzphmvy](https://finder.terra.money/tequila-0004/address/terra1ajawq49hutlsytxstys2x58464dy06rlzphmvy) |
| Oracle | [terra1rz5chzn0g07hp5jx63srpkhv8hd7x8pss20w2e](https://finder.terra.money/tequila-0004/address/terra1rz5chzn0g07hp5jx63srpkhv8hd7x8pss20w2e) |

#### Cw20-Compliant Token Contracts

| Contract | Address |
| :--- | :--- |
| Anchor UST \(aUST\) | [terra1xhxx7tgth24d8f9pz6vkjmvulp88xh9vl9kmxu](https://finder.terra.money/tequila-0004/address/terra1xhxx7tgth24d8f9pz6vkjmvulp88xh9vl9kmxu) |
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
| Liquidation Contract | [terra1sm76ssl55vnwnu96d00t8jl8pzwg5nvm02m5k7](https://finder.terra.money/tequila-0004/address/terra1sm76ssl55vnwnu96d00t8jl8pzwg5nvm02m5k7) |
{% endtab %}
{% endtabs %}

