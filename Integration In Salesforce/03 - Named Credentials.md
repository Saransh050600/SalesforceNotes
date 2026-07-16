
## Overview

Named Credentials provide a secure and centralized way to store:

- Endpoint URLs
- Authentication Settings
- OAuth Configuration
- External Credentials

Instead of hardcoding URLs and authentication details in Apex, Salesforce manages them securely.

---

# Why Named Credentials?

## Without Named Credentials

```apex
HttpRequest req = new HttpRequest();

req.setEndpoint(
    'https://api.example.com/accounts'
);

req.setHeader(
    'Authorization',
    'Bearer xxxxxxxxx'
);
```

Problems:

- Hardcoded URL
- Hardcoded Authentication
- Difficult Maintenance
- Security Risks

---

## With Named Credentials

```apex
HttpRequest req = new HttpRequest();

req.setEndpoint(
    'callout:MyAPI/accounts'
);
```

Benefits:

- No hardcoded URLs
- No hardcoded tokens
- Secure authentication
- Easier maintenance
- OAuth support

---

# Architecture

```text
Apex
  |
  | callout:MyAPI
  |
  v
Named Credential
  |
  v
External Credential
  |
  v
Authentication
  |
  v
External System
```

---

# Components

## 1. Named Credential

Stores:

- Endpoint URL
- Connection Configuration

Example:

```text
MyAPI
https://api.example.com
```

---

## 2. External Credential

Stores:

- Authentication Protocol
- Identity Information

Examples:

```text
OAuth 2.0
JWT
AWS Signature
Custom Header
```

---

## 3. Principal

Defines who is authenticated.

Types:

### Named Principal

Single shared identity.

```text
All users share same credentials.
```

---

### Per User Principal

Each user authenticates separately.

```text
Each user has own access token.
```

---

# Creating a Named Credential

## Step 1

Navigate to:

```text
Setup
→ Named Credentials
→ New
```

---

## Step 2

Provide:

```text
Label:
My API

Name:
MyAPI

URL:
https://api.example.com
```

---

## Step 3

Select:

```text
External Credential
```

---

## Step 4

Save.

---

# Using Named Credential in Apex

## GET Request

```apex
HttpRequest req =
    new HttpRequest();

req.setEndpoint(
    'callout:MyAPI/accounts'
);

req.setMethod('GET');

Http http = new Http();

HttpResponse res =
    http.send(req);
```

Salesforce automatically:

- Resolves URL
- Adds Authentication
- Sends Request

---

# POST Request

```apex
HttpRequest req =
    new HttpRequest();

req.setEndpoint(
    'callout:MyAPI/accounts'
);

req.setMethod('POST');

req.setHeader(
    'Content-Type',
    'application/json'
);

req.setBody(
    '{"name":"Acme"}'
);

Http http =
    new Http();

HttpResponse res =
    http.send(req);
```

---

# OAuth Authentication

Most common use case.

```text
Salesforce
    |
    v
Named Credential
    |
    v
OAuth Flow
    |
    v
Access Token
    |
    v
External API
```

Salesforce automatically:

- Requests Token
- Refreshes Token
- Stores Token Securely

---

# Supported Authentication Types

## OAuth 2.0

Most common.

```text
Google
Microsoft
Salesforce
SAP
```

---

## JWT

Server-to-Server Integration.

```text
No user interaction
```

---

## AWS Signature V4

Used for:

```text
Amazon S3
AWS APIs
```

---

## Custom Header

Example:

```text
API Key
```

Header:

```http
x-api-key: abc123
```

---

## Basic Authentication

```http
Authorization:
Basic xxxxxxx
```

---

# Named Principal vs Per User

| Feature | Named Principal | Per User |
|----------|----------|----------|
| Authentication | Shared | Individual |
| Access Token | Single | Multiple |
| Maintenance | Easier | More Complex |
| Most Common | Yes | No |

---

## Named Principal

```text
Salesforce User 1
Salesforce User 2
Salesforce User 3
       |
       v
Same External Account
```

Example:

```text
ERP Integration
```

---

## Per User

```text
Salesforce User 1
       |
       v
Google User 1

Salesforce User 2
       |
       v
Google User 2
```

Example:

```text
Google Calendar Integration
```

---

# Callout Syntax

## Correct

```apex
req.setEndpoint(
    'callout:MyAPI/accounts'
);
```

Format:

```text
callout:NamedCredentialName/path
```

---

# Remote Site Settings

## Old Approach

```text
Setup
→ Remote Site Settings
```

Required for traditional callouts.

---

## Modern Approach

```text
Named Credentials
```

Remote Site Settings are NOT required.

---

# Security Benefits

### Credentials Hidden

Developers cannot see passwords or tokens.

---

### Secure Storage

Salesforce encrypts credentials.

---

### Access Control

Controlled using:

```text
Permission Sets
```

---

### Token Refresh

Handled automatically.

---

# Testing Callouts

Named Credentials work with:

```apex
HttpCalloutMock
```

No special test setup required.

```apex
Test.setMock(
    HttpCalloutMock.class,
    new MyMock()
);
```

---

# Common Interview Questions

## What is a Named Credential?

A secure Salesforce configuration that stores endpoint URLs and authentication information for external integrations.

---

## Why use Named Credentials?

- Better security
- No hardcoded URLs
- No hardcoded tokens
- OAuth support
- Easier maintenance

---

## Difference Between Remote Site Settings and Named Credentials?

| Remote Site Settings | Named Credentials |
|---------------------|-------------------|
| URL Whitelisting Only | URL + Authentication |
| Manual Auth Handling | Automatic Auth Handling |
| Older Approach | Recommended Approach |
| Less Secure | More Secure |

---

## What is an External Credential?

Stores authentication details and identity information used by Named Credentials.

---

## What is Named Principal?

A single shared authentication identity used by all Salesforce users.

---

## What is Per User Authentication?

Each Salesforce user authenticates with their own credentials.

---

## Do Named Credentials Support OAuth?

```text
Yes
```

Salesforce automatically:

- Obtains Access Tokens
- Refreshes Tokens
- Stores Tokens

---

## Is Remote Site Setting Required with Named Credentials?

```text
No
```

---

# Best Practices

### Always Use Named Credentials

Avoid hardcoded URLs and tokens.

---

### Use OAuth Whenever Possible

Avoid storing usernames and passwords.

---

### Use Named Principal

For system-to-system integrations.

---

### Use Per User

Only when external systems require user-specific access.

---

### Restrict Access

Use Permission Sets.

---

# Interview Summary

```text
Named Credential
=
Endpoint URL
+
Authentication

Components:
1. Named Credential
2. External Credential
3. Principal

Authentication Types:
- OAuth 2.0
- JWT
- AWS Signature
- Basic Auth
- API Key

Principal Types:
- Named Principal
- Per User

Benefits:
- Secure
- No Hardcoded URLs
- No Hardcoded Tokens
- Automatic Token Refresh

Callout Syntax:
callout:MyAPI/resource

Named Credentials
replace
Remote Site Settings
for most integrations.
```