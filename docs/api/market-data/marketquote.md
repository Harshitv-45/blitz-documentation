## **Market Quote**

The Market Quote API allows you to retrieve detailed real-time market data for one or more instruments.

Using this API, you can fetch comprehensive quote information including last traded price (LTP), last traded quantity (LTQ), volume, OHLC data, open interest, and market depth (bid/ask levels).

This API is essential for building trading dashboards, order books, and real-time analytics systems.

---

## Endpoint

| Method | URL                                             |
| ------ | ----------------------------------------------- |
| POST    | `http://uat.quantxpress.com/marketfeed/quote` |

---

## Headers

| Header        | Value            | Required |
| ------------  | ---------------- | -------- |
| Content-Type  | application/json | Yes      |
| Authorization | {acess_token}    | Yes      |

---

## Request Payload

```json
{
  "InstrumentIds": ["NSECM|RELIANCE", "NSECM|TCS", 1010010002000001]
}
```

---

## Parameters
|Parameter	|Type	|Required	|Description|
|-----------|-------|-----------|-----------|
InstrumentIds|list / array|Yes|List of instrument identifiers (symbols or numeric instrument IDs).

---

## Example

```bash
curl -X POST 'http://uat.quantxpress.com/marketfeed/quote' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {access_token}' \
  -d '{
    "InstrumentIds": ["NSECM|RELIANCE", "NSECM|TCS", 1010010002000001]
  }'
```

---

## Successful Response (200 OK)

```json
{
  "status": "success",
  "data": {
    "1010010000002885": {
      "InstrumentID": 1010010000002885,
      "ExchangeSegment": 1,
      "ExchangeInstrumentID": 2885,
      "InstrumentName": "RELIANCE",
      "Timestamp": 1748339153,
      "LTP": 1419.4,
      "LTQ": 74,
      "LTT": 1748339153,
      "ATP": 1422.41,
      "VTT": 1651139,
      "TBQ": 0,
      "TSQ": 0,
      "OI": 0,
      "Open": 1426.1,
      "High": 1429.7,
      "Low": 1418.1,
      "Close": 1434.8,
      "BidLevel": [
        { "Qty": 1, "Price": 1419.3, "Orders": 1 },
        { "Qty": 7, "Price": 1419.2, "Orders": 7 }
      ],
      "AskLevel": [
        { "Qty": 1, "Price": 1419.4, "Orders": 1 },
        { "Qty": 4, "Price": 1419.5, "Orders": 4 }
      ]
    }
  }
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
| InstrumentID         | integer | Unique instrument identifier         |
| InstrumentName       | string  | Name of the instrument               |
| ExchangeSegment      | integer | Exchange segment identifier          |
| ExchangeInstrumentID | integer | Exchange-specific instrument ID      |
| Timestamp            | integer | Last update timestamp (epoch)        |
| LTP                  | number  | Last traded price                    |
| LTQ                  | number  | Last traded quantity                 |
| LTT                  | integer | Last traded time (epoch)             |
| ATP                  | number  | Average traded price                 |
| VTT                  | number  | Volume traded today                  |
| TBQ                  | number  | Total buy quantity                   |
| TSQ                  | number  | Total sell quantity                  |
| OI                   | number  | Open interest                        |
| Open                 | number  | Opening price                        |
| High                 | number  | Highest price                        |
| Low                  | number  | Lowest price                         |
| Close                | number  | Previous closing price               |

---

### Market Depth Fields

#### BidLevel (Buy Orders)

| Field  | Type   | Description                     |
| ------ | ------ | ------------------------------- |
| Qty    | number | Quantity available              |
| Price  | number | Bid price                       |
| Orders | number | Number of orders at that level  |

---

#### AskLevel (Sell Orders)

| Field  | Type   | Description                     |
| ------ | ------ | ------------------------------- |
| Qty    | number | Quantity available              |
| Price  | number | Ask price                       |
| Orders | number | Number of orders at that level  |