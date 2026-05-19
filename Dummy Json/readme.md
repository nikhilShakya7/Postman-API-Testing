# Products API Documentation

Base URL: `{{baseUrl}}` (e.g. `https://dummyjson.com/`)

---

## Authentication

### Login

Authenticates a user and returns an access token, which is automatically stored as `{{accessToken}}` in the environment.

**POST** `{{baseUrl}}auth/login`

**Request Body**

```json
{
  "username": "emilys",
  "password": "emilyspass",
  "expiresInMins": 30
}
```

**Response**

| Field         | Type   | Description                                  |
| ------------- | ------ | -------------------------------------------- |
| `accessToken` | string | Bearer token used for authenticated requests |

**Tests**

- Status code is `200`
- `Content-Type` header is present
- Stores `accessToken` to environment variable

---

### Get Current User

Returns the profile of the currently authenticated user.

**GET** `{{baseUrl}}auth/me`

**Response**

| Field                 | Type   | Description                 |
| --------------------- | ------ | --------------------------- |
| `email`               | string | Email of the logged-in user |
| _(other user fields)_ | mixed  | Full user profile           |

**Tests**

- Status code is `200`
- Response time is under `500ms`
- `Content-Type` header is present
- Email equals `emily.johnson@x.dummyjson.com`

---

## Products

### Get Products

Returns a list of all products.

**GET** `{{baseUrl}}/products/`

**Response**

| Field      | Type   | Description                        |
| ---------- | ------ | ---------------------------------- |
| `total`    | number | Total number of products available |
| `products` | array  | Array of product objects           |

**Tests**

- Status code is `200`
- Response time is under `200ms`
- `total` is greater than `2`

---

### Add Product

Creates a new product. Uses a hardcoded URL rather than `{{baseUrl}}`.

**POST** `https://dummyjson.com/products/add`

**Headers**

| Key            | Value              |
| -------------- | ------------------ |
| `Content-Type` | `application/json` |

**Request Body**

```json
{
  "title": "BMW Pencil",
  "description": "Dolce Shine by Dolce & Gabbana is a vibrant and fruity fragrance...",
  "category": "fragrances",
  "price": 69.99,
  "discountPercentage": 0.62,
  "rating": 3.96,
  "stock": 4,
  "tags": ["fragrances", "perfumes"],
  "brand": "Dolce & Gabbana",
  "sku": "FRA-DOL-DOL-009",
  "weight": 6,
  "dimensions": { "width": 27.28, "height": 29.88, "depth": 18.3 },
  "warrantyInformation": "3 year warranty",
  "shippingInformation": "Ships in 1 month",
  "availabilityStatus": "Low Stock",
  "reviews": [
    {
      "rating": 4,
      "comment": "Would buy again!",
      "date": "2025-04-30T09:41:02.053Z",
      "reviewerName": "Mateo Bennett",
      "reviewerEmail": "mateo.bennett@x.dummyjson.com"
    }
  ],
  "returnPolicy": "7 days return policy",
  "minimumOrderQuantity": 2,
  "meta": {
    "createdAt": "2025-04-30T09:41:02.053Z",
    "updatedAt": "2025-04-30T09:41:02.053Z",
    "barcode": "3023868210708",
    "qrCode": "https://cdn.dummyjson.com/public/qr-code.png"
  },
  "images": ["https://cdn.dummyjson.com/product-images/..."],
  "thumbnail": "https://cdn.dummyjson.com/product-images/.../thumbnail.webp"
}
```

**Response**

| Field                    | Type   | Description                        |
| ------------------------ | ------ | ---------------------------------- |
| `title`                  | string | Title of the newly created product |
| _(other product fields)_ | mixed  | Full product object echoed back    |

**Tests**

- Status code is `201`
- Response time is under `500ms`
- `title` equals `"BMW Pencil"`

---

## Cart

### Add to Cart

Creates a new cart for a user with the specified products. Stores the returned cart ID as `{{cart_id}}`.

**POST** `{{baseUrl}}carts/add`

**Request Body**

```json
{
  "userId": 1,
  "products": [
    { "id": 144, "quantity": 4 },
    { "id": 98, "quantity": 1 }
  ]
}
```

**Response**

| Field      | Type   | Description                                           |
| ---------- | ------ | ----------------------------------------------------- |
| `id`       | number | ID of the newly created cart — saved as `{{cart_id}}` |
| `products` | array  | List of products in the cart                          |

**Tests**

- Status code is `201`
- `products` array length is greater than `0`
- Stores `id` to environment variable `cart_id`

---

### Get Cart Details

Retrieves the details of a specific cart. Currently hardcoded to cart `31`; update to `{{cart_id}}` to use the dynamically stored value.

**GET** `{{baseUrl}}carts/31`

**Response**

Returns a cart object with product details, quantities, and totals.

**Tests**

- Status code is `200`

---

## Environment Variables

| Variable      | Set by              | Description                                         |
| ------------- | ------------------- | --------------------------------------------------- |
| `baseUrl`     | Manual              | Base URL of the API (e.g. `https://dummyjson.com/`) |
| `accessToken` | Login request       | JWT access token for authenticated calls            |
| `cart_id`     | Add to Cart request | ID of the most recently created cart                |

---

## Notes

- The **Get Cart Details** request is hardcoded to cart ID `31`. Consider updating it to `{{cart_id}}` to chain it with the **Add to Cart** request.
- The **Add Product** request uses a hardcoded URL (`https://dummyjson.com/products/add`) instead of `{{baseUrl}}`. This should be made consistent with other requests.
- The **Get Current User** request does not pass an `Authorization` header explicitly; ensure the Postman collection or environment is configured to inject the `accessToken` automatically if needed.
