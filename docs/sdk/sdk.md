# **Blitz** module

Blitz SDK for Python.

```python
pip install blitzSDK
```

## **The library**

Blitz provides a comprehensive set of APIs for building trading and investment applications. With Blitz, you can place orders instantly, monitor portfolios, access live market data through WebSockets, and leverage a variety of trading features using a simple, unified HTTP interface.

## **Getting started**

    from blitzSDK import AuthClient, MarketDataClient, BlitzAPIClient

    # Step 1: Authenticate
    auth_client = AuthClient(app_key="your_app_key_here", user_id="USER123")

    # Step 2: Generate Access Token
    access_token = auth_client.get_access_token()
    print("Access Token:", access_token)

    # Step 3: Initialize Market Data Client
    market_client = MarketDataClient(app_key="your_app_key_here", user_id="USER123")

    # Step 4: Fetch LTP for instruments
    ltp_data = market_client.get_ltp(["NSE|RELIANCE", "NSE|TCS"])
    print("LTP Data:", ltp_data)

    # Step 5: Initialize Trading Client
    client = BlitzAPIClient(app_key="your_app_key_here", user_id="USER123")

    # Step 6: Place an Order
    order_data = {
        "symbol": "NSE|RELIANCE",   # or use "instrumentId": 1010010000002885
        "quantity": 10,
        "price": 1420,
        "orderType": "LIMIT",
        "transactionType": "BUY",
        "productType": "CNC",
        "validity": "DAY"
    }
    
    order_response = client.place_order(order_data)
    print("Order Response:", order_response)

**Output**

```python
Access Token: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Downloading instruments...
Loaded 85313 instruments.
INFO:root:App login successful.
Matched → Symbol: NSECM|RELIANCE | Instrument ID: 1010010000002885
Matched → Symbol: NSECM|TCS | Instrument ID: 1010010000011536
LTP Response: {'data': {
    '1010010000002885': {'InstrumentID': 1010010000002885, 'InstrumentName': 'RELIANCE', 'LTP': 1422.7}, '1010010000011536': {'InstrumentID': 1010010000011536, 'InstrumentName': 'TCS', 'LTP': 3507.8}},
    'status': 'success'
    }
```

---

