# **Positions API**

The **Positions API** allows you to retrieve all current positions held by the client across different instruments.

Using this API, you can track your **net positions, quantities, average prices, and profit/loss**, enabling better portfolio and risk management.

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

| **Type** | **Endpoint** | **Description**                     |
| -------- | ------------ | ----------------------------------- |
| GET      | `/positions` | Retrieve all current positions      |

---

## Get Positions {#get-positions}

Retrieve all positions held by the client.

This includes:
- Open positions  
- Intraday positions  
- Carry-forward positions  

This endpoint provides a **complete view of your portfolio exposure**.

---

### cURL Example

```bash
curl -X 'GET' \
  'http://uat.quantxpress.com/interactive/v1/api/v1/positions' \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer {access_token}'
```


## Response

### Success Response (200 OK)

```json
{
  "status": "success",
  "message": "request processed successfully",
  "data": [
    {
      "entityId": "string",
      "clientId": "string",
      "rmsEntityCode": "string",
      "instrumentId": 0,
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
      "exposure": 0.0,
      "unrealized": 0.0,
      "realized": 0.0,
      "lastTradedPrice": 0.0,
      "openPositionTradeValue": 0.0,
      "turnoverValue": 0.0,
      "turnoverValueOptions": 0.0
    }
  ]
}
```

### Response Field Descriptions

| Field | Type | Description |
|-------|------|-------------|
| entityId | string | Identifier for the entity |
| clientId | string | Client identifier |
| rmsEntityCode | string | RMS entity code |
| instrumentId | integer | Internal ID of the instrument |
| exchangeSegment | string | Exchange segment (e.g., NSECM, NSEFO) |
| instrumentName | string | Name of the instrument |
| nonAckBuyQuantity | integer | Non-acknowledged buy quantity |
| nonAckSellQuantity | integer | Non-acknowledged sell quantity |
| openBuyQuantity | integer | Total open buy quantity |
| openSellQuantity | integer | Total open sell quantity |
| shortPosition | integer | Total short position |
| longPosition | integer | Total long position |
| netPosition | integer | Net position in the instrument |
| netPositionInLot | integer | Net position measured in lots |
| orderCount | integer | Total order count |
| tradeCount | integer | Total trade count |
| previousNetPosition | integer | Net position from the previous trading session |
| tradeBuyValue | number | Total buy value |
| tradeSellValue | number | Total sell value |
| tradeNetValue | number | Net trade value |
| avgBuy | number | Average buy price |
| avgSell | number | Average sell price |
| avgNet | number | Average net price |
| exposure | number | Total exposure |
| unrealized | number | Unrealized profit/loss |
| realized | number | Realized profit/loss |
| lastTradedPrice | number | Last traded price (LTP) of the instrument |
| openPositionTradeValue | number | Trade value of the open position |
| turnoverValue | number | Total turnover value |
| turnoverValueOptions | number | Total turnover value for options |