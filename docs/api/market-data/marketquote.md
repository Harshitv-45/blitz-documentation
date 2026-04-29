## **Market Quote**

The Market Quote API allows you to retrieve detailed real-time market data for one or more instruments.

Using this API, you can fetch comprehensive quote information including last traded price (LTP), last traded quantity (LTQ), volume, OHLC data, open interest, and market depth (bid/ask levels).

This API is essential for building trading dashboards, order books, and real-time analytics systems.

---

`BASE URL: http://uat.quantxpress.com/md-api`


## Endpoint

| Method | URL                                             |
| ------ | ----------------------------------------------- |
| POST    | `/marketfeed/quote` |

---

## Headers

| Header        | Value            | Required |
| ------------  | ---------------- | -------- |
| Content-Type  | application/json | Yes      |
| Authorization | Bearer {access_token} | Yes      |
| Accept        | `*/*`              | Yes      |

---

## Request Payload

You can request market quotes by providing either a list of **Instrument IDs** or a list of **Instrument Names**.

=== "Using Instrument IDs"

    ```json
    {
      "InstrumentIds": [1010010002000001]
    }
    ```

=== "Using Instrument Names"

    ```json
    {
      "InstrumentNames": ["NSECM:RELIANCE", "NSEFO:NIFTY28042026FUT"]
    }
    ```

---

## Parameters

| Parameter | Type | Required | Description |
| --------- | ---- | -------- | ----------- |
| `InstrumentIds` | array of Long | No* | List of numeric instrument identifiers. (*Required if `InstrumentNames` is not provided). |
| `InstrumentNames` | array of String | No* | List of exact instrument names. (*Required if `InstrumentIds` is not provided). |

!!! info "Instrument Naming Convention"
    When fetching by Instrument Name, the string follows a specific pattern based on the segment:
    
    * **Cash Market (Equity)**: `[ExchangeSegment]:[Symbol]`
        - *Example*: `NSECM:RELIANCE`
        
    * **Futures & Options**: `[ExchangeSegment]:[Symbol][Date][Strike][OptionType]`
        - **Date Format**: `DDMMYYYY` (e.g., `28042026` represents **28 April 2026**).
        - **Future Example**: `NSEFO:NIFTY28042026FUT` (NIFTY Future, expiring 28-Apr-2026).
        - **Option Example**: `NSEFO:NIFTY2804202625000CE` (NIFTY Option, expiring 28-Apr-2026, 25000 Strike Price, Call Option. Similarly for PUT options use 'PE').

---

## Examples

Here is how you can retrieve detailed Market Quotes programmatically.

=== "cURL"

    ```bash
    curl -X POST 'http://uat.quantxpress.com/md-api/marketfeed/quote' \
      -H 'Content-Type: application/json' \
      -H 'Authorization: Bearer {access_token}' \
      -H 'Accept: */*' \
      -d '{
        "InstrumentNames": ["NSEFO:NIFTY28042026FUT"]
      }'
    ```

=== "Python"

    ```python
    import requests

    url = "http://uat.quantxpress.com/md-api/marketfeed/quote"
    headers = {
        "Authorization": "Bearer YOUR_ACCESS_TOKEN",
        "Content-Type": "application/json"
    }
    payload = {
        "InstrumentNames": ["NSECM:RELIANCE", "NSEFO:NIFTY28042026FUT"]
    }

    response = requests.post(url, json=payload, headers=headers)
    
    if response.status_code == 200:
        data = response.json().get("data", {})
        for key, quote in data.items():
            print(f"--- Quote for {key} ---")
            print(f"LTP: {quote.get('ltp')}")
            print(f"Volume: {quote.get('volume')}")
            print(f"Total Buy Qty: {quote.get('totalBuyQty')}")
            print(f"Total Sell Qty: {quote.get('totalSellQty')}")
    else:
        print("Error fetching Market Quote")
    ```

=== "Node.js"

    ```javascript
    const axios = require('axios');

    const fetchQuote = async () => {
      try {
        const response = await axios.post('http://uat.quantxpress.com/md-api/marketfeed/quote', {
          InstrumentNames: ["NSECM:RELIANCE"]
        }, {
          headers: { 'Authorization': 'Bearer YOUR_ACCESS_TOKEN' }
        });
        
        const data = response.data.data;
        Object.keys(data).forEach(id => {
            const quote = data[id];
            console.log(`\n--- Quote for ${id} ---`);
            console.log(`LTP: ${quote.ltp}`);
            console.log(`Volume: ${quote.volume}`);
            console.log(`Best Bid: ${quote.bidPrice} (${quote.bidQty})`);
            console.log(`Best Ask: ${quote.askPrice} (${quote.askQty})`);
        });
      } catch (error) {
        console.error("Error:", error.message);
      }
    };
    fetchQuote();
    ```

---

## Successful Response (200 OK)

```json
{

  "data": {
    "1010010000002885": {
      "instrumentID": 1010010000002885,
      "exchangeSegment": 1,
      "exchangeInstrumentID": 2885,
      "instrumentName": "RELIANCE",
      "timestamp": 1748339153,
      "ltp": 1419.4,
      "ltq": 74,
      "ltt": 1748339153,
      "atp": 1422.41,
      "vtt": 1651139,
      "tbq": 0,
      "tsq": 0,
      "oi": 0,
      "open": 1426.1,
      "high": 1429.7,
      "low": 1418.1,
      "close": 1434.8,
      "bidLevel": null,
      "askLevel": null
    }
  },
  "status": "success"
}
```

---

## Response Field Descriptions

### Root Fields
|Field	|Type	|Description|
|-------|-------|-----------|
|status	|string	|Status of the request (success or error)|
|data	|object	|Contains quote data mapped by instrument ID|

---

### Quote Fields

| Field                | Type    | Description                          |
| -------------------- | ------- | ------------------------------------ |
| instrumentID         | integer | Unique instrument identifier         |
| instrumentName       | string  | Name of the instrument               |
| exchangeSegment      | integer | Exchange segment identifier          |
| exchangeInstrumentID | integer | Exchange-specific instrument ID      |
| timestamp            | integer | Last update timestamp (epoch)        |
| ltp                  | float  | Last traded price                    |
| ltq                  | float  | Last traded quantity                 |
| ltt                  | integer | Last traded time (epoch)             |
| atp                  | float  | Average traded price                 |
| vtt                  | float  | Volume traded today                  |
| tbq                  | float  | Total buy quantity                   |
| tsq                  | float  | Total sell quantity                  |
| oi                   | float  | Open interest                        |
| open                 | float  | Opening price                        |
| high                 | float  | Highest price                        |
| low                  | float  | Lowest price                         |
| close                | float  | Previous closing price               |

---

### Market Depth Fields

#### BidLevel (Buy Orders)

A bid level represents a specific price at which buyers are willing to purchase an asset, along with the quantity they want to buy at that price.

<!-- | Field  | Type   | Description                     |
| ------ | ------ | ------------------------------- |
| qty    | float | Quantity available              |
| price  | float | Bid price                       |
| orders | integer | Number of orders at that level  | -->

---

#### AskLevel (Sell Orders)

An ask level (also called an offer level) represents a specific price at which sellers are willing to sell an asset, along with the quantity they want to sell at that price.
<!-- | Field  | Type   | Description                     |
| ------ | ------ | ------------------------------- |
| qty    | float | Quantity available              |
| price  | float | Ask price                       |
| orders | integer | Number of orders at that level  | -->