# **Statistics API**

The **Statistics API** allows you to retrieve performance metrics and analytics related to your trading strategies.

Using this API, you can monitor **profit/loss, trade performance, win/loss ratios, and overall strategy effectiveness**, helping you make data-driven trading decisions.

These endpoints let you **analyze trading performance and optimize strategies**.

---

## Base URL
`http://uat.quantxpress.com/api/strategy`


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
  'http://uat.quantxpress.com/api/strategy/statistics' \
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
