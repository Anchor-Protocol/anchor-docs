# Untitled

## Interacting through the Gnosis Safe proxy contract

Under certain cases, such as depending on an external custodian to store and control UST assets, interacting through a separate proxy contract may be essential - as Anchor depends on tokenized forms of deposit positions \(aUST\) to process withdrawals. This is especially true when the said custodian has not yet implemented support for aUST, which is required to initiate a `transfer` of such assets to the contract for redemption.

For such cases, a [Gnosis Safe proxy contract](https://docs.gnosis.io/safe/) can be set up for interacting with EthAnchor contracts. This will add additional contract calls for depositing to and withdrawing from the proxy contract for UST assets, but allows the client to not hold any aUST tokens directly.

### Setup Guide

For general information around the Gnosis Safe contracts and detailed execution instructions through the Gnosis proxy, please refer to the [Gnosis Docs](https://docs.gnosis.io/safe/docs/contracts_tx_execution/). This guide only covers setup and execution instructions that are specific to Anchor Ethereum smart contracts.

**Contract Deployment**

Deployment code and instructions are available [here](https://github.com/gnosis/safe-contracts). In most cases, the Anchor Protocol team will deploy relevant contracts alongside with necessary Ethereum Wrapper contracts on the client's behalf.

**Contract Initialization**

Under `GnosisSafe.sol`, there is a `setup()` function defined as

```text
function setup(
    address[] calldata _owners,
    uint256 _threshold,
    address to,
    bytes calldata data,
    address fallbackHandler,
    address paymentToken,
    uint256 payment,
    address payable paymentReceiver
) external
```

call this function with the following parameters:

`gnosisSafeMastercopy.setup([account_address], 1, 0, "0x", 0, 0, 0)`

where `gnosisSafeMastercopy` is the "mastercopy" of all proxy contracts, each being linked to one wallet address. `account_address` is the Ethereum address that UST deposits will be originated from \(i.e. the custodial address\). **This should be done for every account that will be used for interaction with the Anchor smart contracts.**

Please be sure to store the proxy contract address for `account_address` .

Note **NOT** to use `delegatecall`, as that will result in aUST being returned to the **custodial address** instead of the safe proxy contract.

**Contract Interaction**

Interacting with Ethereum Anchor contracts through the Gnosis Safe proxy involve one function call: `execTransaction()`. This creates a standard function call initiated from the proxy contract \(excluding cases where a `delegatecall` operation mode is specified\), regardless of the actual caller of the `execTransaction()` function.

To call `initDepositStable()`, the following operations should be performed in sequence:

1. Transfer `amount` UST from the custodial address to the proxy contract. 
2. Construct an **unsigned transaction payload**  for `initDepositStable(uint256 amount)` -- Contract ABI for all the contracts will be required. The Anchor Protocol team will provide all necessary contract source code and ABI specifications for this.
3. Create a **signature payload** of the intended contract call transaction signed with the custodial address. More specifically, using the [transaction construction API](https://app.bitgo.com/docs/#operation/v2.wallet.tx.build) provided by the custodian \(such as BitGo\), generate and sign a transaction that calls `initDepositStable(uint256 amount)` . Provided that the unsigned transaction payload is already built, signing the transaction is relatively trivial.
4. **Estimate gas** -- run the following. a transaction will be executed and then immediately be reverted, with the results of the `revert(string(abi.encodePacked(requiredGas)))` statement being the gas requirement for the following function call. As this uses the EVM `REVERT` opcode, no gas is _actually_ consumed - only simulated.

```text
proxyContract.requiredTxGas(
    ETHANCHOR_SUBCONTRACT_ADDRESS, // to
    0, // Ether value - as we don't need any Ether this is set to 0
    data, // unsigned transaction payload
    CALL // set mode as normal contract call - avoid delegatecall
)
```

1. **Transaction execution** -- using `requiredGas`, run `execTransaction` with the following parameters

```text
proxyContract.execTransaction(
    ETHANCHOR_SUBCONTRACT_ADDRESS, // to
    0, // Ether value - as we don't need any Ether this is set to 0
    data, // unsigned transaction payload
    CALL, // set mode as normal contract call - avoid delegatecall
    0,
    0,
    0,
    0,
    0,
    SIGNATURE_PAYLOAD // signed transaction payload - EIP-1271
)
```

in cases of UST withdrawals with `finishRedeemStable()`, the same procedure applies - although a separate ERC-20 `transfer()` call needs to be made to `account_address` with another `execTransaction()` call.

