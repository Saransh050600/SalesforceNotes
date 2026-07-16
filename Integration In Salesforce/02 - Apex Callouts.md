
## Overview

An Apex Callout allows Salesforce to communicate with external systems using HTTP or SOAP requests.

Common use cases:

- Payment Gateways
- ERP Systems
- Banking Systems
- External REST APIs
- Third-Party Applications

---

# What is a Callout?

A callout is an outbound request from Salesforce to an external system.

```text
Salesforce
    |
    | HTTP Request
    v
External System
    |
    | HTTP Response
    v
Salesforce
```

---

# Types of Callouts

## 1. REST Callout

Most common type.

Uses:

- JSON
- HTTP Methods

```text
GET
POST
PUT
PATCH
DELETE
```

---

## 2. SOAP Callout

Uses:

- XML
- WSDL

Generated using:

```text
WSDL2Apex
```

---

# HTTP Classes Used

| Class | Purpose |
|---------|----------|
| HttpRequest | Creates request |
| HttpResponse | Reads response |
| Http | Sends request |

---

# Basic GET Callout

```apex
HttpRequest req = new HttpRequest();

req.setEndpoint(
    'https://api.example.com/accounts'
);

req.setMethod('GET');

Http http = new Http();

HttpResponse res =
    http.send(req);

System.debug(res.getStatusCode());
System.debug(res.getBody());
```

---

# POST Callout Example

```apex
HttpRequest req = new HttpRequest();

req.setEndpoint(
    'https://api.example.com/accounts'
);

req.setMethod('POST');

req.setHeader(
    'Content-Type',
    'application/json'
);

req.setBody(
    '{"name":"Acme"}'
);

Http http = new Http();

HttpResponse res =
    http.send(req);
```

---

# Sending JSON

## Create Wrapper

```apex
public class AccountRequest {
    public String name;
    public String city;
}
```

Serialize:

```apex
AccountRequest reqObj =
    new AccountRequest();

reqObj.name = 'Acme';
reqObj.city = 'Bangalore';

String body =
    JSON.serialize(reqObj);
```

Set body:

```apex
req.setBody(body);
```

---

# Parsing JSON Response

Response:

```json
{
  "id":"123",
  "name":"Acme"
}
```

Wrapper:

```apex
public class AccountResponse {
    public String id;
    public String name;
}
```

Deserialize:

```apex
AccountResponse responseObj =
    (AccountResponse)
    JSON.deserialize(
        res.getBody(),
        AccountResponse.class
    );
```

---

# Named Credentials

Recommended approach.

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

# Remote Site Settings

Required for traditional callouts.

```text
Setup
  → Remote Site Settings
```

Add:

```text
https://api.example.com
```

### Note

Not required when using Named Credentials.

---

# Authentication Headers

## Bearer Token

```apex
req.setHeader(
    'Authorization',
    'Bearer ' + accessToken
);
```

---

## Basic Authentication

```apex
String credentials =
    EncodingUtil.base64Encode(
        Blob.valueOf(
            'username:password'
        )
    );

req.setHeader(
    'Authorization',
    'Basic ' + credentials
);
```

---

# Callouts from Triggers

Direct callouts are not recommended in triggers.

Use asynchronous processing.

### Trigger

```apex
trigger AccountTrigger on Account(
    after insert
){
    AccountService.sendData(
        Trigger.new
    );
}
```

---

# Future Callout

```apex
@future(callout=true)
public static void sendData(
    Set<Id> accountIds
){

}
```

### Limitations

- Primitive parameters only
- No chaining

---

# Queueable Callout

Preferred over Future.

```apex
public class AccountCalloutJob
implements Queueable,
           Database.AllowsCallouts {

    public void execute(
        QueueableContext qc
    ){

    }
}
```

Execute:

```apex
System.enqueueJob(
    new AccountCalloutJob()
);
```

---

# Batch Callout

Used for large data volumes.

```apex
public class AccountBatch
implements Database.Batchable<SObject>,
           Database.AllowsCallouts {

}
```

---

# AllowsCallouts Interface

Required when making callouts from:

- Queueable
- Batch Apex

```apex
Database.AllowsCallouts
```

Example:

```apex
public class MyJob
implements Queueable,
           Database.AllowsCallouts
{

}
```

---

# Timeout

Default timeout:

```text
10 Seconds
```

Custom timeout:

```apex
req.setTimeout(
    120000
);
```

Value in milliseconds.

```text
120000 = 120 seconds
```

---

# Error Handling

Always use try-catch.

```apex
try{

    HttpResponse res =
        http.send(req);

}
catch(Exception ex){

    System.debug(
        ex.getMessage()
    );
}
```

---

# Status Code Validation

```apex
if(
    res.getStatusCode() == 200
){

}
```

Common codes:

| Code | Meaning |
|---------|----------|
| 200 | Success |
| 201 | Created |
| 204 | No Content |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 500 | Server Error |

---

# Testing Callouts

Callouts are not allowed during tests.

Use Mock Classes.

---

## HttpCalloutMock

```apex
@IsTest
global class AccountMock
implements HttpCalloutMock {

    global HttpResponse respond(
        HttpRequest req
    ){

        HttpResponse res =
            new HttpResponse();

        res.setStatusCode(200);

        res.setBody(
            '{"success":true}'
        );

        return res;
    }
}
```

---

## Test Class

```apex
@IsTest
private class AccountServiceTest {

    @IsTest
    static void testCallout(){

        Test.setMock(
            HttpCalloutMock.class,
            new AccountMock()
        );

        Test.startTest();

        AccountService.sendData();

        Test.stopTest();
    }
}
```

---

# Governor Limits

| Limit | Value |
|---------|---------|
| Callouts Per Transaction | 100 |
| Maximum Timeout | 120 Seconds |
| Request Size | 6 MB |
| Response Size | 6 MB |

---

# Best Practices

### Use Named Credentials

Avoid hardcoded URLs.

---

### Use Queueable

Preferred over Future methods.

---

### Handle Failures

Implement:

- Retry Logic
- Logging
- Error Notifications

---

### Use Wrapper Classes

Avoid manual JSON parsing.

---

### Validate Response Codes

Always check:

```apex
res.getStatusCode()
```

---

# Interview Questions

## What is an Apex Callout?

An outbound request from Salesforce to an external system using HTTP or SOAP.

---

## Which classes are used for REST callouts?

```apex
HttpRequest
HttpResponse
Http
```

---

## Can we make callouts from triggers?

Yes, but preferably through:

```text
Future Method
Queueable Apex
Platform Events
```

---

## What interface is required for Queueable Callouts?

```apex
Database.AllowsCallouts
```

---

## Future vs Queueable for Callouts?

| Future | Queueable |
|----------|----------|
| Older | Modern |
| Primitive Params Only | Complex Objects Allowed |
| No Chaining | Supports Chaining |
| Limited Monitoring | Job Monitoring |

Queueable is preferred.

---

## Why use Named Credentials?

- Secure authentication
- OAuth support
- No hardcoded endpoints
- Easier maintenance

---

## How do you test callouts?

Using:

```apex
HttpCalloutMock
Test.setMock()
```

---

# Interview Summary

```text
Apex Callout = Salesforce → External System

REST Callout
SOAP Callout

HttpRequest
HttpResponse
Http

Named Credentials → Preferred

Database.AllowsCallouts
→ Queueable
→ Batch

Queueable > Future

Callouts in Tests
→ HttpCalloutMock

Max Callouts Per Transaction
→ 100
```