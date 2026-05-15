# Market Data Client

This document explains how to use the `MarketDataClient` for fetching market data, option chain data, historical data, and subscribing to real-time WebSocket ticks.

---

# Initialize Client

```python
from market_data import MarketDataClient

client = MarketDataClient(
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

# LTP API

Fetch Last Traded Price (LTP) for instruments.

## Example

```python
Instruments = ["NSECM|IDEA"]

ltp = client.get_ltp(Instruments)

print("LTP Response:", ltp)
```

## Input Format

```python
["EXCHANGE|SYMBOL"]
```

Example:

```python
["NSECM|IDEA"]
```

---

# Quote API

Fetch detailed quote information.

## Example

```python
Instruments = ["NSECM|IDEA"]

quote_data = client.get_quote(Instruments)

print("Quote Response:", quote_data)
```

## Response Includes

- Open Price
- High Price
- Low Price
- Close Price
- Volume
- Bid/Ask Data
- Last Traded Price

---

# Option Chain API

Fetch option chain data for a symbol and expiry date.

## Example

```python
symbol = "NIFTY"
expiry_date = "2026-02-10"

option_chain = client.get_option_chain(symbol, expiry_date)

print("Option Chain Response:", option_chain)
```

## Parameters

| Parameter | Description |
|----------|-------------|
| `symbol` | Underlying symbol |
| `expiry_date` | Expiry date in `YYYY-MM-DD` format |

---

# Historical Data API

Fetch historical candle/bar data.

## Example

```python
instrument = "IDEA"
interval = "D"

hist_data = client.get_historical_data(instrument, interval)

print("Historical Data Response:", hist_data)
```

## Supported Intervals

| Interval | Description |
|----------|-------------|
| `1m` | 1 Minute |
| `5m` | 5 Minutes |
| `15m` | 15 Minutes |
| `1H` | 1 Hour |
| `D` | Daily |

---

# WebSocket Market Data

Subscribe to real-time market data updates.

---

## Define Callback Function

```python
def my_callback_function(data):
    print("New tick data received:", data)
```

---

## Assign Callback

```python
client.on_message = my_callback_function
```

---

## Connect WebSocket

```python
client.connect_ws()

print("WebSocket connection established.")
```

---

## Subscribe Market Data

```python
instrument_ids = [110010002000001]

client.subscribe_market_data(instrument_ids)
```

---

## Unsubscribe Market Data

```python
client.unsubscribe_market_data(instrument_ids)
```

---

# Complete Example

```python
from market_data import MarketDataClient

# =============================== Initialize Client ===============================

client = MarketDataClient(
    app_key="YOUR_APP_KEY",
    user_id="YOUR_USER_ID"
)

# =============================== LTP ===============================

Instruments = ["NSECM|IDEA"]

ltp = client.get_ltp(Instruments)

print("LTP Response:", ltp)

# =============================== Quote ===============================

quote_data = client.get_quote(Instruments)

print("Quote Response:", quote_data)

# =============================== Option Chain ===============================

symbol = "NIFTY"
expiry_date = "2026-02-10"

option_chain = client.get_option_chain(symbol, expiry_date)

print("Option Chain Response:", option_chain)

# =============================== Historical Data ===============================

instrument = "IDEA"
interval = "D"

hist_data = client.get_historical_data(instrument, interval)

print("Historical Data Response:", hist_data)

# =============================== WebSocket Data ===============================

instrument_ids = [110010002000001]

def my_callback_function(data):
    print("New tick data received:", data)

client.on_message = my_callback_function

client.connect_ws()

print("WebSocket connection established.")

client.subscribe_market_data(instrument_ids)

# To unsubscribe
# client.unsubscribe_market_data(instrument_ids)
```