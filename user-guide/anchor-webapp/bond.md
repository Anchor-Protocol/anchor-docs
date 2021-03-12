# BOND

The **BOND** page enables users to easily interact with bAsset tokens. Through this page, users can mint bAssets, burn bAsset to redeem the underlying Asset, and claim bAsset rewards.

{% hint style="info" %}
As of March 17th, 2021, Anchor only supports [Bonded Luna \(bLuna\)](../../protocol/bonded-assets-bassets/bonded-luna-bluna.md) as providable collateral. Other bAssets will soon be added to the protocol. 
{% endhint %}

## Minting bLuna

1. Navigate to the **MINT** tab in the **BOND** page. 

![](../../.gitbook/assets/bond-mint-1.png)

2. Enter the amount of Luna to use in minting or the amount of bLuna to mint.

![](../../.gitbook/assets/bond-mint-2.png)

3. Select a Terra blockchain validator to stake Luna and click the **\[Proceed\]** button to confirm.

![](../../.gitbook/assets/bond-mint-3.png)

4. Station Extension should prompt you to sign a transaction that contains the mint operation. Confirm the details presented and enter your password to sign.

![](../../.gitbook/assets/bond-mint-4.png)

5. Mint complete.

![](../../.gitbook/assets/bond-mint-5.png)

## Burning bLuna to redeem Luna

1. Navigate to the **BURN** tab in the **BOND** page. 

![](../../.gitbook/assets/bond-burn-1.png)

2. Select a burn method. **BURN** and **INSTANT BURN** each correspond to:

* **BURN**: Burn bLuna through the bLuna protocol and redeem Luna. Redeemed Luna can be withdrawn after the Terra blockchain's unbonding period. Redemption is done with the current bLuna exchange rate but requires at least 21 days and the redemption amount may be affected by validator slashing.

![](../../.gitbook/assets/bond-burn-burn.png)

* **INSTANT BURN**: Swap bLuna for Luna through [Terraswap](https://terraswap.io/), an [Uniswap](https://uniswap.org)-like automated market marker \(AMM\) protocol on Terra. This process is instant but may suffer from trade slippage and Terraswap commissions.

![](../../.gitbook/assets/burn-burn-instant.png)

3. Enter the amount of bLuna to burn / instant burn or the amount of Luna and click the **\[Proceed\]** button to confirm.

* **BURN**

![](../../.gitbook/assets/bond-burn-burn-3.png)

* **INSTANT BURN**

![](../../.gitbook/assets/bond-burn-instant-3.png)

4. Station Extension should prompt you to sign a transaction that contains the burn operation. Confirm the details presented and enter your password to sign.

* **BURN**

![](../../.gitbook/assets/bond-burn-burn-4.png)

* **INSTANT BURN**

![](../../.gitbook/assets/bond-burn-instant-4.png)

5. Burn complete. In the case of **BURN**, redeemed Luna can be withdrawn at the **CLAIM** tab after a unbonding period of at least 21 days.

* **BURN**

{% hint style="info" %}
bLuna burn requests currently in the unbonding period can be viewed at the [**CLAIM**](bond.md#withdrawing-luna-from-burnt-luna) tab.
{% endhint %}

![](../../.gitbook/assets/bond-burn-burn-5.png)

* **INSTANT BURN**

![](../../.gitbook/assets/bond-burn-instant-5.png)

### Withdrawing Luna from burnt Luna

{% hint style="info" %}
This subsection is only applicable for those that have burnt Luna through **BURN** and not **INSTANT BURN**.
{% endhint %}

1. Navigate to the **CLAIM** tab in the **BOND** page.

![](../../.gitbook/assets/bond-burn-burn-withdraw-1.png)

2. Check the amount of redeemed Luna available for withdrawal \(**Withdrawable Amount**\). Click the **\[Withdraw\]** button to withdraw redeemed Luna.

![](../../.gitbook/assets/bond-burn-burn-withdraw-2.png)

3. Station Extension should prompt you to sign a transaction that contains the Luna withdraw operation. Confirm the details presented and enter your password to sign.

![](../../.gitbook/assets/bond-burn-burn-withdraw-3.png)

4. Withdraw complete.

![](../../.gitbook/assets/bond-burn-burn-withdraw-4.png)

## Claiming accrued bLuna rewards

{% hint style="info" %}
bLuna rewards only accrue only if the user is currently holding bLuna tokens. Users do not accrue rewards from bLuna tokens deposited to Anchor.
{% endhint %}

1. Navigate to the **CLAIM** tab in the **BOND** page.

![](../../.gitbook/assets/bond-claim-1.png)

2. Check the amount of accrued bLuna rewards \(**Claimable Rewards**\). Click the **\[Claim\]** button to claim accrued rewards.

![](../../.gitbook/assets/bond-claim-2%20%281%29.png)

3. Station Extension should prompt you to sign a transaction that contains the reward claim operation. Confirm the details presented and enter your password to sign.

![](../../.gitbook/assets/bond-claim-3.png)

4. Reward claim complete.

![](../../.gitbook/assets/bond-claim-4%20%281%29.png)

