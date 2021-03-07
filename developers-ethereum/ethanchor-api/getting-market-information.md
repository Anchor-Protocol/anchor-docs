# Getting Market Information

General information about a stablecoin [market](../../protocol/money-market/#depositing-terra-stablecoins) can be retrieved via the below endpoint:

| Endpoint | Method | Description |
| :--- | :--- | :--- |
| [`stablecoin_info`](getting-market-information.md#get-stablecoin-information) | GET | Gets information about the specified stablecoin market |

{% hint style="warning" %}
Due to the asynchronous nature of EthAnchor, the actual aTerra exchange rate applied **will be different** to the exchange rate returned by `stablecoin_info`. The expected receive amount calculated using this value **WILL NOT** match the actual receive amount.
{% endhint %}

{% api-method method="get" host="https://api.anchorprotocol.com" path="/api/v1/stablecoin\_info/{stable\_denom}" %}
{% api-method-summary %}
Get stablecoin information
{% endapi-method-summary %}

{% api-method-description %}
`GET /api/v1/stablecoin_info/{stable_denom}` allows you to query the current status of money market with `stable_denom` as the quote.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="stable\_denom" type="string" required=true %}
Money market quote stablecoin denom.  
Example: `"uusd"`
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Money market state successfully retrieved.
{% endapi-method-response-example-description %}

```text
{
    // Denom
    // denom of the stablecoin money market
    “stable_denom”: “uusd”,

    // LiquidTerra
    // liquidTerra is the currently available stablecoin pool size in money market.
    “liquid_terra”: “100000”,

    // ExchangeRate
    // exchange rate between aTerra <> stablecoin (e.g. aUST <> UST)
    “exchange_rate”: “1.0123”,

    // LastUpdated
    // Unix timestamp at which the last update to this response has been made
    "last_updated": 1608710761,

    // ##################################################
    // # Below fields are borrower-related information. #
    // ##################################################

    // BorrowedTerra
    // Sum of all borrowed liabilities in this money market
    "borrowed_terra": "1000000",

    // UtilizationRatio
    // Ratio between borrowed deposit and total stablecoin deposit  
    "utilization_ratio": "0.5",

    // BorrowInterest
    // Interest rate per block
    "borrow_interest": "0.000000005707763",
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Money market of the provided denom could not be found.
{% endapi-method-response-example-description %}

```text
{
    "error": "money market not found"
}

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}
