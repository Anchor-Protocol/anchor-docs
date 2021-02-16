# Anchor WebApp

**The Anchor WebApp is the official web frontend for interacting with Anchor Protocol on the Terra network. It is located at** [**https://app.anchorprotocol.com**](https://app.anchorprotocol.com/)**.**

The Anchor WebApp offers a graphical user interface for accessing Anchor's core user operations, such as depositing & redeeming Terra stablecoins, borrowing Terra stablecoins with bAssets as collateral, and participating in Anchor governance.

{% hint style="warning" %}
The Mirror web app requires [Google Chrome](https://www.google.com/chrome/) and [Station Extension](https://chrome.google.com/webstore/detail/terra-station/aiifbnbfobpmeekipheeijimdpnlpgpp) to be installed. Please follow the instructions below to set up your browser to be able to access the WebApp.
{% endhint %}

## Terra Station Extension

{% hint style="warning" %}
As of February 16th, 2021, Station Extension is only available for Chromium-based web browsers.
{% endhint %}

Station Extension is a Chrome extension that lets you interact with smart contract web frontends with a wallet embedded in your browser. When interacting with Anchor Protocol through the WebApp, the application will generate a transaction for you in the proper format that encodes your desired interaction. Station Extension will detect and prompt you to sign and broadcast it to actually execute the operation.

### Installing Station Extension

1. Run [Google Chrome](https://www.google.com/chrome/). Station Extension is only available for Chromium-based web browsers.   
2. Install Station Extension [here](https://chrome.google.com/webstore/detail/terra-station/aiifbnbfobpmeekipheeijimdpnlpgpp?hl=en). 
3. **Terra Station** should now be visible on your extensions tray. 

### Creating a new wallet

1. Open **Station Extension**

2. Select **New Wallet**

![](../.gitbook/assets/picture-1%20%282%29.png)

3. Set wallet name and password. **Make sure to record 24 word seed phrase somewhere safe.** Select **\[Next\]** to proceed.

![](../.gitbook/assets/picture-2.png)

4. Confirm your seed phrase by inputting the correct words.

![](../.gitbook/assets/picture-3.png)

5. Select **\[Create a wallet\]** to finish.

![](../.gitbook/assets/picture-4.png)

### Using Hardware Ledger

To use a hardware Ledger wallet on to sign transactions on Terra Station Extension, user must meet the conditions below:

* Install Terra application using [Ledger Live](https://www.ledger.com/ledger-live/download/) application. Make sure that you have enabled Developer Mode on Ledger Live application from Settings &gt; Experimental Features to install Terra application. 
* Both Ledger Nano S and X are supported, but the device must be connected to the computer via USB cable. **Bluetooth connection is not supported** by Station Extension

To access hardware ledger wallet from Terra Station Extension, please follow the steps below: 

1. Connect and unlock your Ledger device

2. Open Terra application from the Ledger device

3. Select **\[Access with ledger\]** on Terra Station Extension menu

![](../.gitbook/assets/picture-5.png)

4. Once the Ledger device is connected to Terra Station Extension successfully, user can start signing transactions with their device. 

![](https://gblobscdn.gitbook.com/assets%2F-MLRzugf7mxc4ryNhTuq%2F-MMsz0hiKUhlI7K6Hu4t%2F-MMt82XaR7Sxw6v-WBj-%2Fimage.png?alt=media&token=0ae3faab-111c-4d95-b69d-4adee41f877d)

### Wallet Recovery

1. Select **\[Recover existing wallet\]**

![](../.gitbook/assets/picture-6.png)

2. Enter a new wallet name and password.

![](../.gitbook/assets/screen-shot-2021-02-16-at-10.46.49-pm.png)

3. Enter the 24 word seed phrase of the wallet to recover and select **\[Next\]** to finish.

![](https://gblobscdn.gitbook.com/assets%2F-MLRzugf7mxc4ryNhTuq%2F-MMsCvhqtM-AVYonMZ2s%2F-MMsp341Aiv2HFDiOGsP%2Fimage.png?alt=media&token=749f32cd-1d34-412b-9af1-486be5057025)

### Sending Tokens

1. Select **\[Send\]** button aligned to the tokens to send.

![](../.gitbook/assets/picture-7.png)

2. Input the information below and select **\[Next\]**:

* Address of the recipient
* Amount of token to send
* Memo is optional

{% hint style="info" %}
Station Extension also support cross-chain transfer to Ethereum addresses through [Shuttle](https://github.com/terra-project/shuttle) bridge. 
{% endhint %}

![](../.gitbook/assets/picture-8.png)

3. Select **\[Send\]** after setting the type and amount of tokens pay as transaction fee and entering the password. 

![](../.gitbook/assets/picture-9.png)

4. Station Extension will display the result of the transaction. Select **\[Ok\]** to return to the main page. 

![](../.gitbook/assets/picture-10.png)

## Getting TerraUSD

Mirror runs on the Terra blockchain, and uses TerraUSD \(UST\), a USD-pegged stablecoin, as the currency of denomination and as a form of collateral. You must have a UST balance in order to use the Mirror Web App, since it pays transaction fees in UST. In addition, it is necessary to trade mAssets and provide liquidity to mAsset/MIR Terraswap pools.

There are several ways to obtain UST:

* Purchasing UST from an exchange
* Swapping LUNA or other Terra stablecoins for UST

### Purchasing UST from an exchange

The most direct way to obtain UST is to purchase it from an exchange. This requires having an exchange account with some funds that you can exchange into UST via a trading pair. For instance, you can purchase UST through [Bittrex](https://bittrex.com) through UST/BTC and UST/USDT markets, or on [Kucoin](https://kucoin.com) against BTC, ETH, USDT and XRP. 

### Swapping LUNA or other Terra stablecoins tokens to UST

If you have a Terra account with a balance of Luna or other stablecoins such as KRT, or SDT, you can turn them into UST through the Terra blockchain's native swap functionality. For instance, on [Terra Station](https://station.terra.money), the official Terra desktop wallet, you can access the swap feature through the "Swap" page.

1. Navigate to the "Swap" page by clicking it on the sidebar. You should see a similar page:

![](../.gitbook/assets/picture-11.png)

2. There is a section near the bottom of the page titled "Swap coins". There, you can select the type of coins you want to swap into UST, based on the current "Terra exchange rate".

{% hint style="info" %}
The approximate spread and fee for swapping Luna to UST or other stablecoins to UST will be shown to you. The rules for determining the fees are covered in [Terra Docs](https://docs.terra.money/dev/spec-market).
{% endhint %}

3. Click **\[Next\]** and sign the transaction to complete the swap. 

