# Option Chain

## Overview

The **Option Chain API** provides the latest option chain data for a given symbol and expiry date. It returns detailed information about call and put options across multiple strike prices, including Greeks, implied volatility, and pricing data.

This API helps traders analyze market sentiment, volatility, and strike-level activity.

---

## Endpoint

| Method | URL                                          |
| ------ | -------------------------------------------- |
| POST   | `http://uat.quantxpress.com/api/optionChain` |

---

## Headers

| Header       | Value            | Required |
| ------------ | ---------------- | -------- |
| Content-Type | application/json | Yes      |
| Accept       | `*/*`            | Yes      |

---

## Request Body

| Field      | Type   | Description                                     | Required |
| ---------- | ------ | ----------------------------------------------- | -------- |
| symbol     | string | Trading symbol (e.g., `"NIFTY"`, `"BANKNIFTY"`) | Yes      |
| expiryDate | string | Expiry date in `YYYY-MM-DD` format              | Yes      |

### Example Request

```json
{
  "symbol": "NIFTY",
  "expiryDate": "2025-04-09"
}
```

---

## Example

```bash
curl -X POST 'http://uat.quantxpress.com/api/optionChain' \
  -H 'Content-Type: application/json' \
  -H 'Accept: */*' \
  -d '{
    "symbol": "NIFTY",
    "expiryDate": "2025-04-09"
  }'
```

---

## Successful Response (200 OK)

### Response Structure

| Field           | Type   | Description                            |
| --------------- | ------ | -------------------------------------- |
| status          | string | API request status                     |
| data            | object | Option chain data                      |
| data.spotPrice  | number | Current underlying price               |
| data.expiryDate | string | Expiry date of options                 |
| data.atm        | number | At-the-money strike price              |
| data.chains     | array  | Option data for multiple strike prices |

---

### Option Object Fields (Call / Put)

| Field        | Type   | Description                     |
| ------------ | ------ | ------------------------------- |
| gamma        | number | Rate of change of delta         |
| vega         | number | Sensitivity to volatility       |
| theta        | number | Time decay value                |
| delta        | number | Price sensitivity to underlying |
| oiPercentage | number | Open interest percentage        |
| oi           | number | Open interest                   |
| ltp          | number | Last traded price               |
| iv           | number | Implied volatility              |
| price        | number | Option price                    |
| rho          | number | Interest rate sensitivity       |

---

### Example Response

```json
{
  "status": "success",
  "data": {
    "spotPrice": 21991.4,
    "expiryDate": "2025-04-09",
    "atm": 22000,
    "chains": [
      {
        "strikePrice": 21000,
        "callOption": {
          "gamma": 0,
          "vega": 0,
          "theta": -3.42,
          "delta": 1,
          "oiPercentage": 0,
          "oi": 0,
          "ltp": 1085,
          "iv": 0,
          "price": 996.75,
          "rho": 1.15
        },
        "putOption": {
          "gamma": 0,
          "vega": 0,
          "theta": 0,
          "delta": 0,
          "oiPercentage": 0,
          "oi": 0,
          "ltp": 69.1,
          "iv": 0,
          "price": 0,
          "rho": 0
        }
      }
    ]
  }
}
```

---

## Error Response (400 / 500)

```json
{
  "status": "error",
  "message": "Invalid input or server error.",
  "error": "Expiry date is required."
}
```

---
