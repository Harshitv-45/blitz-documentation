# Download Instruments (GZ) API

## Overview

The **Download Instruments (GZ) API** allows users to download a compressed `.gz` file containing instrument data in JSON format. This can be used to retrieve a bulk set of instruments for offline processing or further analysis.

---

`BASE URL: http://uat.quantxpress.com/md-api`


## Endpoint

| Method | URL                                         |
|--------|---------------------------------------------|
| GET    | `/api/instruments/gz/download`              |

---
### Extracting the `.gz` File

The downloaded file `instruments.json.gz` is compressed using GZIP format. You will need to extract it before consuming the data.

You can use any tool or language that supports GZIP decompression. Below is an example using **Python**.

>  Note: Python is just one of many ways to extract the file. You can use other methods such as shell commands (`gunzip`), programming languages (Node.js, Java, C#), or GUI tools like 7-Zip and WinRAR.

---

####  Example: Extract Using Python

```python
import requests
import gzip
import io

base_url = "{{baseUrl}}"
endpoint = "/api/instruments/gz/download"
url = f"{base_url}{endpoint}"

headers = {}

response = requests.get(url, headers=headers)

if response.ok:
    # Decompress directly from bytes using io.BytesIO and gzip
    compressed_data = io.BytesIO(response.content)
    with gzip.open(compressed_data, 'rt') as f:
        content = f.read()
        print("Uncompressed content:\n")
        print(content)
else:
    print(f"Failed to download file: {response.status_code}")
    print(response.text)


```

Responses :
Here is a sample response for an **Options Instrument** from the Instrument Master API:

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


