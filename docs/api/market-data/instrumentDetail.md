# **Instrument Detail**

The **Instrument Detail API** allows you to retrieve detailed information about a trading instrument using its instrument ID or instrument name.

Using this API, you can fetch instrument metadata such as symbol details, exchange information, price bands, tick size, lot size, and other trading parameters.

---

`BASE URL: http://uat.quantxpress.com/v1`


## Endpoint

| Method | URL                                             |
| ------ | ----------------------------------------------- |
| GET    | `/api/instruments/{id/name}`                 |

### Path Parameters

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| `id/name` | `string/long` | You can retrieve details using either the numeric **Instrument ID** (e.g., `110010000014366`) or by **Name** (e.g., `NSECM:RELIANCE`). |

!!! info "Instrument Naming Convention"
    When fetching by Instrument Name, the string follows a specific pattern based on the segment:
    
    * **Cash Market (Equity)**: `[ExchangeSegment]:[Symbol]`
        - *Example*: `NSECM:RELIANCE`
        
    * **Futures & Options**: `[ExchangeSegment]:[Symbol][Date][Strike][OptionType]`
        - **Date Format**: `DDMMMYY` (e.g., `28APR26` represents **28 April 2026**).
        - **Month Codes**: January (`JAN`), February (`FEB`), March (`MAR`), April (`APR`), May (`MAY`), June (`JUN`), July (`JUL`), August (`AUG`), September (`SEP`), October (`OCT`), November (`NOV`), December (`DEC`).
        - **Future Example**: `NSEFO:NIFTY28APR26FUT` (NIFTY Future, expiring 28-Apr-2026).
        - **Option Example**: `NSEFO:NIFTY28APR2625000CE` (NIFTY Option, expiring 28-Apr-2026, 25000 Strike Price, Call Option similarly for PUT options use 'PE').

---

## Headers

| Header        | Value            | Required |
| ------------  | ---------------- | -------- |
| Content-Type  | application/json | Yes      |
| Authorization | Bearer {access_token} | Yes      |
| Accept        | `*/*`              | Yes      |

## Examples

=== "By Instrument ID"

    ```bash
    curl -X 'GET' \
      'http://uat.quantxpress.com/v1/api/instruments/110010000014366' \
      -H 'Content-Type: application/json' \
      -H 'Accept: application/json' \
      -H 'Authorization: Bearer {access_token}'
    ```
    
    #### Response

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

=== "By Instrument Name"

    === "Equity"

        ```bash
        curl -X 'GET' \
          'http://uat.quantxpress.com/v1/api/instruments/NSECM:RELIANCE' \
          -H 'Content-Type: application/json' \
          -H 'Accept: application/json' \
          -H 'Authorization: Bearer {access_token}'
        ```

        #### Response

        ```json
        {
            "status": "success",
            "message": "request processed successfully",
            "data": {
                "instrumentId": 110010000002885,
                "exchange": "NSE",
                "symbol": "RELIANCE",
                "ticker": "RELIANCE INDUSTRIES LTD",
                "exchangeSegment": "NSECM",
                "instrumentType": "Equity",
                "instrumentName": "RELIANCE",
                "exchangeInstrumentId": 2885,
                "marketInstrumentId": 110010000002885,
                "series": "EQ",
                "tickSize": 0.1,
                "isin": "INE002A01018",
                "priceBandHigh": 1607.7,
                "priceBandLow": 1315.5,
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
                "open": 1357,
                "high": 1358.2,
                "low": 1328,
                "close": 1350.5,
                "ltp": 1350.9,
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

    === "Options"

        ```bash
        curl -X 'GET' \
          'http://uat.quantxpress.com/v1/api/instruments/NSEFO:NIFTY28APR2625000CE' \
          -H 'Content-Type: application/json' \
          -H 'Accept: application/json' \
          -H 'Authorization: Bearer {access_token}'
        ```

        #### Response

        ```json
        {
            "status": "success",
            "message": "request processed successfully",
            "data": {
                "instrumentId": 110010000003456,
                "exchange": "NSEFO",
                "symbol": "NIFTY",
                "ticker": "NIFTY Option",
                "exchangeSegment": "NSEFO",
                "instrumentType": "Options",
                "instrumentName": "NIFTY28APR2625000CE",
                "exchangeInstrumentId": 3456,
                "marketInstrumentId": 110010000003456,
                "series": "OPT",
                "tickSize": 0.05,
                "isin": "INE002A01018",
                "priceBandHigh": 200.0,
                "priceBandLow": 100.0,
                "multiplier": 1,
                "bvp": 1,
                "priceType": "1.0",
                "priceNumerator": 1,
                "priceDenominator": 1,
                "freezeQty": 999999,
                "lotSize": 50,
                "expiryDate": "2026-04-28",
                "strikePrice": 25000.0,
                "optionType": "CE",
                "assetToken": "1234",
                "underlyingInstrumentId": 110010000001234,
                "open": 150.0,
                "high": 160.0,
                "low": 140.0,
                "close": 155.0,
                "ltp": 158.0,
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

    === "Futures"

        ```bash
        curl -X 'GET' \
          'http://uat.quantxpress.com/v1/api/instruments/NSEFO:NIFTY28APR26FUT' \
          -H 'Content-Type: application/json' \
          -H 'Accept: application/json' \
          -H 'Authorization: Bearer {access_token}'
        ```

        #### Response

        ```json
        {
            "status": "success",
            "message": "request processed successfully",
            "data": {
                "instrumentId": 110010000003457,
                "exchange": "NSEFO",
                "symbol": "NIFTY",
                "ticker": "NIFTY Future",
                "exchangeSegment": "NSEFO",
                "instrumentType": "Futures",
                "instrumentName": "NIFTY28APR26FUT",
                "exchangeInstrumentId": 3457,
                "marketInstrumentId": 110010000003457,
                "series": "FUT",
                "tickSize": 0.05,
                "isin": "INE002A01018",
                "priceBandHigh": 25000.0,
                "priceBandLow": 23000.0,
                "multiplier": 1,
                "bvp": 1,
                "priceType": "1.0",
                "priceNumerator": 1,
                "priceDenominator": 1,
                "freezeQty": 999999,
                "lotSize": 50,
                "expiryDate": "2026-04-28",
                "strikePrice": null,
                "optionType": null,
                "assetToken": "1234",
                "underlyingInstrumentId": 110010000001234,
                "open": 24000.0,
                "high": 24100.0,
                "low": 23900.0,
                "close": 24050.0,
                "ltp": 24080.0,
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

## **Response Field Descriptions**

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
| exchange               | string       | Exchange where instrument is traded (e.g., `NSE`, `BSE`) |
| symbol                 | string       | Trading symbol of the instrument                     |
| ticker                 | string       | Full company or instrument name                      |
| exchangeSegment        | string       | Exchange segment (e.g., `NSECM`, `NSEFO`)                |
| instrumentType         | string       | Type of instrument (`Equity`, `Futures`, `Options`, etc.)  |
| instrumentName         | string       | Name of the instrument                               |
| exchangeInstrumentId   | integer      | Instrument identifier assigned by exchange           |
| marketInstrumentId     | integer      | Market-wide instrument identifier                    |
| series                 | string       | Instrument series (e.g., EQ for equity)              |
| tickSize               | float       | Minimum price movement allowed                       |
| isin                   | string       | International Securities Identification Number       |
| priceBandHigh          | float       | Upper price band limit                               |
| priceBandLow           | float       | Lower price band limit                               |
| multiplier             | float       | Contract multiplier                                  |
| bvp                    | float       | Base value point                                     |
| priceType              | string       | Price type configuration                             |
| priceNumerator         | float       | Price calculation numerator                          |
| priceDenominator       | float       | Price calculation denominator                        |
| freezeQty              | float       | Maximum quantity allowed per order                   |
| lotSize                | float       | Minimum tradable quantity                            |
| expiryDate             | integer/null | Expiry date (for derivatives)                        |
| strikePrice            | float/null  | Strike price (for options)                           |
| optionType             | string/null  | Option type (`CE`/`PE`)                                  |
| assetToken             | string/null  | Asset token reference                                |
| underlyingInstrumentId | integer/null | Underlying instrument ID (for derivatives)           |
| open                   | float       | Opening price                                        |
| high                   | float       | Highest traded price                                 |
| low                    | float       | Lowest traded price                                  |
| close                  | float       | Closing price                                        |
| ltp                    | float       | Last traded price                                    |
| reserve1–reserve8      | string       | Reserved fields                                      |

---

## Notes

- Use instrument ID to fetch trading metadata.
- Derivative-specific fields like `expiryDate`, `strikePrice`, and `optionType` may be `null` for equities.
- Price band values define allowed trading range.
- Reserved fields are placeholders for future system extensions.
