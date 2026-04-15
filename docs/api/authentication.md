# **Authentication API**

The **Authentication API** allows users to securely log in and obtain access to protected trading APIs.  
Using this API, you can authenticate users and generate a session token for authorized requests.

These endpoints let you **authenticate users and manage access control** securely.

| **Type** | **Endpoint**                          | **Description**                             |
| -------- | ------------------------------------- | ------------------------------------------- |
| POST     | [`authentication/login`](#user-login) | Authenticate user and generate access token |

---

## **Authentication** {#user-login}

Authenticate a user using valid credentials and generate an access token.

### Endpoint

```
POST /v1/api/app_login
```

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

## **Request Body**

```json
{
  "appKey": "CPXotUwWdlIpgJbYrguSnVyu05IZawswKaXKNQ",
  "userId": "Algo123"
}
```

---

## **Request Field Descriptions**

| Field    | Type   | Description                               |
| -------- | ------ | ----------------------------------------- |
| userId   | string | Unique user identifier used for login     |
| appKey   | string | AppKey associated with the user account   |

---

## **Response**

### Success Response

```json
{
    "status": "success",
    "message": "request processed successfully.",
    "data": {
        "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwcmVmZXJyZWRfdXNlcm5hbWUiOiJBbGdvMTIzIiwiYXBwTmFtZSI6IkFsZ29UZXN0IiwiaXBBZGRyZXNzIjoiMTcyLjE3LjAuMSIsImVudGl0eUlkIjoiODVhYmJlZWItMmU3Zi00NGE1LWI2NjgtODQ3MjBkMjljZWVkIiwidXNlcklkIjoiQWxnbzEyMyIsImh0dHA6Ly9zY2hlbWFzLm1pY3Jvc29mdC5jb20vd3MvMjAwOC8wNi9pZGVudGl0eS9jbGFpbXMvcm9sZSI6IkFwcCIsImF1ZCI6WyJHYXRld2F5U2VydmVyIiwiQVBJU2VydmVyIl0sImV4cCI6MTc3NTYzMjgzOCwiaXNzIjoiQXV0aFNlcnZlciJ9.GmGFg_Noqw1XhTJ5UDJ1XRcUi6gGwP-HJF3TQsfAJl0"
    }
}
```

---

## **Response Field Descriptions**

| Field        | Type    | Description                                         |
| ------------ | ------- | --------------------------------------------------- |
| status       | string  | Status of the request (`success` or `error`)        |
| message      | string  | Informational message about the login result        |
| data         | object  | Contains authentication details                     |
| accessToken  | string  | JWT or access token used for authenticated requests |

---

## **Error Response**

### Invalid Credentials

```json
{
  "status": "error",
  "message": "invalid userId or appKey",
  "data": null
}
```

---

### **Using the Access Token**

After successful authentication, include the token in the header of all protected API requests:

```
Authorization: Bearer <token>
```

Example:

```bash
curl -X 'GET' \
  'http://uat.quantxpress.com/v1/api/order/trades' \
  -H 'Authorization: Bearer <token>' \
  -H 'accept: */*'
```

---

## Notes

- The token must be included in all secured endpoints.
- Tokens expire after the specified duration (`expiresIn`).
- Re-authentication is required once the token expires.
- Ensure credentials are transmitted securely over HTTPS in production environments.
