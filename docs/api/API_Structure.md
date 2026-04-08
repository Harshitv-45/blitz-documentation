# **API Structure**

This section describes how to interact with the Blitz API, including request format, authentication, and response structure.

---

## **Base URL**

All API requests should be made to:

```http
http://uat.quantxpress.com
```

## **Authentication**

All endpoints require authentication using an access token.

Include the token in the request header:

```http
Authorization: Bearer {access_Token}
```

---

## **HTTP Methods**

Blitz API uses standard HTTP methods:

|Method |	Usage|
|-------|-----------------|
|GET    |	 Retrieve data |
|POST   |	Create resources (e.g., place order)|
|PUT|	Update existing resources|
|DELETE|	Remove resources|


## **Request Structure**

Use the following structure to make requests to the Blitz API:

```bash
curl -X [API_METHOD] \
  'http://uat.quantxpress.com/v1/api/[API_ENDPOINT]' \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer {access_token}' \
  [REQUEST_PAYLOAD]
```

## **Response Structure**
All responses are returned in JSON format:

```json
{
  "status": "success",
  "message": "request processed successfully",
  "data": {}
}
```