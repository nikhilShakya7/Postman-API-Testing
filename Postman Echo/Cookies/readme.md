# Postman Collection: Cookies and Utility Endpoints

This repository contains a Postman collection demonstrating how to:

- Manage cookies (set, get, delete)
- Validate HTTP responses
- Work with compressed responses
- Handle streaming and encoded data

It uses the Postman Echo API for testing and learning purposes.

---

## Setup

### 1. Import Collection

- Open Postman
- Click Import
- Upload the collection JSON file

### 2. Set Environment Variable

Create an environment variable:

| Variable | Value                     |
| -------- | ------------------------- |
| BaseUrl  | https://postman-echo.com/ |

---

## Collection Overview

### Cookie Management

#### Set Cookies

GET {{BaseUrl}}cookies/set?foo1=bar1&foo2=bar2&foo3=bar3&foo4=bar4

Tests:

- Status code is 200
- Cookies `foo1` and `foo3` exist in response

---

#### Get Cookies

GET {{BaseUrl}}cookies

Description:
Retrieves all cookies in the current session.

Example response:

```json
{
  "cookies": {
    "foo1": "bar1",
    "foo2": "bar2"
  }
}

Tests:

Status code is 200
Cookies object is not empty
Delete Cookies
GET {{BaseUrl}}cookies/delete?foo1&foo2

Tests:

foo1 and foo2 are removed
Utility Endpoints
Response Status
GET {{BaseUrl}}status/200
Verifies HTTP 200 response
Stream Response
GET {{BaseUrl}}stream/5
Returns streamed JSON data
Status check included
UTF-8 Encoded Response
GET {{BaseUrl}}encoding/utf8

Purpose:

Validate UTF-8 encoding handling
Displays Unicode characters
GZip Response
GET {{BaseUrl}}gzip

Tests:

Status is 200
Content-Encoding is either gzip or deflate
Deflate Response
GET {{BaseUrl}}deflate

Tests:

Status is 200
Content-Encoding is valid
Get IP Address
GET {{BaseUrl}}ip

Returns the public IP address of the requester.

Get Current Time
GET {{BaseUrl}}time/now

Authentication:

JWT (configured in Postman)

Returns the current UTC time.

Running Tests

Each request includes Postman test scripts such as:

pm.test("Status is 200 OK", () => pm.response.to.have.status(200));
Using Postman Runner
Click Run Collection
Execute all requests in sequence
Using Newman (CLI)
npm install -g newman
newman run collection.json
Suggested Workflow
Set BaseUrl
Run requests in order:
Set Cookies
Get Cookies
Delete Cookies
Explore utility endpoints
Project Structure
.
├── collection.json
└── README.md
Learning Goals
Understand cookie lifecycle in APIs
Practice API testing with Postman
Validate headers and response formats
Work with compressed and streamed responses
```
