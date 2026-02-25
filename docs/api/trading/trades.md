The **Trades API** lets you retrieve all trade details executed under a given entity, strategy, or client.
Using this API, you can monitor executed orders, pending trades, and their execution details in real time.

These endpoints let you **retrieve, filter, and monitor trade executions** efficiently.

| **Type** | **Endpoint**                 | **Description**                  |
| -------- | ---------------------------- | -------------------------------- |
| GET      | [`trades`](#fetching-trades) | Retrieve the list of all trades. |

---

## **Trades** {#fetching-trades}

Retrieve all trades currently recorded for a given client, entity, or strategy instance.

```bash
curl -X 'GET' \
  'http://uat.quantxpress.com/api/order/trades' \
  -H 'accept: */*'
```

#### Response

```json
{
  "status": "success",
  "message": "request processed successfully",
  "data": [
    {
      "id": 27,
      "entityId": "9b4f9a76-f619-41a0-85bf-0d9ecff213c9",
      "strategyId": "36250ee3-ad08-4e09-862f-83deee8cdef9",
      "strategyInstanceId": "498cc797-3908-452a-91be-48157ebb9e71",
      "strategyInstanceName": "NSECM|IDEA",
      "strategyName": "Manual Trading",
      "ivObjectName": "IDEA",
      "instrumentId": 110010000014366,
      "exchangeSegment": "NSECM",
      "exchangeInstrumentId": 110010000014366,
      "instrumentName": "IDEA",
      "instrumentType": 1,
      "blitzOrderId": 225121947920000011,
      "exchangeOrderId": "3204124724133908",
      "executionId": "3204124724134090",
      "account": "",
      "clientId": "PRASHANT",
      "orderType": "Limit",
      "orderSide": "Buy",
      "orderStatus": "Filled",
      "orderQuantity": 1,
      "orderPrice": 11.2,
      "orderStopPrice": 0,
      "orderTriggerPrice": 0,
      "lastTradedQuantity": 1,
      "lastTradedPrice": 11.2,
      "cumulativeQuantity": 1,
      "leavesQuantity": 0,
      "tif": "GFD",
      "orderExpiryDate": 1772002820466,
      "orderDisclosedQuantity": 0,
      "minimumQuantity": 0,
      "orderGeneratedDateTime": 1772002820466,
      "lastRequestDateTime": 1772002820466,
      "exchangeTransactTime": 1772002820000,
      "orderModificationCount": 0,
      "orderTradeCount": 0,
      "averageTradedPrice": 11.2,
      "averageTradedValue": 0,
      "isFictiveOrder": false,
      "rejectType": "None",
      "rejectTypeReason": "",
      "orderTag": "NormalOrder",
      "ctclId": "123456781234567",
      "algoId": "12345",
      "algoCategoryId": "5",
      "clearingFirmId": "",
      "panId": "AAAAC1234A",
      "isOrderCompleted": true,
      "userText": "",
      "executionType": "Fill",
      "strategyTag": "General",
      "sequenceNumber": 84,
      "correlationOrderId": "225121947920000011",
      "productType": "MIS",
      "sourceResponse": null
    },
    {
      "id": 26,
      "entityId": "9b4f9a76-f619-41a0-85bf-0d9ecff213c9",
      "strategyId": "36250ee3-ad08-4e09-862f-83deee8cdef9",
      "strategyInstanceId": "498cc797-3908-452a-91be-48157ebb9e71",
      "strategyInstanceName": "NSECM|IDEA",
      "strategyName": "Manual Trading",
      "ivObjectName": "IDEA",
      "instrumentId": 110010000014366,
      "exchangeSegment": "NSECM",
      "exchangeInstrumentId": 110010000014366,
      "instrumentName": "IDEA",
      "instrumentType": 1,
      "blitzOrderId": 225121947920000010,
      "exchangeOrderId": "3204124724133906",
      "executionId": "3204124724134088",
      "account": "",
      "clientId": "PRASHANT",
      "orderType": "Limit",
      "orderSide": "Buy",
      "orderStatus": "Filled",
      "orderQuantity": 1,
      "orderPrice": 11.49,
      "orderStopPrice": 0,
      "orderTriggerPrice": 0,
      "lastTradedQuantity": 1,
      "lastTradedPrice": 11.49,
      "cumulativeQuantity": 1,
      "leavesQuantity": 0,
      "tif": "GFD",
      "orderExpiryDate": 1772002647910,
      "orderDisclosedQuantity": 0,
      "minimumQuantity": 0,
      "orderGeneratedDateTime": 1772002647910,
      "lastRequestDateTime": 1772002647910,
      "exchangeTransactTime": 1772002647000,
      "orderModificationCount": 0,
      "orderTradeCount": 0,
      "averageTradedPrice": 11.49,
      "averageTradedValue": 0,
      "isFictiveOrder": false,
      "rejectType": "None",
      "rejectTypeReason": "",
      "orderTag": "NormalOrder",
      "ctclId": "123456781234567",
      "algoId": "12345",
      "algoCategoryId": "5",
      "clearingFirmId": "",
      "panId": "AAAAC1234A",
      "isOrderCompleted": true,
      "userText": "",
      "executionType": "Fill",
      "strategyTag": "General",
      "sequenceNumber": 81,
      "correlationOrderId": "225121947920000010",
      "productType": "MIS",
      "sourceResponse": null
    }
  ]
}
```

### Field Descriptions (Order Response)

| Field                  | Type        | Description                                                      |
| ---------------------- | ----------- | ---------------------------------------------------------------- |
| id                     | integer     | Internal unique ID of the order                                  |
| entityId               | string      | Unique identifier of the entity (user/account) placing the order |
| strategyId             | string      | Strategy ID associated with the order                            |
| strategyInstanceId     | string      | Unique ID of the strategy instance executing the order           |
| strategyInstanceName   | string      | Name of the strategy instance                                    |
| strategyName           | string      | Name of the strategy used for trading                            |
| ivObjectName           | string      | Name of the IV object or instrument reference                    |
| instrumentId           | integer     | Internal identifier of the instrument being traded               |
| exchangeSegment        | string      | Exchange segment where the order is placed (e.g., NSECM, NSEFO)  |
| exchangeInstrumentId   | integer     | Instrument ID used by the exchange                               |
| instrumentName         | string      | Symbol or name of the trading instrument                         |
| instrumentType         | integer     | Type of instrument (Equity, Futures, Options, etc.)              |
| blitzOrderId           | integer     | Order ID generated by the OMS (Order Management System)          |
| exchangeOrderId        | string      | Order ID assigned by the exchange                                |
| executionId            | string      | Unique execution ID for the trade                                |
| account                | string      | Account under which the order was placed                         |
| clientId               | string      | Client identifier placing the order                              |
| orderType              | string      | Order type (Limit, Market, SL-M, SL)                             |
| orderSide              | string      | Direction of order: Buy or Sell                                  |
| orderStatus            | string      | Current status of the order (New, Filled, Cancelled, etc.)       |
| orderQuantity          | number      | Total quantity requested in the order                            |
| orderPrice             | number      | Price specified for the order                                    |
| orderStopPrice         | number      | Stop price for stop orders                                       |
| orderTriggerPrice      | number      | Trigger price for conditional orders                             |
| lastTradedQuantity     | number      | Quantity executed in the most recent trade                       |
| lastTradedPrice        | number      | Price at which last trade was executed                           |
| cumulativeQuantity     | number      | Total quantity executed so far                                   |
| leavesQuantity         | number      | Remaining quantity yet to be executed                            |
| tif                    | string      | Time-in-force instruction (GFD, IOC, etc.)                       |
| orderExpiryDate        | integer     | Expiry timestamp of the order                                    |
| orderDisclosedQuantity | number      | Quantity disclosed to the market                                 |
| minimumQuantity        | number      | Minimum quantity allowed per execution                           |
| orderGeneratedDateTime | integer     | Timestamp when the order was created                             |
| lastRequestDateTime    | integer     | Timestamp of the last order modification request                 |
| exchangeTransactTime   | integer     | Timestamp of exchange transaction                                |
| orderModificationCount | number      | Number of times the order was modified                           |
| orderTradeCount        | number      | Number of trades executed for the order                          |
| averageTradedPrice     | number      | Average price of executed trades                                 |
| averageTradedValue     | number      | Total value of executed trades                                   |
| isFictiveOrder         | boolean     | Indicates whether the order is simulated                         |
| rejectType             | string      | Type of rejection if order failed                                |
| rejectTypeReason       | string      | Reason for rejection                                             |
| orderTag               | string      | Optional tag assigned to the order                               |
| ctclId                 | string      | CTCL (Computer to Computer Link) ID used for trading             |
| algoId                 | string      | Algorithm ID if order placed via algorithm                       |
| algoCategoryId         | string      | Category ID of the trading algorithm                             |
| clearingFirmId         | string      | Clearing firm identifier                                         |
| panId                  | string      | PAN ID of the client                                             |
| isOrderCompleted       | boolean     | Indicates whether the order lifecycle is completed               |
| userText               | string      | Optional user-provided notes                                     |
| executionType          | string      | Execution event type (New, PartialFill, Fill, Cancelled, etc.)   |
| strategyTag            | string      | Tag associated with the strategy                                 |
| sequenceNumber         | number      | Sequence number for order tracking                               |
| correlationOrderId     | string      | Correlation ID used to track related orders                      |
| productType            | string      | Product type (MIS, CNC, NRML, etc.)                              |
| sourceResponse         | object/null | Raw response from source system if available                     |
