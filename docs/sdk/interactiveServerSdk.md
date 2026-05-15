# Blitz API Client 

This document explains how to use the `BlitzAPIClient` for order management, trade monitoring, positions, statistics, and signal handling.

---

# Initialize Client

```python
import logging

# Configure logging
logging.basicConfig(level=logging.INFO)

from BLITZAPI.blitz_api_client import BlitzAPIClient

client = BlitzAPIClient(
    app_key="YOUR_APP_KEY",
    user_id="YOUR_USER_ID"
)
```

## Parameters

| Parameter | Description |
|----------|-------------|
| `app_key` | Application key provided by the platform |
| `user_id` | User identifier |

---

# Get Orders

Retrieves the complete order book for the authenticated user account, including open, completed, cancelled, rejected, and partially executed orders along with their execution details and current statuses.

## Example

## Example

```python
order_details = client.get_orders()

print("Order Details Response:", order_details)
```

---
# Get Open Orders

Retrieves all currently active or pending orders for the authenticated user account.

This API returns only orders that are still open at the exchange and waiting for execution, modification, or cancellation. Completed, rejected, or cancelled orders are not included in the response.

Open orders are useful for:

- Monitoring pending executions
- Managing active positions
- Tracking unfilled quantities
- Modifying or cancelling existing orders
- Real-time strategy order management

## Example

```python
open_order_details = client.get_open_orders()

print("Open Order Details Response:", open_order_details)
```

---

# Get Order By Blitz Order ID

Retrieves detailed information for a specific order using the unique Blitz Order ID.

This API returns complete order information including order status, exchange details, quantity, price, execution status, timestamps, and rejection details (if applicable) for the requested order.

Fetching orders by Blitz Order ID is useful for:

- Tracking a specific order
- Verifying order execution status
- Monitoring strategy-generated orders
- Fetching detailed order lifecycle information
- Debugging rejected or failed orders

## Example

```python
blitz_order_id = 226090853560000052

order_details = client.get_order_by_blitz_id(blitz_order_id)

print("Order Details Response:", order_details)
```

---

# Place Order

Creates and submits a new trading order to the exchange for the authenticated user account.

This API is used to place buy or sell orders across supported exchanges and product types. The order request can include details such as trading symbol, quantity, order type, price, product type, validity, and transaction type.

After successful submission, the API returns order confirmation details including the generated order ID and current order status.

This API supports various order workflows such as:

- Market orders
- Limit orders
- Stop-loss orders
- Intraday trading
- Delivery trading
- Strategy-based order execution

Placed orders are processed by the exchange and may move through different statuses such as open, complete, rejected, cancelled, or partially filled.

## Example

```python
order_data = {
    "correlationOrderId": "corr_123",
    "quantity": 1,
    "product": "MIS",
    "tif": "GTD",
    "price": 10,
    "orderType": "LIMIT",
    "symbol": "NSECM|IDEA",
    "orderSide": "BUY",
    "disclosedQuantity": 0,
    "stopPrice": 0,
    "clientId": "Algo123",
    "tiF_GTD_Date": "2025-10-10"
}

order_resp = client.place_order(order_data)

print("Place Order Response:", order_resp)
```

---

## Order Parameters

| Parameter | Description |
|----------|-------------|
| `correlationOrderId` | Unique order identifier |
| `quantity` | Order quantity |
| `product` | Product type (MIS/CNC/etc.) |
| `tif` | Time in force |
| `price` | Order price |
| `orderType` | LIMIT / MARKET |
| `symbol` | Trading symbol |
| `orderSide` | BUY / SELL |
| `disclosedQuantity` | Disclosed quantity |
| `stopPrice` | Stop loss trigger price |
| `clientId` | Client identifier |
| `tiF_GTD_Date` | GTD expiry date |

---
# Modify Order

Modifies an existing open or pending order for the authenticated user account.

This API allows updating specific order parameters such as quantity, price, trigger price, or validity before the order is fully executed or cancelled at the exchange.

Only orders that are currently open or pending can be modified. Completed, rejected, or cancelled orders cannot be updated.

Order modification is commonly used for:

- Changing limit prices
- Updating order quantities
- Revising stop-loss trigger values
- Adjusting strategy-generated orders
- Managing pending exchange orders in real time

After successful modification, the exchange processes the updated order request and returns the latest order status and details.

## Example

```python
modify_data = {
    "modifiedOrderQuantity": 10,
    "price": 10.8,
    "blitzOrderId": 212105238510000007,
    "orderType": "LIMIT",
    "symbol": "NSECM|IDEA",
    "disclosedQuantity": 0,
    "stopPrice": 0,
    "tif": "GFD",
    "tiF_GTD_Date": "2025-10-10"
}

modify_resp = client.modify_order(modify_data)

print("Modify Order Response:", modify_resp)
```

---

# Cancel Order

Cancels an existing open or pending order for the authenticated user account.

This API is used to stop an active order from further execution at the exchange. Only orders that are currently open or partially filled can be cancelled. Completed, rejected, or already cancelled orders cannot be cancelled again.

Order cancellation is commonly used for:

- Removing pending orders from the market
- Managing risk exposure
- Stopping unintended executions
- Updating trading strategies dynamically
- Handling unfilled or partially filled orders

After a successful cancellation request, the exchange updates the order status to `CANCELLED` if the request is accepted and processed successfully.

## Example

```python
cancel_resp = client.cancel_order(
    instrument="NSECM|IDEA",
    blitzOrderId=212105238510000007
)

print("Cancel Order Response:", cancel_resp)
```

---

# Get Trades

Retrieves all executed trade details for the authenticated user account.

This API returns trade-level execution information generated from completed or partially completed orders. Each trade represents an actual execution that occurred at the exchange.

The response typically includes details such as:

- Trade ID
- Order ID
- Trading symbol
- Exchange
- Buy/Sell side
- Executed quantity
- Execution price
- Product type
- Execution timestamp
- Brokerage and charges (if applicable)

This API is useful for:

- Monitoring executed trades
- Calculating realized profit and loss
- Trade reconciliation
- Strategy execution tracking
- Building trade history dashboards and reports

Only successfully executed trades are returned by this API.

## Example

```python
trades = client.get_trades()

print("Trades Response:", trades)
```

---

# Get Positions

Retrieves all current trading positions for the authenticated user account.

This API returns consolidated position details across all executed trades and active holdings for the current trading session. Positions represent the net buy and sell quantities for each traded instrument.

The response typically includes details such as:

- Trading symbol
- Exchange
- Product type
- Net quantity
- Buy quantity
- Sell quantity
- Average buy price
- Average sell price
- Realized profit and loss
- Unrealized profit and loss
- Day positions
- Overnight positions

This API is useful for:

- Monitoring live positions
- Tracking portfolio exposure
- Calculating profit and loss
- Risk management
- Strategy position tracking
- Building portfolio dashboards

Positions are dynamically updated based on executed trades and exchange updates during market hours.

## Example

```python
positions = client.get_positions()

print("Positions Response:", positions)
```

---
# Get Statistics

Retrieves trading and account-related statistical information for the authenticated user account.

This API provides summarized trading metrics and performance-related data that can be used for monitoring trading activity, strategy analysis, and account performance evaluation.

The response may include details such as:

- Total orders placed
- Total executed trades
- Win/loss statistics
- Profit and loss summary
- Trade volume
- Strategy performance metrics
- Execution statistics
- Day-wise or session-wise summaries

This API is useful for:

- Monitoring trading performance
- Building analytics dashboards
- Strategy evaluation
- Performance reporting
- Risk and exposure analysis
- Generating trading summaries

Statistical values are calculated based on available trading and execution data for the authenticated account.

## Example

```python
statistics = client.get_statistics()

print("Statistics Response:", statistics)
```

---

# Send Signals

Sends trading or strategy signals from the source strategy to one or more destination strategies within the QuantXpress platform.

This API is used to trigger strategy actions such as buy, sell, exit, or custom trading events and distribute them to connected destination strategies or execution engines.

Signals can be used for:

- Strategy-to-strategy communication
- Automated trade execution
- Copy trading workflows
- Multi-account signal distribution
- Event-driven trading systems
- Centralized strategy management

The API allows strategies to exchange execution instructions and synchronize trading actions across multiple connected systems in real time.

The response typically includes signal processing status, delivery confirmation, and execution acknowledgement details.

## Example

```python
signals_payload = [
    {
        "sourceStrategy": "StrategyA",
        "destinationStrategy": "Matrix",
        "sourceSID": "SID123",
        "instanceRunningMode": "Start",
        "globalAction": "signal",
        "instruments": [
            {
                "exchangeSegment": "NSE",
                "instrumentName": "NIFTY10FEB2625900CE",
                "action": "EnterShort",
                "lot": "1",
                "timeStamp": "2025-10-23T14:30:00Z",
                "infoText": "Test order"
            }
        ]
    }
]

result = client.send_signals(signals_payload)

print(result)
```

---

# Signal Payload Parameters

| Parameter | Description |
|----------|-------------|
| `sourceStrategy` | Source strategy name |
| `destinationStrategy` | Destination strategy name |
| `sourceSID` | Strategy instance identifier |
| `instanceRunningMode` | Strategy running mode |
| `globalAction` | Signal action |
| `exchangeSegment` | Exchange segment |
| `instrumentName` | Instrument symbol |
| `action` | Trading action |
| `lot` | Quantity/lots |
| `timeStamp` | Signal timestamp |
| `infoText` | Additional information |

---

# Complete Example

```python
import logging

logging.basicConfig(level=logging.INFO)

from BLITZAPI.blitz_api_client import BlitzAPIClient

# =============================== Initialize Client ===============================

client = BlitzAPIClient(
    app_key="YOUR_APP_KEY",
    user_id="YOUR_USER_ID"
)

# =============================== Get Orders ===============================

order_details = client.get_orders()

print("Order Details Response:", order_details)

# =============================== Place Order ===============================

order_data = {
    "correlationOrderId": "corr_123",
    "quantity": 1,
    "product": "MIS",
    "tif": "GTD",
    "price": 10,
    "orderType": "LIMIT",
    "symbol": "NSECM|IDEA",
    "orderSide": "BUY",
    "disclosedQuantity": 0,
    "stopPrice": 0,
    "clientId": "Algo123",
    "tiF_GTD_Date": "2025-10-10"
}

order_resp = client.place_order(order_data)

print("Place Order Response:", order_resp)

# =============================== Modify Order ===============================

modify_data = {
    "modifiedOrderQuantity": 10,
    "price": 10.8,
    "blitzOrderId": 212105238510000007,
    "orderType": "LIMIT",
    "symbol": "NSECM|IDEA",
    "disclosedQuantity": 0,
    "stopPrice": 0,
    "tif": "GFD",
    "tiF_GTD_Date": "2025-10-10"
}

modify_resp = client.modify_order(modify_data)

print("Modify Order Response:", modify_resp)

# =============================== Cancel Order ===============================

cancel_resp = client.cancel_order(
    instrument="NSECM|IDEA",
    blitzOrderId=212105238510000007
)

print("Cancel Order Response:", cancel_resp)

# =============================== Send Signals ===============================

signals_payload = [
    {
        "sourceStrategy": "StrategyA",
        "destinationStrategy": "Matrix",
        "sourceSID": "SID123",
        "instanceRunningMode": "Start",
        "globalAction": "signal",
        "instruments": [
            {
                "exchangeSegment": "NSE",
                "instrumentName": "NIFTY10FEB2625900CE",
                "action": "EnterShort",
                "lot": "1",
                "timeStamp": "2025-10-23T14:30:00Z",
                "infoText": "Test order"
            }
        ]
    }
]

result = client.send_signals(signals_payload)

print(result)
```