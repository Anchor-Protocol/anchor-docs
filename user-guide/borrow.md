# BORROW

The **BORROW** page can be used to borrow Terra stablecoins from Anchor. The protocol requires users to provide bAsset tokens as collateral before making a loan.

Borrows can be made until the loan's loan-to-value \(LTV\) ratio reaches the MAX LTV, calculated based on collateral types, their prices, and deposit amount. Loans with a LTV ratio higher than MAX LTV can be liquidated, and thus it is important for borrowers to closely maintain their LTV ratios as changes in collateral prices may occur.

{% hint style="info" %}
As of March 17th, 2021, Anchor only supports Terra USD as the base currency.
{% endhint %}

The **BORROW** page displays:

* **Collateral Value**: Total USD-denominated value of all collaterals provided by user. 
* **Borrowed Value**: Total USD-denominated value of Terra stablecoins borrowed by user. 
  * **Borrowed**: Total amount of Terra stablecoins borrowed by user.
* **Net APY**: Net annualized percentage yield \(APY\) on borrows from both Borrow APR and Distribution APY



  * **Borrow APR**: Current annualized percentage rate \(APR\) of borrow interest.

  * **Distribution APY**: Current APY of ANC distribution to borrowers 

* **Linear Graph**: Displays the [risk ratio](../protocol/liquidations.md#collateral-liquidation) of user's loan position. 
  * **Borrow Limit**: Maximum USD value borrowable by user based on provided collaterals. 
  * **Borrowed Value / Borrow Limit Ratio**: Ratio value that signals user loan position's liquidation riskiness. Calculated by dividing user's borrowed value with their borrow limit.

## Providing bAsset collateral

{% hint style="info" %}
Users should first obtain bAsset tokens in order to utilize them as loan collateral. bAsset minting can be done at the [**BOND**](bond.md) page in Anchor WebApp.
{% endhint %}

1. Navigate to the **BORROW** page.

![](../.gitbook/assets/borrow-provide-1.png)

2. Decide a bAsset collateral to deposit and click **\[Provide\]**.

![](../.gitbook/assets/borrow-provide-2.png)

3. Enter the amount of collateral to provide. If the user already has a loan position, the provide amount can also be specified by selecting the loan's target LTV ratio via the slider bar. Click the **\[Proceed\]** button to confirm.

![](../.gitbook/assets/borrow-provide-3.png)

4. Station Extension should prompt you to sign a transaction that contains the collateral provide operation. Confirm the details presented and enter your password to sign.

![](../.gitbook/assets/borrow-provide-4.png)

5. Collateral provide complete.

![](../.gitbook/assets/borrow-provide-5.png)

## Borrowing Terra stablecoins

1. Navigate to the **BORROW** page.

![](../.gitbook/assets/borrow-borrow-1.png)

2. Click **\[Borrow\]**.

![](../.gitbook/assets/borrow-borrow-2.png)

3. Enter the amount of stablecoins to borrow. The borrow amount can also be specified by selecting the loan's target LTV ratio via the slider bar. Click the **\[Proceed\]** button to confirm.

![](../.gitbook/assets/borrow-borrow-3.png)

4. Station Extension should prompt you to sign a transaction that contains the borrow operation. Confirm the details presented and enter your password to sign.

![](../.gitbook/assets/borrow-borrow-4.png)

5. Borrow complete.

![](../.gitbook/assets/borrow-borrow-5.png)

## Repaying borrowed Terra stablecoins

1. Navigate to the **BORROW** page.

![](../.gitbook/assets/borrow-repay-1.png)

2. Click **\[Repay\]**.

![](../.gitbook/assets/borrow-repay-2.png)

3. Enter the amount of stablecoins to repay. The repay amount can also be specified by selecting the loan's target LTV ratio via the slider bar. Click the **\[Proceed\]** button to confirm.

![](../.gitbook/assets/borrow-repay-3.png)

4. Station Extension should prompt you to sign a transaction that contains the repay operation. Confirm the details presented and enter your password to sign.

![](../.gitbook/assets/borrow-repay-4.png)

5. Repay complete.

![](../.gitbook/assets/borrow-repay-5.png)

## Withdrawing bAsset collateral

{% hint style="info" %}
Withdrawn bAsset collaterals can be redeemed for their underlying assets at the Anchor WebApp's [**BOND**](bond.md) page.
{% endhint %}

1. Navigate to the **BORROW** page.



2. Decide a bAsset collateral to withdraw and click **\[Withdraw\]**.



3. Enter the amount of collateral to withdraw. The withdraw amount can also be specified by selecting the loan's target LTV ratio via the slider bar. Click the **\[Proceed\]** button to confirm.



4. Station Extension should prompt you to sign a transaction that contains the collateral withdraw operation. Confirm the details presented and enter your password to sign.



5. Collateral withdraw complete.



