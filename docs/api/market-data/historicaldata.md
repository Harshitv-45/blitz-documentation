# Historical Data API

The **Historical Data API** allows you to retrieve past market data for a specific instrument over a defined time interval.

Using this API, you can fetch OHLC (Open, High, Low, Close) data along with volume and timestamp information, which is essential for backtesting, technical analysis, and building charting applications.

!!! tip "Common Use Case"
    Use this endpoint to generate historical candlestick charts or run quantitative backtests before executing live trading strategies.

---

`BASE URL: http://uat.quantxpress.com/md-api`

## Endpoint

| Method | URL                                             |
| ------ | ----------------------------------------------- |
| POST   | `/marketfeed/historicalData`                    |

## Headers

| Header        | Value                 | Required |
| ------------- | --------------------- | -------- |
| Content-Type  | application/json      | Yes      |
| Authorization | Bearer {access_token} | Yes      |

---

## Request Payload

You must provide the instrument identifier and the desired time interval for the candles.

```json
{
  "instrument": "NSECM:IRFC",
  "interval": "D"
}
```

### Parameters

| Parameter  | Type   | Required | Description |
|------------|--------|----------|--------------|
| `instrument` | string | Yes      | The exact instrument name or numeric ID (e.g., `NSECM:IRFC`, `NSEFO:NIFTY28042026FUT`, or `1010010002000001`). |
| `interval`   | string | Yes      | Time interval of the candle data. |

### Supported Intervals
| Interval Code | Represents |
|---------------|------------|
| `1` | 1 Minute |
| `5` | 5 Minutes |
| `15` | 15 Minutes |
| `30` | 30 Minutes |
| `60` | 60 Minutes (1 Hour) |
| `D` | Daily (1 Day) |

!!! info "Instrument Naming Convention"
    When fetching by Instrument Name, the string follows a specific pattern:
    * **Cash Market (Equity)**: `[ExchangeSegment]:[Symbol]` (e.g., `NSECM:RELIANCE`)
    * **Futures & Options**: `[ExchangeSegment]:[Symbol][Date][Strike][OptionType]` (e.g., `NSEFO:NIFTY2804202625000CE`)

---

## Examples

=== "cURL"

    ```bash
    curl -X POST 'http://uat.quantxpress.com/md-api/marketfeed/historicalData' \
      -H 'Content-Type: application/json' \
      -H 'Authorization: Bearer {access_token}' \
      -H 'Accept: */*' \
      -d '{
        "instrument": "NSECM:IRFC",
        "interval": "D"
      }'
    ```

=== "Python"

    ```python
    import requests

    url = "http://uat.quantxpress.com/md-api/marketfeed/historicalData"
    headers = {
        "Authorization": "Bearer YOUR_ACCESS_TOKEN",
        "Content-Type": "application/json"
    }
    payload = {
        "instrument": "NSECM:IRFC",
        "interval": "D"
    }

    response = requests.post(url, json=payload, headers=headers)
    print(response.json())
    ```

=== "Node.js"

    ```javascript
    const axios = require('axios');

    const fetchHistory = async () => {
      try {
        const response = await axios.post('http://uat.quantxpress.com/md-api/marketfeed/historicalData', {
          instrument: "NSECM:IRFC",
          interval: "D"
        }, {
          headers: { 'Authorization': 'Bearer YOUR_ACCESS_TOKEN' }
        });
        console.log(response.data);
      } catch (error) {
        console.error("Error:", error.message);
      }
    };
    fetchHistory();
    ```

---

## Understanding the Response

A successful request returns an array of OHLC candle objects sorted chronologically.

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

### Response Field Descriptions

| Field | Type | Description |
|-------|------|-------------|
| `open` | number | Opening price of the interval |
| `high` | number | Highest price traded during the interval |
| `low` | number | Lowest price traded during the interval |
| `close` | number | Closing price of the interval |
| `volume` | number | Total number of shares or contracts traded during this period |
| `oi` | number | Open interest (Only applicable and populated for derivative instruments) |
| `timestamp` | string | Date and start time of the candle (`DD-MM-YYYY HH:mm:ss`) |


