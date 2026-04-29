# Download Instruments (GZ) API

The **Download Instruments API** allows you to retrieve the *entire master list* of tradable instruments across all exchanges in one single request. 

Because this list is massive (containing hundreds of thousands of instruments), the response is compressed into a `.gz` (GZIP) file to save bandwidth and reduce download time.

!!! tip "Best Practice: Fetch Once Daily"
    You should only call this endpoint **once per day** (ideally early in the morning before market open) and cache the results locally in your database or memory. Do not call this endpoint repeatedly during live trading, as downloading and decompressing large files is resource-intensive.

---

`BASE URL: http://uat.quantxpress.com/v1`

## Endpoint

| Method | URL                                         |
|--------|---------------------------------------------|
| GET    | `/api/instruments/gz/download`              |

## Headers

| Header        | Value            | Required |
| ------------  | ---------------- | -------- |
| Authorization | Bearer {access_token} | Yes      |

---

## How to Download and Extract

The downloaded file is a compressed GZIP file containing a massive JSON array. If you try to open the raw response directly as text, it will look like gibberish. You must decompress it first.

Here are a few ways to download and read the file programmatically:

=== "cURL (Command Line)"

    To download the file directly to your computer using your terminal:

    ```bash
    curl -X 'GET' \
      'http://uat.quantxpress.com/v1/api/instruments/gz/download' \
      -H 'Authorization: Bearer {access_token}' \
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
    headers = {
        "Authorization": "Bearer YOUR_ACCESS_TOKEN"
    }

    response = requests.get(url, headers=headers)

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
      headers: { 'Authorization': 'Bearer YOUR_ACCESS_TOKEN' },
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

Once decompressed, the file will contain a JSON array of objects. Below is a sample object representing a single **Options Instrument**.

```json
[
  {
    "InstrumentId": 2042700000155916,
    "InstrumentName": "ZYDUSLIFE26SEP241520PE",
    "ExchangeSegment": "NSEFO",
    "Exchange": "NSE",
    "ExpiryDate": "2024-09-26T00:00:00",
    "InstrumentType": "Options",
    "LotSize": 900,
    "OptionType": "PE",
    "PriceBandHigh": 346.95,
    "PriceBandLow": 454.75,
    "StrikePrice": 1520,
    "Symbol": "ZYDUSLIFE",
    "TickSize": 0.05
  }
]
```

### Key Fields Explained

| Field | Description |
|-------|-------------|
| `InstrumentId` | The unique numeric ID used for placing orders and fetching Live Market Data. |
| `InstrumentName` | The exact string name used as an alternative identifier (`ExchangeSegment:Symbol...`). |
| `ExchangeSegment` | Identifies the market (e.g., `NSECM` for Cash Equity, `NSEFO` for Futures & Options). |
| `InstrumentType` | Determines if the instrument is `Equity`, `Futures`, or `Options`. |
| `LotSize` | The minimum quantity you can buy/sell. Your order quantity must be a multiple of this number. |
| `TickSize` | The minimum price movement allowed for this instrument. Your order price must be a multiple of this value. |

---

## Common Questions (FAQ)

!!! question "Why am I getting weird gibberish characters when I print the response?"
    You are trying to read the raw binary response as a standard text string. Because the file is GZIP compressed, you **must** use a GZIP library (like `gzip` in Python or `zlib` in Node.js) to decompress the response bytes first before trying to parse the JSON.

!!! question "Do I need to download this every single day?"
    **Yes.** Instruments (especially options and futures contracts) expire, and new ones are added daily. Furthermore, trading parameters like `PriceBandHigh` and `PriceBandLow` change every single day based on the previous day's closing prices. It is highly recommended to run an automated script to fetch this file every morning before the market opens.
