# Quote API

## Overview

The **Quote API** provides detailed market depth and quote-related information for a list of instrument IDs. This includes Last Traded Price (LTP), Last Traded Quantity (LTQ), Open/High/Low/Close prices, Bid/Ask levels, and other relevant data points used in real-time trading.

---

## Endpoint

| Method | URL                     |
| ------ | ----------------------- |
| POST   | `/api/marketfeed/quote` |

---

## Headers

| Header       | Value            | Required |
| ------------ | ---------------- | -------- |
| Content-Type | application/json | Yes      |
| Accept       | `*/*`            | Yes      |

---

## Request Body

| Field         | Type  | Description                            | Required |
| ------------- | ----- | -------------------------------------- | -------- |
| InstrumentIds | array | List of instrument IDs (numeric array) | Yes      |

### Example

```json
{
  "InstrumentIds": [1010010000000025]
}
```

### Successful Response (200 OK)

```json
{
  "data": {
    "1010010000000025": {
      "InstrumentID": 1010010000000025,
      "ExchangeSegment": 1,
      "ExchangeInstrumentID": 25,
      "InstrumentName": "ADANIENT",
      "Timestamp": 1720603052,
      "LTP": 3118.75,
      "LTQ": 15,
      "LTT": 1720603052,
      "ATP": 3120.54,
      "VTT": 22851,
      "TBQ": 67386,
      "TSQ": 96501,
      "OI": 0,
      "Open": 3120.1,
      "High": 3125,
      "Low": 3115,
      "Close": 3110.75,
      "BidLevel": [
        {
          "Qty": 2,
          "Price": 3118.75,
          "Orders": 2
        },
        {
          "Qty": 1,
          "Price": 3118.7000000000003,
          "Orders": 1
        },
        {
          "Qty": 2,
          "Price": 3118.6,
          "Orders": 2
        },
        {
          "Qty": 1,
          "Price": 3118.55,
          "Orders": 1
        },
        {
          "Qty": 2,
          "Price": 3118.4,
          "Orders": 2
        }
      ],
      "AskLevel": [
        {
          "Qty": 2,
          "Price": 3120.3,
          "Orders": 2
        },
        {
          "Qty": 1,
          "Price": 3120.35,
          "Orders": 1
        },
        {
          "Qty": 1,
          "Price": 3120.5,
          "Orders": 1
        },
        {
          "Qty": 1,
          "Price": 3120.65,
          "Orders": 1
        },
        {
          "Qty": 2,
          "Price": 3120.75,
          "Orders": 2
        }
      ]
    }
  }
}
```

### Field Descriptions

| Field                | Type   | Description                             |
| -------------------- | ------ | --------------------------------------- |
| InstrumentID         | number | Unique identifier of the instrument     |
| InstrumentName       | string | Name of the instrument                  |
| ExchangeSegment      | number | Segment code (e.g., 1 for NSECM)        |
| ExchangeInstrumentID | number | Exchange-specific instrument ID         |
| Timestamp            | number | UNIX timestamp of the latest quote      |
| LTP                  | number | Last traded price                       |
| LTQ                  | number | Last traded quantity                    |
| LTT                  | number | Last traded time (UNIX timestamp)       |
| ATP                  | number | Average traded price                    |
| VTT                  | number | Volume traded today                     |
| TBQ                  | number | Total buy quantity                      |
| TSQ                  | number | Total sell quantity                     |
| OI                   | number | Open interest                           |
| Open                 | number | Opening price                           |
| High                 | number | Highest price of the day                |
| Low                  | number | Lowest price of the day                 |
| Close                | number | Previous day closing price              |
| BidLevel             | array  | List of bid levels (price, qty, orders) |
| AskLevel             | array  | List of ask levels (price, qty, orders) |

### Error Response (400 / 500)

```json
{
  "status": "error",
  "message": "Invalid Instrument IDs.",
  "error": "InstrumentIds array must contain valid entries."
}
```
