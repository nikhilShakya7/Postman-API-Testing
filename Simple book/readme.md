# Simple Books API – Postman Collection Documentation

## Overview

This document explains how to use the Simple Books API collection in Postman. The collection includes endpoints for:

- Checking API status
- Fetching all books
- Fetching a single book
- Registering an API client
- Ordering books

The API uses Bearer Token authentication for protected endpoints.

---

# Base URL

Set the following collection or environment variable before running requests:

```txt
https://simple-books-api.glitch.me
```

Variable:

| Variable | Value                                                                    |
| -------- | ------------------------------------------------------------------------ |
| baseURL  | [https://simple-books-api.glitch.me](https://simple-books-api.glitch.me) |

---

# Collection Variables

| Variable    | Description                                  |
| ----------- | -------------------------------------------- |
| baseURL     | Base API URL                                 |
| accessToken | Token generated after registering API client |

---

# Authentication

Protected endpoints use Bearer Token authentication.

Authorization Type:

```txt
Bearer Token
```

Token Variable:

```txt
{{accessToken}}
```

The token is automatically stored after running the Register API Client request.

---

# Required Headers

For all POST requests, add the following header:

| Key          | Value            |
| ------------ | ---------------- |
| Content-Type | application/json |

To add headers in Postman:

1. Open the request
2. Select the Headers tab
3. Add:

```txt
Content-Type    application/json
```

---

# API Endpoints

## 1. API Status

### Description

Checks whether the API is running.

### Request

```http
GET {{baseURL}}/status
```

### Authentication

No authentication required.

### Expected Response

```json
{
  "status": "OK"
}
```

---

## 2. Get All Books

### Description

Returns a list of available books.

### Request

```http
GET {{baseURL}}/books
```

### Optional Query Parameters

| Parameter | Description               |
| --------- | ------------------------- |
| type      | fiction / non-fiction     |
| limit     | Number of books to return |

### Example

```http
GET {{baseURL}}/books?type=fiction&limit=4
```

### Authentication

No authentication required.

### Test Script

---

## 3. Get Single Book

### Description

Returns details for a specific book.

### Request

```http
GET {{baseURL}}/books/:bookId
```

### Path Variable

| Variable | Example |
| -------- | ------- |
| bookId   | 6       |

### Example

```http
GET {{baseURL}}/books/6
```

### Authentication

No authentication required.

### Expected Response

```json
{
  "id": 6,
  "name": "The Russian",
  "type": "fiction",
  "available": true
}
```

---

## 4. Register API Client

### Description

Registers a new API client and generates an access token.

### Request

```http
POST {{baseURL}}/api-clients
```

### Headers

| Key          | Value            |
| ------------ | ---------------- |
| Content-Type | application/json |

### Request Body

```json
{
  "clientName": "Harry Potter",
  "clientEmail": "Harrypotter.postman.test2@example.com"
}
```

### Authentication

No authentication required.

### Expected Response

```json
{
  "accessToken": "your_generated_token"
}
```

### Notes

The generated token is automatically stored in the collection variable:

```txt
{{accessToken}}
```

---

## 5. Order Book

### Description

Creates a new book order.

### Request

```http
POST {{baseURL}}/orders
```

### Authentication

Bearer Token required.

### Authorization

```txt
Bearer {{accessToken}}
```

### Headers

| Key          | Value            |
| ------------ | ---------------- |
| Content-Type | application/json |

### Request Body

```json
{
  "bookId": 1,
  "customerName": "John"
}
```

### Expected Response

```json
{
  "created": true,
  "orderId": "abc123"
}
```

### Test Script

---

# Recommended Execution Order

Run requests in the following order:

1. API Status
2. Get All Books
3. Get Single Book
4. Register API Client
5. Order Book

---

# Common Errors

## 401 Unauthorized

### Cause

Missing or invalid Bearer Token.

### Solution

Run the Register API Client request first to generate a valid token.

---

## 400 Bad Request

### Cause

Invalid request body or missing required fields.

### Solution

Verify JSON body format and required fields.

---

## Unsupported Media Type

### Cause

Missing Content-Type header.

### Solution

Add:

```txt
Content-Type: application/json
```

in the Headers tab.

---

# Postman Setup Guide

## Import Collection

1. Open Postman
2. Click Import
3. Select the collection JSON file
4. Import the collection

---

## Configure Variables

Set:

| Variable | Value                                                                    |
| -------- | ------------------------------------------------------------------------ |
| baseURL  | [https://simple-books-api.glitch.me](https://simple-books-api.glitch.me) |

---

## Run Collection

1. Open the collection
2. Click Run Collection
3. Execute requests in sequence

---

# Tools Used

- Postman
- Simple Books API

---

# Conclusion

This Postman collection provides complete API coverage for the Simple Books API, including authentication, data retrieval, and book ordering workflows. The included test scripts help validate API responses and automate token management for authenticated requests.
