# Redeeming Stablecoins

ERC20 aTokens can be redeemed to ERC20 stablecoins using the below endpoints:

| Endpoint Name | Method | Description |
| :--- | :--- | :--- |
| \`\`[`init_redeem_stable`](redeeming-stablecoins.md#initiate-stablecoin-redemption)\`\` | POST | Initiates the redemption of ERC20 aTokens |
| \`\`[`finish_redeem_stable`](redeeming-stablecoins.md#finish-stablecoin-redemption)\`\` | POST | Claims redeemed ERC20 stablecoins |
| \`\`[`redeem_stable_status`](redeeming-stablecoins.md#check-stablecoin-redemption-status)\`\` | GET | Gets status of an ongoing stablecoin redemption request |

{% api-method method="post" host="https://api.anchorprotocol.com" path="/v1/init\_redeem\_stable" %}
{% api-method-summary %}
Initiate stablecoin redemption
{% endapi-method-summary %}

{% api-method-description %}
`init_redeem_stable` allows you to initialize a stablecoin redemption request.This endpoint returns an unsigned Ethereum transaction payload. You can sign this transaction yourself and send to the network, or broadcast using any custodian API that supports signing a raw Tx payload.Note that only **one** `init_redeem_stable` operation can take place at the same time; even if you successfully broadcast the resulting Tx to the network, the Ethereum contract will block any subsequent operation until an ongoing redeem stable is finished with `finish_redeem_stable`.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Anchor client key.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="underlying\_denom" type="string" required=true %}
Underlying denomination of aToken to redeem from Anchor.Example: `"uusd"`
{% endapi-method-parameter %}

{% api-method-parameter name="a\_token\_amount" type="string" required=false %}
\(uint256\) amount of aTokens to redeem from Anchor.if empty, redeems all aToken holdings.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
`init_redeem_stable` unsigned Tx hash.
{% endapi-method-response-example-description %}

```text
{
    "success": true,
    "tx_hash": "0x......",
    "action": "anchor/init_redeem_stable",
    "underlying_denom": "uusd", 
    "a_token_amount": "500000000"
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

{% api-method method="post" host="https://api.anchorprotocol.com" path="/v1/finish\_redeem\_stable" %}
{% api-method-summary %}
Finish stablecoin redemption
{% endapi-method-summary %}

{% api-method-description %}
`finish_redeem_stable` allows you to finish a previously requested redeem stable operation.This endpoint returns an unsigned Ethereum transaction payload. You can sign this transaction yourself and send to the network, or broadcast using any custodian API that supports signing a raw Tx payload.
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
`finsish_redeem_stable` unsigned Tx hash.
{% endapi-method-response-example-description %}

```text
{
    "success": true,
    "tx_hash": "0x......",
    "action": "anchor/finish_redeem_stable"
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

{% api-method method="get" host="https://api.anchorprotocol.com" path="/v1/redeem\_stable\_status" %}
{% api-method-summary %}
Check stablecoin redemption status
{% endapi-method-summary %}

{% api-method-description %}
`GET /v1/redeem_stable_status` allows you to check the status of an ongoing `redeem_stable` operation.You may want to periodically check the progress of your `redeem_stable` request, since an operation may take up to minutes to finish due to congestion on either side of the networks.Please note that status being `"finished"` does **NOT** mean you have run a full cycle of `redeem_stable` operation; you still need to send another transaction from `POST /v1/finish_redeem_stable` to finalize your operation.This endpoint responds with HTTP 204 when there is no ongoing operation.
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
Current status of ongoing `redeem_stable` operation.
{% endapi-method-response-example-description %}

```text
{
    // Phase
    // 0 - (Ethereum) wrapper contract has received aTokens (e.g. aUST) and 
    //     dispatched through Shuttle
    // 1 - (Terra) terra-side client account has received aTokens
    //     triggering RedeemStable soon
    // 2 - (Terra) RedeemStable action is processed and stablecoins (e.g. UST) have
    //     been received
    // 3 - (Terra) stablecoins are sent to the ether-side wrapper contract
    //     through Shuttle
    // 4 - (Ethereum) contract has received stablecoins; operation finished
    "phase": 0,

    // LastUpdated
    // Unix timestamp at which the last update to this response has been made
    "last_updated": 1608662606,

    // Status
    // Operation status
    // pending   - operation in flight
    // failed    - operation failed; last known tx has been recorded
    // finished  - operation finished; you can call /finish_redeem_stable
    "status": "pending",

    // UnderlyingDenom
    // Underlying denomination of aTokens to be redeemed
    "underlying_denom": "uusd", 

    // aTokenAmount
    // amount of aTokens to be redeemed
    "a_token_amount": :20000000:,

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
No ongoing `redeem_stable` operation.
{% endapi-method-response-example-description %}

```text
{
    "status": "idle"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

