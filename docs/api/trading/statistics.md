# **Statistics API**

The **Statistics API** allows you to retrieve performance metrics and analytics related to your trading strategies.

Using this API, you can monitor **profit/loss, trade performance, win/loss ratios, and overall strategy effectiveness**, helping you make data-driven trading decisions.

These endpoints let you **analyze trading performance and optimize strategies**.

---

## Base URL
`http://uat.quantxpress.com/interactive/v1/api/v1`


---

## Headers

| Header        | Value                 | Required |
| ------------- | --------------------- | -------- |
| Content-Type  | application/json      | Yes      |
| Accept        | application/json      | Yes      |
| Authorization | Bearer {access_token} | Yes      |

---

## Endpoint

| **Type** | **Endpoint**         | **Description**                         |
| -------- | -------------------- | --------------------------------------- |
| GET      | `/strategy/statistics` | Retrieve strategy performance statistics |

---

## Get Statistics {#get-statistics}

Retrieve performance statistics for trading strategies.

This endpoint provides insights into:
- Overall performance  
- Profit and loss  
- Trade count and success ratio  
- Strategy efficiency  

---

### cURL Example

```bash
curl -X 'GET' \
  'http://uat.quantxpress.com/interactive/v1/api/v1/strategy/statistics' \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer {access_token}'
```

### Root Fields

| Field   | Type   | Description                                  |
| ------- | ------ | -------------------------------------------- |
| status  | string | Status of the request (`success` or `error`) |
| message | string | Response message                             |
| data    | object | Contains strategy statistics                 |

### Response Field Descriptions

| Field | Type | Description |
|-------|------|-------------|
| entityId | string | Identifier for the entity |
| strategyId | string | Strategy identifier |
| strategyName | string | Name of the strategy |
| strategyInstanceId | string | Identifier for the strategy instance |
| strategyInstanceName | string | Name of the strategy instance |
| exchangeClientId | string | Client ID for the exchange |
| instrumentId | integer | Internal ID of the instrument |
| ivName | string | IV name |
| exchangeSegment | string | Exchange segment |
| instrumentName | string | Name of the instrument |
| nonAckBuyQuantity | integer | Non-acknowledged buy quantity |
| nonAckSellQuantity | integer | Non-acknowledged sell quantity |
| openBuyQuantity | integer | Open buy quantity |
| openSellQuantity | integer | Open sell quantity |
| shortPosition | integer | Short position |
| longPosition | integer | Long position |
| netPosition | integer | Net position |
| netPositionInLot | integer | Net position in lots |
| orderCount | integer | Total order count |
| tradeCount | integer | Total trade count |
| previousNetPosition | integer | Net position from previous session |
| tradeBuyValue | number | Total buy trade value |
| tradeSellValue | number | Total sell trade value |
| tradeNetValue | number | Net trade value |
| avgBuy | number | Average buy price |
| avgSell | number | Average sell price |
| avgNet | number | Average net price |
| opBuyPrice | number | Open buy price |
| opSellPrice | number | Open sell price |
| exposure | number | Exposure |
| unrealized | number | Unrealized profit/loss |
| realized | number | Realized profit/loss |
| lastTradedPrice | number | Last traded price |
| openPositionTradeValue | number | Trade value of open position |
| turnoverValue | number | Turnover value |
| turnoverValueOptions | number | Turnover value for options |

### Response Example

```json
{
  "status": "success",
  "message": "request processed successfully",
  "data": {
    "entityId": "string",
    "strategyId": "string",
    "strategyName": "string",
    "strategyInstanceId": "string",
    "strategyInstanceName": "string",
    "exchangeClientId": "string",
    "instrumentId": 0,
    "ivName": "string",
    "exchangeSegment": "string",
    "instrumentName": "string",
    "nonAckBuyQuantity": 0,
    "nonAckSellQuantity": 0,
    "openBuyQuantity": 0,
    "openSellQuantity": 0,
    "shortPosition": 0,
    "longPosition": 0,
    "netPosition": 0,
    "netPositionInLot": 0,
    "orderCount": 0,
    "tradeCount": 0,
    "previousNetPosition": 0,
    "tradeBuyValue": 0.0,
    "tradeSellValue": 0.0,
    "tradeNetValue": 0.0,
    "avgBuy": 0.0,
    "avgSell": 0.0,
    "avgNet": 0.0,
    "opBuyPrice": 0.0,
    "opSellPrice": 0.0,
    "exposure": 0.0,
    "unrealized": 0.0,
    "realized": 0.0,
    "lastTradedPrice": 0.0,
    "openPositionTradeValue": 0.0,
    "turnoverValue": 0.0,
    "turnoverValueOptions": 0.0
  }
}
```
