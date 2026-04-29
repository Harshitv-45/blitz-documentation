# Download Instruments (GZ) API

The **Download Instruments API** allows you to retrieve the *entire master list* of tradable instruments across all exchanges in one single request. 

Because this list is massive (containing hundreds of thousands of instruments), the response is compressed into a `.gz` (GZIP) file.

!!! tip "Best Practice: Fetch Once Daily"
    You should only call this endpoint **once per day** (ideally early in the morning before market open).

---

`BASE URL: http://uat.quantxpress.com/v1`

## Endpoint

| Method | URL                                         |
|--------|---------------------------------------------|
| GET    | `/api/instruments/gz/download`              |


---

## How to Download and Extract

The downloaded file is a compressed GZIP file containing a massive JSON array. If you try to open the raw response directly as text, it will look like gibberish. You must decompress it first.

Here are a few ways to download and read the file programmatically:

=== "cURL (Command Line)"

    To download the file directly to your computer using your terminal:

    ```bash
    curl -X 'GET' \
      'http://uat.quantxpress.com/v1/api/instruments/gz/download' \
      --output instruments.json.gz
    ```
    *After downloading, you can extract it using terminal commands like `gunzip instruments.json.gz` or using GUI extraction tools like WinRAR or 7-Zip.*

=== "Python"

    This script downloads the file, unzips it in memory, and parses the JSON seamlessly without needing to save it to your hard drive.

    ```python
    import requests
    import gzip
    import io
    import json

    url = "http://uat.quantxpress.com/v1/api/instruments/gz/download"

    response = requests.get(url)

    if response.status_code == 200:
        # Decompress directly from bytes
        compressed_data = io.BytesIO(response.content)
        with gzip.open(compressed_data, 'rt') as f:
            data = json.load(f)
            
        print(f"Successfully loaded {len(data)} instruments!")
        print("First instrument:", data[0])
    else:
        print(f"Failed: {response.status_code} - {response.text}")
    ```

=== "Node.js"

    Node.js provides a built-in `zlib` module to handle gzip extraction easily. Ensure you set `responseType: 'arraybuffer'` so axios doesn't corrupt the binary data!

    ```javascript
    const axios = require('axios');
    const zlib = require('zlib');

    const url = 'http://uat.quantxpress.com/v1/api/instruments/gz/download';

    axios.get(url, {
      responseType: 'arraybuffer' // Crucial for downloading binary gzip data
    }).then(response => {
      // Decompress the binary data
      const unzipped = zlib.gunzipSync(response.data);
      const instruments = JSON.parse(unzipped.toString());
      
      console.log(`Successfully loaded ${instruments.length} instruments!`);
      console.log("First instrument:", instruments[0]);
    }).catch(error => {
      console.error('Error fetching instruments:', error.message);
    });
    ```

---

## Understanding the Response

Once decompressed, the file will contain a JSON array of objects. Below is a sample object representing  **Options Instrument**.

```json
[
  {
    "instrumentId": 110010000011536,
    "exchangeInstrumentId": 11536,
    "instrumentName": "TCS",
    "exchangeSegment": "NSECM",
    "exchange": "NSE",
    "expiryDate": "0001-01-01T00:00:00",
    "instrumentType": "Equity",
    "lotSize": 1,
    "optionType": "",
    "priceBandHigh": 2636.5,
    "priceBandLow": 2157.3,
    "strikePrice": 0,
    "symbol": "TCS",
    "tickSize": 0.1,
    "freezeQty": 999999,
    "ticker": "TATA CONSULTANCY SERV LT",
    "multiplier": 1,
    "open": 0,
    "high": 0,
    "low": 0,
    "close": 0,
    "ltp": 0,
    "isin": "INE467B01029"
  },
  {
    "instrumentId": 110010000002885,
    "exchangeInstrumentId": 2885,
    "instrumentName": "RELIANCE",
    "exchangeSegment": "NSECM",
    "exchange": "NSE",
    "expiryDate": "0001-01-01T00:00:00",
    "instrumentType": "Equity",
    "lotSize": 1,
    "optionType": "",
    "priceBandHigh": 1460.5,
    "priceBandLow": 1195.1,
    "strikePrice": 0,
    "symbol": "RELIANCE",
    "tickSize": 0.1,
    "freezeQty": 999999,
    "ticker": "RELIANCE INDUSTRIES LTD",
    "multiplier": 1,
    "open": 0,
    "high": 0,
    "low": 0,
    "close": 0,
    "ltp": 0,
    "isin": "INE002A01018"
  }
]
```

### Key Fields Explained

| Field | Description |
|-------|-------------|
| `instrumentId` | The unique numeric ID across the entire system. Use this for placing orders and fetching Live Market Data. |
| `exchangeInstrumentId` | The internal numeric ID assigned to the instrument by the exchange itself. |
| `instrumentName` | The exact string name used as an alternative identifier (`ExchangeSegment:Symbol` or similar format). |
| `exchangeSegment` | Identifies the market segment (e.g., `NSECM` for Cash Equity, `NSEFO` for Futures & Options). |
| `exchange` | The exchange code (e.g., `NSE`, `BSE`, `MCX`). |
| `expiryDate` | Expiration date of the contract. Returns `0001-01-01T00:00:00` for Equities without an expiry. |
| `instrumentType` | The asset class. Can be `Equity`, `Futures`, or `Options`. |
| `lotSize` | The minimum quantity you can buy/sell. Order quantities must be a multiple of this number. |
| `optionType` | The option type (`CE` for Call, `PE` for Put). Empty for Equities and Futures. |
| `priceBandHigh` | The upper circuit limit. Orders placed above this price will be rejected by the exchange. |
| `priceBandLow` | The lower circuit limit. Orders placed below this price will be rejected by the exchange. |
| `strikePrice` | The strike price for options contracts. Returns `0` for Equities and Futures. |
| `symbol` | The base symbol of the underlying asset (e.g., `TCS`, `RELIANCE`). |
| `tickSize` | The minimum price movement allowed for this instrument. Your order price must be a multiple of this value. |
| `freezeQty` | Maximum quantity allowed in a single order by the exchange. Orders exceeding this will be frozen/rejected. |
| `ticker` | Full company name or descriptive ticker name (e.g., `TATA CONSULTANCY SERV LT`). |
| `multiplier` | The value multiplier for calculating total contract value. |
| `open` | Opening price of the day (often `0` before the market opens or if illiquid). |
| `high` | Highest traded price of the day. |
| `low` | Lowest traded price of the day. |
| `close` | The previous day's closing price. |
| `ltp` | Last traded price from the master snapshot. |
| `isin` | International Securities Identification Number (ISIN) for the instrument. |

