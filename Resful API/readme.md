# Products API Documentation

**Base URL:** `{{baseURL}}`  
**Authentication:** API Key via header `x-api-key: {{apiKey}}`

---

## Overview

This API provides CRUD operations for managing product objects within the `products` collection. All endpoints are secured with an API key passed as a request header.

---

## Authentication

All requests require an API key in the request header:

| Header      | Value        |
| ----------- | ------------ |
| `x-api-key` | `{{apiKey}}` |

---

## Endpoints

### 1. Get All Products

Retrieves a list of all product objects in the collection.

**`GET /collections/products/objects`**

#### Query Parameters

| Parameter | Type   | Required | Description              |
| --------- | ------ | -------- | ------------------------ |
| `price`   | number | No       | Filter products by price |

#### Example Request

```
GET {{baseURL}}/collections/products/objects
x-api-key: {{apiKey}}
```

#### Expected Response

- **Status:** `200 OK`
- **Response Time:** < 300ms

---

### 2. Create a Product

Creates a new product object in the collection.

**`POST /collections/products/objects`**

#### Request Body

```json
{
  "id": "9",
  "name": "Beats Studio3 Wireless",
  "data": {
    "Color": "Red",
    "Description": "High-performance wireless noise cancelling headphones"
  }
}
```

#### Body Fields

| Field  | Type   | Required | Description                                     |
| ------ | ------ | -------- | ----------------------------------------------- |
| `id`   | string | Yes      | Unique identifier for the product               |
| `name` | string | Yes      | Product name                                    |
| `data` | object | Yes      | Additional product attributes (key-value pairs) |

#### Expected Response

- **Status:** `200 OK`
- **Response Time:** < 500ms

---

### 3. Partially Update a Product (PATCH)

Updates one or more fields of an existing product without replacing the entire object.

**`PATCH /collections/products/objects/{id}`**

#### Path Parameters

| Parameter | Description                            |
| --------- | -------------------------------------- |
| `id`      | The unique ID of the product to update |

#### Example Request

```
PATCH {{baseURL}}/collections/products/objects/ff8081819d82fab6019e208208d83138
```

#### Request Body

```json
{
  "name": "Nokia",
  "data": {
    "color": "Black",
    "price": "8999"
  }
}
```

#### Expected Response

- **Status:** `200 OK`
- **Response Time:** < 300ms
- **Body:** Returns the updated product. Validated fields:
  - `name` equals `"Nokia"`
  - `data.color` equals `"Black"`

---

### 4. Replace a Product (PUT)

Fully replaces an existing product object with the provided data.

**`PUT /collections/products/objects/{id}`**

#### Path Parameters

| Parameter | Description                             |
| --------- | --------------------------------------- |
| `id`      | The unique ID of the product to replace |

#### Example Request

```
PUT {{baseURL}}/collections/products/objects/ff8081819d82fab6019e208a3b34315c
```

#### Request Body

```json
{
  "name": "Realme",
  "data": {
    "color": "Red",
    "price": 32222
  }
}
```

#### Expected Response

- **Status:** `200 OK`
- **Response Time:** < 200ms
- **Body:** Returns the replaced product. Validated fields:
  - `name` equals `"Realme"`

---

### 5. Delete a Product

Deletes a product object from the collection.

**`DELETE /collections/products/objects/{id}`**

#### Path Parameters

| Parameter | Description                            |
| --------- | -------------------------------------- |
| `id`      | The unique ID of the product to delete |

#### Example Request

```
DELETE {{baseURL}}/collections/products/objects/ff8081819d82fab6019e20900efd3165
```

#### Expected Response

- **Status:** `200 OK`

---

## Summary Table

| Method | Endpoint                             | Description                | Auth Required |
| ------ | ------------------------------------ | -------------------------- | ------------- |
| GET    | `/collections/products/objects`      | Get all products           | Yes           |
| POST   | `/collections/products/objects`      | Create a product           | Yes           |
| PATCH  | `/collections/products/objects/{id}` | Partially update a product | No\*          |
| PUT    | `/collections/products/objects/{id}` | Fully replace a product    | No\*          |
| DELETE | `/collections/products/objects/{id}` | Delete a product           | No\*          |

> \*Auth is configured at the collection level but not explicitly set on PATCH, PUT, and DELETE requests in the collection.

---

## Environment Variables

| Variable      | Description                     |
| ------------- | ------------------------------- |
| `{{baseURL}}` | The base URL of the API server  |
| `{{apiKey}}`  | Your API key for authentication |

---

## Test Assertions Reference

| Endpoint | Assertion                                                                                 |
| -------- | ----------------------------------------------------------------------------------------- |
| GET      | Status 200; Response time < 300ms                                                         |
| POST     | Status 200; Response time < 500ms                                                         |
| PATCH    | Status 200; Response body `name` = `Nokia`, `data.color` = `Black`; Response time < 300ms |
| PUT      | Status 200; Response body `name` = `Realme`; Response time < 200ms                        |
| DELETE   | Status 200                                                                                |
