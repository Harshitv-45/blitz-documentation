# **Blitz Documentation**

Welcome to **Blitz** — a platform that provides standardized access to trading APIs, developer SDKs, and broker integrations through a scalable and broker-independent architecture.

Blitz enables developers and trading systems to place orders, access market data, manage portfolios, and integrate multiple brokers using a single consistent interface.

---

## Platform Overview

Blitz consists of three core components:

- **Blitz API** — Direct REST interface for trading operations and market data
- **Blitz SDK** — Client libraries for simplified integration
- **TPOMS** — Broker communication and order routing system

Each component serves a different purpose in the trading workflow.

---

## **Blitz API**

The **Blitz API** provides a set of REST endpoints that allow applications to interact directly with the trading platform.

Using the API, you can:

- Place, modify, and cancel orders
- Retrieve trades and positions
- Access real-time and historical market data
- Track order execution and status
- Manage portfolio information

The API uses resource-oriented URLs and returns responses in JSON format using standard HTTP methods and status codes.

Refer to the **API Reference** section to explore available endpoints.

---

## **Blitz SDK**

The **Blitz SDK** provides developer-friendly client libraries that simplify interaction with Blitz APIs.

The SDK handles:

- Authentication management
- Request formatting
- API communication
- Data parsing
- Error handling

It allows developers to integrate trading functionality quickly without managing low-level API calls.

Refer to the **SDK Documentation** section to get started.

---

## **TPOMS**

**TPOMS (Third-Party Order Management System)** is the broker integration layer of Blitz.

It acts as a translation bridge between Blitz and external broker systems by:

- Converting Blitz requests into broker-specific formats
- Normalizing broker responses
- Managing order lifecycle events
- Supporting multiple broker integrations through adapters

This architecture enables Blitz to work with different brokers using a unified interface.

Refer to the **TPOMS Documentation** section for architecture and adapter details.

---

## **Documentation Structure**

This documentation is organized into the following sections:

- **API Reference** — Trading and market data endpoints
- **SDK** — Client library usage
- **TPOMS** — Broker integration and adapter architecture

---

## **Getting Started**

To start using Blitz:

1. Generate an access token
2. Authenticate API requests
3. Use the API or SDK to interact with the platform
4. Integrate brokers using TPOMS if required
