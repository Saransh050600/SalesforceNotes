
## Overview

Composite API allows multiple REST API operations to be executed in a single HTTP request.

Instead of making multiple API calls, Salesforce processes multiple operations together.

```text
Without Composite API

Client
 ├── API Call 1
 ├── API Call 2
 ├── API Call 3
 └── API Call 4

Total = 4 API Calls
```

```text
With Composite API

Client
    |
    v
Composite API Request
    |
    v
Salesforce

Total = 1 API Call
```

---

# Why Composite API?

Benefits:

- Reduce API Calls
- Improve Performance
- Reduce Network Overhead
- Support Parent-Child Creation
- Support Dependent Requests

---

# Composite API Family

Salesforce provides four APIs:

```text
1. Composite API
2. Composite Batch API
3. Composite Tree API
4. Composite Graph API
```

---

# 1. Composite API

Allows:

- Multiple Requests
- Request Dependencies
- Reference Previous Responses

Most flexible option.

---

## Endpoint

```http
/services/data/v65.0/composite
```

---

## Example

### Create Account

```json
{
  "method": "POST",
  "url": "/services/data/v65.0/sobjects/Account",
  "referenceId": "newAccount",
  "body": {
    "Name": "Acme"
  }
}
```

### Create Contact Using Account Id

```json
{
  "method": "POST",
  "url": "/services/data/v65.0/sobjects/Contact",
  "referenceId": "newContact",
  "body": {
    "LastName": "Smith",
    "AccountId": "@{newAccount.id}"
  }
}
```

---

## Full Request

```json
{
  "compositeRequest": [
    {
      "method": "POST",
      "url": "/services/data/v65.0/sobjects/Account",
      "referenceId": "newAccount",
      "body": {
        "Name": "Acme"
      }
    },
    {
      "method": "POST",
      "url": "/services/data/v65.0/sobjects/Contact",
      "referenceId": "newContact",
      "body": {
        "LastName": "Smith",
        "AccountId": "@{newAccount.id}"
      }
    }
  ]
}
```

---

# Reference IDs

Reference IDs allow later requests to use previous results.

Example:

```text
@{newAccount.id}
```

Meaning:

```text
Use the Account Id
created in Request #1
```

---

# allOrNone

Controls transaction behavior.

```json
{
  "allOrNone": true
}
```

---

## allOrNone = true

```text
If one request fails
Everything rolls back.
```

---

## allOrNone = false

```text
Successful requests commit.

Failed requests rollback.
```

---

# 2. Composite Batch API

Used to group independent API requests.

No dependency between requests.

---

## Endpoint

```http
/services/data/v65.0/composite/batch
```

---

## Example

```json
{
  "batchRequests": [
    {
      "method": "GET",
      "url": "v65.0/sobjects/Account"
    },
    {
      "method": "GET",
      "url": "v65.0/sobjects/Contact"
    }
  ]
}
```

---

# Composite Batch Characteristics

```text
Independent Requests

No Reference IDs

No Dependency Support
```

---

# When to Use Batch?

Example:

```text
Get Accounts
Get Contacts
Get Opportunities
```

No relationship required.

---

# 3. Composite Tree API

Creates parent-child records in a single request.

---

## Endpoint

```http
/services/data/v65.0/composite/tree
```

---

## Example

Create:

```text
Account
 ├── Contact
 ├── Contact
 └── Contact
```

Single request.

---

## Request Example

```json
{
  "records": [
    {
      "attributes": {
        "type": "Account",
        "referenceId": "AccountRef1"
      },
      "Name": "Acme",
      "Contacts": {
        "records": [
          {
            "attributes": {
              "type": "Contact",
              "referenceId": "ContactRef1"
            },
            "LastName": "Smith"
          }
        ]
      }
    }
  ]
}
```

---

# Tree API Characteristics

```text
Parent Child Creation

Single Object Hierarchy

Insert Only
```

---

# When to Use Tree API?

Example:

```text
Create Account

Create Contacts

Link Contacts to Account
```

All in one request.

---

# 4. Composite Graph API

Most advanced Composite API.

Supports:

- Complex Relationships
- Multiple Dependency Chains
- Large Transactions

---

## Endpoint

```http
/services/data/v65.0/composite/graph
```

---

# Example

```text
Account
    |
    v
Contact
    |
    v
Case
    |
    v
Task
```

Single Graph Request.

---

# Graph API Benefits

```text
Complex Dependencies

Better Organization

Multiple Composite Requests
```

---

# Composite API Comparison

| Feature | Composite | Batch | Tree | Graph |
|----------|----------|----------|----------|----------|
| Multiple Requests | Yes | Yes | Yes | Yes |
| Dependencies | Yes | No | Parent-Child | Yes |
| Reference IDs | Yes | No | Yes | Yes |
| Parent-Child Insert | Limited | No | Yes | Yes |
| Complex Transactions | Medium | No | No | Yes |

---

# Composite API Limits

Interview-level knowledge:

```text
Up to 25 Subrequests

Reference IDs Supported

Single API Call Consumption
```

---

# Common Use Cases

## Mobile Applications

```text
Load Multiple Datasets
```

Single request.

---

## Integration Middleware

```text
Account
Contact
Opportunity

Created Together
```

---

## Parent-Child Creation

```text
Account
    |
    ├── Contact
    └── Contact
```

Use Tree API.

---

## Complex Business Processes

```text
Account
Contact
Case
Task
Opportunity
```

Use Graph API.

---

# Composite API vs REST API

| REST API | Composite API |
|-----------|-----------|
| Multiple Calls | Single Call |
| More Network Traffic | Less Network Traffic |
| Slower | Faster |
| Simple Operations | Complex Operations |

---

# Composite API vs Bulk API

| Composite API | Bulk API |
|---------------|-----------|
| Real-Time | Asynchronous |
| Small-Medium Volume | Large Volume |
| Complex Relationships | Mass Data Processing |
| Immediate Response | Background Processing |

---

# Best Practices

## Use Composite API

When multiple operations belong together.

---

## Use Batch API

For unrelated requests.

---

## Use Tree API

For parent-child inserts.

---

## Use Graph API

For complex dependency chains.

---

## Use Bulk API

For millions of records.

---

# Common Interview Questions

## What is Composite API?

A Salesforce REST API that allows multiple operations to be executed in a single HTTP request.

---

## Why Use Composite API?

```text
Reduce API Calls

Improve Performance

Support Dependencies
```

---

## Difference Between Composite and Batch API?

| Composite | Batch |
|------------|--------|
| Dependencies Supported | No Dependencies |
| Reference IDs | No Reference IDs |

---

## What is Tree API?

Used to create parent-child records in one request.

Example:

```text
Account + Contacts
```

---

## What is Graph API?

An advanced Composite API used for complex transactions with multiple dependency chains.

---

## What is allOrNone?

```text
true
```

All requests succeed or all rollback.

```text
false
```

Partial success allowed.

---

## Composite API vs Bulk API?

```text
Composite API
→ Real-Time Operations

Bulk API
→ Large Data Volumes
```

---

# Interview Summary

```text
Composite API
=
Multiple REST Operations
in One Request

Types:

1. Composite API
   → Dependencies

2. Batch API
   → Independent Requests

3. Tree API
   → Parent-Child Inserts

4. Graph API
   → Complex Transactions

Key Features:

Reference IDs

allOrNone

Reduced API Calls

Improved Performance

Most Asked:

Composite vs Batch

Tree API

Graph API

allOrNone

Composite vs Bulk API
```