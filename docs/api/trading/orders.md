The order APIs let you place orders of various types, modify or cancel pending orders, and retrieve daily order history. With OMS(Order Management System.), you can manage your trades efficiently, execute orders in real time, track their status, and maintain full control over your portfolio .

These endpoints let you place, modify, cancel, and retrieve orders efficiently.

| **Type** | **Endpoint**                            | **Description**                                                  |
| -------- | --------------------------------------- | ---------------------------------------------------------------- |
| GET      | [orders](#fetching-orders)              | Retrieve the list of all orders (open and executed) for the day. |
| GET      | [orders/blitzOrderId](#get-order-by-id) | Get all states of a specific order by its BlitzOrderId.          |
| POST     | [orders/placeOrder](#place-order)       | Place an order of a particular variety.                          |
| PUT      | [orders/modifyOrder](#modify-order)     | Modify an open or pending order.                                 |
| DELETE   | [order/cancelOrder](#cancel-order)      | Cancel an open or pending order.                                 |

#### Glossary / Notes { .no-toc }

- `orders` → Refers to all orders for the day.
- `placeOrder` → Endpoint to create a new order.
- `modifyOrder` → Endpoint to update an existing order.
- `cancelOrder` → Endpoint to delete/cancel an order.

## 1. Fetching Orders {#fetching-orders}

Retrieve a complete list of all orders placed by a user for the current trading day. This includes:

- **Open orders:** Orders that have been placed but not yet executed.
- **Pending orders:** Orders waiting for certain conditions (like trigger price) before execution.
- **Executed orders:** Orders that have been fully or partially filled.
- **Cancelled orders:** Orders that were cancelled either by the user or the system.

This endpoint provides a **centralized view of your daily trading activity**, making it easier to manage your portfolio and maintain control over all executed, pending, or cancelled trades.

```bash
curl 'http://uat.quantxpress.com/v1/api/order' \
-H 'accept: */*'
```

#### Response Structure

<details>
<summary>Click to view Order List JSON Body</summary>

```json
[
  {
    "id": 860,
    "entityId": "0559cd2a-8903-46f8-a852-1c6e2ec0f3bc",
    "strategyId": "36250ee3-ad08-4e09-862f-83deee8cdef9",
    "strategyInstanceId": "88c11710-41f7-4304-ba81-bb92511cdf42",
    "strategyInstanceName": "NSECM|IDEA",
    "strategyName": "Manual Trading",
    "ivObjectName": "IDEA",
    "instrumentId": 110010000014366,
    "exchangeSegment": "NSECM",
    "exchangeInstrumentId": 110010000014366,
    "instrumentName": "IDEA",
    "instrumentType": 1,
    "blitzOrderId": 224151307880000035,
    "exchangeOrderId": "",
    "executionId": "",
    "account": "",
    "clientId": "XYZ",
    "orderType": "Market",
    "orderSide": "Buy",
    "orderStatus": "PendingNew",
    "orderQuantity": 1,
    "orderPrice": 0,
    "orderStopPrice": 0,
    "orderTriggerPrice": 0,
    "lastTradedQuantity": 0,
    "lastTradedPrice": 0,
    "cumulativeQuantity": 0,
    "leavesQuantity": 1,
    "tif": "GFD",
    "orderExpiryDate": 1771931071019,
    "orderDisclosedQuantity": 0,
    "minimumQuantity": 0,
    "orderGeneratedDateTime": 1771931071019,
    "lastRequestDateTime": 1771931071019,
    "exchangeTransactTime": 1771931071019,
    "orderModificationCount": 0,
    "orderTradeCount": 0,
    "averageTradedPrice": 0,
    "averageTradedValue": 0,
    "isFictiveOrder": false,
    "rejectType": "None",
    "rejectTypeReason": "",
    "orderTag": "NormalOrder",
    "ctclId": "111111111111111",
    "algoId": "123456789",
    "algoCategoryId": "123654789123",
    "clearingFirmId": "",
    "panId": "LMASV1230D",
    "isOrderCompleted": false,
    "userText": "",
    "executionType": "None",
    "strategyTag": "General",
    "sequenceNumber": 237,
    "correlationOrderId": "224151307880000035",
    "productType": "MIS",
    "sourceResponse": null
  }
]
```

</details>

## 2. Retrieve Order by BlitzOrderID {#get-order-by-id}

Fetch the details of a specific order in the OMS system using its unique BlitzOrderId. This endpoint returns all order-related information including status, executed trades, and metadata.

```bash
curl 'http://uat.quantxpress.com/v1/api/order/{BlitzOrderId}' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer YOUR_JWT_TOKEN'
```

#### Response Structure

<details>
<summary>Click to view Order by BlitzOrderID JSON Body</summary>

```json
[
  {
    "id": 860,
    "entityId": "0559cd2a-8903-46f8-a852-1c6e2ec0f3bc",
    "strategyId": "36250ee3-ad08-4e09-862f-83deee8cdef9",
    "strategyInstanceId": "88c11710-41f7-4304-ba81-bb92511cdf42",
    "strategyInstanceName": "NSECM|IDEA",
    "strategyName": "Manual Trading",
    "ivObjectName": "IDEA",
    "instrumentId": 110010000014366,
    "exchangeSegment": "NSECM",
    "exchangeInstrumentId": 110010000014366,
    "instrumentName": "IDEA",
    "instrumentType": 1,
    "blitzOrderId": 224151307880000035,
    "exchangeOrderId": "",
    "executionId": "",
    "account": "",
    "clientId": "HARSHIT",
    "orderType": "Market",
    "orderSide": "Buy",
    "orderStatus": "PendingNew",
    "orderQuantity": 1,
    "orderPrice": 0,
    "orderStopPrice": 0,
    "orderTriggerPrice": 0,
    "lastTradedQuantity": 0,
    "lastTradedPrice": 0,
    "cumulativeQuantity": 0,
    "leavesQuantity": 1,
    "tif": "GFD",
    "orderExpiryDate": 1771931071019,
    "orderDisclosedQuantity": 0,
    "minimumQuantity": 0,
    "orderGeneratedDateTime": 1771931071019,
    "lastRequestDateTime": 1771931071019,
    "exchangeTransactTime": 1771931071019,
    "orderModificationCount": 0,
    "orderTradeCount": 0,
    "averageTradedPrice": 0,
    "averageTradedValue": 0,
    "isFictiveOrder": false,
    "rejectType": "None",
    "rejectTypeReason": "",
    "orderTag": "NormalOrder",
    "ctclId": "111111111111111",
    "algoId": "123456789",
    "algoCategoryId": "123654789123",
    "clearingFirmId": "",
    "panId": "LMASV1230D",
    "isOrderCompleted": false,
    "userText": "",
    "executionType": "None",
    "strategyTag": "General",
    "sequenceNumber": 237,
    "correlationOrderId": "224151307880000035",
    "productType": "MIS",
    "sourceResponse": null
  }
]
```

</details>

#### Field Descriptions

| Field                  | Type    | Description                                                    |
| ---------------------- | ------- | -------------------------------------------------------------- |
| id                     | integer | Internal unique ID of the order                                |
| entityId               | string  | ID of the entity (user) who placed the order                   |
| strategyId             | string  | Strategy associated with the order, if any                     |
| strategyInstanceId     | string  | Instance of the strategy executing the order                   |
| strategyInstanceName   | string  | Name of the strategy instance                                  |
| strategyName           | string  | Name of the strategy                                           |
| ivObjectName           | string  | Name of the IV object linked to this order                     |
| instrumentId           | integer | Internal ID of the instrument being traded                     |
| exchangeSegment        | string  | Exchange segment (e.g., NSEFO, BSECM)                          |
| exchangeInstrumentId   | integer | Instrument ID used by the exchange                             |
| instrumentName         | string  | Symbol or name of the instrument                               |
| instrumentType         | integer | Type of instrument (Equity, Futures, Options, etc.)            |
| blitzOrderId           | integer | Order ID in Blitz system                                       |
| exchangeOrderId        | string  | Exchange-assigned order ID                                     |
| executionId            | string  | Execution ID for trades generated by this order                |
| account                | string  | Account under which the order was placed                       |
| clientId               | string  | Client ID who placed the order                                 |
| orderType              | string  | Type of order (LIMIT, MARKET, SL, SL-M)                        |
| orderSide              | string  | Side of the order: Buy or Sell                                 |
| orderStatus            | string  | Current status of the order (New, Filled, Cancelled, Rejected) |
| orderQuantity          | number  | Quantity of the order                                          |
| orderPrice             | number  | Price specified for the order                                  |
| orderStopPrice         | number  | Stop price, if applicable                                      |
| orderTriggerPrice      | number  | Trigger price for conditional orders                           |
| lastTradedQuantity     | number  | Quantity executed in the last trade                            |
| lastTradedPrice        | number  | Price executed in the last trade                               |
| cumulativeQuantity     | number  | Total quantity executed                                        |
| leavesQuantity         | number  | Quantity remaining to be executed                              |
| tif                    | string  | Time-in-force (GFD, IOC)                                       |
| orderExpiryDate        | integer | Expiry date of the order                                       |
| orderDisclosedQuantity | number  | Disclosed quantity for iceberg orders                          |
| minimumQuantity        | number  | Minimum quantity allowed per execution                         |
| orderGeneratedDateTime | integer | Timestamp when order was generated                             |
| lastRequestDateTime    | integer | Timestamp of the last modification request                     |
| exchangeTransactTime   | integer | Exchange timestamp for the transaction                         |
| orderModificationCount | number  | Number of modifications made                                   |
| orderTradeCount        | number  | Number of trades executed                                      |
| averageTradedPrice     | number  | Average price for executed trades                              |
| averageTradedValue     | number  | Total value of executed trades                                 |
| isFictiveOrder         | boolean | True if the order is fictive                                   |
| rejectType             | string  | Type of rejection, if any                                      |
| rejectTypeReason       | string  | Reason for rejection                                           |
| orderTag               | string  | Optional tag for order categorization                          |
| algoId                 | string  | Algorithm ID if placed via algo                                |
| algoCategoryId         | string  | Algorithm category ID                                          |
| clearingFirmId         | string  | Clearing firm ID                                               |
| panId                  | string  | PAN ID of the client                                           |
| isOrderCompleted       | boolean | True if order is completed                                     |
| userText               | string  | Optional user notes                                            |
| executionType          | string  | Execution type (New, PartialFill, Fill, Cancelled)             |
| strategyTag            | string  | Tag assigned to the strategy                                   |
| sequenceNumber         | number  | Sequence number of the order in the system                     |
| ctclId                 | string  | CTCL terminal ID                                               |
| correlationOrderId     | string  | Correlation ID linked to the order                             |
| productType            | string  | Product type for the order (MIS, NRML, CNC)                    |
| sourceResponse         | object  | Raw response received from the source broker                   |

## 3. Place an Order {#place-order}

This endpoint allows you to submit a new order to the OMS trading system.
Orders can be placed across supported instruments and exchanges, with full control over price, quantity, type, and validity.
When an order is submitted, the system validates it for trading session availability, risk checks, and sufficient margin before routing it for execution.

> Note:Successful API submission does not guarantee execution at the exchange. The response only confirms that the order has been registered with the OMS system.

```bash
curl 'http://uat.quantxpress.com/v1/api/order/placeOrder' \
  -H 'accept: */*' \
  -H 'Content-Type: application/json' \
  -d 'Request JSON'

```

#### Request Body

```json
{
  "quantity": 0,
  "product": "NONE",
  "tif": "None",
  "price": 0,
  "orderType": "Unknown",
  "instrumentId": 0,
  "orderSide": "None",
  "disclosedQuantity": 0,
  "stopPrice": 0,
  "clientId": "string",
  "tiF_GTD_Date": "string",
  "positionEffectOrderFlag": true,
  "exchangeTradingSessionOrderFlag": true,
  "isFictive": true,
  "correlationOrderId": "string"
}
```

> **Note**

    - Either `instrumentId` **or** `symbol` must be provided in the request.
        If `symbol` is sent, the system will automatically fetch the corresponding `instrumentId`.
        If `instrumentId` is sent, the system will verify its validity before placing the order.
    - If both are sent, `instrumentId` will take priority.

#### Response Body

```json
{
  "status": "success",
  "message": "Request processed successfully",
  "data": {
    "blitzOrderId": 23071859690000006
  }
}
```

#### Field Descriptions

| Field                           | Type    | Description                                                                      |
| ------------------------------- | ------- | -------------------------------------------------------------------------------- |
| quantity                        | number  | Quantity of the order to be placed                                               |
| product                         | string  | Product type for the order: MIS, NRML, CNC                                       |
| tif                             | string  | Time-in-force for the order: GFD, IOC                                            |
| price                           | number  | Price at which the order should be executed                                      |
| orderType                       | string  | Type of order: MARKET, LIMIT, SL, SL-M                                           |
| instrumentId                    | integer | Internal ID of the instrument to trade                                           |
| orderSide                       | string  | Side of the order: Buy or Sell                                                   |
| disclosedQuantity               | number  | Quantity disclosed for iceberg orders                                            |
| stopPrice                       | number  | Stop price for stop or stop-limit orders                                         |
| clientId                        | string  | ID of the client placing the order                                               |
| tiF_GTD_Date                    | string  | Expiry date for GTD orders in YYYY-MM-DD format                                  |
| positionEffectOrderFlag         | boolean | Flag indicating whether this order opens or closes a position                    |
| exchangeTradingSessionOrderFlag | boolean | Flag specifying the exchange trading session (Pre-open, Normal, Pre-close, etc.) |
| isFictive                       | boolean | True if the order is simulated/fictive (not a real order)                        |
| correlationOrderId              | string  | Correlation ID for tracking the order                                            |

#### Response Codes

| HTTP Code | Description                                                                                        |
| --------- | -------------------------------------------------------------------------------------------------- |
| **200**   | **Order successfully placed.** The order has been accepted and registered in the OMS system.       |
| **400**   | **Bad Request.** One or more parameters are invalid or missing in the request body.                |
| **401**   | **Unauthorized.** The request is missing a valid JWT token or the token has expired.               |
| **403**   | **Forbidden.** The order cannot be placed due to insufficient margin, risk checks, or permissions. |
| **404**   | **Not Found.** The specified instrument or client ID does not exist.                               |
| **500**   | **Internal Server Error.** A system or exchange-level error occurred while processing the order.   |

> Tip for Placing Orders
>
> - Always confirm the **order status** after placement using the returned `orderId` or `exchangeOrderId`.
> - Ensure **sufficient margin/funds** are available before placing the order to prevent rejections.
> - Place orders **only during active market hours** for faster acknowledgment and execution.

## 4. Modify Order {#modify-order}

The **Modify Order** endpoint allows you to update an existing order’s parameters such as **quantity**, **price**, **stop price**, or **validity**.
Use this API to adjust open or pending orders before they are executed at the exchange.

> Note

    Only orders in *open* or *pending* states can be modified.
    > Once an order is executed or cancelled, modification requests will be rejected.
    - After modification, query the [orders](#fetching-orders) to confirm that the changes were successfully applied.

---

```bash
curl 'http://uat.quantxpress.com/v1/api/order/modifyOrder' \
  -H 'accept: */*' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer YOUR_JWT_TOKEN' \
  -d '{
    "modifiedOrderQuantity": 2,
    "price": 0,
    "exchangeOrderId": "224151307880000035",
    "orderType": "Market",
    "instrumentId": 110010000014366,
    "strategyInstanceId": "88c11710-41f7-4304-ba81-bb92511cdf42",
    "disclosedQuantity": 0,
    "stopPrice": 0,
    "tif": "GFD",
    "tiF_GTD_Date": ""
  }'
```

#### Field Descriptions

| Field                 | Type    | Description                                                          |
| --------------------- | ------- | -------------------------------------------------------------------- |
| modifiedOrderQuantity | number  | Updated quantity of the order                                        |
| price                 | number  | Updated price at which the order should be executed                  |
| exchangeOrderId       | string  | Exchange-assigned order ID identifying which order to modify         |
| orderType             | string  | Type of order: Market, Limit, SL, SL-M                               |
| instrumentId          | integer | Internal ID of the instrument being traded                           |
| strategyInstanceId    | string  | Unique identifier of the strategy instance associated with the order |
| disclosedQuantity     | number  | Updated disclosed quantity for the order                             |
| stopPrice             | number  | Updated stop price for stop or stop-limit orders                     |
| tif                   | string  | Time-in-force for the order: GFD, IOC                                |
| tiF_GTD_Date          | string  | Expiry date for GTD orders in `YYYY-MM-DD` format                    |

#### Response Body

```json
{
  "status": "success",
  "message": "Request processed successfully",
  "data": {
    "blitzOrderId": 23071859690000006
  }
}
```

#### Response Codes

| HTTP Code | Description                                                                                             |
| --------- | ------------------------------------------------------------------------------------------------------- |
| **200**   | **Order modified successfully.** The order update has been accepted by the Blitz system.                |
| **400**   | **Bad Request.** One or more parameters are invalid or missing.                                         |
| **401**   | **Unauthorized.** Missing or invalid JWT token.                                                         |
| **403**   | **Forbidden.** The order cannot be modified (already executed or cancelled).                            |
| **404**   | **Not Found.** The specified `exchangeOrderId` or instrument does not exist.                            |
| **500**   | **Internal Server Error.** A system or exchange-level error occurred while processing the modification. |

> Tip for Modifying Orders

- Always verify the **current order status** before attempting to modify it. Only open or pending orders can be changed.
- Modify orders **during market hours** to ensure updates are accepted and reflected quickly.
- Avoid frequent or large modifications in short intervals to prevent exchange-level throttling or rejection.
- Confirm the **modified parameters** (price, quantity, stop price) meet exchange and instrument-specific limits.

## 5. Cancel Order {#cancel-order}

The **Cancel Order** endpoint allows you to cancel an existing open or pending order using its unique `InstrumentId` and `ExchangeOrderId`.

> Note

- Only orders that are in _open_ or _pending_ state can be cancelled.
- Orders that are already executed or expired cannot be cancelled.

#### Parameters

| Name            | Type    | Description                                                  |
| --------------- | ------- | ------------------------------------------------------------ |
| InstrumentId    | integer | Internal ID of the instrument for which the order was placed |
| ExchangeOrderId | number  | Exchange-assigned ID of the order to cancel                  |

```bash
curl 'http://uat.quantxpress.com/v1/api/order/cancelOrder?InstrumentId=123&ExchangeOrderId=456' \
  -H 'accept: */*' \
  -H 'Authorization: Bearer YOUR_JWT_TOKEN'
```

#### Response

```json
{
  "Message": "Order cancelled successfully."
}
```

#### Response Codes

| HTTP Code | Description                                                                                             |
| --------- | ------------------------------------------------------------------------------------------------------- |
| **200**   | **Order cancelled successfully.** The order has been removed from the OMS system.                       |
| **400**   | **Bad Request.** Invalid or missing parameters.                                                         |
| **401**   | **Unauthorized.** Missing or invalid JWT token.                                                         |
| **403**   | **Forbidden.** The order cannot be cancelled (already executed or expired).                             |
| **404**   | **Not Found.** The specified `ExchangeOrderId` or instrument does not exist.                            |
| **500**   | **Internal Server Error.** A system or exchange-level error occurred while processing the cancellation. |

> Tip for Cancelling Orders

- Confirm that the order is in an **open or pending** state before attempting cancellation. Executed or expired orders cannot be cancelled.
- Use the correct combination of **InstrumentId** and **ExchangeOrderId** to ensure the intended order is cancelled.
- Always verify the **cancellation status** using the order status or history endpoint after sending the request.
- Avoid sending multiple cancellation requests for the same order to prevent duplicate or conflicting actions.

> **Note:** Ensure the JWT token is valid and included in the `Authorization` header. Without it, the server will respond with `401 Unauthorized`.
