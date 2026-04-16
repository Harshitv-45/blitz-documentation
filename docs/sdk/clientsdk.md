# Client SDK

## **BlitzAPIClient**

```
class BlitzAPIClient
```

The `BlitzAPIClient` class provides access to order management, positions, trades, and strategy operations along with integrated authentication and WebSocket handling.

---


### **`__init__`**

```
def __init__(self, app_key: str, user_id: str)
```

??? info "Source Code"
    ```python
    def __init__(self, app_key: str, user_id: str):
        self.app_key = app_key
        self.user_id = user_id
        self.api_base_url = API_BASE_URL
        self.api_hist_base_url = API_BASE_URL
        self.auth_base_url = AUTH_BASE_URL
        self.token = None 
        self.auth_client = AuthClient(app_key, user_id)
        self.access_token = None

        self._ensure_logged_in()

        self.ws_client = MarketDataWebSocketClient(self.access_token)
        self.ws_client.set_on_connect(self.on_connect)
        self.ws_client.set_on_close(self.on_close)

        self.redis_client = redis.Redis.from_url(REDIS_URL, decode_responses=True)
    ```


Initializes the client SDK with authentication and WebSocket connection.


**Parameters**

|Parameter |	Type|	Required|	Description|
|----------|---------|----------|--------------|
|`app_key`|	`string`|	Yes|	API key provided for authentication|
|`user_id`	|`string`	|Yes	|Unique user/client identifier|


**Example**

```python
client = BlitzAPIClient(app_key="abc123xyz", user_id="U12345")
```

---

## Order Management


### **`get_orders`**

```
def get_orders(self)
```

??? info "Source Code"
    ```python
    def get_orders(self):
        """Fetch all orders."""
        logging.info("Fetching all orders")
        endpoint = "orders"
        return self._send_request(endpoint, method="GET")
    ```

Fetch all orders placed by the client.

**Example**

```python
orders = client.get_orders()
print(orders)

# Example Response:
# {
#   'status': 'success',
#   'message': 'request processed successfully',
#   'data': [
#     {
#       'id': 136,
#       'entityId': '9b4f9a76-f619-41a0-85bf-0d9ecff213c9',
#       'strategyId': '36250ee3-ad08-4e09-862f-83deee8cdef9',
#       'strategyInstanceId': '498cc797-3908-452a-91be-48157ebb9e71',
#       'strategyInstanceName': 'NSECM|IDEA',
#       'strategyName': 'Manual Trading',
#       'ivObjectName': 'IDEA',
#       'instrumentId': 110010000014366,
#       'exchangeSegment': 'NSECM',
#       'exchangeInstrumentId': 110010000014366,
#       'instrumentName': 'IDEA',
#       'instrumentType': 1,
#       'blitzOrderId': 225121947920000006,
#       'exchangeOrderId': '',
#       'clientId': 'PRASHANT',
#       'orderType': 'Limit',
#       'orderSide': 'Buy',
#       'orderStatus': 'PendingNew',
#       'orderQuantity': 1,
#       'orderPrice': 11.51,
#       'tif': 'GFD',
#       'productType': 'MIS',
#       'isFictiveOrder': False,
#       ...
#     }
#   ]
# }
```


### **`get_open_orders`**

```
def get_open_orders(self)
```

??? info "Source Code"
    ```python
    def get_open_orders(self):
        """Fetch all Open Orders"""
        logging.info("Fetching all Open Orders")
        endpoint = "orders/openOrders"
        return self._send_request(endpoint, method="GET")
    ```

Fetch all currently open (active) orders.


**Example**

```python
open_orders = client.get_open_orders()
print(open_orders)

# Example Response:
# {
#   'status': 'success',
#   'message': 'request processed successfully',
#   'data': [
#     {
#       'id': 136,
#       'entityId': '9b4f9a76-f619-41a0-85bf-0d9ecff213c9',
#       'strategyInstanceName': 'NSECM|IDEA',
#       'instrumentName': 'IDEA',
#       'blitzOrderId': 225121947920000006,
#       'exchangeOrderId': '',
#       'clientId': 'PRASHANT',
#       'orderType': 'Limit',
#       'orderSide': 'Buy',
#       'orderStatus': 'PendingNew',
#       'orderQuantity': 1,
#       'orderPrice': 11.51,
#       'leavesQuantity': 1,
#       'tif': 'GFD',
#       'productType': 'MIS',
#       ...
#     }
#   ]
# }
```


### **`get_order_by_blitz_id`**

```
def get_order_by_blitz_id(self, blitz_order_id: int)
```

??? info "Source Code"
    ```python
    def get_order_by_blitz_id(self, blitz_order_id: int):
        """Fetch a single order by BlitzOrderId."""
        logging.info(f"Fetching order for BlitzOrderId={blitz_order_id}")
        endpoint = f"orders/{blitz_order_id}"
        return self._send_request(endpoint, method="GET")
    ```

Fetch details of a specific order using its Blitz Order ID.

**Parameters**

| Parameter      | Type    | Required | Description           |
| -------------- | ------- | -------- | --------------------- |
| blitz_order_id | integer | Yes      | Unique Blitz order ID |


**Example**

```python
order = client.get_order_by_blitz_id(225121947920000011)
print(order)

# Example Response:
# {
#   'status': 'success',
#   'message': 'request processed successfully',
#   'data': [
#     {
#       'id': 157,
#       'entityId': '9b4f9a76-f619-41a0-85bf-0d9ecff213c9',
#       'strategyInstanceName': 'NSECM|IDEA',
#       'instrumentName': 'IDEA',
#       'blitzOrderId': 225121947920000011,
#       'exchangeOrderId': '3204124724133908',
#       'executionId': '3204124724134090',
#       'clientId': 'PRASHANT',
#       'orderType': 'Limit',
#       'orderSide': 'Buy',
#       'orderStatus': 'Filled',
#       'orderQuantity': 1,
#       'orderPrice': 11.2,
#       'lastTradedQuantity': 1,
#       'lastTradedPrice': 11.2,
#       'cumulativeQuantity': 1,
#       'leavesQuantity': 0,
#       'averageTradedPrice': 11.2,
#       'isOrderCompleted': True,
#       'executionType': 'Fill',
#       'productType': 'MIS',
#       ...
#     }
#   ]
# }
```


### **`place_order`**

```
def place_order(self, order_data: dict)
```

??? info "Source Code"
    ```python
    def place_order(self, order_data: dict):
        """Place a new order."""
        logging.info(f"Placing order: {order_data}")
        endpoint = "orders/placeOrder"
        return self._send_request(endpoint, payload=order_data, method="POST")
        ```

Place a new order in the market.

**Parameters**

| Parameter  | Type | Required | Description |
| ---------- | ---- | -------- | ----------- |
| order_data | dict | Yes      | Order payload containing instrument details, quantity, price, and order configuration. |

**order_data Fields**

| Field         | Type    | Required | Description                                                                 |
| ------------- | ------- | -------- | --------------------------------------------------------------------------- |
| instrumentId  | integer | No*      | Unique instrument identifier. Either `instrumentId` or `symbol` is required |
| symbol        | string  | No*      | Trading symbol e.g., NSE|RELIANCE. Either `symbol` or `instrumentId` required |
| quantity      | integer | Yes      | Number of units to trade                                                    |
| price         | number  | No       | Price for limit orders                                                      |
| orderType     | string  | Yes      | Type of order (`MARKET`, `LIMIT`)                                           |
| transactionType | string | Yes     | Buy or Sell (`BUY`, `SELL`)                                                 |
| productType   | string  | Yes      | Product type (`CNC`, `MIS`, etc.)                                           |
| validity      | string  | Yes      | Order validity (`DAY`, `IOC`)                                               |

> **Note:** Either `instrumentId` or `symbol` must be provided.

**Example**

```python
order_data = {
  "correlationOrderId": "corr_123",
  "quantity": 1,
  "product": "MIS",
  "tif": "GTD",
  "price": 10.1,
  "orderType": "LIMIT",
  #"instrumentId": 1010010000010666,
  "symbol": "NSECM|IDEA",
  "orderSide": "BUY",
  "disclosedQuantity": 0,
  "stopPrice": 0,
  "clientId": "ABC",
  "tiF_GTD_Date": "2025-10-10"
}

response = client.place_order(order_data)

# Example Response:
# {
#   'status': 'success',
#   'message': 'request processed successfully',
#   'data': {
#     'blitzOrderId': 225121947920000028,
#     'correlationOrderId': '225121947920000028'
#   }
# }
```


### **`modify_order`**

```
def modify_order(self, order_data: dict)
```

Modify an existing order.


**Parameters**

| Parameter  | Type | Required | Description |
| ---------- | ---- | -------- | ----------- |
| order_data | dict | Yes      | Payload containing updated order details. |

**order_data Fields**

| Field         | Type    | Required | Description                                      |
| ------------- | ------- | -------- | ------------------------------------------------ |
| blitzOrderId  | integer | Yes      | Unique Blitz order ID                            |
| quantity      | integer | No       | Updated quantity                                 |
| price         | number  | No       | Updated price                                    |
| orderType     | string  | No       | Updated order type (`LIMIT`, `MARKET`)           |

> Only fields to be modified need to be passed.



**Example**

```python

modify_data = {
  "modifiedOrderQuantity": 10,
  "price": 10.8,
  "blitzOrderId": 29104141260000005,
  "orderType": "LIMIT",
  #"instrumentId": 1010010000010666,
  "symbol": "NSECM|IDEA",
  "disclosedQuantity": 0,
  "stopPrice": 0,
  "tif": "GFD",
  #"strategyInstanceId": "656abb03-eec9-4cd0-a226-025090b3ff27",
  "tiF_GTD_Date": "2025-10-10"
}
response = client.modify_order(modify_data)

# Example Response:
# {
#   'status': 'success',
#   'message': 'request processed successfully',
#   'data': 'successfully send modify order request to blitz server'
# }
```

### **`cancel_order`**

```
def cancel_order(self, instrument: str, blitzOrderId: int)
```

Cancel an existing order.

**Parameters**

| Parameter    | Type    | Required | Description                                                                 |
| ------------ | ------- | -------- | --------------------------------------------------------------------------- |
| instrument   | string  | Yes      | Instrument identifier. Can be either `instrumentId` (numeric) or `symbol`   |
| blitzOrderId | integer | Yes      | Unique Blitz Order ID                                                       |


**Example**

```python

response = client.cancel_order("NSE|RELIANCE", 12345)

# Example Response:
# {
#   'status': 'success',
#   'message': 'request processed successfully',
#   'data': 'successfully send cancel order request to blitz server'
# }
```


## Position Management


### **`get_positions`**

```
def get_positions(self)
```

Fetch all current positions held by the client.

**Example**

```python
positions = client.get_positions()

# Example Response:
# {
#   'status': 'success',
#   'message': 'request processed successfully',
#   'data': [
#     {
#       'instrumentName': 'IDEA',
#       'exchangeSegment': 'NSECM',
#       'productType': 'MIS',
#       'netQuantity': 1,
#       'buyQuantity': 1,
#       'sellQuantity': 0,
#       'buyValue': 11.51,
#       'sellValue': 0.0,
#       'ltp': 11.2,
#       'pnl': -0.31,
#       ...
#     }
#   ]
# }
```


<!-- ## Statistics Management


### **`get_statistics`**

```
def get_statistics(self)
```

Fetch strategy statistics and performance metrics.


**Example**

```python
stats = client.get_statistics()

# Example Response:
# {
#   'status': 'success',
#   'message': 'request processed successfully',
#   'data': {
#     'totalTrades': 12,
#     'profitableTrades': 7,
#     'lossTrades': 5,
#     'winRate': 58.33,
#     'totalPnl': 1250.75,
#     'realizedPnl': 980.50,
#     'unrealizedPnl': 270.25,
#     'totalTurnover': 125000.0
#   }
# }
```
 -->


## Trade Management


### **`get_trades`**

```
def get_trades(self)
```

Fetch all executed trades.


**Example**

```python

trades = client.get_trades()

# Example Response:
# {
#   'status': 'success',
#   'message': 'request processed successfully',
#   'data': [
#     {
#       'id': 27,
#       'entityId': '9b4f9a76-f619-41a0-85bf-0d9ecff213c9',
#       'instrumentName': 'IDEA',
#       'exchangeSegment': 'NSECM',
#       'blitzOrderId': 225121947920000011,
#       'exchangeOrderId': '3204124724133908',
#       'executionId': '3204124724134090',
#       'clientId': 'PRASHANT',
#       'orderType': 'Limit',
#       'orderSide': 'Buy',
#       'orderStatus': 'Filled',
#       'orderQuantity': 1,
#       'orderPrice': 11.2,
#       'lastTradedQuantity': 1,
#       'lastTradedPrice': 11.2,
#       'averageTradedPrice': 11.2,
#       'executionType': 'Fill',
#       'productType': 'MIS',
#       ...
#     }
#   ]
# }
```


## Signals


### **`send_signals`**

```
def send_signals(self, signals: list)
```


Send trading signals and optionally publish them to Redis.


**Parameters**

| Parameter | Type | Required | Description |
| --------- | ---- | -------- | ----------- |
| signals   | list | Yes      | List of signal objects containing trading instructions. |

### signals Fields

| Field        | Type   | Required | Description                                      |
| ------------ | ------ | -------- | ------------------------------------------------ |
| instrumentId | int    | No*      | Instrument ID                                    |
| symbol       | string | No*      | Trading symbol                                   |
| action       | string | Yes      | Signal action (`BUY`, `SELL`)                    |
| quantity     | int    | Yes      | Quantity to trade                                |
| price        | number | No       | Price (if applicable)                            |

> Either `instrumentId` or `symbol` must be provided.

**Example**

```python
signals = [
    {"symbol": "NSE|RELIANCE", "action": "BUY"}
]

response = client.send_signals(signals)

# Example Response:
# {
#    "status":"success",
#    "message":"request processed successfully.",
#    "data":
#        {"message":"Signal received successfully."}}'
```


## WebSocket Callbacks


### **`on_connect`**

```
def on_connect(self)
```


Callback triggered when WebSocket connection is established.

### **`on_close`**

```
def on_close(self, close_status_code, close_msg)
```

Callback triggered when WebSocket connection is closed.

### **`on_message`**

```
@property def on_message(self)
```

Setter to register callback for receiving real-time tick data.


**Example**

```python
def handle_tick(data):
    print(data)

client.on_message = handle_tick
```