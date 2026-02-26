# Signals API Documentation

## Overview

The **Signals API** allows strategies or external systems to send actionable trading instructions to the Blitz OMS/Strategy.

Using this API, you can broadcast **Start / Stop / Buy / Sell / Multi-instrument signals** between strategies or modules in real time.

This API is useful for monitoring, routing, and executing strategy-level instructions across different modules.

!!! note
Signals are typically used to automate strategy communication and reduce manual order placement.

---

## Endpoint

| Method | URL            | Description                                     |
| ------ | -------------- | ----------------------------------------------- |
| POST   | `/api/signals` | Send one or more signals to the strategy engine |

!!! note
Use the `Authorization` header with a valid token for authentication, just like other Blitz APIs.

---

## Send Signals {#post-signal}

Send an array of trading signals from one strategy or module to another.  
Each signal contains **action**, **instruments**, **time**, and **routing information**.

Signals are also published to the internal Redis channel `STRATEGYHOSTSERVICE`.

!!! note
Signals are not persisted in a traditional database.  
 They are meant strictly for real-time strategy routing.

---

## Examples

### 1. One Signal – One Instrument

```json
[
  {
    "SourceStrategy": "MLegX2",
    "DestinationStrategy": "Nifty Ghost",
    "SourceSID": "Test1",
    "InstanceRunningMode": "Started",
    "GlobalAction": "Signal",
    "Instruments": [
      {
        "ExchangeSegment": "NSEFO",
        "InstrumentName": "NIFTY16SEP2524700PE",
        "Action": "EnterLong",
        "Lot": "1",
        "TimeStamp": "2025-06-24T12:57:00",
        "InfoText": "Entry Signal"
      }
    ]
  }
]
```

---

### 2. One Signal – Multiple Instruments

```json
[
  {
    "SourceStrategy": "MLegX2",
    "DestinationStrategy": "Calculus",
    "SourceSID": "Test1",
    "InstanceRunningMode": "Started",
    "GlobalAction": "Signal",
    "Instruments": [
      {
        "ExchangeSegment": "NSEFO",
        "InstrumentName": "NIFTY29MAY2524600PE",
        "Action": "EnterLong",
        "Lot": "1",
        "TimeStamp": "2025-06-24T12:57:00",
        "InfoText": "Leg 1"
      },
      {
        "ExchangeSegment": "NSEFO",
        "InstrumentName": "NIFTY29MAY2524650PE",
        "Action": "EnterLong",
        "Lot": "1",
        "TimeStamp": "2025-06-24T12:57:05",
        "InfoText": "Leg 2"
      }
    ]
  }
]
```

---

### 3. Multiple Signals – One Instrument Each

```json
[
  {
    "SourceStrategy": "MLegX2",
    "DestinationStrategy": "Calculus",
    "SourceSID": "Test1",
    "InstanceRunningMode": "Started",
    "GlobalAction": "Signal",
    "Instruments": [
      {
        "ExchangeSegment": "NSEFO",
        "InstrumentName": "NIFTY29MAY2524600PE",
        "Action": "EnterLong",
        "Lot": "1",
        "TimeStamp": "2025-06-24T12:57:00",
        "InfoText": "Signal 1"
      }
    ]
  },
  {
    "SourceStrategy": "MLegX2",
    "DestinationStrategy": "Calculus",
    "SourceSID": "Test1",
    "InstanceRunningMode": "Started",
    "GlobalAction": "Signal",
    "Instruments": [
      {
        "ExchangeSegment": "NSEFO",
        "InstrumentName": "NIFTY29MAY2524650PE",
        "Action": "EnterShort",
        "Lot": "1",
        "TimeStamp": "2025-06-24T12:58:00",
        "InfoText": "Signal 2"
      }
    ]
  }
]
```

---

### 4. Multiple Signals – Multiple Instruments

```json
[
  {
    "SourceStrategy": "MLegX2",
    "DestinationStrategy": "Calculus",
    "SourceSID": "Test1",
    "InstanceRunningMode": "Started",
    "GlobalAction": "Signal",
    "Instruments": [
      {
        "ExchangeSegment": "NSEFO",
        "InstrumentName": "NIFTY29MAY2524600PE",
        "Action": "EnterLong",
        "Lot": "1",
        "TimeStamp": "2025-06-24T12:57:00",
        "InfoText": "Signal A - Leg 1"
      },
      {
        "ExchangeSegment": "NSEFO",
        "InstrumentName": "NIFTY29MAY2524650PE",
        "Action": "EnterLong",
        "Lot": "1",
        "TimeStamp": "2025-06-24T12:57:05",
        "InfoText": "Signal A - Leg 2"
      }
    ]
  },
  {
    "SourceStrategy": "MLegX2",
    "DestinationStrategy": "Calculus",
    "SourceSID": "Test1",
    "InstanceRunningMode": "Started",
    "GlobalAction": "Signal",
    "Instruments": [
      {
        "ExchangeSegment": "NSEFO",
        "InstrumentName": "NIFTY29MAY2524700PE",
        "Action": "GoFlat",
        "Lot": "1",
        "TimeStamp": "2025-06-24T12:58:00",
        "InfoText": "Signal B - Exit"
      }
    ]
  }
]
```

---

## cURL Example

```bash
curl -X POST "http://api.blitz.com/api/signals" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer <TOKEN>" \
  -H "Content-Type: application/json" \
  -d '<YOUR_SIGNALS_PAYLOAD>'
```

---

## Successful Response

```json
{
  "status": "Published"
}
```

!!! tip
To debug signal flow, subscribe to the Redis channel `STRATEGYHOSTSERVICE` to monitor real-time messages.

---

# Field Descriptions

## Signal Object

| Field                 | Type   | Description                                      |
| --------------------- | ------ | ------------------------------------------------ |
| `SourceStrategy`      | string | Strategy sending the signal                      |
| `DestinationStrategy` | string | Strategy that should receive the signal          |
| `SourceSID`           | string | Unique strategy instance identifier              |
| `InstanceRunningMode` | string | Mode: `Started`, `Stopped`, etc.                 |
| `GlobalAction`        | string | High-level instruction: `Signal`, `GoFlat`, etc. |
| `Instruments`         | array  | List of instrument-level actions                 |

- `InstanceRunningMode` tells the receiver strategy whether the sending strategy instance is running or stopped.
- If `GlobalAction` is `GoFlat`, the strategy must close existing positions and avoid new entries until another `Signal` action is received.

---

## Instrument Object

| Field             | Type                  | Description                         |
| ----------------- | --------------------- | ----------------------------------- |
| `ExchangeSegment` | string                | NSECM / NSEFO / BSECM / BSEFO       |
| `InstrumentName`  | string                | Name of the instrument              |
| `Action`          | string                | `EnterLong`, `EnterShort`, `GoFlat` |
| `Lot`             | string                | Lot size or quantity                |
| `TimeStamp`       | string (ISO datetime) | Signal generation time              |
| `InfoText`        | string                | Remarks or additional information   |

---

# Edge Cases & Error Handling

## No Signals Provided

```json
{
  "status_code": 400,
  "message": "No signals provided"
}
```

!!! note
**Cause:** Payload array is empty (`[]`).  
 **Effect:** API rejects the request with `400 Bad Request`.

!!! tip
Always ensure the signals array contains at least one valid signal object before sending.

---

## Missing Required Fields in Signal Object

```json
{
  "status_code": 400,
  "response_json": {
    "type": "https://tools.ietf.org/html/rfc9110#section-15.5.1",
    "title": "One or more validation errors occurred.",
    "status": 400,
    "errors": {
      "[1].SourceSID": ["The SourceSID field is required."],
      "[1].GlobalAction": ["The GlobalAction field is required."],
      "[1].DestinationStrategy": ["The DestinationStrategy field is required."],
      "[1].InstanceRunningMode": ["The InstanceRunningMode field is required."]
    }
  }
}
```

!!! tip
Validate each signal object before sending. Ensure all required fields are present.
