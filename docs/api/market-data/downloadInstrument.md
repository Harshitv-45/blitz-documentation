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

Once decompressed, the file will contain a JSON array of objects. Below are sample objects representing different types of instruments.

=== "Equity"

    ```json
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
    ```

    #### Equity Fields Explained

    | Field | Description |
    |-------|-------------|
    | `instrumentId` | Numeric ID across the entire system. Use this for placing orders and fetching Live Market Data. |
    | `exchangeInstrumentId` | Numeric ID assigned to the instrument by the exchange itself. |
    | `instrumentName` | String name used as an alternative identifier. |
    | `exchangeSegment` | Identifies the market segment (Possibilities: `NSECM`, `BSECM`). |
    | `exchange` | The exchange code (Possibilities: `NSE`, `BSE`). |
    | `expiryDate` | Returns `0001-01-01T00:00:00` since equities do not have an expiration date. |
    | `instrumentType` | The asset class (`Equity`). |
    | `lotSize` | Always `1` for cash equities. |
    | `optionType` | Empty (`""`) since equities are not options. |
    | `priceBandHigh` / `Low` | The upper/lower circuit limits. Orders placed outside this will be rejected. |
    | `strikePrice` | Returns `0` for equities. |
    | `symbol` | The base symbol (e.g., `RELIANCE`). |
    | `tickSize` | The minimum price movement allowed for this instrument. |
    | `freezeQty` | Maximum quantity allowed in a single order by the exchange. |
    | `ticker` | Full company name (e.g., `RELIANCE INDUSTRIES LTD`). |
    | `isin` | International Securities Identification Number (ISIN) for the instrument. |

=== "Future"

    ```json
    {
      "instrumentId": 263320000036687,
      "exchangeInstrumentId": 36687,
      "instrumentName": "011NSETEST27NOV36FUT",
      "exchangeSegment": "NSEFO",
      "exchange": "NSE",
      "expiryDate": "2036-11-27T00:00:00+00:00",
      "instrumentType": "Futures",
      "lotSize": 50,
      "optionType": "XX",
      "priceBandHigh": 220,
      "priceBandLow": 180,
      "strikePrice": 0,
      "symbol": "011NSETEST",
      "tickSize": 0.05,
      "freezeQty": 100000,
      "ticker": "011NSETEST 27NOV36 FUT",
      "multiplier": 1,
      "open": 0,
      "high": 0,
      "low": 0,
      "close": 0,
      "ltp": 0,
      "isin": ""
    }
    ```

    #### Future Fields Explained

    | Field | Description |
    |-------|-------------|
    | `instrumentId` | Numeric ID across the entire system. Use this for placing orders and fetching Live Market Data. |
    | `exchangeInstrumentId` | Numeric ID assigned to the instrument by the exchange itself. |
    | `instrumentName` | String name representing the contract (e.g., `011NSETEST27NOV36FUT`). |
    | `exchangeSegment` | Identifies the market segment (Possibilities: `NSEFO`, `BSEFO`, `MCXFO`, `BFO`, `NCDEX`). |
    | `exchange` | The exchange code (Possibilities: `NSE`, `BSE`, `MCX`, `NCDEX`). |
    | `expiryDate` | The specific expiration date of the futures contract. |
    | `instrumentType` | The asset class (`Futures`). |
    | `lotSize` | The contract lot size. Order quantities must be a multiple of this number. |
    | `optionType` | Empty (`""`) since this is a futures contract. |
    | `priceBandHigh` / `Low` | The upper/lower circuit limits for the contract. |
    | `strikePrice` | Returns `0` since futures don't have strike prices. |
    | `symbol` | The base symbol of the underlying asset (e.g., `011NSETEST`). |
    | `tickSize` | The minimum price movement allowed for this instrument. |
    | `freezeQty` | Maximum quantity allowed in a single order by the exchange. |
    | `ticker` | Display name of the contract (e.g., `011NSETEST 27NOV36 FUT`). |
    | `isin` | Usually empty (`""`) for derivatives. |

=== "Option"

    ```json
    {
      "instrumentId": 261180000035082,
      "exchangeInstrumentId": 35082,
      "instrumentName": "360ONE28APR26440CE",
      "exchangeSegment": "NSEFO",
      "exchange": "NSE",
      "expiryDate": "2026-04-28T00:00:00+00:00",
      "instrumentType": "Options",
      "lotSize": 500,
      "optionType": "CE",
      "priceBandHigh": 668.7,
      "priceBandLow": 531.1,
      "strikePrice": 440,
      "symbol": "360ONE",
      "tickSize": 0.05,
      "freezeQty": 20000,
      "ticker": "360ONE 28APR26 440 CE",
      "multiplier": 1,
      "open": 0,
      "high": 0,
      "low": 0,
      "close": 0,
      "ltp": 0,
      "isin": ""
    }
    ```

    #### Option Fields Explained

    | Field | Description |
    |-------|-------------|
    | `instrumentId` | Numeric ID across the entire system. Use this for placing orders and fetching Live Market Data. |
    | `exchangeInstrumentId` | Numeric ID assigned to the instrument by the exchange itself. |
    | `instrumentName` | String name representing the contract (e.g., `360ONE28APR26440CE`). |
    | `exchangeSegment` | Identifies the market segment (Possibilities: `NSEFO`, `BSEFO`, `MCXFO`, `BFO`, `NCDEX`). |
    | `exchange` | The exchange code (Possibilities: `NSE`, `BSE`, `MCX`, `NCDEX`). |
    | `expiryDate` | The expiration date of the options contract. |
    | `instrumentType` | The asset class (`Options`). |
    | `lotSize` | The contract lot size. Order quantities must be a multiple of this number. |
    | `optionType` | The option type (Possibilities: `CE` for Call, `PE` for Put). |
    | `priceBandHigh` / `Low` | The upper/lower price limits. Orders placed outside this will be rejected. |
    | `strikePrice` | The strike price of the options contract (e.g., `440.0`). |
    | `symbol` | The base symbol of the underlying asset (e.g., `360ONE`). |
    | `tickSize` | The minimum price movement allowed for this instrument. |
    | `freezeQty` | Maximum quantity allowed in a single order by the exchange. |
    | `ticker` | Display name of the contract (e.g., `360ONE 28APR26 440 CE`). |
    | `isin` | Usually empty (`""`) for derivatives. |

=== "Index"

    ```json
    {
      "instrumentId": 1110010000300047,
      "exchangeInstrumentId": 300047,
      "instrumentName": "BANKEX",
      "exchangeSegment": "BSECM",
      "exchange": "BSE",
      "expiryDate": "0001-01-01T00:00:00",
      "instrumentType": "INDEX",
      "lotSize": 1,
      "optionType": "",
      "priceBandHigh": 0,
      "priceBandLow": 0,
      "strikePrice": 0,
      "symbol": "BANKEX",
      "tickSize": 0,
      "freezeQty": 1,
      "ticker": "BANKEX",
      "multiplier": 1,
      "open": 0,
      "high": 0,
      "low": 0,
      "close": 0,
      "ltp": 0,
      "isin": ""
    }
    ```

    #### Index Fields Explained

    | Field | Description |
    |-------|-------------|
    | `instrumentId` | Numeric ID across the entire system. Use this for placing orders and fetching Live Market Data. |
    | `exchangeInstrumentId` | Numeric ID assigned to the instrument by the exchange itself. |
    | `instrumentName` | String name representing the index (e.g., `BANKEX`). |
    | `exchangeSegment` | Identifies the market segment (Possibilities: `NSECM`, `BSECM`). |
    | `exchange` | The exchange code (Possibilities: `NSE`, `BSE`). |
    | `expiryDate` | Returns `0001-01-01T00:00:00` since base indices do not have an expiration date. |
    | `instrumentType` | The asset class (`INDEX`). |
    | `lotSize` | The lot size (typically `1` for the base index). |
    | `optionType` | Empty (`""`) since this is an index. |
    | `priceBandHigh` / `Low` | The upper/lower price limits (typically `0` for an index). |
    | `strikePrice` | Returns `0` since an index does not have a strike price. |
    | `symbol` | The base symbol of the underlying index (e.g., `BANKEX`). |
    | `tickSize` | The minimum price movement allowed for this instrument. |
    | `freezeQty` | Maximum quantity allowed in a single order by the exchange. |
    | `ticker` | Display name of the index (e.g., `BANKEX`). |
    | `isin` | Usually empty (`""`) for indices. |

