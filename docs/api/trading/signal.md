# Signals API Documentation

The **Signals API** allows strategies or external systems to send actionable trading instructions to the Blitz OMS/Strategy.  
Using this API, you can broadcast **Start / Stop / Buy / Sell / Multi-instrument signals** between strategies or modules in real time.

This API is useful for monitoring, routing, and executing strategy-level instructions across different modules.

---

<details>
<summary><b>Tip</b></summary>

<br>

Signals are typically used to automate strategy communication and reduce manual order placement.

</details>

## Endpoints

| Type | Endpoint                | Description                                     |
| ---- | ----------------------- | ----------------------------------------------- |
| POST | [signals](#post-signal) | Send one or more signals to the strategy engine |

---

<details>
<summary><b>Tip</b></summary>

<br>

Use the Authorization header with a valid token for authentication, just like other Blitz APIs.

</details>

## Send Signals {#post-signal}

Send an array of trading signals from one strategy or module to another.  
Each signal contains **action**, **instruments**, **time**, and **routing information**.

Signals are also published to the internal **Redis channel `STRATEGYHOSTSERVICE`**.

<details>
<summary><b>Note</b></summary>

<br>

Signals are not persisted in a traditional database — they are meant for real-time strategy routing.

</details>

---

## 1. One Signal - One Instrument

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
        "TimeStamp": "24-06-2025 12:57:00",
        "InfoText": "Entry Signal"
      }
    ]
  }
]
```

## 2. One Signal - Multiple Instruments

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
        "TimeStamp": "24-06-2025 12:57:00",
        "InfoText": "Leg 1"
      },
      {
        "ExchangeSegment": "NSEFO",
        "InstrumentName": "NIFTY29MAY2524650PE",
        "Action": "EnterLong",
        "Lot": "1",
        "TimeStamp": "24-06-2025 12:57:05",
        "InfoText": "Leg 2"
      }
    ]
  }
]
```

## 3. Multiple Signals – One Instrument Each

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
        "TimeStamp": "24-06-2025 12:57:00",
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
        "TimeStamp": "24-06-2025 12:58:00",
        "InfoText": "Signal 2"
      }
    ]
  }
]
```

## 4. Multiple Signals – Multiple Instruments

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
        "TimeStamp": "24-06-2025 12:57:00",
        "InfoText": "Signal A - Leg 1"
      },
      {
        "ExchangeSegment": "NSEFO",
        "InstrumentName": "NIFTY29MAY2524650PE",
        "Action": "EnterLong",
        "Lot": "1",
        "TimeStamp": "24-06-2025 12:57:05",
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
        "TimeStamp": "24-06-2025 12:58:00",
        "InfoText": "Signal B - Exit"
      }
    ]
  }
]
```

---

## cURL

```bash
curl -X POST \
  "http://api.blitz.com/api/signals" \
  -H "accept: application/json" \
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

<details>
<summary><b>Tip</b></summary>

<br>

If you want to debug, subscribe to SignalChannel in Redis to see incoming signals in real time.  
Signals are published to Redis channel `STRATEGYHOSTSERVICE`.

</details>

---

## Field Descriptions

### Signal Object

| Field                 | Type   | Description                             |
| --------------------- | ------ | --------------------------------------- |
| `sourceStrategy`      | string | Strategy sending the signal             |
| `destinationStrategy` | string | Strategy that should receive the signal |
| `sourceSID`           | string | Unique strategy instance identifier     |
| `instanceRunningMode` | string | Mode: `Started`, `Stopped`, etc.        |
| `globalAction`        | string | High-level instruction                  |
| `instruments`         | array  | List of instrument-level actions        |

<details>
<summary><b>Note</b></summary>

<br>

- `InstanceRunningMode` tells the receiver strategy whether the sending strategy instance is actively running or stopped.
- If `GlobalAction` is `GoFlat`, the strategy should not execute any trading logic other than closing existing positions. No fresh entries are allowed until another `Signal` action is received.

</details>

---

### Instrument Object

| Field             | Type                  | Description                           |
| ----------------- | --------------------- | ------------------------------------- |
| `exchangeSegment` | string                | NSECM / NSEFO / BSECM / BSEFO         |
| `instrumentName`  | string                | Name of the instrument                |
| `action`          | string                | `EnterLong` / `EnterShort` / `GoFlat` |
| `lot`             | string                | Lot size or quantity                  |
| `timeStamp`       | string (ISO datetime) | Signal generation time                |
| `infoText`        | string                | Remarks or custom info                |

---

## Edge Cases & Error Handling

### No Signals Provided

```json
{
  "status_code": 400,
  "message": "No signals provided"
}
```

<details>
<summary><b>Cause</b></summary>

<br>

Triggered when the payload array is empty (`[]`).

</details>

<details>
<summary><b>Effect</b></summary>

<br>

API rejects the request with a 400 Bad Request.

</details>

<details>
<summary><b>Tip</b></summary>

<br>

Always ensure your signals array contains at least one valid signal object before sending.

</details>

<details>
<summary><b>Note</b></summary>

<br>

Requests must include a valid, non-empty payload.

</details>

---

### Missing Required Fields in Signal Object

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

<details>
<summary><b>Tip</b></summary>

<br>

Validate each signal object before sending. Ensure all required fields are present.

</details>
