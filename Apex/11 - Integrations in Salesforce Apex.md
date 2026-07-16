
Integrations allow Salesforce to communicate with external systems such as:

- ERP Systems (SAP, Oracle)
- Payment Gateways
- Banking Systems
- HR Applications
- Third-Party APIs
- External Databases

Apex provides multiple integration mechanisms including REST, SOAP, HTTP Callouts, Named Credentials, and JSON/XML processing.

---

# Integration Architecture

```text
Salesforce
     |
     |
     v
REST / SOAP
     |
     |
     v
External System
```

---

# Types of Integrations

| Type | Usage |
|---------|---------|
| REST API | Modern web services |
| SOAP API | Enterprise integrations |
| HTTP Callouts | External API communication |
| Named Credentials | Secure authentication |
| JSON Processing | REST payload handling |
| XML Processing | SOAP payload handling |

---

# 1. REST API

REST (Representational State Transfer) is the most commonly used integration approach.

Uses:

```text
HTTP Methods
JSON Payloads
URLs (Endpoints)
```

---

# REST Architecture

```text
Client
  |
HTTP Request
  |
REST Endpoint
  |
HTTP Response
```

---

# HTTP Methods

| Method | Purpose |
|----------|----------|
| GET | Retrieve data |
| POST | Create data |
| PUT | Update entire resource |
| PATCH | Partial update |
| DELETE | Delete resource |

---

# Salesforce REST Endpoint

Example:

```text
/services/data/v64.0/sobjects/Account
```

---

# Sample REST Request

```http
GET /services/data/v64.0/sobjects/Account
```

---

# REST Response

```json
{
  "Id":"001xx000001ABC",
  "Name":"ABC Corp"
}
```

---

# Creating REST Service in Apex

Use:

```apex
@RestResource
```

---

## Example

```apex
@RestResource(
    urlMapping='/accounts/*'
)
global with sharing class AccountAPI {

    @HttpGet
    global static Account getAccount(){

        String accountId =
            RestContext.request
            .requestURI
            .substringAfterLast('/');

        return
        [
            SELECT Id, Name
            FROM Account
            WHERE Id=:accountId
        ];
    }
}
```

---

# REST URL

```text
/services/apexrest/accounts/{id}
```

---

# REST Methods

---

## GET

```apex
@HttpGet
global static Account getData()
```

---

## POST

```apex
@HttpPost
global static void createData()
```

---

## PUT

```apex
@HttpPut
global static void updateData()
```

---

## DELETE

```apex
@HttpDelete
global static void deleteData()
```

---

# REST Response Status

```apex
RestContext.response.statusCode = 200;
```

Common Codes:

| Code | Meaning |
|---------|---------|
| 200 | Success |
| 201 | Created |
| 400 | Bad Request |
| 401 | Unauthorized |
| 404 | Not Found |
| 500 | Server Error |

---

# 2. SOAP API

SOAP (Simple Object Access Protocol) is XML-based.

Often used in legacy enterprise systems.

---

# SOAP Architecture

```text
Salesforce
    |
 SOAP XML
    |
External System
```

---

# Characteristics

- XML Messages
- WSDL Based
- Strongly Typed
- Enterprise Integrations

---

# WSDL

Web Service Definition Language.

Contains:

```text
Operations
Request Structure
Response Structure
```

---

# Apex SOAP Service

```apex
global class CalculatorService {

    webservice static Integer add(
        Integer a,
        Integer b
    ){

        return a + b;
    }
}
```

---

# Generate WSDL

```text
Setup
→ Apex Classes
→ Generate WSDL
```

---

# SOAP Request Example

```xml
<soapenv:Envelope>
  <soapenv:Body>
    <add>
      <a>10</a>
      <b>20</b>
    </add>
  </soapenv:Body>
</soapenv:Envelope>
```

---

# SOAP Response

```xml
<addResponse>
   <result>30</result>
</addResponse>
```

---

# Consuming SOAP Services

Use:

```text
WSDL2Apex
```

---

# WSDL2Apex

```text
Setup
→ Apex Classes
→ Generate from WSDL
```

Generates Apex proxy classes automatically.

---

# REST vs SOAP

| Feature | REST | SOAP |
|----------|----------|----------|
| Format | JSON | XML |
| Speed | Faster | Slower |
| Complexity | Low | High |
| Mobile Friendly | Yes | No |
| Enterprise Support | Good | Excellent |

---

# 3. HTTP Callouts

Callouts allow Apex to communicate with external APIs.

---

# Basic Flow

```text
Salesforce
    |
HttpRequest
    |
External API
    |
HttpResponse
```

---

# GET Callout Example

```apex
HttpRequest req =
new HttpRequest();

req.setEndpoint(
    'https://api.example.com'
);

req.setMethod('GET');

Http http =
new Http();

HttpResponse res =
http.send(req);

System.debug(
    res.getBody()
);
```

---

# POST Callout Example

```apex
HttpRequest req =
new HttpRequest();

req.setEndpoint(
    'https://api.example.com'
);

req.setMethod('POST');

req.setHeader(
    'Content-Type',
    'application/json'
);

req.setBody(
    '{"name":"John"}'
);

Http http =
new Http();

HttpResponse res =
http.send(req);
```

---

# Response Handling

```apex
Integer statusCode =
    res.getStatusCode();

String responseBody =
    res.getBody();
```

---

# Callout Exception Handling

```apex
try {

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

# Callout Limits

```text
100 Callouts Per Transaction
```

---

# Callouts in Async Apex

Use:

```apex
@future(callout=true)
```

or

```apex
Database.AllowsCallouts
```

---

# 4. Named Credentials

Named Credentials securely store endpoint URLs and authentication details.

---

# Why Use Named Credentials?

Without Named Credentials:

```apex
Hardcoded URLs
Hardcoded Tokens
Security Risks
```

---

# With Named Credentials

```apex
req.setEndpoint(
    'callout:MyAPI/accounts'
);
```

---

# Create Named Credential

```text
Setup
→ Named Credentials
→ New
```

Provide:

```text
Name
Endpoint URL
Authentication Type
```

---

# Example

Named Credential:

```text
ERP_API
```

Apex:

```apex
HttpRequest req =
new HttpRequest();

req.setEndpoint(
    'callout:ERP_API/customers'
);

req.setMethod('GET');
```

---

# Benefits

- Secure authentication
- OAuth support
- Easier maintenance
- No hardcoded credentials

---

# Supported Authentication

- OAuth 2.0
- Username Password
- JWT
- AWS Signature
- Custom Headers

---

# 5. JSON Processing

JSON is the most common format used in REST integrations.

---

# Sample JSON

```json
{
  "name":"John",
  "age":25
}
```

---

# Deserialize JSON

Convert JSON → Apex.

---

## Example

```apex
String jsonString =
'{"name":"John","age":25}';

Map<String,Object> data =
(Map<String,Object>)
JSON.deserializeUntyped(
    jsonString
);

System.debug(
    data.get('name')
);
```

Output:

```text
John
```

---

# Deserialize to Class

JSON:

```json
{
  "name":"John",
  "age":25
}
```

Class:

```apex
public class Person {

    public String name;
    public Integer age;
}
```

Deserialize:

```apex
Person p =
(Person)
JSON.deserialize(
    jsonString,
    Person.class
);
```

---

# Serialize JSON

Convert Apex → JSON.

---

## Example

```apex
Person p =
new Person();

p.name = 'John';
p.age = 25;

String jsonString =
JSON.serialize(p);
```

Output:

```json
{
  "name":"John",
  "age":25
}
```

---

# Deserialize List

```apex
List<Person> persons =
(List<Person>)
JSON.deserialize(
    jsonString,
    List<Person>.class
);
```

---

# JSONGenerator

Create JSON manually.

```apex
JSONGenerator gen =
JSON.createGenerator(
    true
);

gen.writeStartObject();

gen.writeStringField(
    'name',
    'John'
);

gen.writeEndObject();

String json =
gen.getAsString();
```

---

# 6. XML Processing

XML is commonly used in SOAP integrations.

---

# Sample XML

```xml
<Person>
   <Name>John</Name>
   <Age>25</Age>
</Person>
```

---

# Parse XML

Use:

```apex
Dom.Document
```

---

## Example

```apex
Dom.Document doc =
new Dom.Document();

doc.load(xmlString);

Dom.XmlNode root =
doc.getRootElement();

String name =
root.getChildElement(
    'Name',
    null
).getText();
```

---

# Create XML

```apex
Dom.Document doc =
new Dom.Document();

Dom.XmlNode root =
doc.createRootElement(
    'Person',
    null,
    null
);

root.addChildElement(
    'Name',
    null,
    null
).addTextNode('John');
```

---

# Integration Best Practices

---

## Use Named Credentials

❌

```apex
https://api.example.com
```

Hardcoded.

---

✅

```apex
callout:MyAPI
```

---

## Use Typed JSON Classes

Preferred:

```apex
JSON.deserialize()
```

instead of:

```apex
deserializeUntyped()
```

for predictable payloads.

---

## Handle Exceptions

```apex
try-catch
```

for all callouts.

---

## Use Async Callouts

```apex
Queueable Apex
```

or

```apex
Future Methods
```

for long-running integrations.

---

## Validate HTTP Status Codes

```apex
200
201
400
401
500
```

before processing responses.

---

# Interview Questions

### Difference Between REST and SOAP?

| REST | SOAP |
|--------|--------|
| JSON | XML |
| Faster | Slower |
| Lightweight | Heavyweight |
| Most Modern APIs | Legacy Enterprise Systems |

---

### What Is a Named Credential?

A Salesforce feature that securely stores endpoint URLs and authentication details.

---

### What Is WSDL2Apex?

A Salesforce tool that generates Apex classes from a WSDL file for SOAP integrations.

---

### Difference Between JSON.serialize() and JSON.deserialize()?

| Method | Purpose |
|----------|----------|
| serialize() | Apex → JSON |
| deserialize() | JSON → Apex |

---

### What Is the Callout Limit?

```text
100 Callouts Per Transaction
```

---

### Can a Trigger Make a Callout Directly?

❌ No

Use:

```apex
Queueable Apex
@future(callout=true)
```

---

### Which Integration Approach Is Most Common Today?

```text
REST API + Named Credentials + JSON
```

This is the standard integration architecture used in modern Salesforce implementations.

---

**Next Topic:** Security in Apex (with sharing, without sharing, CRUD/FLS, User Mode, Security.stripInaccessible, SOQL Injection Prevention).