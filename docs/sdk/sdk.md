# Getting Started with SDK

This guide helps you quickly set up and start using the Blitz SDK for trading and market data operations.

---

# Prerequisites

Before using the SDK, ensure the following are installed and configured:

- Python 3.9 or above
- pip package manager
- Internet connectivity
- Valid `app_key`
- Valid `user_id`

---

# Install Python

Download and install Python from:

- [Python Official Website](https://www.python.org/downloads/)

Verify installation:

```bash
python --version
```

---

# Upgrade pip

Ensure pip is updated to the latest version:

```bash
python -m pip install --upgrade pip
```

---

# Install Dependencies

Install required packages using:

```bash
pip install -r requirements.txt
```

---


---

# Initialize SDK Client

## Blitz Trading SDK

```python
from BLITZAPI.blitz_api_client import BlitzAPIClient

client = BlitzAPIClient(
    app_key="YOUR_APP_KEY",
    user_id="YOUR_USER_ID"
)
```

---

## Market Data SDK

```python
from market_data import MarketDataClient

client = MarketDataClient(
    app_key="YOUR_APP_KEY",
    user_id="YOUR_USER_ID"
)
```

---

# Generate API Keys

Before using the SDK, generate your API credentials.

- [Generate API Key](../userSetup/generateKey.md)

---

# Basic Usage

## Fetch Orders

```python
orders = client.get_orders()

print(orders)
```

---

## Fetch LTP

```python
ltp = client.get_ltp(["NSECM|IDEA"])

print(ltp)
```

---

# Run Examples

Execute sample files:

```bash
python example.py
```

---

# Documentation

## SDK Modules

- [Blitz Trading SDK](../blitzSdk/blitzSdk.md)
- [Market Data SDK](../marketDataSdk/marketDataSdk.md)

---

# Troubleshooting

## pip not recognized

Reinstall Python and ensure:

```text
Add Python to PATH
```

is checked during installation.

---

## Module not found

Install dependencies again:

```bash
pip install -r requirements.txt
```

---

# Summary

You are now ready to use the Blitz SDK for:

- Trading operations
- Market data access
- Real-time WebSocket streaming
- Strategy execution
- Order and position management

---

!!! tip
    Start with the example scripts to quickly understand SDK usage and workflows.