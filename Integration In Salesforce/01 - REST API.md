
## Overview

REST (Representational State Transfer) API is the most commonly used Salesforce integration mechanism.

It allows external applications to interact with Salesforce using standard HTTP methods and JSON payloads.

### Base URL

```text
https://yourInstance.salesforce.com/services/data/vXX.X/
```

Example:

```text
https://mydomain.my.salesforce.com/services/data/v65.0/
```

---

# Why REST API?

### Advantages

- Lightweight
- Uses JSON
- Easy to consume
- Language independent
- Faster than SOAP
- Widely supported

### Common Use Cases

- Mobile Applications
- Web Applications
- External System Integrations
- Middleware Integrations
- Data Synchronization

---

# REST Architecture

```text
Client
   |
   | HTTP Request
   v
Salesforce REST API
   |
   | JSON Response
   v
Client
```

---

# Authentication

Before calling Salesforce REST APIs, authentication is required.

Common methods:

### OAuth 2.0

Recommended approach.

### Connected App

Used to obtain Access Tokens.

Example:

```text
Client
   |
   v
Connected App
   |
   v
Access Token
   |
   v
REST API Calls
```

---

# HTTP Methods

## GET

Retrieve records.

```http
GET /services/data/v65.0/sobjects/Account
```

### Example

```http
GET /services/data/v65.0/sobjects/Account/001XXXXXXXXXXXX
```

---

## POST

Create records.

```http
POST /services/data/v65.0/sobjects/Account
```

Request Body:

```json
{
    "Name": "Acme Corp"
}
```

Response:

```json
{
    "id": "001XXXXXXXXXXXX",
    "success": true,
    "errors": []
}
```

---

## PATCH

Update records.

```http
PATCH /services/data/v65.0/sobjects/Account/001XXXXXXXXXXXX
```

Request Body:

```json
{
    "Name": "Updated Account"
}
```

Response:

```http
204 No Content
```

---

## DELETE

Delete records.

```http
DELETE /services/data/v65.0/sobjects/Account/001XXXXXXXXXXXX
```

Response:

```http
204 No Content
```

---

# REST API Resources

## SObject Resource

Work with Salesforce records.

### Get Metadata

```http
GET /services/data/v65.0/sobjects
```

### Get Record

```http
GET /services/data/v65.0/sobjects/Account/{Id}
```

### Create Record

```http
POST /services/data/v65.0/sobjects/Account
```

---

## Query Resource

Execute SOQL.

### Endpoint

```http
GET /services/data/v65.0/query?q=
```

Example:

```http
GET /services/data/v65.0/query?q=SELECT+Id,Name+FROM+Account
```

Response:

```json
{
    "totalSize": 1,
    "done": true,
    "records": [
        {
            "Id": "001XXXXXXXXXXXX",
            "Name": "Acme Corp"
        }
    ]
}
```

---

## Search Resource

Execute SOSL.

Example:

```http
GET /services/data/v65.0/search?q=FIND+{Acme}
```

---

# Apex REST API

Salesforce can expose custom REST endpoints.

## Example

```apex
@RestResource(urlMapping='/account/*')
global with sharing class AccountAPI {

    @HttpGet
    global static Account getAccount() {

        String accountId =
            RestContext.request.requestURI
            .substringAfterLast('/');

        return [
            SELECT Id, Name
            FROM Account
            WHERE Id = :accountId
            LIMIT 1
        ];
    }
}
```

Endpoint:

```text
/services/apexrest/account/{AccountId}
```

---

# HTTP Annotations

| Annotation | Purpose |
|------------|----------|
| @HttpGet | Retrieve data |
| @HttpPost | Create data |
| @HttpPatch | Update data |
| @HttpDelete | Delete data |
| @HttpPut | Insert or Update |

Example:

```apex
@HttpPost
global static void createAccount() {

}
```

---

# Consuming External REST APIs from Apex

## Callout Example

```apex
HttpRequest req = new HttpRequest();

req.setEndpoint(
    'https://api.example.com/accounts'
);

req.setMethod('GET');

Http http = new Http();

HttpResponse res =
    http.send(req);

System.debug(res.getBody());
```

---

# Named Credential

Recommended for all integrations.

Instead of:

```apex
req.setEndpoint(
    'https://api.example.com/accounts'
);
```

Use:

```apex
req.setEndpoint(
    'callout:MyAPI/accounts'
);
```

Benefits:

- Secure
- OAuth Support
- No Hardcoded URLs
- Easier Maintenance

---

# REST API Limits

REST API usage counts against API limits.

Check usage:

```http
GET /services/data/v65.0/limits
```

Example Response:

```json
{
    "DailyApiRequests": {
        "Max": 100000,
        "Remaining": 95000
    }
}
```

---

# REST API Error Codes

| Code | Meaning |
|--------|----------|
| 200 | Success |
| 201 | Created |
| 204 | No Content |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 500 | Server Error |

---

# REST API vs SOAP API

| Feature | REST | SOAP |
|----------|--------|--------|
| Format | JSON | XML |
| Speed | Faster | Slower |
| Complexity | Simple | Complex |
| WSDL Required | No | Yes |
| Mobile Friendly | Yes | No |
| Most Common | Yes | No |

---

# Interview Questions

## What is REST API?

A lightweight web service architecture that uses HTTP methods and JSON payloads to interact with Salesforce.

---

## Difference between REST and SOAP?

REST uses JSON and standard HTTP methods while SOAP uses XML and WSDL-based contracts.

---

## Which HTTP method is used to update records?

```text
PATCH
```

---

## Which HTTP method is used to create records?

```text
POST
```

---

## How do you expose custom REST APIs in Salesforce?

Using:

```apex
@RestResource
```

and HTTP method annotations such as:

```apex
@HttpGet
@HttpPost
@HttpPatch
@HttpDelete
```

---

## What is the best way to authenticate REST API calls?

```text
OAuth 2.0 using Connected Apps
```

---

## Why use Named Credentials?

- Secure authentication
- Endpoint management
- No hardcoded URLs
- Easier maintenance

---

# Interview Summary

### Must Remember

```text
REST = JSON + HTTP

GET    → Read
POST   → Create
PATCH  → Update
DELETE → Delete

@RestResource → Expose APIs

HttpRequest → Consume APIs

Named Credentials → Secure Callouts

OAuth + Connected App → Authentication

SOQL → Query Resource
SOSL → Search Resource
```