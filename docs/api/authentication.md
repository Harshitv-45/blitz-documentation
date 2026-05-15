# Authentication API

The **Authentication API** is used to securely authenticate users and generate an access token required for accessing protected APIs within the QuantXpress platform.

Before accessing any secured APIs such as Orders, Trades, Positions, Strategies, or WebSocket services, users must first authenticate using a valid `userId` and `appKey`.

Once authentication is successful, the API returns a JWT access token. This token must be included in the `Authorization` header for all subsequent secured API requests.

These endpoints help manage secure API access and prevent unauthorized usage.

| Type | Endpoint | Description |
|------|-----------|-------------|
| POST | `api/app_login` | Authenticate user and generate access token |

---

# Authentication

Authenticate a user using valid credentials and generate an access token.

### Endpoint

```http
POST /v1/api/app_login
```

---

### Authentication Flow

```text
Client Application
       │
       │ 1. Send userId + appKey
       ▼
Authentication API
       │
       │ 2. Validate credentials
       ▼
Generate JWT Access Token
       │
       │ 3. Return access token
       ▼
Client Stores Token
       │
       │ 4. Use token in secured APIs
       ▼
Protected APIs Accessible
```

---

### Request Headers

```http
Content-Type: application/json
Accept: */*
```

---

### cURL Example

```bash
curl -X 'POST' \
  'http://uat.quantxpress.com/api_gateway/v1/api/app_login' \
  -H 'accept: */*' \
  -H 'Content-Type: application/json' \
  -d '{
    "appKey": "CPXotUwWdlIpgJbYrguSnVyu05IZawswKaXKNQ",
    "userId": "Algo123"
  }'
```

---

### Request Body

```json
{
  "appKey": "CPXotUwWdlIpgJbYrguSnVyu05IZawswKaXKNQ",
  "userId": "Algo123"
}
```

---

### Request Field Descriptions

| Field | Type | Description |
|------|------|-------------|
| `userId` | string | Unique user identifier used for authentication |
| `appKey` | string | Application key assigned to the user |

---

# Response

### Success Response

Upon successful authentication, the server will return a `200 OK` status with the following JSON structure:

```json
{
  "status": "success",
  "message": "request processed successfully.",
  "data": {
    "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
}
```

---

### Response Field Descriptions

| Field | Type | Description |
|------|------|-------------|
| `status` | string | Status of the API request |
| `message` | string | Informational response message |
| `data` | object | Contains authentication response data |
| `accessToken` | string | JWT access token used for authenticated API requests |

---

### Access Token

After successful authentication, the API returns a JWT (JSON Web Token).

Example:

```text
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

This token contains authenticated session details and is required for accessing secured APIs.

---

### Using the Access Token

Include the access token in the `Authorization` header of all protected API requests.

```http
Authorization: Bearer <accessToken>
```

---

### Example Authenticated Request

```bash
curl -X 'GET' \
  'http://uat.quantxpress.com/v1/api/order/trades' \
  -H 'Authorization: Bearer <accessToken>' \
  -H 'accept: */*'
```

---

### Authentication Lifecycle

```text
Application Starts
       │
       ▼
Call app_login API
       │
       ▼
Receive Access Token
       │
       ▼
Store Token Securely
       │
       ▼
Use Token in APIs
       │
       ▼
Token Expires
       │
       ▼
Re-authenticate
```

---

# Error Response

### Invalid Credentials

```json
{
  "status": "error",
  "message": "invalid userId or appKey",
  "data": null
}
```

---

### Common HTTP Status Codes

| Status Code | Description |
|-------------|-------------|
| `200` | Authentication successful |
| `400` | Invalid request payload |
| `401` | Unauthorized or invalid credentials |
| `403` | Access denied |
| `500` | Internal server error |

---

> **Tips**
>
> - Store the access token securely in memory, cache, or encrypted storage.
> - Authenticate once during application startup and reuse the token until it expires.
> - Always validate whether the token is available before making secured API requests.
> - Use centralized token management if multiple services or threads are using APIs simultaneously.

!!!Notes

    - The access token is mandatory for all secured APIs.
    - Authentication should be completed before using trading or market-data APIs.
    - Re-authentication is required after token expiration.
    - Tokens are session-based and may expire after a configured duration.
    - The same token can be reused across multiple API requests until expiry.

---

!!!Warnings

    - Never expose `appKey` or `accessToken` in frontend applications, logs, screenshots, or public repositories.

    - Do not hardcode credentials directly inside production source code.

    - Expired or invalid tokens will result in authorization failures (`401 Unauthorized`).

    - Always use HTTPS endpoints in production environments to avoid credential leakage.

    - Avoid generating multiple unnecessary login sessions repeatedly in short intervals.

---

# Best Practices

### Store Tokens Securely

Access tokens should never be exposed in logs, source code, or public repositories.

---

### Use HTTPS

Always use HTTPS endpoints in production environments to ensure secure credential transmission.

---

### Handle Token Expiry

Applications should re-authenticate once the token expires.

---

### Integration Recommendation

```text
1. Start Application
2. Authenticate using app_login
3. Store access token
4. Initialize APIs/WebSocket connections
5. Use token in all secured requests
6. Re-authenticate on token expiry
```