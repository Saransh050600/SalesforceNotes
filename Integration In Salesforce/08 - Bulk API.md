
## Overview

Bulk API is Salesforce's API designed for processing large volumes of data efficiently.

Instead of processing records one at a time, Bulk API processes records in batches asynchronously.

```text
Client
   |
   v
Bulk API Job
   |
   v
Salesforce
   |
   v
Batch Processing
```

---

# Why Bulk API?

Using REST API:

```text
Insert 1 Record
Insert 1 Record
Insert 1 Record
...
```

Problems:

- Slow
- API Intensive
- Not Suitable for Millions of Records

---

Using Bulk API:

```text
100,000 Records
      |
      v
Single Bulk Job
      |
      v
Asynchronous Processing
```

Benefits:

- High Performance
- Handles Large Volumes
- Reduced API Consumption
- Asynchronous Processing

---

# Bulk API Versions

## Bulk API 1.0

Older version.

Features:

```text
Manual Batch Management
XML/CSV Support
```

---

## Bulk API 2.0

Recommended version.

Features:

```text
Automatic Batch Handling
CSV Upload
Simpler Process
Better Performance
```

Most interview questions focus on Bulk API 2.0.

---

# Bulk API Architecture

```text
Create Job
    |
    v
Upload CSV
    |
    v
Close Job
    |
    v
Salesforce Processes Records
    |
    v
Get Results
```

---

# Supported Operations

## Insert

```text
Create Records
```

---

## Update

```text
Modify Existing Records
```

---

## Upsert

```text
Insert or Update
```

Based on:

```text
External Id
```

---

## Delete

```text
Delete Records
```

---

## Hard Delete

```text
Permanent Delete
```

Skips Recycle Bin.

Requires:

```text
Bulk API Hard Delete Permission
```

---

# Bulk API Endpoint

Example:

```http
/services/data/v65.0/jobs/ingest
```

---

# Bulk API Job Lifecycle

## Step 1 - Create Job

Request:

```json
{
  "object": "Account",
  "operation": "insert",
  "contentType": "CSV"
}
```

Response:

```json
{
  "id": "750XXXXXXXXXXXX"
}
```

---

## Step 2 - Upload Data

Upload CSV file.

Example:

```csv
Name,Industry
Acme,Technology
ABC Corp,Banking
```

---

## Step 3 - Close Job

```json
{
  "state":"UploadComplete"
}
```

---

## Step 4 - Monitor Job

Check status.

Possible values:

```text
Open

UploadComplete

InProgress

JobComplete

Failed

Aborted
```

---

## Step 5 - Download Results

Retrieve:

```text
Successful Records

Failed Records

Unprocessed Records
```

---

# Bulk Query

Used for exporting large data volumes.

Example:

```sql
SELECT Id, Name
FROM Account
```

---

# Bulk Query Process

```text
Create Query Job
      |
      v
Execute Query
      |
      v
Download Results
```

---

# Bulk API vs REST API

| Feature | REST API | Bulk API |
|----------|----------|----------|
| Volume | Small-Medium | Large |
| Processing | Synchronous | Asynchronous |
| Performance | Lower | Higher |
| Millions of Records | No | Yes |
| Data Migration | No | Yes |

---

# Bulk API vs Data Loader

| Bulk API | Data Loader |
|-----------|------------|
| API | Tool |
| Programmatic | Manual |
| Integrations | Data Operations |
| Backend Processing | UI Tool |

Data Loader internally uses Bulk API for large loads.

---

# Bulk API vs Composite API

| Bulk API | Composite API |
|-----------|-----------|
| Large Volumes | Small-Medium Volume |
| Asynchronous | Real-Time |
| Millions of Records | Limited Records |
| Data Migration | Business Transactions |

---

# Serial vs Parallel Mode

## Parallel Mode

Default.

```text
Faster
Multiple Batches Processed Together
```

Example:

```text
Batch 1
Batch 2
Batch 3

Run Simultaneously
```

---

## Serial Mode

```text
One Batch at a Time
```

Example:

```text
Batch 1
    ↓
Batch 2
    ↓
Batch 3
```

---

# When to Use Serial Mode?

When record locking may occur.

Examples:

```text
Same Parent Account

Master Detail Relationships

Heavy Automation
```

---

# Upsert with External ID

Example:

```text
Employee_Id__c
```

If record exists:

```text
Update
```

If record doesn't exist:

```text
Insert
```

---

# Error Handling

Download failed records.

Example:

```csv
Name,Error
Acme,Required field missing
```

Common Errors:

```text
Validation Rules

Required Fields

Duplicate Rules

Record Locking
```

---

# Common Use Cases

## Data Migration

```text
Legacy System
      |
      v
Salesforce
```

---

## Initial Data Load

```text
Millions of Accounts
Millions of Contacts
```

---

## ERP Synchronization

```text
ERP
  |
  v
Bulk API
  |
  v
Salesforce
```

---

## Data Warehouse Loads

```text
Snowflake
Databricks
SAP BW
```

---

# Performance Best Practices

## Use Bulk API 2.0

Preferred over Bulk API 1.0.

---

## Use Parallel Mode

When possible.

---

## Use Serial Mode

When locking issues occur.

---

## Use External IDs

For Upserts.

---

## Monitor Failed Records

Always review failure files.

---

# Governor Limits

Bulk API operates outside Apex transaction limits.

Important interview point.

```text
Not Subject To Apex Governor Limits
```

Because processing occurs through Salesforce API infrastructure.

---

# Common Interview Questions

## What is Bulk API?

A Salesforce API designed to process large volumes of records asynchronously.

---

## Why Use Bulk API?

```text
Large Data Volumes

Better Performance

Asynchronous Processing
```

---

## Bulk API vs REST API?

| REST | Bulk |
|--------|--------|
| Real-Time | Async |
| Small Volume | Large Volume |
| Immediate Response | Background Processing |

---

## Bulk API 1.0 vs 2.0?

| Bulk 1.0 | Bulk 2.0 |
|-----------|-----------|
| Manual Batches | Automatic Batches |
| More Complex | Simpler |
| Older | Recommended |

---

## What Operations Are Supported?

```text
Insert

Update

Upsert

Delete

Hard Delete
```

---

## What is Upsert?

Insert or Update using an External ID.

---

## What is the Difference Between Serial and Parallel Mode?

### Parallel

```text
Faster
Multiple Batches Together
```

### Serial

```text
One Batch At A Time
```

Used to avoid record locking.

---

## What is Hard Delete?

Permanent deletion that bypasses the Recycle Bin.

---

## What is Bulk Query?

A Bulk API operation used to export large datasets asynchronously.

---

## Is Bulk API Subject to Apex Governor Limits?

```text
No
```

Because it runs through Salesforce API infrastructure.

---

# Mid-Level Interview Focus

### Must Know

```text
Bulk API 2.0

Insert
Update
Upsert
Delete

Job Lifecycle

Serial vs Parallel

External IDs

Bulk Query

Bulk API vs REST API

Bulk API vs Composite API

Data Migration Use Cases
```

---

# Interview Summary

```text
Bulk API
=
Large Volume Data Processing

Recommended Version:
Bulk API 2.0

Operations:
- Insert
- Update
- Upsert
- Delete
- Hard Delete

Lifecycle:

Create Job
→ Upload CSV
→ Close Job
→ Process Data
→ Get Results

Modes:

Parallel
→ Faster

Serial
→ Avoid Locking

Best For:

Data Migration

ERP Loads

Mass Updates

Millions of Records

Bulk API
=
Asynchronous

REST API
=
Synchronous
```