
## Overview

Change Sets are Salesforce's native deployment mechanism used to move metadata between related Salesforce orgs.

Typical deployment flow:

```text
Developer Sandbox
        ↓
QA/UAT Sandbox
        ↓
Production
```

---

# What is a Change Set?

A Change Set is a collection of metadata components that can be deployed from one Salesforce org to another connected org.

Examples:

```text
Apex Classes
Triggers
LWC
Flows
Custom Objects
Fields
Validation Rules
Permission Sets
Page Layouts
```

---

# Types of Change Sets

## Outbound Change Set

Created in the source org.

Example:

```text
Developer Sandbox
```

Purpose:

```text
Package metadata for deployment.
```

---

## Inbound Change Set

Received in the target org.

Example:

```text
Production
```

Purpose:

```text
Validate and deploy metadata.
```

---

# Deployment Process

```text
Source Org
     |
     v
Create Outbound Change Set
     |
     v
Add Components
     |
     v
Upload
     |
     v
Target Org
     |
     v
Inbound Change Set
     |
     v
Validate
     |
     v
Deploy
```

---

# Creating an Outbound Change Set

Navigate:

```text
Setup
→ Outbound Change Sets
→ New
```

Provide:

```text
Name
Description
```

Save.

---

# Adding Components

Click:

```text
Add
```

Select metadata:

```text
Apex Classes
Triggers
LWC
Flows
Objects
Fields
Layouts
```

Add to the Change Set.

---

# Adding Dependencies

Salesforce can automatically identify dependent components.

Example:

```text
Custom Object
      |
      v
Custom Fields
      |
      v
Page Layouts
```

Use:

```text
View/Add Dependencies
```

Best Practice:

```text
Always review dependencies before uploading.
```

---

# Uploading a Change Set

Click:

```text
Upload
```

Select target org.

Example:

```text
Production
```

The Change Set is now available in the target org as an Inbound Change Set.

---

# Validating a Change Set

Navigate:

```text
Setup
→ Inbound Change Sets
```

Select the Change Set.

Click:

```text
Validate
```

Validation:

```text
Runs Apex Tests
Checks Dependencies
Checks Metadata
```

No deployment occurs.

---

# Deploying a Change Set

After successful validation:

```text
Deploy
```

Salesforce moves metadata into the target org.

---

# Test Execution Options

## Run Local Tests

```text
Runs all local Apex tests.
```

Most commonly used.

---

## Run Specified Tests

```text
Runs selected test classes only.
```

Faster deployments.

---

## Run All Tests

```text
Runs all tests including managed package tests.
```

Slowest option.

---

# Code Coverage Requirement

Production deployments require:

```text
75% Overall Apex Code Coverage
```

Deployment fails if:

```text
Coverage < 75%
```

---

# Validation Deployment

Best Practice:

```text
Validate First
      ↓
Deploy Later
```

Benefits:

```text
Detect Issues Early
Reduce Deployment Failures
```

---

# Quick Deploy

Available after successful validation.

Benefits:

```text
No Test Re-execution
Faster Production Deployment
```

Interview Favorite:

### What is Quick Deploy?

```text
Deploy a previously validated Change Set without rerunning tests.
```

---

# Advantages of Change Sets

### Native Salesforce Tool

No installation required.

---

### Easy to Use

Point-and-click deployment.

---

### Dependency Management

Can automatically include dependencies.

---

### Good for Small Projects

Simple deployment process.

---

# Limitations of Change Sets

### No Version Control

No Git integration.

---

### Manual Process

Requires manual deployment.

---

### No Rollback

Cannot automatically revert deployments.

---

### No CI/CD Support

Difficult to automate.

---

### Related Orgs Only

Can deploy only between connected Salesforce orgs.

---

# Change Sets vs Salesforce DX

| Change Sets | Salesforce DX |
|------------|---------------|
| Manual | Source Driven |
| No Git | Git Integrated |
| No CI/CD | CI/CD Friendly |
| Org Based | Source Based |
| Limited Automation | Fully Automated |

---

# Common Interview Questions

## What is a Change Set?

A Salesforce deployment mechanism used to move metadata between related Salesforce orgs.

---

## What are the Types of Change Sets?

```text
Outbound Change Set
Inbound Change Set
```

---

## Can Change Sets Deploy Data?

```text
No
```

They deploy metadata only.

---

## What Can Be Deployed Using Change Sets?

```text
Apex Classes
Triggers
LWC
Flows
Objects
Fields
Layouts
Permission Sets
Validation Rules
```

---

## What is Validation Deployment?

Running deployment checks and tests without actually deploying metadata.

---

## What is Quick Deploy?

Deploying a successfully validated Change Set without rerunning tests.

---

## What is the Minimum Code Coverage Required?

```text
75%
```

---

## What are the Limitations of Change Sets?

```text
No Version Control

Manual Process

No Rollback

No CI/CD

Related Orgs Only
```

---

## Why are Modern Projects Moving Away from Change Sets?

Because organizations prefer:

```text
Git
Salesforce DX
CI/CD Pipelines
Automated Deployments
```

---

# Interview Summary

```text
Change Sets
=
Native Salesforce Deployment Tool

Types:
- Outbound Change Set
- Inbound Change Set

Deployment Flow:
Create → Upload → Validate → Deploy

Production Requirement:
75% Code Coverage

Important Concepts:
Dependencies
Validation
Quick Deploy

Limitations:
No Git
No CI/CD
No Rollback
Manual Process

Modern Alternative:
Salesforce DX + Git + CI/CD
```