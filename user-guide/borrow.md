# BORROW

The **BORROW** page can be used to borrow Terra stablecoins from Anchor. The protocol requires users to provide bAsset tokens as collateral before making a loan.

Borrows can be made until the loan's loan-to-value \(LTV\) ratio reaches the MAX LTV, calculated based on collateral types, their prices, and deposit amount. Loans with a LTV ratio higher than MAX LTV can be liquidated, and thus it is important for borrowers to closely maintain their LTV ratios as changes in collateral prices may occur.

{% hint style="info" %}
As of February 18th, 2021, Anchor only supports Terra USD as the base currency.
{% endhint %}

The **BORROW** page displays:

* **Collateral Value**: Total USD-denominated value of all collaterals provided by user. 
* **Borrowed Value**: Total USD-denominated value of Terra stablecoins borrowed by user. 
  * **Borrowed**: Total amount of Terra stablecoins borrowed by user.
* **APR**: Current borrow annualized percentage rate \(APR\). 
  * **Accrued**: Amount of Terra stablecoins accrued as borrow interest. 
* **Linear Graph**: Displays the liquidation riskiness of user's loan position. 
  * **Borrow Limit**: Maximum USD value borrowable by user based on provided collaterals. 
  * **Borrowed Value / Borrow Limit Ratio**: Ratio value that signals user loan position's liquidation riskiness. Calculated by dividing user's borrowed value with their borrow limit.

## Providing bAsset collateral

{% hint style="info" %}
Users should first obtain bAsset tokens in order to utilize them as loan collateral. bAsset minting can be done at the [**BOND**](basset.md) page in Anchor WebApp.
{% endhint %}

1. Navigate to the **BORROW** page.



2. Decide a bAsset collateral to deposit and click **\[Provide\]**.



3. Enter the amount of collateral to provide. The provide amount can also be specified by selecting the loan's target LTV ratio via the slider bar. Click the **\[Proceed\]** button to confirm.



4. Station Extension should prompt you to sign a transaction that contains the collateral provide operation. Confirm the details presented and enter your password to sign.



5. Collateral provide complete.



## Borrowing Terra stablecoins

1. Navigate to the **BORROW** page.



2. Click **\[Borrow\]**.



3. Enter the amount of stablecoins to borrow. The borrow amount can also be specified by selecting the loan's target LTV ratio via the slider bar. Click the **\[Proceed\]** button to confirm.



4. Station Extension should prompt you to sign a transaction that contains the borrow operation. Confirm the details presented and enter your password to sign.



5. Borrow complete.



## Repaying borrowed Terra stablecoins

1. Navigate to the **BORROW** page.



2. Click **\[Repay\]**.



3. Enter the amount of stablecoins to repay. The repay amount can also be specified by selecting the loan's target LTV ratio via the slider bar. Click the **\[Proceed\]** button to confirm.



4. Station Extension should prompt you to sign a transaction that contains the repay operation. Confirm the details presented and enter your password to sign.



5. Repay complete.



## Withdrawing bAsset collateral

{% hint style="info" %}
Withdrawn bAsset collaterals can be redeemed to its underlying asset via the Anchor WebApp's [**BOND**](basset.md) page.
{% endhint %}

1. Navigate to the **BORROW** page.



2. Decide a bAsset collateral to withdraw and click **\[Withdraw\]**.



3. Enter the amount of collateral to withdraw. The withdraw amount can also be specified by selecting the loan's target LTV ratio via the slider bar. Click the **\[Proceed\]** button to confirm.



4. Station Extension should prompt you to sign a transaction that contains the collateral withdraw operation. Confirm the details presented and enter your password to sign.



5. Collateral withdraw complete.



