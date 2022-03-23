# xAnchor Core

The xAnchor Core contract receives messages from the foreign chain (via the xAnchor Bridge), and forwards them to the AddressProxy contract. If needed, it will initialize a copy of the AddressProxy contract for any foreign address that doesn’t have a corresponding proxy contract already deployed.

### Config

| Key                     | Type     | Description                                             |
| ----------------------- | -------- | ------------------------------------------------------- |
| `address_proxy_code_id` | `u64`    | The cosmwasm Code ID of the Address Proxy contract WASM |
| `owner`                 | `String` | Address of contract owner                               |
| `overseer_address`      | `String` | Address of Anchor Overseer Contract                     |
| `anc_gov_address`       | `String` | Address of Anchor Governance Contract                   |

### InstantiateMsg

```rust
pub struct InstantiateMsg {
    pub address_proxy_code_id: u64,
    pub owner: String,
    pub overseer_address: String,
    pub anc_gov_address: String,
}
```

### ExecuteMsg

```rust
pub enum ExecuteMsg {
    InitializeAddressProxy {
        chain_id: u16,
        address: Vec<u8>,
    },
    Receive(Cw20ReceiveMsg),
    /// depositors
    DepositStable {
        sender_chain: u16,
        sender_address: Vec<u8>,
    },
    /// borrowers
    RepayStable {
        sender_chain: u16,
        sender_address: Vec<u8>,
    },

    /// unlock + withdraw collateral (withdraw and unlock on anchor are combined into a single op)
    UnlockCollateral {
        sender_chain: u16,
        sender_address: Vec<u8>,
        asset: Asset,
    },
    BorrowStable {
        sender_chain: u16,
        sender_address: Vec<u8>,
        borrow_amount: Uint256,
    },
    ClaimRewards {
        sender_chain: u16,
        sender_address: Vec<u8>,
    },

    /// stuck assets
    WithdrawAsset {
        sender_chain: u16,
        sender_address: Vec<u8>,
        asset_info: AssetInfo,
    },

    /// support governance
    StakeVotingTokens {
        sender_chain: u16,
        sender_address: Vec<u8>,
        stake_amount: Uint256,
    },
    WithdrawVotingTokens {
        sender_chain: u16,
        sender_address: Vec<u8>,
        unstake_amount: Uint256,
    },
    CastVote {
        sender_chain: u16,
        sender_address: Vec<u8>,
        poll_id: u64,
        vote: VoteOption,
        amount: Uint128,
    },

    /// admin and internal
    AddBridges {
        bridges: Vec<String>,
    },

    UpdateConfig {
        owner: Option<String>,
        address_proxy_code_id: Option<u64>,
    },

    /// internal Only
    ForwardDifferenceAndInitiateBridgeTransfer {
        asset_info: AssetInfo,
        to: String,
    },
}
```
