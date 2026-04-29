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

## Request Payload

You can request the Last Trading Price (LTP) by providing either a list of **Instrument IDs** or a list of **Instrument Names**.

=== "Using Instrument IDs"

    ```json
    {
      "InstrumentIds": [1010010002000001, 1010010002000003]
    }
    ```

=== "Using Instrument Names"

    ```json
    {
      "InstrumentNames": ["NSECM:RELIANCE", "NSEFO:NIFTY28042026FUT"]
    }
    ```

---

## Parameters

| Parameter | Type | Required | Description |
| --------- | ---- | -------- | ----------- |
| `InstrumentIds` | array of Long | No* | List of numeric instrument identifiers. (*Required if `InstrumentNames` is not provided). |
| `InstrumentNames` | array of String | No* | List of exact instrument names. (*Required if `InstrumentIds` is not provided). |

!!! info "Instrument Naming Convention"
    When fetching by Instrument Name, the string follows a specific pattern based on the segment:
    
    * **Cash Market (Equity)**: `[ExchangeSegment]:[Symbol]`
        - *Example*: `NSECM:RELIANCE`
        
    * **Futures & Options**: `[ExchangeSegment]:[Symbol][Date][Strike][OptionType]`
        - **Date Format**: `DDMMYYYY` (e.g., `28042026` represents **28 April 2026**).
        - **Future Example**: `NSEFO:NIFTY28042026FUT` (NIFTY Future, expiring 28-Apr-2026).
        - **Option Example**: `NSEFO:NIFTY2804202625000CE` (NIFTY Option, expiring 28-Apr-2026, 25000 Strike Price, Call Option. Similarly for PUT options use 'PE').

---

## Examples

Here is how you can retrieve the LTP programmatically. You can send either `InstrumentIds` or `InstrumentNames`.

=== "cURL"

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

=== "Python"

    ```python
    import requests

    url = "http://uat.quantxpress.com/md-api/marketfeed/ltp"
    headers = {
        "Authorization": "Bearer YOUR_ACCESS_TOKEN",
        "Content-Type": "application/json"
    }
    payload = {
        # Using Instrument Names instead of IDs
        "InstrumentNames": ["NSECM:RELIANCE", "NSEFO:NIFTY28042026FUT"]
    }

    response = requests.post(url, json=payload, headers=headers)
    
    if response.status_code == 200:
        data = response.json().get("data", {})
        for key, value in data.items():
            print(f"Instrument {key} LTP: {value.get('ltp')}")
    else:
        print("Error fetching LTP")
    ```

=== "Node.js"

    ```javascript
    const axios = require('axios');

    const fetchLTP = async () => {
      try {
        const response = await axios.post('http://uat.quantxpress.com/md-api/marketfeed/ltp', {
          InstrumentIds: [110010002000001, 110010002000002]
        }, {
          headers: { 'Authorization': 'Bearer YOUR_ACCESS_TOKEN' }
        });
        
        const data = response.data.data;
        Object.keys(data).forEach(id => {
            console.log(`Instrument ID ${id} LTP is ${data[id].ltp}`);
        });
      } catch (error) {
        console.error("Error:", error.message);
      }
    };
    fetchLTP();
    ```

---

## Understanding the Response

The response object maps the requested Instrument ID (or Name) directly to its Live Trading Data, allowing for extremely fast key-value lookups in your application.

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
| `instrumentName` | string | Name or symbol of the instrument (if requested by Name). |
| `ltp` | float | **Last Traded Price** of the instrument. |
