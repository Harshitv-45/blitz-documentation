# Authentication API

The **Authentication API** allows users to securely log in and obtain access to protected trading APIs.  
Using this API, you can authenticate users and generate a session token for authorized requests.

These endpoints let you **authenticate users and manage access control** securely.

| **Type** | **Endpoint**                          | **Description**                             |
| -------- | ------------------------------------- | ------------------------------------------- |
| POST     | [`authentication/login`](#user-login) | Authenticate user and generate access token |

---

## Authentication {#user-login}

Authenticate a user using valid credentials and generate an access token.

### Endpoint

```
POST /v1/api/authentication/login
```

### cURL Example

```bash
curl -X 'POST' \
  'http://uat.quantxpress.com/v1/api/authentication/login' \
  -H 'accept: */*' \
  -H 'Content-Type: application/json' \
  -d '{
  "userId": "string",
  "password": "string"
}'
```

---

## Request Body

```json
{
  "userId": "string",
  "password": "string"
}
```

---

## Request Field Descriptions

| Field    | Type   | Description                               |
| -------- | ------ | ----------------------------------------- |
| userId   | string | Unique user identifier used for login     |
| password | string | Password associated with the user account |

---

## Response

### Success Response

```json
{
  "status": "success",
  "message": "Request processed successfully",
  "data": {
    "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbnRpdHlJZCI6IjliNGY5YSIsInVzZXJJZCI6IlBSQVNIQU5UIiwiaXBBZGRyZXNzIjoiMTcyLjE4LjAuMSIsInNlc3Npb25JZCI6ImI4YjA1ZjE2LWQ0MmMtNDM1NS05MWNiLTY1MTI0NWFiOWEyZiIsImRldmljZVR5cGUiOiJTREsiLCJodHRwOi8vc2NoZgvMDYvaWRlbnRpdHI",
    "refreshToken": "9B1ja8YwHPZ8W8zZ5MAgRFeal1S39mI2V7KzsacLMrmmRp2NQhiiknsDQe3nMgR9po6QHkOH8pM53RSfKRUS3SoNRoy5wrYJHeZPMclRIJzDvtjyVqzUlR3blfitF3Qnuq0c0ceztxAJKoODLrCzLlJVMkz5ckGPPH0nG3HqYttvyPID6eWGs4yZKjHFVI1gUMjnzx97IcGmpJsxfoBQniU15MzLBr1cU0zeZw",
    "requiresOtp": false,
    "requireTotp": false
  }
}
```

---

## Response Field Descriptions

| Field        | Type    | Description                                         |
| ------------ | ------- | --------------------------------------------------- |
| status       | string  | Status of the request (`success` or `error`)        |
| message      | string  | Informational message about the login result        |
| data         | object  | Contains authentication details                     |
| accessToken  | string  | JWT or access token used for authenticated requests |
| refreshToken | string  | Token used to refresh access token                  |
| requiresOtp  | boolean | Indicates if OTP verification is required           |
| requireTotp  | boolean | Indicates if TOTP verification is required          |

---

## Error Response

### Invalid Credentials

```json
{
  "status": "error",
  "message": "invalid userId or password",
  "data": null
}
```

---

## Using the Access Token

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
