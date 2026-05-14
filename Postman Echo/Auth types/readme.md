# Postman Collection: Authentication Types

This repository contains a Postman collection demonstrating different authentication mechanisms used in APIs. It covers:

- Basic Authentication
- Digest Authentication
- Hawk Authentication
- OAuth 1.0

The collection uses Postman Echo endpoints to simulate and validate authentication workflows.

---

## Setup

### 1. Import Collection

- Open Postman
- Click Import
- Upload the collection JSON file

### 2. Set Environment Variables

Create the following variables:

| Variable       | Description              |
| -------------- | ------------------------ |
| BaseUrl        | API base URL             |
| basicAuth      | Password for Basic Auth  |
| digestAuth     | Password for Digest Auth |
| authId         | Hawk Auth ID             |
| authKey        | Hawk Auth Key            |
| consumerKey    | OAuth1 Consumer Key      |
| consumerSecret | OAuth1 Consumer Secret   |

Example:

BaseUrl = https://postman-echo.com/

---

## Collection Overview

### Basic Authentication

#### Request

GET {{BaseUrl}}basic-auth

Authentication:

- Type: Basic Auth
- Username: postman
- Password: {{basicAuth}}

Tests:

- Status code is 200
- Response is JSON
- Authentication flag is true

---

### Digest Authentication

#### Request

GET {{BaseUrl}}digest-auth

Authentication:

- Type: Digest Auth
- Username: postman
- Password: {{digestAuth}}
- Algorithm: MD5

Tests:

- Status code is 200
- Response time is less than 800ms
- Algorithm used is MD5

---

### Hawk Authentication

#### Request

GET {{BaseUrl}}auth/hawk

Authentication:

- Type: Hawk Auth
- Auth ID: {{authId}}
- Auth Key: {{authKey}}
- Algorithm: sha256

Tests:

- Status code is 200
- Authorization header is present
- Header contains "Hawk"

---

### OAuth 1.0

#### Request

GET {{BaseUrl}}oauth1

Authentication:

- Type: OAuth 1.0
- Signature Method: HMAC-SHA1
- Version: 1.0

Tests:

- Status code is 200
- Consumer key is defined
- Signature method is HMAC-SHA1

---

## Global Scripts

### Test Script

```javascript id="8w1mra"
pm.test("Environment Check: BaseUrl is defined", function () {
    pm.expect(pm.collectionVariables.get("BaseUrl")).to.not.be.empty;
});

console.log(`Running ${pm.info.requestName} on ${pm.request.url}`);

Purpose:

Ensures BaseUrl is set
Logs request details for debugging
Running Tests

Each request includes validation scripts using Postman test assertions.

Example:

pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
Using Postman Runner
Click Run Collection
Execute all requests sequentially
Using Newman (CLI)
npm install -g newman
newman run collection.json
Suggested Workflow
Set all required variables
Run each authentication request
Verify test results for each auth type
Project Structure
.
├── collection.json
└── README.md
Learning Objectives
Understand different API authentication mechanisms
Practice implementing authentication in Postman
Validate request headers and responses
Compare authentication strategies
```
