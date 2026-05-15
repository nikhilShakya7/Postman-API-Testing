# Cookies Collection — Postman Documentation

## Overview

This collection tests cookie management, response types, encoding, compression, and utility endpoints against a configurable base API. It covers the full cookie lifecycle (set → get → delete) as well as response handling scenarios like streaming, compression, encoding, and JWT-authenticated requests.

---

## Collection Variables

| Variable  | Description                                                    |
|-----------|----------------------------------------------------------------|
| `BaseUrl` | Base URL for all requests (e.g., `https://postman-echo.com/`) |

> **Note:** The `baseUrl` variable is empty by default. Set it in your Postman Environment or Collection Variables panel before running.

---

## Requests

### 1. Set Cookies

**Endpoint:** `GET {{BaseUrl}}cookies/set?foo1=bar1&foo2=bar2&foo3=bar3&foo4=bar4`

Sets four cookies (`foo1`–`foo4`) via query parameters. The server responds with a JSON body reflecting the cookies that were stored.

**Query Parameters:**

| Key    | Value  |
|--------|--------|
| `foo1` | `bar1` |
| `foo2` | `bar2` |
| `foo3` | `bar3` |
| `foo4` | `bar4` |

**Tests:**

| Test | Assertion |
|------|-----------|
| `Status code is 200` | Response status equals `200` |
| `Cookies are returned in the response body` | `jsonData.cookies` contains `foo1: bar1` and `foo3: bar3` |

> **Note:** Run this request first to populate cookies before calling **Get Cookies**.

---

### 2. Get Cookies

**Endpoint:** `GET {{BaseUrl}}cookies`

Retrieves all cookies currently associated with the session. Returns a JSON object with a `cookies` key containing all active cookie name-value pairs.

**Auth:** None required  
**Headers:** None required

**Example Response:**
```json
{
  "cookies": {
    "foo1": "bar1",
    "foo2": "bar2"
  }
}
```

**Tests:**

| Test | Assertion |
|------|-----------|
| `Verify cookies are retrieved` | Status is `200` and `jsonData.cookies` is not empty |

---

### 3. Delete Cookies

**Endpoint:** `GET {{BaseUrl}}cookies/delete?foo1&foo2`

Deletes the specified cookies (`foo1` and `foo2`) by passing their keys as query parameters with no values.

**Query Parameters:**

| Key    | Value  |
|--------|--------|
| `foo1` | *(none)* |
| `foo2` | *(none)* |

**Tests:**

| Test | Assertion |
|------|-----------|
| `Cookies removed successfully` | `jsonData.cookies` does not contain `foo1` or `foo2` |

---

### 4. Response Status

**Endpoint:** `GET {{BaseUrl}}status/200`

Validates that the server correctly returns a `200 OK` status code. Useful as a basic health/smoke check.

**Tests:**

| Test | Assertion |
|------|-----------|
| `Status is 200 OK` | Response status equals `200` |

---

### 5. Stream Response

**Endpoint:** `GET {{BaseUrl}}stream/5`

Requests a streamed response consisting of 5 chunks. Verifies that the server handles chunked/streaming responses and returns a successful status.

**Tests:**

| Test | Assertion |
|------|-----------|
| `Status is 200 OK` | Response status equals `200` |

---

### 6. Encoded Response

**Endpoint:** `GET {{BaseUrl}}encoding/utf8`

Fetches a UTF-8 encoded HTML demo page from the server. The response displays a wide range of Unicode characters and scripts (Latin, Greek, Cyrillic, Arabic, CJK, and more). Use this to verify that the client correctly handles UTF-8 encoded responses.

**Auth:** None  
**Tests:** None defined

---

### 7. GZip Compressed Response

**Endpoint:** `GET {{BaseUrl}}gzip`

Requests a GZip-compressed response. Verifies that the server returns a success status and that the `Content-Encoding` header indicates compression.

**Tests:**

| Test | Assertion |
|------|-----------|
| `Status is 200 OK` | Response status equals `200` |
| `Content-Encoding is present` | `Content-Encoding` header is `"gzip"` or `"deflate"` |

---

### 8. Deflate Compressed Response

**Endpoint:** `GET {{BaseUrl}}deflate`

Requests a Deflate-compressed response. Applies the same compression checks as the GZip request.

**Tests:**

| Test | Assertion |
|------|-----------|
| `Status is 200 OK` | Response status equals `200` |
| `Content-Encoding is present` | `Content-Encoding` header is `"gzip"` or `"deflate"` |

---

### 9. IP Address

**Endpoint:** `GET {{BaseUrl}}ip`

Returns the public IP address of the requesting client as a JSON response. No tests are defined; useful for debugging network/proxy configurations.

**Auth:** None  
**Tests:** None defined

---

### 10. UTC Time

**Endpoint:** `GET {{BaseUrl}}time/now`

Returns the current UTC time from the server. This request uses JWT authentication.

**Auth Config:**

| Setting             | Value      |
|---------------------|------------|
| Type                | JWT        |
| Algorithm           | `HS256`    |
| Secret Base64 encoded | `false`  |
| Payload             | `{}`       |
| Token added to      | Header     |
| Header prefix       | `Bearer`   |

**Tests:** None defined

---

## Request Summary

| # | Request Name               | Method | Endpoint                  | Has Tests |
|---|----------------------------|--------|---------------------------|-----------|
| 1 | Set Cookies                | GET    | `cookies/set?...`         | Yes       |
| 2 | Get Cookies                | GET    | `cookies`                 | Yes       |
| 3 | Delete Cookies             | GET    | `cookies/delete?...`      | Yes       |
| 4 | Response Status            | GET    | `status/200`              | Yes       |
| 5 | Stream Response            | GET    | `stream/5`                | Yes       |
| 6 | Encoded Response           | GET    | `encoding/utf8`           | No        |
| 7 | GZip Compressed Response   | GET    | `gzip`                    | Yes       |
| 8 | Deflate Compressed Response| GET    | `deflate`                 | Yes       |
| 9 | IP Address                 | GET    | `ip`                      | No        |
| 10| UTC Time                   | GET    | `time/now`                | No        |

---

## Recommended Execution Order

For cookie tests to work correctly, run requests in this order:

1. **Set Cookies** — creates session cookies
2. **Get Cookies** — reads and verifies cookies are active
3. **Delete Cookies** — removes cookies and confirms deletion

The remaining requests are stateless and can be run in any order.
