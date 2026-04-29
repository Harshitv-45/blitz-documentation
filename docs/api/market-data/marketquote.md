# **Market Quote**

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

## Request Payload & Examples

You can request market quotes by providing either a list of **Instrument IDs** or a list of **Instrument Names**.

=== "By Instrument ID"

    ```bash
    curl -X POST 'http://uat.quantxpress.com/md-api/marketfeed/quote' \
      -H 'Content-Type: application/json' \
      -H 'Authorization: Bearer {access_token}' \
      -H 'Accept: */*' \
      -d '{
        "InstrumentIds": [1010010000002885]
      }'
    ```

    #### Response

    ```json
    {
      "status": "success",
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
      }
    }
    ```

=== "By Instrument Name"

    ```bash
    curl -X POST 'http://uat.quantxpress.com/md-api/marketfeed/quote' \
      -H 'Content-Type: application/json' \
      -H 'Authorization: Bearer {access_token}' \
      -H 'Accept: */*' \
      -d '{
        "InstrumentNames": ["NSECM:RELIANCE", "NSEFO:NIFTY28042026FUT"]
      }'
    ```

    #### Response

    ```json
    {
      "status": "success",
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
        },
        "101002000012345": {
          "instrumentID": 101002000012345,
          "exchangeSegment": 2,
          "exchangeInstrumentID": 12345,
          "instrumentName": "NSEFO:NIFTY28042026FUT",
          "timestamp": 1748339155,
          "ltp": 24500.0,
          "ltq": 50,
          "ltt": 1748339155,
          "atp": 24495.5,
          "vtt": 354890,
          "tbq": 0,
          "tsq": 0,
          "oi": 1500000,
          "open": 24400.0,
          "high": 24550.0,
          "low": 24350.0,
          "close": 24380.0,
          "bidLevel": null,
          "askLevel": null
        }
      }
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