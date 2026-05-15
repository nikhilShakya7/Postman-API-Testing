# Auth Types — Postman Collection Documentation

## Overview

This collection covers testing of four authentication mechanisms against a configurable base API. Each folder isolates one auth type, applies the corresponding Postman auth config at the folder level, and includes request-level tests to verify correct behavior.

---

## Collection Variables

These variables must be set before running the collection. They are referenced using `{{variable}}` syntax throughout.

| Variable         | Description                                                   |
| ---------------- | ------------------------------------------------------------- |
| `BaseUrl`        | Base URL for all requests (e.g., `https://postman-echo.com/`) |
| `basicAuth`      | Password for Basic Auth                                       |
| `digestAuth`     | Password for Digest Auth                                      |
| `authId`         | Auth ID for Hawk Auth                                         |
| `authKey`        | Auth Key (secret) for Hawk Auth                               |
| `consumerKey`    | Consumer Key for OAuth 1.0                                    |
| `consumerSecret` | Consumer Secret for OAuth 1.0                                 |

> **Note:** All variable values are empty by default. Populate them in your Postman Environment or Collection Variables panel before running.

---

## Collection-Level Tests

These tests run after **every** request in the collection.

| Test                                    | Description                                                           |
| --------------------------------------- | --------------------------------------------------------------------- |
| `Environment Check: BaseUrl is defined` | Asserts that `BaseUrl` collection variable is not empty               |
| Debug logging                           | Logs the request name and URL to the Postman console for traceability |

---

## Folders

### 1. Basic Auth

**Endpoint:** `GET {{BaseUrl}}basic-auth`

**Auth Config:**

- Type: `basic`
- Username: `postman`
- Password: `{{basicAuth}}`

**Tests:**

| Test                         | Assertion                          |
| ---------------------------- | ---------------------------------- |
| `Status code is 200`         | Response status equals `200 OK`    |
| `Response is JSON`           | Response body is valid JSON        |
| `Authenticated successfully` | `jsonData.authenticated` is `true` |

**How it works:** Postman encodes the `username:password` pair in Base64 and sends it in the `Authorization: Basic <token>` header. The server validates the credentials and returns a JSON body confirming authentication.

---

### 2. Digest Auth

**Endpoint:** `GET {{BaseUrl}}digest-auth`

**Auth Config:**

- Type: `digest`
- Username: `postman`
- Password: `{{digestAuth}}`
- Algorithm: `MD5`

**Tests:**

| Test                                  | Assertion                                                |
| ------------------------------------- | -------------------------------------------------------- |
| `Digest Auth successful`              | Response status equals `200`                             |
| `Response time is less than 800ms`    | Response time is below `800ms`                           |
| `Verify MD5 Algorithm used in header` | `pm.request.auth.digest.get("algorithm")` equals `"MD5"` |

**How it works:** Digest Auth involves a two-step handshake. The server first returns a `401` with a `WWW-Authenticate: Digest` challenge. Postman then hashes credentials using the specified algorithm (MD5 here) and retries with a computed `Authorization` header. The response time test validates that this handshake completes efficiently.

---

### 3. Hawk Auth

**Endpoint:** `GET {{BaseUrl}}auth/hawk`

**Auth Config:**

- Type: `hawk`
- Auth ID: `{{authId}}`
- Auth Key: `{{authKey}}`
- Algorithm: `sha256`

**Tests:**

| Test                              | Assertion                                                                 |
| --------------------------------- | ------------------------------------------------------------------------- |
| `Hawk Auth request successful`    | Response status equals `200`                                              |
| `Authorization header is present` | Request includes an `Authorization` header containing the string `"Hawk"` |

**How it works:** Hawk is an HTTP authentication scheme using a MAC (Message Authentication Code). Postman generates a time-limited signature from the auth ID, key, request method, URL, and a timestamp using SHA-256, then sends it in the `Authorization: Hawk ...` header.

---

### 4. OAuth 1.0

**Endpoint:** `GET {{BaseUrl}}oauth1`

**Auth Config (set at folder level):**

- Type: `oauth1`
- Consumer Key: `{{consumerKey}}`
- Consumer Secret: `{{consumerSecret}}`
- Signature Method: `HMAC-SHA1`
- Version: `1.0`
- Add params to header: `false`
- Add empty params to sign: `false`

**Tests:**

| Test                            | Assertion                                                      |
| ------------------------------- | -------------------------------------------------------------- |
| `OAuth 1.0 status check`        | Response status equals `200`                                   |
| `Consumer Key is not empty`     | `pm.collectionVariables.get("consumerKey")` is not `undefined` |
| `Signature method is HMAC-SHA1` | `signatureMethod` in auth data equals `"HMAC-SHA1"`            |

**How it works:** OAuth 1.0 signs each request using the consumer key/secret pair and a calculated signature. Here, the signature is generated with HMAC-SHA1 and appended to the request (as query params, since `addParamsToHeader` is `false`). The test confirms the consumer key variable is populated and the correct signature algorithm is configured.

---

## Running the Collection

1. Clone or import the collection JSON into Postman.
2. Set all [Collection Variables](#collection-variables) to valid values.
3. Run the full collection via **Collection Runner** or individual folders as needed.
4. Review test results — all tests should pass with a correctly configured server and valid credentials.

---

## Auth Type Comparison

| Feature               | Basic Auth     | Digest Auth         | Hawk Auth            | OAuth 1.0                       |
| --------------------- | -------------- | ------------------- | -------------------- | ------------------------------- |
| Credentials sent      | Base64 encoded | Hashed (MD5)        | Signed MAC (SHA-256) | Signed (HMAC-SHA1)              |
| Replay protection     | No             | Partial (nonce)     | Yes (timestamp)      | Yes (nonce + timestamp)         |
| Server challenge      | No             | Yes (401 handshake) | No                   | No                              |
| Postman variable used | `basicAuth`    | `digestAuth`        | `authId`, `authKey`  | `consumerKey`, `consumerSecret` |
