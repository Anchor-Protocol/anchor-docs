# Address Proxy

The Address Proxy contract represents a foreign wallet address on the Terra chain. The xAnchor core creates an Address Proxy creates for every foreign address that interacts with Anchor. This will forward all actions received from xAnchor to Anchor.

### Config

| Key                | Type      | Description                                                             |
| ------------------ | --------- | ----------------------------------------------------------------------- |
| `core_address`     | `String`  | Address of xAnchor Core Contract                                        |
| `market_address`   | `String`  | Address of Anchor Market Contract                                       |
| `overseer_address` | `String`  | Address of Anchor Overseer Contract                                     |
| `aterra_address`   | `String`  | Address of aUST Token                                                   |
| `anc_token`        | `String`  | Address of ANC Token                                                    |
| `anc_gov_address`  | `String`  | Address of Anchor Governance Contract                                   |
| `chain_id`         | `u16`     | Chain ID of the foreign wallet that this address proxy instance maps to |
| `address`          | `Vec<u8>` | Address of the foreign wallet that this address proxy instance maps to. |

### InstantiateMsg

```rust
pub struct InstantiateMsg {
    pub market_address: String,
    pub overseer_address: String,
    pub anc_gov_address: String,
    pub aterra_address: String,
    pub anc_token: String,
    pub core_address: String,
    pub chain_id: u16,
    pub address: Vec<u8>,
}
```

### ExecuteMsg

```rust
pub enum ExecuteMsg {
    Receive(Cw20ReceiveMsg),

    /// depositors
    DepositStable {},
    /// borrowers
    RepayStable {},

    /// unlock + withdraw collateral (withdraw and unlock on anchor are combined into a single op)
    UnlockCollateral {
        asset: Asset,
    },
    BorrowStable {
        borrow_amount: Uint256,
    },
    ClaimRewards {},
    WithdrawAsset {
        asset_info: AssetInfo,
    },

    /// Internal Only
    ForwardDifference {
        asset_info: AssetInfo,
        to: String,
    },

    /// Governance
    StakeVotingTokens {
        stake_amount: Uint256,
    },
    WithdrawVotingTokens {
        unstake_amount: Uint256,
    },
    CastVote {
        poll_id: u64,
        vote: VoteOption,
        amount: Uint128,
    },
}
```
