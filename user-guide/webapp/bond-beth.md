# BOND \[bETH]

Minting and redeeming bETH is a cross-chain operation, requiring interactions from both the Ethereum and Terra blockchain. The below web interfaces are used to achieve certain types of actions on corresponding blockchains.

* [Lido's stETH WebApp](https://stake.lido.fi) - for minting stETH using ETH
* [Lido's bETH WebApp](https://anchor.lido.fi) - for minting and redeeming bETH using stETH
* [Anchor WebApp's BOND page](https://app.anchorprotocol.com/bond/claim) - for claiming bETH rewards
* [Terra Bridge WebApp](https://bridge.terra.money) - for transferring bETH to/from Ethereum from/to Terra
* [Curve's WebApp](https://curve.fi) - for swapping stETH for ETH

For holders of ETH, bETH can be acquired by [staking ETH](bond-beth.md#minting-beth-by-staking-eth) on [Lido's bETH WebApp](https://anchor.lido.fi/?)

Alternatively, bETH can be minted by going through stETH. This process requires the steps of: &#x20;

1. [Minting stETH by staking ETH](bond-beth.md#minting-steth-by-staking-eth) through [Lido's stETH WebApp](https://stake.lido.fi)
2. [Converting stETH to bETH](bond-beth.md#converting-steth-to-beth) via [Lido's bETH WebApp](https://anchor.lido.fi)

As for redeeming bETH tokens back to ETH, the following process can be taken:&#x20;

1. [Transferring bETH from Terra to Ethereum](bond-beth.md#1-transferring-beth-from-terra-to-ethereum) via [Terra Bridge](https://bridge.terra.money)
2. [Converting bETH to stETH](bond-beth.md#2-converting-beth-to-steth) on [Lido's bETH WebApp](https://anchor.lido.fi)
3. [Swapping stETH for ETH](bond-beth.md#swapping-steth-for-eth) on [Curve](https://curve.fi)

bETH tokens on Terra accrue rewards in TerraUSD, funded by staking rewards of Ethereum 2.0. [Accrued rewards can be claimed](bond-beth.md#claiming-accrued-beth-rewards) with the use of the [BOND page of Anchor's WebApp](https://app.anchorprotocol.com/bond/claim/).

## Connecting with Lido's web interfaces

1\. Navigate to Lido's stETH or bETH WebApp and click **\[Connect wallet]**.

![](../../.gitbook/assets/connecting-with-interface.png)

2\. Select the Ethereum wallet of choice that contains a balance of stETH / bETH and ETH.

![](../../.gitbook/assets/connecting-with-interface-2.png)

3\. Wallet connection complete.

![](../../.gitbook/assets/connecting-with-interface-3.png)

## Minting bETH by staking ETH

bETH tokens can be minted by submitting ETH to Ethereum-side bETH smart contracts. The process is achievable via Lido's bETH WebApp.

1\. Navigate to the **To bETH** page of Lido's web interface.

![](../../.gitbook/assets/minting-beth-with-eth-1.png)

2\. Click on the dropdown and select Ethereum.

![](../../.gitbook/assets/minting-beth-with-eth-2.png)

3\. Enter the amount of ETH to use in minting bETH and the Terra address to receive the resulting bETH tokens. Click **\[Convert]** to proceed.

![](../../.gitbook/assets/minting-beth-with-eth-3.png)

4\. Metamask should prompt you to sign a transaction that contains the mint operation. Confirm the details presented and click **\[Confirm]** to sign.

![](../../.gitbook/assets/minting-beth-with-eth-4.png)

5\. Mint complete. After the transaction is processed by Shuttle, the specified Terra address will now hold an increased balance of bETH tokens.

![](../../.gitbook/assets/minting-beth-with-eth-5.png)

## Minting stETH by staking ETH

bETH tokens are backed by Lido's stETH tokens. stETH can be created by staking ETH via Lido's stETH smart contracts, which can be accessed through Lido's stETH WebApp.&#x20;

1\. Navigate to the **STAKE** page of Lido's web interface.

![](../../.gitbook/assets/minting-steth-1.png)

2\. Enter the amount of ETH to stake and use in minting stETH. Click **\[Submit]** to proceed.

![](../../.gitbook/assets/minting-steth-2.png)

3\. Metamask should prompt you to sign a transaction that contains the stake operation. Confirm the details presented and click **\[Confirm]** to sign.&#x20;

![](../../.gitbook/assets/minting-steth-3.png)

4\. Staking complete.

![](../../.gitbook/assets/minting-steth-4.png)

## Converting stETH to bETH

{% hint style="info" %}
Unlike bLuna, bETH tokens are minted / redeemed through [Lido's web interface for bETH](https://anchor.lido.fi).
{% endhint %}

1\. Navigate to the **To bETH** tab in the **Convert** page.

![](<../../.gitbook/assets/minting-beth-1 (1).png>)

2\. Enter the amount of stETH to use in minting bETH and click the **\[Unlock token for conversion]** button to confirm. This process authorizes Lido's web interfaces to interact with you stETH tokens.

![](<../../.gitbook/assets/minting-beth-2 (1).png>)

3\. Metamask should prompt you to sign a transaction that contains the authorize operation. Confirm the details presented and click **\[Confirm]** to sign.

![](../../.gitbook/assets/minting-beth-3.png)

4\. Enter the Terra address to receive the resulting bETH tokens. Click **\[Convert]** to proceed.

![](../../.gitbook/assets/minting-beth-4.png)

5\. Metamask should prompt you to sign a transaction that contains the convert operation. Confirm the details presented and enter your password to sign.

![](../../.gitbook/assets/minting-beth-5.png)

6\. Conversion complete. After the transaction is processed by Shuttle, the specified Terra address will now hold an increased balance of bETH tokens.

![](../../.gitbook/assets/minting-beth-6.png)



## Redeeming bETH to stETH

bETH tokens that reside on the Terra blockchain should be first transferred to Ethereum before they can be redeemed for stETH.

Redemption is a two-step process, first requiring a cross-chain transfer, which is the followed by a redemption to stETH.

### 1. Transferring bETH from Terra to Ethereum

{% hint style="warning" %}
Shuttle applies a fee of **0.1%** (with a minimum fee of 1 UST) for Terra to Ethereum transfers. Transfer requests below a transfer value of 1 UST are ignored.
{% endhint %}

Cross-chain transfers between Ethereum / Terra can be achieved through Terra Bridge.

1-a. Navigate to Terra Bridge.

![](../../.gitbook/assets/redeeming-beth-1-a.png)

1-b. Select bETH as the asset to Transfer. Specify the amount to transfer, as well as the Ethereum address to receive transferred bETH tokens.

![](../../.gitbook/assets/redeeming-beth-1-b.png)

1-c. Click **\[Confirm]** to proceed.

![](../../.gitbook/assets/redeeming-beth-1-c.png)

1-d. Station Extension should prompt you to sign a transaction that contains the mint operation. Confirm the details presented and enter your password to sign.

![](../../.gitbook/assets/redeeming-beth-1-d.png)

1-e. Transfer complete.

![](../../.gitbook/assets/redeeming-beth-1-e1.png)

After the transaction is processed by Shuttle, the specified Ethereum address should now include a balance of bETH.

![](../../.gitbook/assets/redeeming-beth-1-e2.png)

### 2. Converting bETH to stETH

2-a. Navigate to the **To stETH** tab in the **Convert** page.

![](../../.gitbook/assets/redeeming-beth-2.png)

2-b. Enter the amount of bETH to convert to stETH and click the **\[Transfer]** button to proceed.

![](../../.gitbook/assets/redeeming-beth-3.png)

2-c. Metamask should prompt you to sign a transaction that contains the conversion operation. Confirm the details presented and click **\[Confirm]** to sign.

![](../../.gitbook/assets/redeeming-beth-4.png)

2-d. Conversion complete.

## Swapping stETH for ETH

1\. Navigate to Curve Finance's web interface, [curve.fi](https://curve.fi).

![](../../.gitbook/assets/redeeming-steth-1.png)

2\. Select STETH and ETH. Enter the amount of stETH to swap for ETH and click **\[Sell]** to proceed.

![](../../.gitbook/assets/redeeming-steth-2.png)

3\. Metamask should prompt you to sign a first transaction that contains an authorization for Curve's web interface to interact with your stETH tokens. Confirm the details presented and click **\[Confirm]** to sign.

![](../../.gitbook/assets/redeeming-steth-3.png)



4\. Metamask should then prompt you to sign a second transaction that contains the swap operation. Confirm the details presented and click **\[Confirm]** to sign.

![](../../.gitbook/assets/redeeming-steth-4.png)

5\. Swap complete.

## Claiming accrued bETH rewards

{% hint style="info" %}
bETH rewards only accrue only if the user is currently holding bETH tokens. Users do not accrue rewards from bETH tokens deposited to Anchor.
{% endhint %}

1\. Navigate to the **CLAIM** tab in the **BOND** page.

![](../../.gitbook/assets/claiming-rewards-1.png)

2\. Select bETH in the dropdown.

![](../../.gitbook/assets/claiming-rewards-2.png)

3\. Check the amount of accrued bETH rewards (**Claimable Rewards**). Click the **\[Claim]** button to claim accrued rewards.

![](../../.gitbook/assets/claiming-rewards-3.png)

4\. Station Extension should prompt you to sign a transaction that contains the reward claim operation. Confirm the details presented and enter your password to sign.

![](../../.gitbook/assets/claiming-rewards-4.png)

5\. Reward claim complete.

![](<../../.gitbook/assets/minting-beth-with-eth-5 (1).png>)
