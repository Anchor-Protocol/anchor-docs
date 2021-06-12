# Deployed Contracts

## Contract Addresses

Smart contracts that power EthAnchor are deployed to the [Ethereum blockchain](https://ethereum.org/en/), and can be found at the below networks:

| Network Classification | Name | Chain ID |
| :--- | :--- | :--- |
| Mainnet | Ethereum Mainnet | `1` |
| Testnet | Ethereum Testnet Ropsten | `3` |

Deposits and withdrawal requests made through supported Ethereum networks are each handled on the core Anchor smart contracts which reside on the [Terra blockchain](https://www.terra.money/). Requests from Ethereum mainnet and testnet networks are each processed on the below Terra networks:

| Network Classification | Chain ID |
| :--- | :--- |
| Mainnet | `columbus-4` |
| Testnet | `tequila-0004` |

{% tabs %}
{% tab title="Mainnet" %}
{% hint style="info" %}
Mainnet EthAnchor contracts use [Curve](https://curve.fi/) for swapping stablecoins.
{% endhint %}

#### Core Contracts

{% hint style="info" %}
EthAnchor core contracts have a proxy contract layer on top. Below are addresses of the proxy contracts \(excluding ExchangeRateFeeder\).
{% endhint %}

| Contract | Contract Address |
| :--- | :--- |
| Router | [0xcEF9E167d3f8806771e9bac1d4a0d568c39a9388](https://etherscan.io/address/0xcEF9E167d3f8806771e9bac1d4a0d568c39a9388) |
| ExchangeRateFeeder | [0xd7c4f5903De8A256a1f535AC71CeCe5750d5197a](https://etherscan.io/address/0xd7c4f5903De8A256a1f535AC71CeCe5750d5197a) |
| ConversionPool - DAI | [0x83dd0a8E6F3A51c4cCA6c3f95721f9926DD9e7E7](https://etherscan.io/address/0x83dd0a8E6F3A51c4cCA6c3f95721f9926DD9e7E7) |
| ConversionPool - USDT | [0xEd8C41774E71f9BF0c2C223d3a3554F496656D16](https://etherscan.io/address/0xEd8C41774E71f9BF0c2C223d3a3554F496656D16) |
| ConversionPool - USDC | [0x53fD7e8fEc0ac80cf93aA872026EadF50cB925f3](https://etherscan.io/address/0x53fD7e8fEc0ac80cf93aA872026EadF50cB925f3) |
| ConversionPool - BUSD | [0x242876001d04D5782aEE4f69fB26Ee6264Cc1d21](https://etherscan.io/address/0x242876001d04D5782aEE4f69fB26Ee6264Cc1d21) |

#### ERC20-Compliant Token Contracts \(Stablecoins\)

| Token Name | Symbol | Decimals | Contract Address |
| :--- | :--- | :--- | :--- |
| Wrapped UST Token | UST | 18 | [0xa47c8bf37f92abed4a126bda807a7b7498661acd](https://etherscan.io/token/0xa47c8bf37f92abed4a126bda807a7b7498661acd) |
| Dai Stablecoin | DAI | 18 | [0x6b175474e89094c44da98b954eedeac495271d0f](https://etherscan.io/token/0x6b175474e89094c44da98b954eedeac495271d0f) |
| Tether USD | USDT | 6 | [0xdac17f958d2ee523a2206206994597c13d831ec7](https://etherscan.io/token/0xdac17f958d2ee523a2206206994597c13d831ec7) |
| USD Coin | USDC | 6 | [0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48](https://etherscan.io/token/0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48) |
| Binance USD | BUSD | 18 | [0x4Fabb145d64652a948d72533023f6E7A623C7C53](https://etherscan.io/token/0x4Fabb145d64652a948d72533023f6E7A623C7C53) |

#### ERC20-Compliant Token Contracts \(aTerra\)

| Token Name | Symbol | Decimals | Contract Address |
| :--- | :--- | :--- | :--- |
| Wrapped Anchor UST Token | aUST | 18 | [0xa8De3e3c934e2A1BB08B010104CcaBBD4D6293ab](https://etherscan.io/token/0xa8De3e3c934e2A1BB08B010104CcaBBD4D6293ab) |
| Anchor DAI Token | aDAI | 18 | [0x23afFce94d2A6736DE456a25eB8Cc96612Ca55CA](https://etherscan.io/token/0x23afFce94d2A6736DE456a25eB8Cc96612Ca55CA) |
| Anchor USDT Token | aUSDT | 18 | [0x54E076dBa023251854f4C29ea750566528734B2d](https://etherscan.io/token/0x54E076dBa023251854f4C29ea750566528734B2d) |
| Anchor USDC Token | aUSDC | 18 | [0x94eAd8f528A3aF425de14cfdDA727B218915687C](https://etherscan.io/token/0x94eAd8f528A3aF425de14cfdDA727B218915687C) |
| Anchor BUSD Token | aBUSD | 18 | [0x5A6a33117EcBc6EA38B3a140F3E20245052CC647](https://etherscan.io/token/0x5A6a33117EcBc6EA38B3a140F3E20245052CC647) |
{% endtab %}
{% endtabs %}

