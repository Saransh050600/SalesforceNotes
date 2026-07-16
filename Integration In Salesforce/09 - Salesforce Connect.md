
## Overview

Salesforce Connect allows Salesforce users to access external data in real time without storing that data inside Salesforce.

Instead of copying data into Salesforce, Salesforce queries the external system when users access records.

```text
Salesforce
     |
     v
Salesforce Connect
     |
     v
External System
     |
     v
Data Returned
```

---

# Why Salesforce Connect?

Traditional Approach:

```text
External System
      |
      v
Data Import
      |
      v
Salesforce
```

Problems:

- Data Duplication
- Storage Consumption
- Synchronization Issues
- ETL Maintenance

---

Using Salesforce Connect:

```text
Salesforce
     |
     v
External Database
```

Benefits:

- No Data Duplication
- Real-Time Access
- Reduced Storage Usage
- Simplified Integration

---

# Key Components

## Salesforce Connect

Framework that connects Salesforce to external data sources.

---

## External Objects

Virtual objects representing external data.

---

## External Data Source

Connection configuration.

---

## OData

Most common protocol used by Salesforce Connect.

---

# Architecture

```text
User
 |
 v
Salesforce
 |
 v
External Object
 |
 v
External Data Source
 |
 v
External System
```

Examples:

```text
SAP

Oracle

ERP

SQL Server

External CRM

Web Services
```

---

# What is an External Object?

An External Object is a virtual object that maps to external data.

Looks similar to:

```text
Account
Contact
Opportunity
```

But data remains outside Salesforce.

---

# Naming Convention

External Objects end with:

```text
__x
```

Examples:

```text
Customer__x

Order__x

Invoice__x
```

---

# Example

ERP Table:

```text
Customer
```

Mapped as:

```text
Customer__x
```

Users can:

```text
View
Search
Report
```

Without storing data in Salesforce.

---

# External Data Source

Represents the connection to an external system.

Navigate:

```text
Setup
→ External Data Sources
```

---

# Common Data Sources

## OData 2.0

Supported.

---

## OData 4.0

Recommended.

---

## Cross-Org

Salesforce-to-Salesforce connection.

---

## Custom Adapter

Custom integration implementation.

---

# OData

Open Data Protocol.

Most commonly asked interview topic.

Salesforce Connect primarily works with:

```text
OData 2.0

OData 4.0
```

---

# Configuring Salesforce Connect

## Step 1

Create External Data Source.

```text
Setup
→ External Data Sources
```

---

## Step 2

Select Type.

Example:

```text
OData 4.0
```

---

## Step 3

Provide Endpoint URL.

Example:

```text
https://api.company.com/odata
```

---

## Step 4

Authenticate.

Options:

```text
OAuth

Named Credentials

Password Authentication
```

---

## Step 5

Validate and Sync.

Salesforce automatically creates:

```text
External Objects
```

---

# Querying External Objects

SOQL works with External Objects.

Example:

```apex
List<Customer__x> customers =
[
    SELECT Id,
           Name
    FROM Customer__x
];
```

---

# Relationships

External Objects support relationships.

---

## Lookup Relationship

```text
Standard Object
      |
      v
External Object
```

Example:

```text
Account
    |
    v
Customer__x
```

---

## External Lookup

Relationship using External ID.

---

## Indirect Lookup

Most common interview topic.

Uses:

```text
External ID
```

To relate Salesforce records with external records.

---

# Indirect Lookup Example

ERP Customer:

```text
Customer Number
```

Salesforce Account:

```text
Customer_Number__c
```

External Object:

```text
Customer__x
```

Relationship:

```text
Account.Customer_Number__c

matches

Customer__x.Customer_Number
```

---

# External Lookup Example

```text
External Object
      |
      v
External Object
```

Relationship based on external identifiers.

---

# Search Capabilities

External Objects support:

```text
Global Search

SOQL Queries

Reports
```

Depending on the adapter capabilities.

---

# Reporting

External Objects can be used in reports.

Example:

```text
Salesforce Accounts

ERP Customers

External Orders
```

Single report.

---

# Data Access

External Objects support:

```text
Read Only
```

Most common.

---

Some adapters support:

```text
Create

Update

Delete
```

---

# Salesforce Connect vs Data Integration

## Salesforce Connect

```text
Real-Time Access

No Storage
```

---

## ETL Integration

```text
Copy Data

Store Data
```

Examples:

```text
MuleSoft

Boomi

Informatica
```

---

# Salesforce Connect vs External Services

| Salesforce Connect | External Services |
|--------------------|------------------|
| Access Data | Invoke APIs |
| External Objects | Flow Actions |
| Real-Time Queries | Real-Time Actions |

---

# Salesforce Connect vs CDC

| Salesforce Connect | CDC |
|-------------------|-----|
| Query External Data | Track Salesforce Changes |
| Read External Records | Publish Events |
| Virtual Data Access | Event Driven |

---

# Salesforce Connect vs Platform Events

| Salesforce Connect | Platform Events |
|-------------------|----------------|
| Data Access | Messaging |
| Query Records | Publish Events |
| Virtual Objects | Event Bus |

---

# Security

Supports:

```text
OAuth

Named Credentials

Per User Authentication

Named Principal
```

---

# Benefits

## No Data Storage

External data remains outside Salesforce.

---

## Real-Time Data

Always latest version.

---

## Reduced Storage Costs

No Salesforce storage consumption.

---

## Simplified Architecture

No synchronization jobs.

---

# Limitations

## Performance Depends On External System

Slow external system:

```text
Slow Salesforce Response
```

---

## External Availability Required

If system is down:

```text
Data Unavailable
```

---

## Limited Functionality

Compared to standard objects.

---

# Common Use Cases

## ERP Integration

```text
SAP

Oracle ERP
```

---

## Financial Systems

```text
Invoices

Payments

Transactions
```

---

## Legacy Databases

```text
SQL Server

Oracle DB
```

---

## Customer Master Data

```text
Millions of Customers
```

Without storing in Salesforce.

---

# Common Interview Questions

## What is Salesforce Connect?

A framework that allows Salesforce to access external data in real time without storing the data in Salesforce.

---

## What is an External Object?

A virtual object representing external data.

Suffix:

```text
__x
```

---

## Does Salesforce Connect Store Data?

```text
No
```

Data remains in the external system.

---

## Which Protocol Is Commonly Used?

```text
OData
```

Especially:

```text
OData 4.0
```

---

## What is an Indirect Lookup Relationship?

A relationship that connects Salesforce records to external records using an External ID field.

---

## What is an External Data Source?

A connection definition between Salesforce and an external system.

---

## Salesforce Connect vs ETL?

| Salesforce Connect | ETL |
|-------------------|------|
| Real-Time | Data Copy |
| No Storage | Stores Data |
| Virtual Access | Physical Data |

---

## Salesforce Connect vs Platform Events?

```text
Salesforce Connect
→ Access External Data

Platform Events
→ Event Messaging
```

---

## What Suffix Is Used For External Objects?

```text
__x
```

---

# Mid-Level Interview Focus

### Must Know

```text
Salesforce Connect

External Objects

External Data Sources

OData

Indirect Lookup

External Lookup

__x Suffix

Real-Time Access

No Data Storage

Salesforce Connect vs ETL
```

---

# Interview Summary

```text
Salesforce Connect
=
Real-Time External Data Access

Key Components:

External Data Source

External Object (__x)

OData

Relationships:

Indirect Lookup

External Lookup

Benefits:

No Data Storage

Real-Time Access

Reduced Storage Costs

Most Common Use Case:

ERP Integration

Most Asked Question:

Salesforce Connect vs ETL

Salesforce Connect
=
Virtual Data

ETL
=
Copied Data
```