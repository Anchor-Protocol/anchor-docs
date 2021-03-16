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
| Hub | TBD |
| Reward | TBD |
| Airdrop Registry | TBD |

#### Cw20-Compliant Token Contracts

| Contract | Address |
| :--- | :--- |
| Bonded LUNA \(bLUNA\) | TBD |
{% endtab %}

{% tab title="Testnet" %}
#### Core Contracts

| Contract | Address |
| :--- | :--- |
| Hub | [terra1fflas6wv4snv8lsda9knvq2w0cyt493r8puh2e](https://finder.terra.money/tequila-0004/address/terra1fflas6wv4snv8lsda9knvq2w0cyt493r8puh2e) |
| Reward | [terra1ac24j6pdxh53czqyrkr6ygphdeftg7u3958tl2](https://finder.terra.money/tequila-0004/address/terra1ac24j6pdxh53czqyrkr6ygphdeftg7u3958tl2) |
| Airdrop Registry | [terra1334h20c9ewxguw9p9vdxzmr8994qj4qu77ux6q](https://finder.terra.money/tequila-0004/address/terra1334h20c9ewxguw9p9vdxzmr8994qj4qu77ux6q) |

#### Cw20-Compliant Token Contracts

| Contract | Address |
| :--- | :--- |
| Bonded LUNA \(bLUNA\) | [terra1u0t35drzyy0mujj8rkdyzhe264uls4ug3wdp3x](https://finder.terra.money/tequila-0004/address/terra1u0t35drzyy0mujj8rkdyzhe264uls4ug3wdp3x) |
{% endtab %}
{% endtabs %}

### ANC-Related Smart Contracts

{% tabs %}
{% tab title="Mainnet" %}
#### Core Contracts

| Contract | Address |
| :--- | :--- |
| Gov | TBD |
| Staking | TBD |
| Community | TBD |
| Collector | TBD |
| Distributor | TBD |
| Team Vesting | TBD |
| Investor Vesting | TBD |

#### Cw20-Compliant Token Contracts

| Contract | Address |
| :--- | :--- |
| Anchor Token \(ANC\) | TBD |
{% endtab %}

{% tab title="Testnet" %}
#### Core Contracts

| Contract | Address |
| :--- | :--- |
| Gov | [terra16ckeuu7c6ggu52a8se005mg5c0kd2kmuun63cu](https://finder.terra.money/tequila-0004/address/terra16ckeuu7c6ggu52a8se005mg5c0kd2kmuun63cu) |
| Staking | [terra19nxz35c8f7t3ghdxrxherym20tux8eccar0c3k](https://finder.terra.money/tequila-0004/address/terra19nxz35c8f7t3ghdxrxherym20tux8eccar0c3k) |
| Community | [terra17g577z0pqt6tejhceh06y3lyeudfs3v90mzduy](https://finder.terra.money/tequila-0004/address/terra17g577z0pqt6tejhceh06y3lyeudfs3v90mzduy) |
| Collector | [terra1hlctcrrhcl2azxzcsns467le876cfuzam6jty4](https://finder.terra.money/tequila-0004/address/terra1hlctcrrhcl2azxzcsns467le876cfuzam6jty4) |
| Distributor | [terra1z7nxemcnm8kp7fs33cs7ge4wfuld307v80gypj](https://finder.terra.money/tequila-0004/address/terra1z7nxemcnm8kp7fs33cs7ge4wfuld307v80gypj) |

#### Cw20-Compliant Token Contracts

| Contract | Address |
| :--- | :--- |
| Anchor Token \(ANC\) | [terra1747mad58h0w4y589y3sk84r5efqdev9q4r02pc](https://finder.terra.money/tequila-0004/address/terra1747mad58h0w4y589y3sk84r5efqdev9q4r02pc) |
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
| Overseer | TBD |
| Market | TBD |
| bLuna Custody | TBD |
| Interest Model | TBD |
| Distribution Model | TBD |
| Oracle | TBD |

#### Cw20-Compliant Token Contracts

| Contract | Address |
| :--- | :--- |
| Anchor Terra USD \(aUST\) | TBD |
{% endtab %}

{% tab title="Testnet" %}
#### Core Contracts

| Contract | Address |
| :--- | :--- |
| Overseer | [terra1qljxd0y3j3gk97025qvl3lgq8ygup4gsksvaxv](https://finder.terra.money/tequila-0004/address/terra1qljxd0y3j3gk97025qvl3lgq8ygup4gsksvaxv) |
| Market | [terra15dwd5mj8v59wpj0wvt233mf5efdff808c5tkal](https://finder.terra.money/tequila-0004/address/terra15dwd5mj8v59wpj0wvt233mf5efdff808c5tkal) |
| bLuna Custody | [terra1ltnkx0mv7lf2rca9f8w740ashu93ujughy4s7p](https://finder.terra.money/tequila-0004/address/terra1ltnkx0mv7lf2rca9f8w740ashu93ujughy4s7p) |
| Interest Model | [terra1m25aqupscdw2kw4tnq5ql6hexgr34mr76azh5x](https://finder.terra.money/tequila-0004/address/terra1m25aqupscdw2kw4tnq5ql6hexgr34mr76azh5x) |
| Distribution Model | [terra1u64cezah94sq3ye8y0ung28x3pxc37tv8fth7h](https://finder.terra.money/tequila-0004/address/terra1u64cezah94sq3ye8y0ung28x3pxc37tv8fth7h) |
| Oracle | [terra1p4gg3p2ue6qy2qfuxtrmgv2ec3f4jmgqtazum8](https://finder.terra.money/tequila-0004/address/terra1p4gg3p2ue6qy2qfuxtrmgv2ec3f4jmgqtazum8) |

#### Cw20-Compliant Token Contracts

| Contract | Address |
| :--- | :--- |
| Anchor Terra USD \(aUST\) | [terra1ajt556dpzvjwl0kl5tzku3fc3p3knkg9mkv8jl](https://finder.terra.money/tequila-0004/address/terra1ajt556dpzvjwl0kl5tzku3fc3p3knkg9mkv8jl) |
{% endtab %}
{% endtabs %}

#### Liquidation Contract

{% tabs %}
{% tab title="Mainnet" %}
#### Core Contracts

| Contract | Address |
| :--- | :--- |
| Liquidation Contract | TBD |
{% endtab %}

{% tab title="Testnet" %}
#### Core Contracts

| Contract | Address |
| :--- | :--- |
| Liquidation Contract | [terra16vc4v9hhntswzkuunqhncs9yy30mqql3gxlqfe](https://finder.terra.money/tequila-0004/address/terra16vc4v9hhntswzkuunqhncs9yy30mqql3gxlqfe) |
{% endtab %}
{% endtabs %}

