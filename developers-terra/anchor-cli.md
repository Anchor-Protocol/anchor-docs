# AnchorCLI

{% hint style="info" %}
This section provides a brief guide on how to interact with Anchor Protocol via `anchorcli`. For further information, please check its source code and documentation on [GitHub](https://github.com/Anchor-Protocol/anchorcli).
{% endhint %}

`anchorcli` is a command-line interface for Anchor Protocol on Terra and allows more advanced users to perform operations directly from their shell or terminal without having to interact with a graphical interface. `anchorcli` is built on top of `terracli` and allows you to use keys saved in its keychain.

## Installation

### Requirements

* Node.js 12+
* NPM
* `terracli` in your path

`anchorcli` can be installed through NPM:

```text
$ npm install -g @anchor-protocol/cli
```

The entrypoint `anchorcli` should then be available in your `path`:

```text
$ anchorcli
        
  Usage: anchorcli [options] [command]

  Command-line interface for interacting with Anchor Protocol on Terra

  Options:
    -V, --version   output the version number
    -v,--verbose    Show verbose error logs
    -h, --help      display help for command

  Commands:
    exec|x          Execute a function on a smart contract
    query|q         Run a smart contract query function
    help [command]  display help for command
```

## Configuration

By default, `anchorcli` works with the default configuration which is set to be for contracts on `tequila-0004`. This setting provides the address of contracts and specifies the setting for LCD provider, gas prices for fee estimation.

### Specifying LCD settings

Each network config should define how to connect to the Terra blockchain via LCD parameters.

```javascript
{
  "lcd": {
    "chainID": "tequila-0004",
    "URL": "https://tequila-lcd.terra.dev",
    "gasPrices": {
      "uluna": 0.15,
      "usdr": 0.1018,
      "uusd": 0.15,
      "ukrw": 178.05,
      "umnt": 431.6259
    },
    "gasAdjustment": 1.2
  }
}
```

#### Specifying Contracts

Each address configuration should point to the correct Anchor core contract addresses.

```javascript
{
  "contracts": {
    "bLunaHub": "terra1zdu9ph3429dtstv57ve3sfzr2vz2fclvmhn6td",
    "blunaToken": "terra1wq9f8p8f7gldztpdc4v3awngupfkap8wpxhtjr",
    "blunaReward": "terra16rjk255rjc6vt2qg7h8ntfdykrzfkt0e5wykus",
    "blunaAirdrop": "terra1093jc6g8gcuxp0vvfuzkk26rvnz38du886c88m",
    "mmInterestModel": "terra1rrutuqshjkgfh22n5eau8jac0vn4hsyhcz3ju2",
    "mmOracle": "terra1rz5chzn0g07hp5jx63srpkhv8hd7x8pss20w2e",
    "mmMarket": "terra1shmnertem9ujjxys2vxy2x92h0jzhctkjdv956",
    "mmOverseer": "terra174dcdqlkdwvsxqpkt47f9cy3anlv56ge5c05ex",
    "mmCustody": "terra1urn8z5uqukjzr8sqdjdryj6nt5v3qttfta2zwn",
    "mmLiquidation": "terra1sm76ssl55vnwnu96d00t8jl8pzwg5nvm02m5k7",
    "mmDistributionModel": "terra1ajawq49hutlsytxstys2x58464dy06rlzphmvy",
    "aTerra": "terra1xhxx7tgth24d8f9pz6vkjmvulp88xh9vl9kmxu",
    "terraswapblunaLunaPair": "terra1ykeemrj3nj6jlx5jxatmxkmjg894q3ftwnxn6k",
    "terraswapblunaLunaLPToken": "terra1uhf9u4a6vtkvnwn4cw6hmzaxm5zzzn6ukmjq2g",
    "terraswapAncUstPair": "terra10lkkzutjesqpphugfuzdzy5995u37tmc72a255",
    "terraswapAncUstLPToken": "terra1usrmk383nc6vjqq9sahkaca0p9k6cu0arvys43",
    "gov": "terra1dakqt3s8dywea9advxz4duxkuvglz3a34yczw9",
    "distributor": "terra1ytyge2vqtl9kcj8amrx9pxjypmw00244e7l3ye",
    "collector": "terra1hz6wk7psk5d0sh3u3vwtjrawvrk8hkt6vgnemm",
    "community": "terra15l0pep3ww9k4aa50jmf2dnj68ak9tc2s30m2d3",
    "staking": "terra1tcmhs005clcakqtquk58j3s5z0gkjm4c7wkzhu",
    "ANC": "terra1800p00qlxh0nmt0r0u9hv7m4lg042fnafng2t6",
    "airdrop": "terra18vlmtqhxgdp49vsfsk6pwvye8rg33nc2x92alr"
  }
}
```

## Usage

`anchorcli` allows you to:

* [**execute**](https://github.com/Anchor-Protocol/anchorcli#execute) state-changing functions on Anchor smart contracts
* [**query**](https://github.com/Anchor-Protocol/anchorcli#query) read-only data endpoints on Anchor smart contracts

### Execute

**USAGE: `anchorcli exec|x [options] [command]`**

```text
Execute a function on a smart contract

Options:
  --yaml                         Encode result as YAML instead of JSON
  -y,--yes                       Sign transaction without confirming (yes)
  --home <string>                Directory for config of terracli
  --from <key-name>              *Name of key in terracli keyring
  --generate-only                Build an unsigned transaction and write it to stdout
  -G,--generate-msg              Build an ExecuteMsg (good for including in poll)
  --base64                       For --generate-msg: returns msg as base64
  -b,--broadcast-mode <string>   Transaction broadcasting mode (sync|async|block) (default: sync) (default: "sync")
  --chain-id <string>            Chain ID of Terra node
  -a,--account-number <int>      The account number of the signing account (offline mode)
  -s,--sequence <int>            The sequence number of the signing account (offline mode)
  --memo <string>                Memo to send along with transaction
  --fees <coins>                 Fees to pay along with transaction
  --gas <int|auto>               *Gas limit to set per-transaction; set to "auto" to calculate required gas automatically
  --gas-adjustment <float>       Adjustment factor to be multiplied against the estimate returned by the tx simulation
  --gas-prices <coins>           Gas prices to determine the transaction fee (e.g. 10uluna,12.5ukrw)
  -h, --help                     display help for command

Commands:
  basset-hub [options]     Anchor bAsset Hub contract functions
  basset-reward [options]  Anchor bAsset reward contract functions
  basset-token [options]   Anchor bAsset token contract functions
  liquidation [options]    Anchor MoneyMarket Liquidation contract functions
  oracle [options]         Anchor MoneyMarket Liquidation contract functions
  market [options]         Anchor MoneyMarket Market contract functions
  custody [options]        Anchor MoneyMarket Custody contract functions
  overseer [options]       Anchor MoneyMarket Overseer contract functions
  interest [options]       Anchor MoneyMarket Interest contract functions
  help [command]           display help for command

```

### Query

**USAGE: `anchorcli query|q [options] [command]`**

```text
Run a smart contract query function

Options:
  -h, --help               display help for command

Commands:
  basset-hub [options]     Anchor bAsset hub contract queries
  basset-reward [options]  Anchor bAsset reward contract queries
  basset-token [options]   Anchor bAsset token  contract queries
  liquidation [options]    Anchor liquidation contract queries
  oracle [options]         Anchor oracle contract queries
  market [options]         Anchor market contract queries
  custody [options]        Anchor custody contract queries
  overseer [options]       Anchor overseer contract queries
  interest [options]       Anchor interest contract queries
  help [command]           display help for command

```

## Examples

This section illustrates the usage of `anchorcli` through some use cases. All examples assume you have a key in `terracli` keychain called `test1`.

#### Bond Luna to mint bLuna

Anchor protocol requires you to provide bAsset collaterals to borrow Terra stablecoins. bLuna tokens which are whitelisted as an eligible collateral can be used.

In order to obtain bLuna tokens, a user needs to bond Luna first, which then the contract will issue bLuna for the user. The following example is the way a user can bond Luna to gain bLuna:

```text
anchorcli x basset-hub bond --amount $BOND_AMOUNT --validator $VALIDATOR_ADDRESS --from test1 --gas auto --fees 100000uluna --b block
```

#### Query bLuna Balance

After bonding your Luna, you can get your bLuna balance with the following query:

```text
anchorcli q basset-token balance --address $USER_ADDRESS
```

