# xAnchor

xAnchor (CrossAnchor) is an extension to Anchor protocol, bringing all of Anchor’s functionality to other non-Terra blockchains, providing a seamless native-like experience. xAnchor is built on top of decentralized infrastructure, and requires little off-chain infrastructure. xAnchor currently supports the following chains:

* [Avalanche Network](https://www.avax.network)

### Usage

xAnchor is available through the Anchor web app. Switching from native Terra to other chains requires clicking the chain button (next to the Wallet Connection widget). All of xAnchor’s functionality is available through the web app. Note that transactions will take longer to process when using xAnchor, depending on how long transactions take to be bridged back and forth. Because this is the case, xAnchor allows for minimizing pending transactions, to continue using the app. In the case that a transaction fails or hangs there are options:

* The transaction hangs in the web app: The “Clear All” can purge broken transactions from the web app. Note that this action does _not_ cancel pending transactions on the blockchain, it simply fixes the interface.
* The operation doesn’t appear to have fully completed: The interface allows a user to “restore” an operation from a transaction hash associated with the operation (Found from their wallet explorer). This should allow the user to complete operations that fail on the last step of the process.
* The operation fails on the Terra chain: The interface allows users to withdraw tokens that appear to be “stuck” on the Terra-side xAnchor contracts.

Some features that exist on the native Terra-side Anchor web app will not be available on other chains, as they are Terra specific features. (e.g.: Governance, bAsset pages, etc.)

### Lifetime of a Transaction

Since xAnchor facilitates cross-chain transactions, each operation requires three transactions:

1. The initial transaction on the foreign chain, initiated by the user
2. A transaction to “redeem” the incoming messages (and token transfers, if applicable), on the Terra chain.
   1. In this case, redeem means paying the gas fee for the wormhole bridge transaction. This is currently done automatically by a bot.
3. A transaction to redeem the returning token transfers, if any. (e.g.: the `aUST` that would be returned from depositing `UST` in Anchor Earn)

### Developers

xAnchor contract documentation can be found here:[ xAnchor Contracts](../developers-xanchor/xanchor-contracts.md)
