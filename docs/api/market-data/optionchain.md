# Option Chain

## Overview

The **Option Chain API** provides the latest option chain data for a given symbol and expiry date. It returns detailed information about call and put options across multiple strike prices, including Greeks, implied volatility, and pricing data.

This API helps traders analyze market sentiment, volatility, and strike-level activity.

---

## Endpoint

| Method | URL                                             |
| ------ | ----------------------------------------------- |
| GET    | `http://uat.quantxpress.com/v1/api/optionChain` |

---

## Headers

| Header       | Value            | Required |
| ------------ | ---------------- | -------- |
| Content-Type | application/json | Yes      |
| Accept       | `*/*`            | Yes      |

---

## Example

```bash
curl -X GET 'http://uat.quantxpress.com/v1/api/optionChain' \
  -H 'Content-Type: application/json' \
  -H 'Accept: */*'
```

---

## Successful Response (200 OK)

```json
{
  "status": "success",
  "message": "request processed successfully",
  "data": [
    {
      "symbol": "NIFTY",
      "instrumentId": 1010010002000001
    },
    {
      "symbol": "BANKNIFTY",
      "instrumentId": 1010010002000002
    },
    {
      "symbol": "FINNIFTY",
      "instrumentId": 1010010002000030
    },
    {
      "symbol": "MIDCPNIFTY",
      "instrumentId": 1010010002000008
    }
  ]
}
```
