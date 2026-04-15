# **Positions API**

The **Positions API** allows you to retrieve all current positions held by the client across different instruments.

Using this API, you can track your **net positions, quantities, average prices, and profit/loss**, enabling better portfolio and risk management.

---

## Base URL
`http://uat.quantxpress.com/api/positions`


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
  'http://uat.quantxpress.com/api/positions' \
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
    
  ]
}
```