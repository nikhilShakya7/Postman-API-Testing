# Comments API Collection Documentation

````md
# Comments API Collection

This document describes the API requests, payloads, and test validations for the `/comments` collection.

---

# Base URL

```env
baseURL=
```
````

Example:

```env
baseURL=https://jsonplaceholder.typicode.com
```

---

# Endpoints Overview

| Method | Endpoint      | Description                 |
| ------ | ------------- | --------------------------- |
| GET    | `/comments/`  | Fetch comments              |
| POST   | `/comments/`  | Create comments             |
| DELETE | `/comments/3` | Delete a comment            |
| PATCH  | `/comments/2` | Update partial comment data |
| PUT    | `/comments/1` | Replace comment data        |

---

# 1. GET Request

## Endpoint

```http
GET {{baseURL}}/comments/
```

## Query Parameters

| Parameter | Value | Description                |
| --------- | ----- | -------------------------- |
| postId    | 2     | Optional filter by post ID |

## Tests

### Validate Status Code

```javascript
pm.test("Status code is 200", function () {
  pm.expect(pm.response.code).to.eql(200);
});
```

### Validate Response Time

```javascript
pm.test("Response time is less than 200", () => {
  pm.expect(pm.response.responseTime).to.be.below(200);
});
```

---

# 2. POST Request

## Endpoint

```http
POST {{baseURL}}/comments/
```

## Request Body

```json
[
  {
    "postId": 1,
    "id": 78,
    "name": "Post",
    "email": "Post@sydney.com",
    "body": "est natu"
  },
  {
    "postId": 3,
    "id": 79,
    "name": "Post2",
    "email": "Pos2t@sydney.com",
    "body": "est2 natu"
  }
]
```

## Tests

### Validate Status Code

```javascript
pm.test("Status code is 201", function () {
  pm.expect(pm.response.code).to.eql(201);
});
```

### Validate Response Time

```javascript
pm.test("Response time is less than 500ms", function () {
  pm.expect(pm.response.responseTime).to.be.below(500);
});
```

### Validate Response Body

```javascript
pm.test("Response body contains the correct data", function () {
  pm.expect(pm.response.json()[0].postId).to.eql(1);
  pm.expect(pm.response.json()[0].id).to.eql(78);
  pm.expect(pm.response.json()[0].name).to.eql("Post");
  pm.expect(pm.response.json()[0].email).to.eql("Post@sydney.com");
  pm.expect(pm.response.json()[0].body).to.eql("est natu");

  pm.expect(pm.response.json()[1].postId).to.eql(3);
  pm.expect(pm.response.json()[1].id).to.eql(79);
  pm.expect(pm.response.json()[1].name).to.eql("Post2");
  pm.expect(pm.response.json()[1].email).to.eql("Pos2t@sydney.com");
  pm.expect(pm.response.json()[1].body).to.eql("est2 natu");

  pm.expect(pm.response.json().id).to.eql(501);
});
```

---

# 3. DELETE Request

## Endpoint

```http
DELETE {{baseURL}}/comments/3
```

## Tests

### Validate Status Code

```javascript
pm.test("Status code is 200", function () {
  pm.response.to.have.status(200);
});
```

### Validate Empty Response Body

```javascript
pm.test("Response body is empty", function () {
  pm.expect(pm.response.text()).to.eql("{}");
});
```

### Validate Response Time

```javascript
pm.test("Response time is less tan 500", () => {
  pm.expect(pm.response.responseTime).to.be.below(500);
});
```

---

# 4. PATCH Request

## Endpoint

```http
PATCH {{baseURL}}/comments/2
```

## Request Body

```json
[
  {
    "postId": 1,
    "id": 2,
    "name": "patch",
    "email": "Jayne_Kuhic@sydney.com",
    "body": "est natus enim nihil est dolore omnis volupta"
  }
]
```

## Tests

### Validate Status Code

```javascript
pm.test("Response status code is 200", function () {
  pm.response.to.have.status(200);
});
```

### Validate Response Body

```javascript
pm.test("Response body contains the correct data", function () {
  pm.expect(pm.response.json().id).to.eql(2);
  pm.expect(pm.response.json().email).to.eql("Jayne_Kuhic@sydney.com");
  pm.expect(pm.response.json().body).to.include(
    "est natus enim nihil est dolore omnis volupta",
  );
});
```

### Validate postId

```javascript
pm.test("Response body contains the correct postId", function () {
  pm.expect(pm.response.json().postId).to.eql(1);
});
```

---

# 5. PUT Request

## Endpoint

```http
PUT {{baseURL}}/comments/1
```

## Request Body

```json
{
  "postId": 1,
  "id": 1,
  "name": "put",
  "email": "put@gardner.biz",
  "body": "laudantiumte accusantium"
}
```

## Tests

### Validate Status Code

```javascript
pm.test("Status code is 200", function () {
  pm.response.to.have.status(200);
});
```

### Validate Response Time

```javascript
pm.test("Response time is less than 5000ms", function () {
  pm.expect(pm.response.responseTime).to.be.below(500);
});
```

### Validate postId

```javascript
pm.test("Response body contains the correct postId", function () {
  pm.expect(pm.response.json().postId).to.eql(1);
});
```

---

# Notes

- Ensure the `baseURL` variable is configured before executing requests.
- Response time assertions are used for basic performance validation.
- The collection uses Postman test scripts with `pm.expect()` assertions.
- PATCH request updates partial resource data.
- PUT request replaces the entire resource.

---

```

```
