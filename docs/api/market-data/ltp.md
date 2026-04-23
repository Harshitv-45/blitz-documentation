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

```json
{
  "InstrumentIds": [1010010002000001, 1010010002000003]
}
```

---

## Parameters
| Parameter      | Type         | Required | Description |
|----------------|--------------|----------|--------------|
| InstrumentIds  | list / array | Yes      | List of instrument identifiers (symbols or numeric instrument IDs). |

---

## Example

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

---

## **Response**

```json
{
    "data": {
        "110010002000001": {
            "instrumentId": 110010002000001,
            "ltp": 24188.100000000002
        },
        "110010002000002": {
            "instrumentId": 110010002000002,
            "ltp": 56308.3
        }
    },
    "status": "success"
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