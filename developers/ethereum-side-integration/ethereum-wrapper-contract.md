# Ethereum Wrapper Contract

Anchor's Ethereum wrapper contract is a client-specifically generated smart contract on the Ethereum blockchain to handle wrapped UST deposits to Anchor Protocol. Both depositing wrapped UST and redeeming wrapped aUST is processed with an `init` - `finish` architecture. It is important to note that additional processing time \(separate from time required for Ethereum tx confirmation\) is needed in order for `init` requests, until which `finish` requests will result in failure.

Additionally, wrapper contracts can only process requests in series, allowing an additional request to be made only after the finish operation for the previous request was successfully executed.

Anchor Ethereum wrapper contracts have two execution modes: **standard** and **custodied**. **Standard** mode functions return aUST back to `msg.sender`, in which they can be potentially utilized with other Ethereum DeFi applications. **Custodied** mode functions do not return aUST back to `msg.sender`, but only holds aUST under the contract account. As there can be only one custody contract per authorized account, redeeming custodied aUST back to UST can be done at any time as long as the sender is authorized.

## Events

### `InitDeposit`

Emitted when wrapped UST is requested for deposit to Anchor via `initDepositStable`.

```text
event InitDeposit(address indexed sender, uint256 amount, bytes32 to);
```

### `FinishDeposit`

Emitted when wrapped aUST is claimed from Anchor via `finishDepositStable`.

```text
event FinishDeposit(address indexed sender);
```

### `InitRedeem`

Emitted when wrapped aUST is requested for redemption to Anchor via `initRedeemStable`.

```text
event InitRedeem(address indexed sender, uint256 amount, bytes32 to);
```

### `FinishRedeem`

Emitted when wrapped UST is claimed from Anchor via `finishRedeemStable`.

```text
event FinishDeposit(address indexed sender);
```

### `EmergencyWithdrawActivated`

Emitted when `emergencyWithdraw` is activated for withdrawing ERC-20 tokens from the contract.

```text
event EmergencyWithdrawActivated(address tokenAddress, uint256 amount);
```

## Functions

### `initDepositStable`

Accepts new wrapped UST deposits.

```text
function initDepositStable(uint256 amount) external;
```

**Prerequisite**: must have called `approve()` for an `allowance` of at least `amount` for the wrapped UST contract, `ActionFlag` is set to `false`  
**Accepts**: `amount` - how much UST to deposit  
**Updates**: `ActionFlag` to `true`  
**Emits**: `InitDeposit`

### `finishDepositStable`

Claims resulting wrapped aUST after deposit.

```text
// standard mode
function finishDepositStable() external;
```

**Prerequisite**: aUST balance of account-specific endpoint contract must be greater than 0, `ActionFlag` is set to `true`  
**Updates**: sets `ActionFlag` to `false`, `transfer`s all aUST balances from contract address to `tx.origin`  
**Emits**: `FinishDeposit`

```text
// custodied mode
function finishDepositStableCustody() external;
```

**Prerequisite**: aUST balance of account-specific endpoint contract must be greater than 0, `ActionFlag` is set to `true`  
**Updates**: sets `ActionFlag` to `false`  
**Emits**: `FinishDeposit`

```text
// fallback function
function finishDepositStable(bool _isCustodyEnabled) external;
```

**Prerequisite**: aUST balance of account-specific endpoint contract must be greater than 0, `ActionFlag` is set to `true`  
**Checks**: `_isCustodyEnabled`. If this value is set to `true`, `delegatecall`s `finishDepositStableCustody`. Otherwise, `delegatecall`s `finishDepositStable`.  
**Emits**: `FinishDeposit`

### `initRedeemStable`

Accepts wrapped aUST for redemption back to wrapped UST.

```text
// standard mode
function initRedeemStable(uint256 amount) external;
```

**Prerequisite**: must have called `approve()` for an allowance of at least `amount` for the wrapped aUST contract, `ActionFlag` is set to `false`  
**Accepts**: `amount` - how much aUST to redeem back to UST  
**Updates**: `ActionFlag` to `true`**IMPORTANT**: aUST redemptions may fail if UST buffer is low on the Terra side Anchor money market → be sure to check account contract balances & `initRedeemStable()` `success` parameters.  
**Emits**: `InitRedemption`

```text
// custodied mode
function initRedeemStableCustody(uint256 amount) external;
```

**Prerequisite**: `ActionFlag` is set to `false`  
**Accepts**: `amount` - how much aUST to redeem back to UST. If this value is set to 0, all balances held within the contract are redeemed back to UST. Otherwise, `amount` aUST held under the contract account is redeemed back to UST \(assuming that contract aUST balances is equal to or larger than `amount`\).  
**Updates**: `ActionFlag` to `true`**IMPORTANT**: aUST redemptions may fail if UST buffer is low on the Terra side Anchor money market → be sure to check account contract balances & `initRedeemStable()` `success` parameters.  
**Emits**: `InitRedemption`

```text
// fallback function
function initRedeemStableCustody(uint256 amount, bool _isCustodyEnabled) external;
```

**Prerequisite**: `ActionFlag` is set to `false`  
**Accepts**: `amount` - how much aUST to redeem back to UST. If this value is set to 0, all balances held within the contract; `_isCustodyEnabled` - an indicator to which mode `amount` should be passed as a parameter to.  
**Checks**: `_isCustodyEnabled`. If this value is set to `true`, `delegatecall`s `initRedeemStableCustody`. Otherwise, `delegatecall`s `initRedeemStable`.  
**Emits**: `InitRedemption`

### `finishRedeemStable`

Claims resulting wrapped UST after withdrawal.

```text
function finishRedeemStable() external;
```

**Prerequisite**: UST balance of account-specific endpoint contract must be greater than 0, `ActionFlag` is set to `true`  
**Updates**: sets `ActionFlag` to `false`, transfers all UST balances from contract address to `tx.origin`  
**Emits**: `FinishRedemption`

### `reportFailure`

Reports any failures in-between `init` operations to allow the AnchorEth bot to return any funds, and reset `ActionFlag` back to `false`. Only callable by contract owner.

```text
function reportFailure() external;
```

**Prerequisite**: UST balance of account-specific endpoint contract must be 0, `ActionFlag` is set to `true`  
**Updates**: sets `RedemptionFlag` to `false`

### `emergencyWithdraw`

Withdraws all balances of any ERC-20 token from the contract address. Only callable by contract owner.

```text
function emergencyWithdraw(address _tokenAddress) external;
```

**Prerequisite**: ERC-20 token balances of token contract `_tokenAddress` at contract address must be greater than 0  
**Updates**: transfers all ERC-20 token balances of token contract `_tokenAddress` back to `msg.sender`

## ~~\[DEPRECATED\] Interacting through the Gnosis Safe proxy contract~~

Under certain cases, such as depending on an external custodian to store and control UST assets, interacting through a separate proxy contract may be essential - as Anchor depends on tokenized forms of deposit positions \(aUST\) to process withdrawals. This is especially true when the said custodian has not yet implemented support for aUST, which is required to initiate a `transfer` of such assets to the contract for redemption.

For such cases, we recommend setting up a [Gnosis Safe proxy contract](https://docs.gnosis.io/safe/) for interacting with Anchor contracts. This will add additional contract calls for depositing to and withdrawing from the proxy contract for UST assets, but allows the client to not hold any aUST tokens directly.

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
    )
        external
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

