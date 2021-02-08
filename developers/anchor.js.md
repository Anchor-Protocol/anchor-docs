# Anchor.js

{% hint style="info" %}
This section 
{% endhint %}

The Anchor.js SDK allows applications with JavaScript runtimes to interact with Anchor Protocol. It supports easy fabrication of messages relevant to Terra-side Anchor smart contracts, used to make contract calls or query contract state.

Anchor.js is developed to be used in tandem with [Terra.js](https://terra-project.github.io/terra.js/), required to interact with the Terra blockchain.

The full API reference of Anchor.js can be found [here](https://terra-project.github.io/terra.js/).

### Installation

Anchor.js is available as a package on NPM ~~\(TBD\)~~. 

```text
$ npm install -S @terra-money/terra.js @anchor-protocol/anchor.js
```

### Usage \(TBD\)

#### `Anchor` Object

Anchor.js can be utilized to either query the state of Anchor smart contracts or fabricate `MsgExecuteContract` objects to be included in Terra transactions.

Creating an `Anchor` object:

```javascript
import { LCDClient } from '@terra-money/terra.js';
import { Anchor } from '@anchor-protocl/anchor.js';

// default -- uses Columbus-4 core contract addresses
const anchor = new Anchor();

// optional -- specify contract addresses and assets
const anchor = new Anchor({
  lcd: new LCDClient(...),
  key: new MnemonicKey(), // or other Terra.js-compliant key
  bluna: {
    Hub
    Reward
    blunaToken: {
      name: 'BLUNA'; 
      symbol: 'BLUNA'; 
      token: 'terra1...';
    }
  }, 
  moneyMarket: {
    overseer: 'terra1...'; 
    market: 'terra1...'; 
    blunaCustody: 'terra1...'; 
    interestModel: 'terra1...'; 
    oracle: 'terra1...'; 
  }
  liquidations: {
    liquidation: 'terra1...'; 
  }
});
```

#### Query

The `Anchor` object contains smart contract queries for all Anchor contracts, which will be run via the specified `LCDClient`.

```javascript
async function main() {
  const result = await anchor.moneyMarket.overseer.getConfig();
}

main().catch(console.error);
```

#### Execute

The `Anchor` object contains functions for fabricating `MsgExecuteContract` messages to be included in a transaction and broadcasted.

```javascript
const wallet = anchor.lcd.wallet(mirror.key);

async function claimRewards() {
  const tx = await wallet.createAndSignTx({
    msgs: [anchor.bluna.claimRewards('terra1...')],
    fee: new StdFee(200_000, { uluna: 20_000_000 })
  });
  return await anchor.lcd.tx.broadcast(tx);
}

async function main() {
  await claimRewards();
}

main().catch(console.error);
```

