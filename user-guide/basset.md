# BOND

The **BOND** page enables users to easily interact with bAsset tokens. Through this page, users can mint bAssets, burn bAsset to redeem the underlying Asset, and claim bAsset rewards.

{% hint style="info" %}
As of February 18th, 2021, Anchor only supports bonded Luna \(bLuna\) as providable collateral.
{% endhint %}

## Minting bLuna

1. Navigate to the **MINT** tab in the **BOND** page. 



2. Enter the amount of Luna to use in minting or the amount of bLuna to mint.



3. Select a Terra blockchain validator to stake Luna and click the **\[Proceed\]** button to confirm.



4. Station Extension should prompt you to sign a transaction that contains the mint operation. Confirm the details presented and enter your password to sign.



5. Mint complete.



## Burning bLuna to redeem Luna

1. Navigate to the **BURN** tab in the **BOND** page. 



2. Select a burn method. **BURN** and **INSTANT BURN** each correspond to:

* **BURN**: Burn bLuna through the bLuna protocol and redeem Luna. Redeemed Luna can be withdrawn after the Terra blockchain's unbonding period. Redemption is done with the current bLuna exchange rate but requires at least 21 days and the redemption amount may be affected by validator slashing. 
* **INSTANT BURN**: Swap bLuna to Luna through Terraswap. This process is instant but may suffer from trade slippage and Terraswap commissions.



3. Enter the amount of bLuna to burn / instant burn or the amount of Luna and click the **\[Proceed\]** button to confirm.



4. Station Extension should prompt you to sign a transaction that contains the burn operation. Confirm the details presented and enter your password to sign.



5. Burn complete. In the case of **BURN**, redeemed Luna can be withdrawn at the **CLAIM** tab after a unbonding period of at least 21 days.

{% hint style="info" %}
bLuna burn requests currently in the unbonding period can be viewed at the **CLAIM** tab.
{% endhint %}

### Withdrawing Luna from burnt Luna

{% hint style="info" %}
This subsection is only applicable for those that have burnt Luna through **BURN** and not **INSTANT BURN**.
{% endhint %}

1. Navigate to the **CLAIM** tab in the **BOND** page.



2. Check the amount of redeemed Luna available for withdrawal \(**Withdrawable Amount**\). Click the **\[Withdraw\]** button to withdraw redeemed Luna.



3. Station Extension should prompt you to sign a transaction that contains the Luna withdraw operation. Confirm the details presented and enter your password to sign.



4. Withdraw complete.



## Claiming accrued bLuna rewards

{% hint style="info" %}
bLuna rewards only accrue only if the user is currently holding bLuna tokens. Users do not accrue rewards from bLuna tokens deposited to Anchor.
{% endhint %}

1. Navigate to the **CLAIM** tab in the **BOND** page.



2. Check the amount of accrued bLuna rewards \(**Claimable Rewards**\). Click the **\[Claim\]** button to claim accrued rewards.



3. Station Extension should prompt you to sign a transaction that contains the reward claim operation. Confirm the details presented and enter your password to sign.



4. Reward claim complete.

