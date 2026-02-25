# Instrument API

The **Instrument API** allows you to retrieve detailed information about a trading instrument using its instrument ID.

Using this API, you can fetch instrument metadata such as symbol details, exchange information, price bands, tick size, lot size, and other trading parameters.

These endpoints let you **retrieve instrument details and metadata** for trading and market analysis.

| **Type** | **Endpoint**                            | **Description**                   |
| -------- | --------------------------------------- | --------------------------------- |
| GET      | [`instruments/{id}`](#fetch-instrument) | Retrieve instrument details by ID |

---

## Instrument Details {#fetch-instrument}

Retrieve detailed information for a specific instrument using its instrument ID.

### Endpoint

```
GET /api/instruments/{id}
```

---

### cURL Example

```bash
curl -X 'GET' \
  'http://uat.quantxpress.com/api/instruments/{instrumentId}' \
  -H 'accept: */*'
```

---

## Response

### Success Response

```json
{
  "status": "success",
  "message": "request processed successfully",
  "data": {
    "instrumentId": 110010000014366,
    "exchange": "NSE",
    "symbol": "IDEA",
    "ticker": "VODAFONE IDEA LIMITED",
    "exchangeSegment": "NSECM",
    "instrumentType": "Equity",
    "instrumentName": "IDEA",
    "exchangeInstrumentId": 14366,
    "marketInstrumentId": 110010000014366,
    "series": "EQ",
    "tickSize": 0.01,
    "isin": "INE669E01016",
    "priceBandHigh": 12.23,
    "priceBandLow": 10.01,
    "multiplier": 1,
    "bvp": 1,
    "priceType": "1.0",
    "priceNumerator": 1,
    "priceDenominator": 1,
    "freezeQty": 999999,
    "lotSize": 1,
    "expiryDate": null,
    "strikePrice": null,
    "optionType": null,
    "assetToken": null,
    "underlyingInstrumentId": null,
    "open": 0,
    "high": 0,
    "low": 0,
    "close": 0,
    "ltp": 0,
    "reserve1": "",
    "reserve2": "",
    "reserve3": "",
    "reserve4": "",
    "reserve5": "",
    "reserve6": "",
    "reserve7": "",
    "reserve8": ""
  }
}
```

---

## Response Field Descriptions

### Root Response Fields

| Field   | Type   | Description                                  |
| ------- | ------ | -------------------------------------------- |
| status  | string | Status of the request (`success` or `error`) |
| message | string | Response message                             |
| data    | object | Contains instrument details                  |

---

### Instrument Fields

| Field                  | Type         | Description                                          |
| ---------------------- | ------------ | ---------------------------------------------------- |
| instrumentId           | integer      | Unique internal identifier of the instrument         |
| exchange               | string       | Exchange where instrument is traded (e.g., NSE, BSE) |
| symbol                 | string       | Trading symbol of the instrument                     |
| ticker                 | string       | Full company or instrument name                      |
| exchangeSegment        | string       | Exchange segment (e.g., NSECM, NSEFO)                |
| instrumentType         | string       | Type of instrument (Equity, Futures, Options, etc.)  |
| instrumentName         | string       | Name of the instrument                               |
| exchangeInstrumentId   | integer      | Instrument identifier assigned by exchange           |
| marketInstrumentId     | integer      | Market-wide instrument identifier                    |
| series                 | string       | Instrument series (e.g., EQ for equity)              |
| tickSize               | number       | Minimum price movement allowed                       |
| isin                   | string       | International Securities Identification Number       |
| priceBandHigh          | number       | Upper price band limit                               |
| priceBandLow           | number       | Lower price band limit                               |
| multiplier             | number       | Contract multiplier                                  |
| bvp                    | number       | Base value point                                     |
| priceType              | string       | Price type configuration                             |
| priceNumerator         | number       | Price calculation numerator                          |
| priceDenominator       | number       | Price calculation denominator                        |
| freezeQty              | number       | Maximum quantity allowed per order                   |
| lotSize                | number       | Minimum tradable quantity                            |
| expiryDate             | integer/null | Expiry date (for derivatives)                        |
| strikePrice            | number/null  | Strike price (for options)                           |
| optionType             | string/null  | Option type (CE/PE)                                  |
| assetToken             | string/null  | Asset token reference                                |
| underlyingInstrumentId | integer/null | Underlying instrument ID (for derivatives)           |
| open                   | number       | Opening price                                        |
| high                   | number       | Highest traded price                                 |
| low                    | number       | Lowest traded price                                  |
| close                  | number       | Closing price                                        |
| ltp                    | number       | Last traded price                                    |
| reserve1–reserve8      | string       | Reserved fields for future use                       |

---

## Notes

- Use instrument ID to fetch trading metadata.
- Derivative-specific fields like `expiryDate`, `strikePrice`, and `optionType` may be `null` for equities.
- Price band values define allowed trading range.
- Reserved fields are placeholders for future system extensions.
