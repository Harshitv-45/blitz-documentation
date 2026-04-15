# **Option Chain**

## **Overview**

The **Option Chain API** provides the latest option chain data for a given symbol and expiry date. It returns detailed information about call and put options across multiple strike prices, including Greeks, implied volatility, and pricing data.

This API helps traders analyze market sentiment, volatility, and strike-level activity.

---

## Endpoint

| Method | URL                                             |
| ------ | ----------------------------------------------- |
| GET    | `http://uat.quantxpress.com/v1/api/optionChain` |

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
  "symbol": "NIFTY",
  "expiryDate": "2025-05-29"
}
```

## Parameters
|Parameter|	Type	|Required|	Description|
|---------|--------|-------|-------------|
|symbol	|string	|Yes	|Underlying asset symbol (e.g., NIFTY, BANKNIFTY, or equity symbol).|
|expiryDate	|string	|Yes	|Expiry date in YYYY-MM-DD format.|

## Example

```bash
curl -X POST 'http://uat.quantxpress.com/marketfeed/optionChain' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {access_token}' \
  -d '{
    "symbol": "NIFTY",
    "expiryDate": "2025-05-29"
  }'
```

---

## Successful Response (200 OK)

```json
{
  "status": "success",
  "data": {
    "spotPrice": 24813.5,
    "expiryDate": "2025-05-29",
    "atm": 24800,
    "chains": [
      {
        "strikePrice": 23800,
        "callOption": {
          "gamma": 0,
          "vega": 0,
          "theta": 0,
          "delta": 1,
          "oiPercentage": 0,
          "oi": 0,
          "ltp": 1042.8,
          "iv": 0.2,
          "price": 1013.5,
          "rho": 0
        },
        "putOption": {
          "gamma": 0,
          "vega": 0,
          "theta": 0,
          "delta": 0,
          "oiPercentage": 0,
          "oi": 0,
          "ltp": 8.85,
          "iv": 0.2,
          "price": 0,
          "rho": 0
        }
      }
    ]
  }
}
```
