# Integration Flow

## Overview

Most major tokens and DeFi protocols are built on the Ethereum blockchain. As such, a significant portion of users on DeFi and integration partners often only support Ethereum \(and other EVM-compatible chains\). Thus, enabling Anchor on the Ethereum blockchain is crucial - especially for individuals holding Wrapped UST ERC-20 tokens, and third party integration partners who only supports interactions with the Ethereum blockchain.

The following sections illustrate how the Anchor contracts can be integrated with the Ethereum tech stack. More specifically, this document

* Goes through the asynchronous `init` - `finish` model of Anchor's Ethereum wrapper contracts
* Lists all of the available endpoints of Anchor's Ethereum wrapper contracts
* Lists available HTTP API endpoints for third parties to integrate with the Ethereum contracts, especially regarding transaction message fabrication.

## Considerations

Because Anchor's core logic & contracts live on the Terra blockchain \(and not Ethereum\), there needs to be a number of constraints that need to be met.

* **All operations follow an asynchronous model.** Unlike other DeFi protocols on Ethereum, Anchor is designed to be **interchain**. This means that \(i\) assets being supported on Anchor are **not native to Ethereum**, and \(ii\) all contract calls must be able to wrap core logic that exists on another blockchain. Thus, all Ethereum-side contract calls follow an `init` - `finish` architecture, as operations are not immediately finalized as of with a typical Ethereum contract.
* **Operation validity is partially validated off-chain.** As endpoints and core logic exist on two separate blockchains, operation validity should be partially validated off-chain. In most cases a call forwarding bot will make appropriate RPC calls to both Ethereum \(`ethrpc`\) and Terra \(Cosmos Light Client Daemon - `LCD`\) to verify state on the client level, but this does not guarantee, nor enforce _automatic state integrity_ for all blockchains. It is up to the **client** to resolve potential state clashes on different blockchains \(Ethereum and Terra\), and reject transactions if deemed necessary.
* **The API server provides transaction fabrication and static data queries.** This allows interfaces, building transactions and verifying cross-chain state to be mostly abstracted regardless of platform. While those operations may be done manually, the API server guarantees a minimum level of operation safety, including correct ordering of contract calls and state integrity.

