# Posts API Documentation

Base URL variable: `{{baseURL}}`  
All endpoints are relative to `{{baseURL}}/posts`

---

## Endpoints

### 1. GET /posts

Retrieves a list of all posts.

**Method:** `GET`  
**URL:** `{{baseURL}}/posts`  
**Authentication:** None required

#### Optional Query Parameters

| Parameter | Type    | Description                          |
| --------- | ------- | ------------------------------------ |
| `limit`   | integer | Limit the number of results returned |
| `userId`  | integer | Filter posts by user ID              |
| `id`      | integer | Filter by post ID                    |

> All query parameters are optional and disabled by default.

#### Response

Returns a JSON array of post objects. Each object contains:

| Field    | Type    | Description                         |
| -------- | ------- | ----------------------------------- |
| `id`     | integer | Unique identifier for the post      |
| `userId` | integer | ID of the user who created the post |
| `title`  | string  | Title of the post                   |
| `body`   | string  | Body/content of the post            |

#### Example Response

```json
[
  {
    "userId": 1,
    "id": 1,
    "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
    "body": "quia et suscipit\nsuscipit recusandae consequuntur expedita et cum..."
  }
]
```

#### Tests

- Status code is `200`
- Response body is an array
- Response time is less than `200ms`

---

### 2. POST /posts

Creates one or more new post resources.

**Method:** `POST`  
**URL:** `{{baseURL}}/posts`

#### Request Body

```json
[
  {
    "title": "My First Test",
    "body": "Learning API testing",
    "userId": 5
  },
  {
    "title": "My second Test",
    "body": "Learning API testing2",
    "userId": 2
  }
]
```

#### Tests

- Status code is `201`
- Response time is less than `300ms`

---

### 3. PUT /posts/:id

Fully replaces an existing post by ID.

**Method:** `PUT`  
**URL:** `{{baseURL}}/posts/3`

#### Request Body

```json
{
  "userId": 100,
  "id": 3,
  "title": "Put request",
  "body": "This is a put"
}
```

> All fields must be provided. The entire resource is replaced.

---

### 4. PATCH /posts/:id

Partially updates an existing post by ID.

**Method:** `PATCH`  
**URL:** `{{baseURL}}/posts/1`

#### Request Body

```json
[
  {
    "userId": 4,
    "id": 1,
    "title": "PATCH REQUEST",
    "body": "quia et suscipit\nsuscipit recusandae consequuntur expedita et cum..."
  }
]
```

> Only the fields provided will be updated.

---

### 5. DELETE /posts/:id

Deletes a post by ID.

**Method:** `DELETE`  
**URL:** `{{baseURL}}/posts/2`

#### Tests

- Status code is `200`
- Response body is empty (`{}`)
- Response time is less than `300ms`

---

## Summary Table

| Method | Endpoint     | Description                   | Expected Status |
| ------ | ------------ | ----------------------------- | --------------- |
| GET    | `/posts`     | Retrieve all posts            | `200`           |
| POST   | `/posts`     | Create one or more posts      | `201`           |
| PUT    | `/posts/:id` | Fully replace a post by ID    | `200`           |
| PATCH  | `/posts/:id` | Partially update a post by ID | `200`           |
| DELETE | `/posts/:id` | Delete a post by ID           | `200`           |
