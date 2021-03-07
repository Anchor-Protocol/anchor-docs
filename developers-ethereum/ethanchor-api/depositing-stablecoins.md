# Depositing Stablecoins

ERC20 stablecoins \(e.g. UST\) can be redeemed to receive ERC20 aTerra using the below endpoints:

| Endpoint | Method | Description |
| :--- | :--- | :--- |
| [`init_deposit_stable`](depositing-stablecoins.md#initiate-stablecoin-deposit) | POST | Initiates the deposit of ERC20 stablecoins |
| [`finish_deposit_stable`](depositing-stablecoins.md#finish-stablecoin-deposit) | POST | Claims minted ERC20 aTerra |
| [`deposit_stable_status`](depositing-stablecoins.md#get-stablecoin-deposit-status) | GET | Gets status of an ongoing stablecoin deposit request |

{% api-method method="post" host="https://api.anchorprotocol.com" path="/v1/init\_deposit\_stable" %}
{% api-method-summary %}
Initiate stablecoin deposit
{% endapi-method-summary %}

{% api-method-description %}
`init_deposit_stable` allows you to fabricate an unsigned Ethereum Tx payload that initiates a stablecoin deposit request. You can sign the Tx payload transaction yourself and broadcast to the Ethereum network, or broadcast via any custodian API that supports signing a raw Tx payload.  
  
Note that only **one** `init_deposit_stable` operation can take place at the same time; even if you successfully broadcast the resulting Tx to the network, the EthAnchor Account contract will block any subsequent operations \(including stablecoin redemptions\) until an ongoing stablecoin deposit request is finished with `finish_deposit_stable`.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Anchor client key.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter required=true name="stable\_denom" type="string" %}
Denomination of stablecoin to deposit  
Example: `"uusd"`
{% endapi-method-parameter %}

{% api-method-parameter name="stable\_amount" type="string" required=true %}
\(uint256\) Amount of stablecoins to deposit to Anchor in 18 decimals.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
DepositStable raw Tx hash.
{% endapi-method-response-example-description %}

```text
{
    "success": true,
    "tx_hash": "0x......",
    "action": "anchor/init_deposit_stable",
    "stable_denom": "uusd", 
    "stable_amount": "500000000"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
You are not authorized to call this endpoint; client not registered.
{% endapi-method-response-example-description %}

```text
{
    "success": false,
    "error": "unauthorized; client not registered"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://api.anchorprotocol.com" path="/v1/finish\_deposit\_stable" %}
{% api-method-summary %}
Finish stablecoin deposit
{% endapi-method-summary %}

{% api-method-description %}
`finish_deposit_stable` allows you to finish a previously requested deposit stable operation.This endpoint returns an unsigned Ethereum transaction payload. You can sign the transaction yourself and send to the network, or broadcast using any custodian API that supports signing a raw Tx payload.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Anchor client key.
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
FinishDepositStable raw Tx hash
{% endapi-method-response-example-description %}

```text
{
    "success": true,
    "tx_hash": "0x......",
    "action": "anchor/finish_deposit_stable"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
You are not authorized to call this endpoint; client not registered
{% endapi-method-response-example-description %}

```text
{
    "success": false,
    "error": "unauthorized; client not registered"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.anchorprotocol.com" path="/v1/deposit\_stable\_status" %}
{% api-method-summary %}
Check stablecoin deposit status
{% endapi-method-summary %}

{% api-method-description %}
`GET /v1/deposit_stable_status` allows you to check the status of an ongoing `deposit_stable` operation.You may want to periodically check the progress of your `deposit_stable` request, since an operation may take up to minutes due to congestion on the Ethereum network.  
  
Please note that status being `"finished"` does **NOT** mean you have run a full cycle of `deposit_stable` operation; you still need to send another transaction from `POST /v1/finish_deposit_stable` to finalize your operation.This endpoint responds with HTTP 204 when there is no ongoing operation.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Anchor client access key.
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Current status of ongoing deposit stable operation.
{% endapi-method-response-example-description %}

```text
{
    // Phase
    // 0 - (Ethereum) wrapper contract has received stablecoins (e.g. UST) and 
    //     dispatched stablecoins through Shuttle
    // 1 - (Terra) terra-side client account has received stablecoins
    //     triggering DepositStable soon
    // 2 - (Terra) DepositStable action is processed and aTerra tokens (e.g. aUST) have
    //     been received
    // 3 - (Terra) aTerra tokens are sent to the ether-side wrapper contract
    //     through Shuttle
    // 4 - (Ethereum) contract has received aTerra; operation finished
    "phase": 0,

    // LastUpdated
    // Unix timestamp at which the last update to this response has been made
    "last_updated": 1608662606,

    // Status
    // Operation status
    // pending   - operation in flight
    // failed    - operation failed; last known tx has been recorded
    // finished  - operation finished; you can call /finish_deposit_stable
    "status": "pending",

    // Denomination
    // denomination stablecoin to be deposited
    "stable_denom": "uusd", 

    // Amount
    // amount of stablecoins to be deposited
    "stable_amount": "20000000",

    // TxHash
    // List of known tx hashes and the corresponding network name
    "tx_hash": [
        {
            "network": "ethereum",
            "tx_hash": "0x...."
        },
        {
            "network": "terra",
            "tx_hash": "00ABCD..."
        },
        ...
    ]
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=204 %}
{% api-method-response-example-description %}
No ongoing `deposit_stable` operation
{% endapi-method-response-example-description %}

```text
{
    "status": "idle"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
You are not authorized to call this endpoint; client not registered.
{% endapi-method-response-example-description %}

```text
{
    "success": false,
    "error": "unauthorized; client not registered"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

