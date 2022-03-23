# xAnchor Wormhole Bridge



Parses Wormhole messages from foreign chains, and any associated token transfer, if the action requires one. Receives outgoing messages and token transfers from xAnchor Core and forwards them back over the Wormhole Bridge.

### Config

| Key                          | Type     | Description                                           |
| ---------------------------- | -------- | ----------------------------------------------------- |
| `owner`                      | `String` | Address of contract owner                             |
| `wormhole_core_bridge_addr`  | `String` | Address of the Wormhole Core Bridge on Terra Mainnet  |
| `wormhole_token_bridge_addr` | `String` | Address of the Wormhole Token Bridge on Terra Mainnet |
| `xanchor_core_addr`          | `String` | Address of the xAnchor Core contract                  |
| `aust_cw20_addr`             | `String` | Address of the aUST token                             |

### InstantiateMsg

```rust
pub struct InstantiateMsg {
    pub owner: String,
    pub wormhole_core_bridge_addr: String,
    pub wormhole_token_bridge_addr: String,
    pub xanchor_core_addr: String,
    pub aust_cw20_addr: String,
}
```

### ExecuteMsg

```rust
pub enum ExecuteMsg {
    ProcessAnchorMessage {
        instruction_vaa: Binary,
        option_token_transfer_vaa: Option<Binary>,
    },
    SendAsset {
        asset: Asset,
    },

    /// Admin
    RegisterWormholeChainInfo {
        // Wormhole chain id.
        chain_id: u16,
        // address of contract on remote chain
        address: Vec<u8>,
    },
}
```
