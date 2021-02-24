# Update Market Parameters

The mechanism behind Anchor's money market is controlled by a set of carefully determined parameters, listed below alongside their initial values at the time of protocol genesis:

| Parameter Name | Description |
| :--- | :--- |
| `Target Deposit Rate` |  |
| `Threshold Deposit Rate` | Threshold deposit rate to trigger interest buffer distribution |
| `Buffer Distribution Factor` | Maximum portion of interest buffer that can be distributed per deposit rate subsidization |
| `Max Borrow Factor` | Maximum  |
| `Valid Price Timeframe` |  |

Genesis Values

| Parameter Name | Value |
| :--- | :--- |
|  |  |





However, the structure of financial markets of  the underlying assets are not static, so it is not unreasonable to expect that the Mirror Protocol should also be able to adapt to these changes. To further account for the fact that mirrored assets come in a variety of classes, the two mint parameters can be voted separately for each individual asset.

The community is able to change the two parameters of an existing mAsset through a [**Modify Mint Parameters**]() ****poll.

### 4. Modify Mint Parameters

Through this poll, the existing Mirror Protocol parameters for a specific individual mAsset can be modified. The possible parameters to be updated consist of either the `Auction Discount` and the `Minimum Collateral Ratio`.

| Field | Description | Type |
| :--- | :--- | :--- |
| Title | Title of the poll | Required |
| Reason for modifying governance parameter | Short description of the poll | Required |
| Information Link | External URL for further information | Optional |
| Quorum | Minimum quorum required for accepting a poll \(in percentage\) | Optional |
| Threshold | Minimum percentage of `YES` votes to pass a poll \(in percentage\) | Optional |
| Voting Period | Length of poll \(in units of blocks\) | Optional |
| Effective Delay | Length of delay before protocol integration for a passed poll \(in units of blocks\) | Optional |
| Expiration Period | Length of expiration period for a failed poll \(in units of blocks\) | Optional |
| Proposal Deposit | Minimum deposit to start a poll \(in units of MIR\) | Optional |

### 

