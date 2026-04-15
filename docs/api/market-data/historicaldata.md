## **Historical Data**

The Historical Data API allows you to retrieve past market data for a specific instrument over a defined time interval.

Using this API, you can fetch OHLC (Open, High, Low, Close) data along with volume and timestamp information, which is essential for backtesting, technical analysis, and charting applications.

These endpoints let you analyze historical price movements and build data-driven trading strategies.
---

## Endpoint

| Method | URL                                             |
| ------ | ----------------------------------------------- |
| POST    | `http://uat.quantxpress.com/marketfeed/historicalData` |

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
  "symbol": "IRFC",
  "interval": "D"
}
```

---

## Parameters
|Parameter	|Type	|Required	|Description|
|-----------|-------|-----------|-----------|
|instrument	|string	|Yes	|Instrument symbol or ID (e.g., IRFC, `NSE)|
|interval	|string	|Yes	|Time interval of data. Supported values: `D`, `1`, `5`, `15`, `30`, `60`|


**Notes**
Returns time-series OHLC data.
Interval defines candle size:
    -`D` → Daily
    -`1`, `5`, `15`, `30`, `60` → Minutes

---

## Example

```bash
curl -X POST 'http://uat.quantxpress.com/marketfeed/historicalData' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {access_token}' \
  -H 'Accept": "*/*' \
  -d '{
    "instrument": "IRFC",
    "interval": "D"
  }'
```

---

## Successful Response (200 OK)

```json
[
  {
    "open": 115.1,
    "high": 116.7,
    "low": 115.1,
    "close": 115.6,
    "volume": 8940523,
    "oi": 0,
    "timestamp": "10-02-2026 09:15:00"
  }
]
```

---

## Response Field Descriptions

|Field	|Type	|Description|
|-------|-------|-----------|
|open|	number|	Opening price of the interval|
|high	|number	|Highest price during the interval|
|low	|number	|Lowest price during the interval|
|close	|number	|Closing price of the interval|
|volume|	number|	Total traded volume|
|oi	|number	|Open interest (applicable for derivatives)|
|timestamp	|string|	Date and time of the data point|

---


