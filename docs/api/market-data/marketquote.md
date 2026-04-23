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

```json
{
  "InstrumentIds": [1010010002000001]
}
```

---

## Parameters
| Parameter      | Type         | Required | Description |
|----------------|--------------|----------|--------------|
| InstrumentIds  | list / array | Yes      | List of instrument identifiers (symbols or numeric instrument IDs). |

---

## Example

```bash
curl -X POST 'http://uat.quantxpress.com/md-api/marketfeed/quote' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {access_token}' \
  -H 'Accept: */*' \
  -d '{
    "InstrumentIds": [1010010002000001]
  }'
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