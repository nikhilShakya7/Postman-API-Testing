# Photos API Documentation

Base URL variable: `{{baseURL}}`  
All endpoints are relative to `{{baseURL}}/photos`

---

## Endpoints

### 1. GET /photos

Retrieves a list of all photos.

**Method:** `GET`  
**URL:** `{{baseURL}}/photos`

#### Response

Returns an array of photo objects. Each object contains:

| Field          | Type    | Description                               |
| -------------- | ------- | ----------------------------------------- |
| `albumId`      | integer | The ID of the album the photo belongs to  |
| `id`           | integer | Unique identifier for the photo           |
| `title`        | string  | Title/description of the photo            |
| `url`          | string  | URL to the full-size photo                |
| `thumbnailUrl` | string  | URL to the thumbnail version of the photo |

#### Tests

- Status code is `200`
- Response time is less than `200ms`

---

### 2. POST /photos

Creates a new photo resource.

**Method:** `POST`  
**URL:** `{{baseURL}}/photos`

#### Request Body

```json
{
  "albumId": 8,
  "title": "posts aut",
  "url": "https://via.placeholder.com/600/9ba35f",
  "thumbnailUrl": "https://via.placeholder.com/150/9ba35f"
}
```

#### Tests

- Status code is `201`
- Response time is less than `500ms`
- Response body contains `albumId` equal to `8`

---

### 3. PUT /photos/:id

Updates an existing photo by ID.

**Method:** `PUT`  
**URL:** `{{baseURL}}/photos/1`

#### Request Body

```json
{
  "albumId": 3,
  "id": 1,
  "title": "put",
  "url": "<image-url>",
  "thumbnailUrl": "<thumbnail-url>"
}
```

#### Tests

- Status code is `200`
- Response time is less than `700ms`
- Response body contains `albumId` equal to `3`

---

### 4. DELETE /photos/:id

Deletes a photo by ID.

**Method:** `DELETE`  
**URL:** `{{baseURL}}/photos/1`

#### Tests

- Status code is `200`
- Response body is empty (`{}`)
- Response time is less than `500ms`

---

## Summary Table

| Method | Endpoint      | Description          | Expected Status |
| ------ | ------------- | -------------------- | --------------- |
| GET    | `/photos`     | Retrieve all photos  | `200`           |
| POST   | `/photos`     | Create a new photo   | `201`           |
| PUT    | `/photos/:id` | Update a photo by ID | `200`           |
| DELETE | `/photos/:id` | Delete a photo by ID | `200`           |
