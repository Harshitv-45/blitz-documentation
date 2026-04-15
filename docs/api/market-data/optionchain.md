# **Option Chain**

## **Overview**

The **Option Chain API** provides the latest option chain data for a given symbol and expiry date. It returns detailed information about call and put options across multiple strike prices, including Greeks, implied volatility, and pricing data.

This API helps traders analyze market sentiment, volatility, and strike-level activity.

---

## Endpoint

| Method | URL                                             |
| ------ | ----------------------------------------------- |
| POST    | `http://uat.quantxpress.com/v1/api/optionChain` |

---

## Headers

| Header        | Value            | Required |
| ------------  | ---------------- | -------- |
| Content-Type  | application/json | Yes      |
| Authorization | {acess_token}    | Yes      |
| Accept        | `*/*`              | Yes      |

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
  -H 'Accept": "*/*' \
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

---

## Response Field Descriptions

### Root Fields
|Field	|Type	|Description|
|-------|-------|-----------|
|status	|string	|Status of the request (success or error)|
|data	|object	|Contains quote data mapped by instrument ID|

### Data Fields
| Field      | Type   | Description                                               |
| ---------- | ------ | --------------------------------------------------------- |
| spotPrice  | float | Current market price of the underlying asset              |
| expiryDate | string | Expiry date of the options contract (`YYYY-MM-DD`)        |
| atm        | float | At-the-money strike price (closest to spot price)         |
| chains     | array  | List of option data for different strike prices           |

### Chain Fields
| Field        | Type   | Description                                   |
| ------------ | ------ | --------------------------------------------- |
| strikePrice  | float | Strike price of the option contract           |
| callOption   | object | Call option (CE) data for the strike          |
| putOption    | object | Put option (PE) data for the strike           |

### Call / Put Option Fields (CE & PE)
| Field         | Type   | Description                                      |
| ------------- | ------ | ------------------------------------------------ |
| ltp           | float | Last traded price of the option                  |
| price         | float | Last traded or closing price                     |
| iv            | float | Implied volatility of the option                 |
| delta         | float | Sensitivity of option price to underlying price  |
| gamma         | float | Rate of change of delta                          |
| theta         | float | Time decay of the option                         |
| vega          | float | Sensitivity to volatility changes                |
| rho           | float | Sensitivity to interest rate changes            |
| oi            | float | Open interest (total outstanding contracts)      |
| oiPercentage  | float | Change in open interest (percentage)             |

## **Note:**  
 - `callOption` represents **CE (Call Option)**  
 - `putOption` represents **PE (Put Option)**  
 - Greeks (`delta`, `gamma`, `theta`, `vega`, `rho`) are key indicators for options trading analysis.