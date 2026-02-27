# Blitz Order Message Structure & Request–Response Specification

This document defines the **standard broker-independent request and response format** used by Blitz for order communication with broker adapters.

It provides:

- Standard request format
- Standard response format
- Place / Modify / Cancel order structures
- OrderLog response structure
- Field definitions
- Status definitions
- Adapter responsibilities

This specification is broker-agnostic and must be followed by all adapter implementations.

---

# Overview

Blitz uses a standardized message structure to communicate with broker adapters.

All order operations follow a consistent flow:

## Order Flow

1. Blitz publishes request to adapter.
2. Adapter converts request to broker format.
3. Broker processes order.
4. Adapter converts broker response to Blitz format.
5. Blitz consumes standardized response.

---

# Request Structure

All order operations use a common request envelope.

## Common Request Format

```json
{
  "Action": "ACTION_NAME",
  "TPOmsName": "BROKER_NAME",
  "UserId": "USER_ID",
  "UserName": "USER_NAME",
  "Data": {}
}
```

## Request Fields

| Field     | Type   | Description                                                  |
| --------- | ------ | ------------------------------------------------------------ |
| Action    | string | Request type (`PLACE_ORDER`, `MODIFY_ORDER`, `CANCEL_ORDER`) |
| TPOmsName | string | Target broker adapter                                        |
| UserId    | string | Unique user identifier                                       |
| UserName  | string | User name                                                    |
| Data      | object | Action-specific payload                                      |

---

# Place Order Request

## Action

`PLACE_ORDER`

## Structure

```json
{
  "Action": "PLACE_ORDER",
  "TPOmsName": "BROKER_NAME",
  "UserId": "ABC123",
  "UserName": "Harshit",
  "Data": {
    "Account": "ACCOUNT_ID",
    "ExchangeClientID": "CLIENT_ID",
    "ExchangeSegment": "NSECM",
    "ExchangeInstrumentID": 1234,
    "SymbolName": "IDEA",
    "ExchangeInstrumentName": "VODAFONE IDEA LIMITED",
    "ProductType": "MIS",
    "OrderType": "LIMIT",
    "OrderSide": "BUY",
    "TimeInForce": "DAY",
    "DisclosedQuantity": 10,
    "OrderQuantity": 100,
    "LimitPrice": 11.25,
    "StopPrice": 11.0,
    "BlitzAppOrderID": 4576347291,
    "AlgoID": "MOMENTUM_V1",
    "AlgoCategory": "INTRADAY",
    "IsFictiveOrder": false
  }
}
```

## Place Order Fields

| Field                  | Type    | Description                               |
| ---------------------- | ------- | ----------------------------------------- |
| Account                | string  | Trading account identifier                |
| ExchangeClientID       | string  | Client identifier                         |
| ExchangeSegment        | string  | Exchange segment                          |
| ExchangeInstrumentID   | number  | Exchange instrument identifier            |
| SymbolName             | string  | Trading symbol                            |
| ExchangeInstrumentName | string  | Full instrument name                      |
| ProductType            | string  | Order product type (`MIS`, `CNC`, `NRML`) |
| OrderType              | string  | `MARKET`, `LIMIT`, `SL`, `SL-M`           |
| OrderSide              | string  | `BUY`, `SELL`                             |
| TimeInForce            | string  | `GFD`, `IOC`                              |
| DisclosedQuantity      | number  | Quantity disclosed to market              |
| OrderQuantity          | number  | Total quantity                            |
| LimitPrice             | number  | Limit price                               |
| StopPrice              | number  | Trigger price                             |
| BlitzAppOrderID        | number  | Unique order ID                           |
| AlgoID                 | string  | Strategy identifier                       |
| AlgoCategory           | string  | Strategy category                         |
| IsFictiveOrder         | boolean | Simulation flag                           |

---

# Modify Order Request

## Action

`MODIFY_ORDER`

## Structure

```json
{
  "Action": "MODIFY_ORDER",
  "TPOmsName": "BROKER_NAME",
  "UserId": "ABC123",
  "UserName": "Harshit",
  "Data": {
    "Account": "ACCOUNT_ID",
    "ExchangeClientID": "CLIENT_ID",
    "ExchangeSegment": "NSECM",
    "ExchangeInstrumentID": 1234,
    "SymbolName": "IDEA",
    "ModifiedProductType": "NRML",
    "ModifiedOrderType": "LIMIT",
    "ModifiedTimeInForce": "IOC",
    "ModifiedDisclosedQuantity": 10,
    "ModifiedOrderQuantity": 150,
    "ModifiedLimitPrice": 11.15,
    "ModifiedStopPrice": 11.1,
    "BlitzAppOrderID": 1234567891,
    "ExchangeOrderID": "ORDER_ID"
  }
}
```

## Modify Order Fields

| Field                     | Type   | Description                |
| ------------------------- | ------ | -------------------------- |
| ExchangeOrderID           | string | Broker order identifier    |
| ModifiedProductType       | string | Updated product            |
| ModifiedOrderType         | string | Updated order type         |
| ModifiedTimeInForce       | string | Updated validity           |
| ModifiedDisclosedQuantity | number | Updated disclosed quantity |
| ModifiedOrderQuantity     | number | Updated quantity           |
| ModifiedLimitPrice        | number | Updated price              |
| ModifiedStopPrice         | number | Updated trigger price      |

---

# Cancel Order Request

## Action

`CANCEL_ORDER`

## Structure

```json
{
  "Action": "CANCEL_ORDER",
  "TPOmsName": "BROKER_NAME",
  "UserId": "ABC123",
  "UserName": "Harshit",
  "Data": {
    "Account": "ACCOUNT_ID",
    "ExchangeClientID": "CLIENT_ID",
    "ExchangeSegment": "NSECM",
    "ExchangeInstrumentID": 3677697,
    "SymbolName": "IDEA",
    "BlitzAppOrderID": 4058284572,
    "ExchangeOrderID": "ORDER_ID"
  }
}
```

---

# Drop Copy Trade Request

## Action

`DROP_COPY_TRADE`

## Description

Requests real-time or latest trade execution updates from the broker.
Drop copy trades provide a passive stream of executed trades without placing or modifying orders.

```json
{
  "Action": "DROPCOPY_TRADES",
  "TPOmsName": "BROKER_NAME",
  "UserId": "USER_ID",
  "Data": ""
}
```

## Request Fields

| Field     | Type   | Description                       |
| --------- | ------ | --------------------------------- |
| Action    | string | `DROPCOPY_TRADES`                 |
| TPOmsName | string | Target broker adapter             |
| UserId    | string | Unique user identifier            |
| Data      | string | Reserved field (currently unused) |

---

# Drop Copy Trade Response

## Action

`DROPCOPY_INSTRUMENT_POSITION`

## Structure

```json
{
  "Action": "DROPCOPY_INSTRUMENT_POSITION",
  "TPOmsName": "BROKER_NAME",
  "UserId": "USER_ID",
  "Data": ""
}
```

## Request Fields

| Field     | Type   | Description                       |
| --------- | ------ | --------------------------------- |
| Action    | string | `DROPCOPY_INSTRUMENT_POSITION`    |
| TPOmsName | string | Target broker adapter             |
| UserId    | string | Unique user identifier            |
| Data      | string | Reserved field (currently unused) |

---

# Response Structure

All broker responses must be converted into the Blitz standardized response format.

## Common Response Envelope

```json
{
  "MessageType": "ORDER_EVENT",
  "TPOmsName": "BROKER_NAME",
  "UserId": "USER_ID",
  "Data": {}
}
```

## Response Fields

| Field       | Type   | Description             |
| ----------- | ------ | ----------------------- |
| MessageType | string | Response event type     |
| TPOmsName   | string | Broker adapter name     |
| UserId      | string | User identifier         |
| Data        | object | Standard order response |

---

# Standard Order Response (OrderLog)

## Structure

```json
{
  "SequenceNumber": 0,
  "Account": "ACCOUNT_ID",
  "ExchangeClientID": "CLIENT_ID",
  "BlitzAppOrderID": 4576347291,
  "ExchangeOrderID": "ORDER_ID",
  "ExchangeSegment": "NSE",
  "ExchangeInstrumentID": 3677697,
  "OrderSide": "BUY",
  "OrderType": "LIMIT",
  "ProductType": "MIS",
  "TimeInForce": "DAY",
  "OrderPrice": 11.25,
  "OrderQuantity": 100,
  "OrderStopPrice": 11.0,
  "OrderStatus": "NEW",
  "OrderAverageTradedPrice": 0,
  "LeavesQuantity": 100,
  "CumulativeQuantity": 0,
  "OrderDisclosedQuantity": 10,
  "OrderGeneratedDateTime": "2026-01-08 15:23:46",
  "ExchangeTransactTime": "2026-01-08 15:23:46",
  "LastUpdateDateTime": "2026-01-08 15:23:46",
  "CancelRejectReason": null,
  "LastTradedPrice": 0,
  "LastTradedQuantity": 0,
  "LastExecutionTransactTime": null,
  "ExecutionID": ""
}
```

---

# Order Status Values

| Status           | Meaning            |
| ---------------- | ------------------ |
| NEW              | Order accepted     |
| FILLED           | Fully executed     |
| PARTIALLY_FILLED | Partially executed |
| REPLACED         | Order modified     |
| CANCELLED        | Order cancelled    |
| REJECTED         | Order rejected     |

---

# Adapter Responsibilities

- Convert broker responses into Blitz format
- Normalize order status values
- Populate all fields where possible
- Default missing numeric values to `0`
- Default missing strings to `""` or `null`
- Maintain consistent timestamp format
- Ensure request and response structure consistency

---
