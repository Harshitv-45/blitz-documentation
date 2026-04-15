# Instrument SDK

## **InstrumentMaster**

```
class InstrumentMaster
```

The `InstrumentMaster` class provides a fast and efficient way to map trading symbols to instrument IDs using Redis-backed storage with in-memory caching.

---

### **`__init__`**

```
def __init__(self, host="localhost", port=6379, db=0)
```


??? info "Source Code"
```python
def __init__(self, host="localhost", port=6379, db=0):
    self.rds = redis.Redis(host=host, port=port, db=db, decode_responses=True)
    self.cache = {}
    self.load_instruments()
```

Initializes Redis connection and loads instrument data into memory.

**Parameters**

| Parameter | Type    | Required | Description                                  |
| --------- | ------- | -------- | -------------------------------------------- |
| host      | string  | No       | Redis host (default: `localhost`)             |
| port      | integer | No       | Redis port (default: `6379`)                  |
| db        | integer | No       | Redis database index (default: `0`)           |


**Example**

```python
instrument_master = InstrumentMaster()
```

### **`load_instruments`**

```
def load_instruments(self)
```

??? info "Source Code"
```python
def load_instruments(self):
    instruments_data = self.rds.get("INSTRUMENTS")
    if not instruments_data:
        print(" No 'INSTRUMENTS' key found in Redis.")
        return
    
    for line in instruments_data.split("\n"):
        fields = line.split(",")
        if len(fields) < 7:
            continue
        instrument_id = fields[0].strip()
        symbol_name = fields[6].strip().upper()
        self.cache[symbol_name] = instrument_id
```


Loads all instruments from Redis and caches them in memory for fast lookup.


### **`get_instrument_id`**

```
def get_instrument_id(self, symbol)
```


??? info "Source Code"
```python
def get_instrument_id(self, symbol):
    symbol = symbol.upper()
    if symbol in self.cache:
        return self.cache[symbol]

    matches = [sym for sym in self.cache if symbol in sym]
    if not matches:
        raise ValueError(f" Symbol '{symbol}' not found in Redis instruments.")

    if len(matches) == 1:
        return self.cache[matches[0]]

    print(f" Multiple matches found for '{symbol}':")
    for i, sym in enumerate(matches, 1):
        print(f"  {i}. {sym} → {self.cache[sym]}")
    choice = int(input("Select option: ")) - 1
    return self.cache[matches[choice]]
```


Fetch the instrument ID for a given symbol.


**Parameters**

| Parameter | Type   | Required | Description                                  |
| --------- | ------ | -------- | -------------------------------------------- |
| symbol    | string | Yes      | Trading symbol (e.g., `RELIANCE`, `TCS`)     |


**Example**

```python
instrument_master = InstrumentMaster()

instrument_id = instrument_master.get_instrument_id("RELIANCE")
print("Instrument ID:", instrument_id)

# Example Response:
# Instrument ID: 1010010000002885
```