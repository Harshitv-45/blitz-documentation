# **Last Traded Price**

The LTP API allows you to retrieve the latest traded price (Last Traded Price) for one or more instruments using their instrument identifiers.

Using this API, you can fetch real-time price data for instruments across exchanges, which is essential for tracking market movements and making trading decisions.

These endpoints let you retrieve LTP data for trading, analytics, and monitoring purposes.

| **Type** | **Endpoint**                      | **Description**                              |
| -------- | -------------------------------- | -------------------------------------------- |
| POST     | [`/marketfeed/ltp`](#get-ltp)    | Retrieve last traded price for instruments   |


## **Last Traded Price (LTP)**

Retrieve the latest traded price (LTP) for one or more instruments using their instrument identifiers.

## Endpoint

```
POST /marketfeed/ltp
```

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
  "InstrumentIds": ["NSECM|RELIANCE", "NSECM|TCS", 1010010002000001]
}
```

---

## Parameters
|Parameter	|Type	|Required	|Description|
|-----------|-------|-----------|-----------|
InstrumentIds|list / array|Yes|List of instrument identifiers (symbols or numeric instrument IDs).

---

## Example

```bash
curl -X 'POST' \
  'http://uat.quantxpress.com/marketfeed/ltp' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {access_token}' \
  -H 'Accept": "*/*' \
  -d '{
    "InstrumentIds": ["NSE|TCS", "NSE|RELIANCE", 1010010002000001]
  }'
```

---

## **Response**

```json
{
  "status": "success",
  "data": {
    "1010010000002885": {
      "InstrumentID": 1010010000002885,
      "InstrumentName": "RELIANCE",
      "LTP": 1420.2
    },
    "1010010000011536": {
      "InstrumentID": 1010010000011536,
      "InstrumentName": "TCS",
      "LTP": 3500.3
    },
    "1010010002000001": {
      "InstrumentID": 1010010002000001,
      "InstrumentName": "NIFTY 50",
      "LTP": 24834.3
    }
  }
}
```

---

### **Response Field Descriptions**

### Root Response Fields
| Field   | Type   | Description                                  |
| ------- | ------ | -------------------------------------------- |
| status  | string | Status of the request (`success` or `error`) |
| message | string | Response message                             |
| data    | object | Contains instrument details                  |

### LTP Fields

|Field	|Type	|Description|
|-------|--------|-----------|
|InstrumentID|	integer	|Unique identifier of the instrument|
|InstrumentName	|string	|Name or symbol of the instrument|
|LTP	|float	|Last traded price of the instrument|