
## Overview

OAuth is the standard authentication and authorization framework used by Salesforce integrations.

A Connected App is a Salesforce application that enables external systems to authenticate and access Salesforce APIs.

```text
External Application
        |
        v
   Connected App
        |
        v
      OAuth
        |
        v
   Access Token
        |
        v
   Salesforce APIs
```

---

# Why OAuth?

Without OAuth:

```text
Username + Password
```

Problems:

- Security Risk
- Password Changes Break Integrations
- Difficult Maintenance

OAuth provides:

- Secure Authentication
- Token-Based Access
- Controlled Permissions
- Token Expiration
- Token Revocation

---

# Key OAuth Components

## Resource Owner

User who owns the data.

```text
Salesforce User
```

---

## Client Application

Application requesting access.

Examples:

```text
Mobile App
Web App
ERP System
Middleware
```

---

## Authorization Server

Authenticates users and issues tokens.

```text
Salesforce
```

---

## Resource Server

Hosts protected resources.

```text
Salesforce APIs
```

---

# OAuth Tokens

## Access Token

Used to call Salesforce APIs.

Example:

```http
Authorization: Bearer 00Dxxxxxxxx
```

Characteristics:

- Short-lived
- Used in API requests

---

## Refresh Token

Used to obtain a new Access Token.

Characteristics:

- Long-lived
- More secure than storing passwords

---

# Connected App

## What is a Connected App?

A framework that allows external applications to integrate with Salesforce using OAuth.

Used for:

- REST API
- SOAP API
- Mobile Apps
- External Applications

---

# Creating a Connected App

## Step 1

Navigate to:

```text
Setup
→ App Manager
→ New Connected App
```

---

## Step 2

Provide:

```text
Connected App Name
API Name
Contact Email
```

---

## Step 3

Enable OAuth

```text
Enable OAuth Settings
```

---

## Step 4

Configure Callback URL

Example:

```text
https://myapp.com/callback
```

---

## Step 5

Select OAuth Scopes

Common scopes:

```text
Access and manage your data (api)

Perform requests at any time
(refresh_token, offline_access)

Full Access (full)
```

---

## Step 6

Save.

Salesforce generates:

```text
Consumer Key
Consumer Secret
```

---

# Consumer Key vs Consumer Secret

| Item | Purpose |
|--------|----------|
| Consumer Key | Client Identifier |
| Consumer Secret | Client Password |

Example:

```text
Consumer Key:
3MVG9Abcd...

Consumer Secret:
123XYZ...
```

---

# OAuth Authorization Flow

```text
User
 |
 v
Connected App
 |
 v
Login Page
 |
 v
Authorization Code
 |
 v
Access Token
 |
 v
Salesforce API
```

---

# OAuth Flows

## 1. Web Server Flow

Most common OAuth flow.

### Used For

```text
Web Applications
```

### Process

```text
User Login
     ↓
Authorization Code
     ↓
Access Token
     ↓
API Access
```

### Characteristics

- Interactive Login
- Most Secure
- Supports Refresh Tokens

### Interview Favorite

Most common OAuth flow.

---

# 2. JWT Bearer Flow

Used for server-to-server integrations.

### Process

```text
System
   |
   v
Signed JWT
   |
   v
Salesforce
   |
   v
Access Token
```

### Characteristics

- No User Interaction
- Highly Secure
- Certificate Based

### Common Use Cases

```text
ERP Integration
Middleware
Backend Services
```

---

# 3. Username Password Flow

### Process

```text
Username
Password
Security Token
```

### Characteristics

- Simple
- Legacy Approach

### Drawbacks

```text
Not Recommended
```

Because:

- Password Storage Required
- Less Secure

---

# 4. Client Credentials Flow

### Process

```text
Client Id
Client Secret
      |
      v
Access Token
```

### Characteristics

- Machine-to-Machine
- No User Login

### Use Cases

```text
Backend Integration
Microservices
```

---

# OAuth Flow Comparison

| Flow | User Login | Refresh Token | Common Usage |
|---------|---------|---------|---------|
| Web Server | Yes | Yes | Web Apps |
| JWT Bearer | No | No | Server-to-Server |
| Username Password | No | No | Legacy Integrations |
| Client Credentials | No | No | Machine-to-Machine |

---

# OAuth Scopes

Scopes define what an application can access.

## Common Scopes

### API

```text
Access and manage your data
```

---

### Refresh Token

```text
Perform requests at any time
```

---

### Full Access

```text
Full Salesforce Access
```

---

### OpenID

```text
Identity Information
```

---

# Access Token Usage

Example REST API Call:

```http
GET /services/data/v65.0/sobjects/Account

Authorization:
Bearer 00DXXXXXXXX
```

---

# Refresh Token Flow

```text
Access Token Expired
        |
        v
Refresh Token
        |
        v
New Access Token
```

No user login required.

---

# Connected App Policies

Configure:

```text
IP Relaxation

Session Timeout

Permitted Users

Refresh Token Policies
```

---

# Permitted Users

## All Users May Self Authorize

```text
Anyone can authorize
```

---

## Admin Approved Users

```text
Permission Set Required
```

Recommended for enterprise integrations.

---

# Security Best Practices

## Use JWT or Client Credentials

For server-to-server integrations.

---

## Avoid Username Password Flow

Legacy and less secure.

---

## Restrict Connected App Access

Use:

```text
Profiles
Permission Sets
```

---

## Protect Consumer Secret

Never expose in code repositories.

---

## Use Least Privilege

Grant only required OAuth scopes.

---

# Common Interview Questions

## What is OAuth?

A token-based authentication and authorization framework used to securely access Salesforce APIs.

---

## What is a Connected App?

A Salesforce application that enables external systems to authenticate and access Salesforce resources using OAuth.

---

## Difference Between Consumer Key and Consumer Secret?

| Consumer Key | Consumer Secret |
|-------------|----------------|
| Client ID | Client Password |
| Public | Confidential |

---

## Which OAuth Flow is Most Common?

```text
Web Server Flow
```

---

## Which Flow is Used for Server-to-Server Integration?

```text
JWT Bearer Flow
```

Most common answer in Salesforce interviews.

---

## Which Flow is Used for Machine-to-Machine Authentication?

```text
Client Credentials Flow
```

---

## Which OAuth Flow is Not Recommended?

```text
Username Password Flow
```

Because credentials must be stored.

---

## What is a Refresh Token?

A token used to obtain a new Access Token without requiring the user to log in again.

---

## What is the Difference Between Authentication and Authorization?

| Authentication | Authorization |
|---------------|---------------|
| Who are you? | What can you access? |

Example:

```text
Login = Authentication

Permissions = Authorization
```

---

# Mid-Level Interview Focus

### Must Know

```text
Connected App

Consumer Key
Consumer Secret

Access Token
Refresh Token

OAuth Scopes

Web Server Flow

JWT Bearer Flow

Client Credentials Flow

Authentication vs Authorization

Connected App Policies
```

---

# Interview Summary

```text
OAuth
=
Secure Token-Based Authentication

Connected App
=
OAuth Configuration in Salesforce

Tokens:
- Access Token
- Refresh Token

Most Common Flow:
- Web Server Flow

Server-to-Server:
- JWT Bearer Flow

Machine-to-Machine:
- Client Credentials Flow

Avoid:
- Username Password Flow

Consumer Key
=
Client ID

Consumer Secret
=
Client Password
```