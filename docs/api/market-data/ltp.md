# **Last Traded Price**

The LTP API allows you to retrieve the latest traded price (Last Traded Price) for one or more instruments using their instrument identifiers.

Using this API, you can fetch real-time price data for instruments across exchanges, which is essential for tracking market movements and making trading decisions.

These endpoints let you retrieve LTP data for trading, analytics, and monitoring purposes.

`BASE URL: http://uat.quantxpress.com/md-api`

| **Type** | **Endpoint**                      | **Description**                              |
| -------- | -------------------------------- | -------------------------------------------- |
| POST     | `/marketfeed/ltp`    | Retrieve last traded price for instruments   |


## Endpoint

```
POST /marketfeed/ltp
```

---

## Headers

| Header        | Value            | Required |
| ------------  | ---------------- | -------- |
| Content-Type  | application/json | Yes      |
| Authorization | Bearer {access_token} | Yes      |
| Accept        | `*/*`              | Yes      |

---

## Parameters

| Parameter | Type | Required | Description |
| --------- | ---- | -------- | ----------- |
| `InstrumentIds` | array of Long | No* | List of numeric instrument identifiers. (*Required if `InstrumentNames` is not provided). |
| `InstrumentNames` | array of String | No* | List of exact instrument names. (*Required if `InstrumentIds` is not provided). |

---

## Request Payload & Examples

You can request the Last Trading Price (LTP) by providing either a list of **Instrument IDs** or a list of **Instrument Names**.

=== "By Instrument ID"

    ```bash
    curl -X 'POST' \
      'http://uat.quantxpress.com/md-api/marketfeed/ltp' \
      -H 'Content-Type: application/json' \
      -H 'Authorization: Bearer {access_token}' \
      -H 'Accept: */*' \
      -d '{
        "InstrumentIds": [110010002000001, 110010002000002]
      }'
    ```

    **Empty Payload for User**

    ```json
    {
      "InstrumentIds": []
    }
    ```

    #### Response

    ```json
    {
        "status": "success",
        "data": {
            "110010002000001": {
                "instrumentId": 110010002000001,
                "ltp": 24188.10
            },
            "110010002000002": {
                "instrumentId": 110010002000002,
                "ltp": 56308.30
            }
        }
    }
    ```


=== "By Instrument Name"

    ```bash
    curl -X 'POST' \
      'http://uat.quantxpress.com/md-api/marketfeed/ltp' \
      -H 'Content-Type: application/json' \
      -H 'Authorization: Bearer {access_token}' \
      -H 'Accept: */*' \
      -d '{
        "InstrumentNames": ["NSECM:RELIANCE", "NSEFO:NIFTY28APR26FUT"]
      }'
    ```

    **Empty Payload for User**

    ```json
    {
      "InstrumentNames": []
    }
    ```

    #### Response

    ```json
    {
        "status": "success",
        "data": {
            "110010002000001": {
                "instrumentId": 110010002000001,
                "ltp": 24188.10
            },
            "110010002000002": {
                "instrumentId": 110010002000002,
                "ltp": 56308.30
            }
        }
    }
    ```


---

!!! info "Instrument Naming Convention"
    When fetching by Instrument Name, the string follows a specific pattern based on the segment:
    
    * **Cash Market (Equity)**: `[ExchangeSegment]:[Symbol]`
        - *Example*: `NSECM:RELIANCE`
        
    * **Futures & Options**: `[ExchangeSegment]:[Symbol][Date][Strike][OptionType]`
        - **Date Format**: `DDMMMYY` (e.g., `28APR26` represents **28 April 2026**).
        - **Month Codes**: January (`JAN`), February (`FEB`), March (`MAR`), April (`APR`), May (`MAY`), June (`JUN`), July (`JUL`), August (`AUG`), September (`SEP`), October (`OCT`), November (`NOV`), December (`DEC`).
        - **Future Example**: `NSEFO:NIFTY28APR26FUT` (NIFTY Future, expiring 28-Apr-2026).
        - **Option Example**: `NSEFO:NIFTY28APR2625000CE` (NIFTY Option, expiring 28-Apr-2026, 25000 Strike Price, Call Option. Similarly for PUT options use 'PE').

---

## Understanding the Response

The response object maps the requested Instrument ID (or Name) directly to its Live Trading Data, allowing for extremely fast key-value lookups in your application.

### Root Fields
| Field   | Type   | Description                                  |
| ------- | ------ | -------------------------------------------- |
| `status`  | string | Status of the request (`success` or `error`). |
| `message` | string | Optional response message.                    |
| `data`    | object | A dictionary containing instrument details mapped by ID/Name. |

### LTP Fields
Inside each object within the `data` dictionary:

| Field | Type | Description |
|-------|------|-------------|
| `instrumentId` | integer | Unique numeric identifier of the instrument. |
| `ltp` | float | **Last Traded Price** of the instrument. |
