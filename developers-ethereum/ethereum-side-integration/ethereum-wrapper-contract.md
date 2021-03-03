# Ethereum Wrapper Contract

{% hint style="info" %}
As of March 12th, 2021, both Anchor and EthAnchor only supports UST.
{% endhint %}

The Ethereum wrapper contract handles 

Deposit / redeem requests of wrapped ERC20 stablecoins are first handled by the Ethereum wrapper contract.

Deposits and redemptions of wrapped UST and wrapped aUST are first handled

Anchor's Ethereum wrapper contract is a **client-specifically** generated smart contract on the Ethereum blockchain to handle wrapped UST deposits to Anchor. Both depositing wrapped UST and redeeming wrapped aUST is processed with an `init` - `finish` architecture. It is important to note that additional processing time \(separate from time required for Ethereum tx confirmation\) is needed in order for `init` requests, until which `finish` requests will result in failure.

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

### `InitRedemption`

Emitted when wrapped aUST is requested for redemption to Anchor via `initRedeemStable`.

```text
event InitRedeem(address indexed sender, uint256 amount, bytes32 to);
```

### `FinishRedemption`

Emitted when wrapped UST is claimed from Anchor via `finishRedeemStable`.

```text
event FinishDeposit(address indexed sender);
```

### `FailureReported`

Emitted when aUST redemption fails due to a lack of stablecoin liquidity in the money market.

```text
event FailureReported(); 
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
function initDepositStable(uint256 amount) public onlyAuthSender checkInit terraAddressSet 
```

**Prerequisite**: must have called `approve()` for an `allowance` of at least `amount` for the wrapped UST contract, `ActionFlag` is set to `false`  
**Accepts**: `amount` - how much UST to deposit  
**Updates**: `ActionFlag` to `true`  
**Emits**: `InitDeposit`

### `finishDepositStable`

Claims resulting wrapped aUST after deposit.

```text
// standard mode
function finishDepositStable() function initDepositStable(uint256 amount) public onlyAuthSender checkInit terraAddressSet 
```

**Prerequisite**: aUST balance of account-specific endpoint contract must be greater than 0, `ActionFlag` is set to `true`  
**Updates**: sets `ActionFlag` to `false`, `transfer`s all aUST balances from contract address to `tx.origin`  
**Emits**: `FinishDeposit`

```text
// custodied mode
function finishDepositStableCustody() public onlyAuthSender checkFinish terraAddressSet 
```

**Prerequisite**: aUST balance of account-specific endpoint contract must be greater than 0, `ActionFlag` is set to `true`  
**Updates**: sets `ActionFlag` to `false`  
**Emits**: `FinishDeposit`

```text
// fallback function
function finishDepositStable(bool _isCustodyEnabled) public onlyAuthSender checkFinish terraAddressSet 
```

**Prerequisite**: aUST balance of account-specific endpoint contract must be greater than 0, `ActionFlag` is set to `true`  
**Checks**: `_isCustodyEnabled`. If this value is set to `true`, `delegatecall`s `finishDepositStableCustody`. Otherwise, `delegatecall`s `finishDepositStable`.  
**Emits**: `FinishDeposit`

### `initRedeemStable`

Accepts wrapped aUST for redemption back to wrapped UST.

```text
// standard mode
function initRedeemStable(uint256 amount) public onlyAuthSender checkInit terraAddressSet 
```

**Prerequisite**: must have called `approve()` for an allowance of at least `amount` for the wrapped aUST contract, `ActionFlag` is set to `false`  
**Accepts**: `amount` - how much aUST to redeem back to UST  
**Updates**: `ActionFlag` to `true`**IMPORTANT**: aUST redemptions may fail if UST liquidity is low in the Terra side Anchor money market → be sure to check account contract balances & `initRedeemStable()` `success` parameters.  
**Emits**: `InitRedemption`

```text
// custodied mode
function initRedeemStableCustody(uint256 amount) public onlyAuthSender checkInit terraAddressSet 
```

**Prerequisite**: `ActionFlag` is set to `false`  
**Accepts**: `amount` - how much aUST to redeem back to UST. If this value is set to 0, all balances held within the contract are redeemed back to UST. Otherwise, `amount` aUST held under the contract account is redeemed back to UST \(assuming that contract aUST balances is equal to or larger than `amount`\).  
**Updates**: `ActionFlag` to `true`**IMPORTANT**: aUST redemptions may fail if UST buffer is low on the Terra side Anchor money market → be sure to check account contract balances & `initRedeemStable()` `success` parameters.  
**Emits**: `InitRedemption`

```text
// fallback function
function initRedeemStableCustody(uint256 amount, bool _isCustodyEnabled) public onlyAuthSender checkInit terraAddressSet 
```

**Prerequisite**: `ActionFlag` is set to `false`  
**Accepts**: `amount` - how much aUST to redeem back to UST. If this value is set to 0, all balances held within the contract; `_isCustodyEnabled` - an indicator to which mode `amount` should be passed as a parameter to.  
**Checks**: `_isCustodyEnabled`. If this value is set to `true`, `delegatecall`s `initRedeemStableCustody`. Otherwise, `delegatecall`s `initRedeemStable`.  
**Emits**: `InitRedemption`

### `finishRedeemStable`

Claims resulting wrapped UST after withdrawal.

```text
function finishRedeemStable() public onlyAuthSender checkFinish terraAddressSet 
```

**Prerequisite**: UST balance of account-specific endpoint contract must be greater than 0, `ActionFlag` is set to `true`  
**Updates**: sets `ActionFlag` to `false`, transfers all UST balances from contract address to `tx.origin`  
**Emits**: `FinishRedemption`

### `reportFailure`

Reports any failures in-between `init` operations to allow the AnchorEth bot to return any funds, and reset `ActionFlag` back to `false`. Only callable by contract owner.

```text
function reportFailure() public onlyController checkFinish 
```

**Prerequisite**: UST balance of account-specific endpoint contract must be 0, `ActionFlag` is set to `true`  
**Updates**: sets `RedemptionFlag` to `false`

### `emergencyWithdraw`

Withdraws all balances of any ERC-20 token from the contract address. Only callable by contract owner.

```text
function emergencyWithdraw(address _tokenAddress) public onlyController 
```

**Prerequisite**: ERC-20 token balances of token contract `_tokenAddress` at contract address must be greater than 0  
**Updates**: transfers all ERC-20 token balances of token contract `_tokenAddress` back to `msg.sender`

