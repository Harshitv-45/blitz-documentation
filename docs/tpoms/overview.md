# Blitz Broker Adapter Documentation

Welcome to the Blitz Broker Adapter documentation.

---

# Adapter Architecture & Structure

## Overview

The Adapter acts as a **translation layer** between Blitz and broker systems.

It converts:

- Blitz requests → broker-specific API format
- Broker responses → standardized Blitz format

This allows Blitz to work with multiple brokers using a unified interface.

The adapter serves as a bridge between Blitz and external broker systems and ensures consistent communication.

---

## Adapter Responsibilities

The adapter is responsible for:

- Request validation
- Request transformation
- Broker API communication
- Response normalization
- Error handling
- Publishing responses back to Blitz
- Maintaining standardized message format

The adapter must not contain trading or strategy logic.

---

## Architecture

```
Blitz → Redis → Adapter → Broker API
Broker → Adapter → Redis → Blitz
```

### Flow Description

1. Blitz publishes order request.
2. Adapter consumes request from Redis.
3. Adapter converts request to broker format.
4. Broker processes order.
5. Adapter receives broker response.
6. Adapter converts response to Blitz format.
7. Adapter publishes standardized response.

---

## Adapter Components

### 1. Request Listener

Subscribes to Redis channels and receives Blitz requests.

**Responsibilities**

- Message consumption
- Request validation
- Action routing
- Forward request to handler

---

### 2. Request Mapper

Converts Blitz request fields into broker API fields.

**Responsibilities**

- Field mapping
- Enum conversion
- Format transformation
- Payload construction

---

### 3. Broker Client

Handles communication with broker APIs.

**Responsibilities**

- Order placement
- Order modification
- Order cancellation
- Authentication handling
- API request execution

---

### 4. WebSocket / Event Listener

Receives broker order updates and execution events.

**Responsibilities**

- Subscribe to broker updates
- Receive execution events
- Forward raw responses to mapper

---

### 5. Response Mapper

Converts broker responses into Blitz standardized response format.

**Responsibilities**

- Field mapping
- Timestamp conversion
- Data validation
- Response construction

---

### 6. Response Publisher

Publishes standardized responses back to Blitz.

**Responsibilities**

- Publish response messages
- Ensure message structure consistency
- Deliver events to Redis channel

---

## Order Processing Flow

### Place Order Flow

1. Blitz publishes `PLACE_ORDER` request.
2. Adapter validates request.
3. Request Mapper converts request to broker format.
4. Broker Client sends request.
5. Broker returns response.
6. Response Mapper converts response.
7. Response Publisher publishes result.

---

### Order Update Flow

1. Broker sends order update or execution event.
2. Adapter receives update.
3. Response Mapper converts update to Blitz format.
4. Adapter publishes standardized response.

---

## Error Handling

The adapter must:

- Capture broker errors
- Handle failed API calls
- Normalize error responses
- Publish failure messages
- Maintain consistent response structure

---
