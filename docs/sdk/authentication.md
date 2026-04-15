## **AuthClient**

```
class AuthClient
```


### **`__init__`**

```
def __init__(self, app_key: str, user_id: str)
```

??? info "Source code"
    ```python
    def __init__(self, app_key: str, user_id: str):
        self.app_key = app_key
        self.user_id = user_id
        self.auth_base_url = "http://182.70.127.254:44347"
        self.access_token = None
    ```

Authentication of the client.

**Parameters**

| Parameter | Type     | Required | Description                                                |
| --------- | -------- | -------- | ---------------------------------------------------------- |
| `app_key` | `string` | Yes      | Unique key provided to the client for API authentication.  |
| `user_id` | `string` | Yes      | Unique identifier of the client or account used for login. |

**Example:**

```python
auth = AuthClient(app_key="abc123xyz", user_id="U12345")
```

---

### **`get_access_token`**

```
def get_access_token(self):
```

??? info "Source Code"
    ```python
    def get_access_token(self):
        """Return the access token, logging in if necessary."""
        if not self.access_token:
            self._app_login()
        return self.access_token
    ```

Return the access token of the client

**Example:**

```python
auth = AuthClient(app_key="abc123xyz", user_id="U12345")
token = auth.get_access_token()
print("Access Token:", token)

# Example Response:
# Access Token: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwcmVmZXJyZWRfdXNlcm5hbWUiOiJBbGdvMTIzIiwiYXBwTmFtZSI6IkFsZ29UZXN0IiwidXNlcklkIjoiQWxnbzEyMyIsImV4cCI6MTc3NTYzMjgzOCwiaXNzIjoiQXV0aFNlcnZlciJ9.GmGFg_Noqw1XhTJ5UDJ1XRcUi6gGwP-HJF3TQsfAJl0
```

---