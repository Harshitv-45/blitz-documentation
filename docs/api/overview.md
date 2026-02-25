# **Introduction**

The Blitz API provides a set of REST-like endpoints that enable seamless integration with our trading platform. It allows you to execute and modify orders in real time, manage your portfolio, and access live market data — all through a fast and reliable API suite.

Our APIs use resource-oriented URLs and accept both JSON and form-encoded requests. Responses are returned in JSON format, following standard HTTP methods, status codes, and authentication practices.

Install Python Package using the following command line.

`pip install blitz_sdk`

This install the whole Blitz Python Client along with the reqquired packages and the user can start using blitz client using python script.

Then user can import blitz module and connect the blitz account

```python
    from blitz import blitz

    client = blitz("user_id", "access_token")
```

## **Why Blitz Gateway?**

- **Speed** – Optimized for low-latency data transfer and high throughput.
- **Security** – Built-in authentication, authorization, and encryption options.
- **Scalability** – Designed to handle everything from small internal tools to enterprise-scale traffic.
- **Flexibility** – Supports multiple protocols and can integrate with a wide range of platforms.

## **Getting Started & Prerequisites**

To use Blitz API you'll need:

- An active Blitz account
- Access Token

## **Request Structure**

User can use the following structure to make HTTPS requests to the Blitz GAteway API:

```bash
curl -x [API_METHOD] 'http://[BASE_URL]/api/[API_ENDPOINT]' \
-H 'Authorization: Bearer [YOUR_ACCESS_TOKEN]' \
-H 'accept: application/json' \
[CONTENT_TYPE_HEADER] \
[REQUEST_PAYLOAD]
```
