# **Welcome to the BlitzTrader Ecosystem**

The **BlitzTrader API** ecosystem provides all the tools you need. Our platform is designed to be user-friendly and easy to integrate.

The ecosystem is built around three core pillars, each handling a crucial part of the trading lifecycle:

| Component | What it does | Who it's for |
| --- | --- | --- |
| **Blitz API** | Direct REST-based endpoints for trading and market data. | Users who want full control over their HTTP requests. |
| **Blitz SDK** | Pre-built libraries to quickly connect to Blitz without writing boilerplate code. | Users who want to build features fast in their preferred language. |
| **TPOMS** | A built-in dashboard feature where you can configure and manage your external broker connections. | Any user who needs to link their trading account to external brokers. |

---

## **1. Blitz API: Direct Control**

The **Blitz API** gives you direct, unmediated access to our platform via standard HTTP requests. Everything you can do on a trading terminal, you can do through our API.

### What can you build with it?
- **Order Management:** Place, modify, cancel, and track orders in real-time.
- **Portfolio Tracking:** Retrieve your daily trades, open orders, and current positions.
- **Market Data:** Access live streaming quotes or pull historical data for backtesting.

### Why use the API?
- **Universally Compatible:** Since it uses JSON over REST, you can connect to it using any programming language or tool (like Postman or curl).
- **Predictable:** Uses standard HTTP methods (GET, POST) and clear status codes.

➡️ *Dive into the **[API Reference](api/API_Structure.md)** for detailed endpoint documentation.*

---

## **2. Blitz SDK: Build Faster**

Don't want to handle the low-level HTTP networking yourself? The **Blitz SDK** provides ready-to-use client libraries that wrap the API for you.

### How the SDK makes your life easier:
- **Automatic Authentication:** Handles login and token management automatically.
- **Built-in Error Handling:** Parses errors cleanly so your app doesn't crash unexpectedly.
- **Data Formatting:** Automatically formats your requests and responses into native objects.

Instead of writing networking code, you can focus entirely on your trading strategy and business logic.

➡️ *Check out the **[Blitz SDK](sdk/about_sdk.md)** section to get started.*

---

## **3. TPOMS: Centralized Broker Management**

**TPOMS** stands for *Third-Party Order Management System*. It is a built-in feature available directly from the Blitz UI that allows you to connect and manage your external market brokers (like Zerodha, Motilal, etc.) in one place.
 

➡️ *Learn more about broker integrations in the **[TPOMS Overview](tpoms/overview.md)**.*

---

## **Quickstart: Your First Trade**

Ready to see it in action? Here is a simple three-step process to place your first order using raw HTTP requests.

### Step 1: Generate an Access Token

Before you can interact with the system, you must log in to verify your identity. This request returns a secure token that you must include in all future requests.

```bash
curl -X 'POST' \
  'http://uat.quantxpress.com/interactive/v1/api/v1/authentication/login' \
  -H 'accept: */*' \
  -H 'Content-Type: application/json' \
  -d '{
  "userId": "string",
  "password": "string"
}'
```

### Step 2: Place an Order

Using the access token you just received (replace `{access_token}`), you can place an order. The following payload contains the required fields to route your order to the exchange. *(Make sure to replace placeholder values with actual trading parameters when testing).*

```bash
curl -X 'POST' \
  'http://uat.quantxpress.com/interactive/v1/api/v1/order/placeOrder' \
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

```json
{
  "status": "success",
  "message": "request processed successfully",
  "data": {
    "blitzOrderId": 225121947920000028,
    "correlationOrderId": "225121947920000028"
  }
}
```

### Step 3: Check Order Status

Once your order is placed, you can fetch its current status to see if it was executed, pending, or rejected by the broker.

```bash
curl -X 'GET' \
  'http://uat.quantxpress.com/interactive/v1/api/v1/order' \
  -H 'Content-Type: application/json' \
  -H 'Accept: */*' \
  -H 'Authorization: Bearer {access_token}'
```

!!! tip "Where to go next?"
    If you want to build direct integrations, start with the **[API Reference](api/API_Structure.md)**. If you want a smoother development experience, head over to the **[Blitz SDK](sdk/about_sdk.md)**.