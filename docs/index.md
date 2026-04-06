# Blitz Documentation

Welcome to **Blitz** — a unified platform for accessing trading APIs, developer SDKs, and broker integrations through a scalable, broker-agnostic architecture.

Blitz enables developers and trading systems to seamlessly:
- Place and manage orders
- Access real-time and historical market data
- Track portfolios and positions
- Integrate multiple brokers using a single, consistent interface

---

## Platform Overview

Blitz is built around three core components, each designed to handle a specific layer of the trading workflow:

| Component     | Description |
|--------------|------------|
| **Blitz API** | REST-based interface for trading operations and market data |
| **Blitz SDK** | Developer-friendly libraries for faster and easier integration |
| **TPOMS**     | Broker communication and order routing layer |

---

## Blitz API

The **Blitz API** provides a comprehensive set of REST endpoints that allow direct interaction with the trading platform.

### Key Capabilities

- Place, modify, and cancel orders
- Retrieve trades, orders, and positions
- Access real-time and historical market data
- Track order execution and status
- Manage portfolio information

### Highlights

- Resource-oriented REST architecture  
- JSON-based request and response format  
- Standard HTTP methods and status codes  

➡️ Refer to the **API Reference** section for detailed endpoint documentation.

---

## Blitz SDK

The **Blitz SDK** offers client libraries that abstract away low-level API complexities, enabling faster and more efficient integration.

### What the SDK Handles

- Authentication and session management  
- Request construction and formatting  
- API communication  
- Response parsing  
- Error handling  

This allows developers to focus on building features rather than managing infrastructure.

➡️ Refer to the **SDK Documentation** to get started.

---

## TPOMS

**TPOMS (Third-Party Order Management System)** acts as the broker integration layer within Blitz.

It bridges the gap between Blitz and external broker systems by:

- Translating Blitz requests into broker-specific formats  
- Normalizing broker responses  
- Managing the full order lifecycle  
- Supporting multiple brokers through adapter-based architecture  

This ensures seamless multi-broker support through a unified interface.

➡️ Refer to the **TPOMS Documentation** for architecture and integration details.

---

## Quickstart

### 1. Generate Access Token
Authenticate with Blitz to obtain your access token.
```bash
curl -X 'POST' \
  'http://uat.quantxpress.com/v1/api/authentication/login' \
  -H 'accept: */*' \
  -H 'Content-Type: application/json' \
  -d '{
  "userId": "string",
  "password": "string"
}'
```

### 2. Place Your First Order

```bash
curl -X 'POST' \
  'http://uat.quantxpress.com/api/order/placeOrder' \
  -H 'Content-Type: application/json' \
  -H 'Accept: */*' \
  -H 'Authorization: Bearer {access_token}' \
  -d '{
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
}'

```

### 3. Check Order Status
```bash
curl -X 'GET' \
  'http://uat.quantxpress.com/v1/api/order' \
  -H 'Content-Type: application/json' \
  -H 'Accept: */*' \
  -H 'Authorization: Bearer {access_token}' \
```

!!! tip
    New to Blitz? Start with the **API Reference** for direct integration, or use the **SDK** for a faster setup.

---