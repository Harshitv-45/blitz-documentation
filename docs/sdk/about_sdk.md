# What is SDK?

A Software Development Kit (SDK) is a collection of tools and libraries that allows developers to interact with the Blitz API in a simpler and more efficient way.

Instead of manually handling HTTP requests, headers, and response parsing, the SDK provides ready-to-use functions that abstract these complexities.

---

## Why use SDK?

Using the SDK simplifies integration and reduces development effort.

### Key Advantages

- **Simplified Integration**  
  No need to manually construct API requests or handle low-level details.

- **Built-in Authentication Handling**  
  Automatically manages tokens and headers.

- **Error Handling**  
  Standardized error handling across all API calls.

- **Faster Development**  
  Focus on business logic instead of API plumbing.

- **Cleaner Code**  
  Reduces boilerplate and improves readability.

---

## API vs SDK

| Feature | Direct API | SDK |
|--------|-----------|-----|
| Request Handling | Manual | Automated |
| Authentication | Manual headers | Handled internally |
| Error Handling | Manual | Standardized |
| Development Speed | Slower | Faster |
| Code Complexity | Higher | Lower |

---

## How SDK Works

The SDK internally:

1. Builds API requests  
2. Adds authentication headers  
3. Sends requests to Blitz API  
4. Parses responses into usable objects  

This allows developers to interact with the system using simple function calls instead of raw HTTP requests.

---

## When to Use SDK

Use the SDK when:

- You want faster development  
- You are building applications in a supported language  
- You want cleaner and maintainable code  

---

## When to Use API Directly

Use the API directly when:

- You need full control over requests  
- You are working in an unsupported language  
- You are building custom integrations or infrastructure  

---

## Summary

The Blitz SDK acts as a convenience layer over the API, making it easier to integrate and work with the platform while maintaining full functionality.

---

!!! tip
    Use the SDK for faster development. Use the API directly when you need full control.