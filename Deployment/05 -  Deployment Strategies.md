
## Overview

Deployment Strategy refers to the approach used to safely move Salesforce changes from development environments to production.

A good deployment strategy minimizes:

```text
Production Issues
Downtime
Failed Deployments
Rollback Risks
```

---

# Deployment Lifecycle

Typical Salesforce deployment flow:

```text
Developer Sandbox
        ↓
QA Sandbox
        ↓
UAT Sandbox
        ↓
Production
```

Modern Projects:

```text
Developer
    ↓
Git
    ↓
CI/CD
    ↓
QA
    ↓
UAT
    ↓
Production
```

---

# Common Deployment Strategies

```text
1. Validate First
2. Quick Deploy
3. Big Bang Deployment
4. Phased Deployment
5. Feature Branch Deployment
6. Hotfix Deployment
7. Blue-Green Deployment (Conceptual)
```

---

# 1. Validate First Strategy

Most common Salesforce deployment strategy.

Process:

```text
Validate
     ↓
Fix Errors
     ↓
Deploy
```

---

## Benefits

```text
Early Error Detection

Reduced Production Risk

Faster Final Deployment
```

---

## Example

```text
Deploy Change Set
       ↓
Validate
       ↓
Run Tests
       ↓
Success
       ↓
Deploy Later
```

---

## Interview Question

### Why validate before deployment?

```text
To identify deployment issues
before affecting production.
```

---

# 2. Quick Deploy Strategy

Used after successful validation.

Process:

```text
Validate
      ↓
Quick Deploy
```

---

## Benefits

```text
No Test Re-execution

Faster Production Deployment
```

---

## Example

```text
Validate Friday

Deploy Saturday
```

Without rerunning tests.

---

## Interview Favorite

### What is Quick Deploy?

```text
Deployment of a previously validated package
without rerunning tests.
```

---

# 3. Big Bang Deployment

All changes deployed together.

```text
Feature A
Feature B
Feature C
Feature D
        ↓
Single Production Release
```

---

## Advantages

```text
Single Release Window

Simple Planning
```

---

## Disadvantages

```text
Higher Risk

Harder Troubleshooting

Larger Rollback Scope
```

---

## Use Cases

```text
Small Projects

Minor Releases
```

---

# 4. Phased Deployment

Deploy features gradually.

```text
Release 1
      ↓
Release 2
      ↓
Release 3
```

---

## Advantages

```text
Lower Risk

Easier Testing

Controlled Rollout
```

---

## Disadvantages

```text
Longer Delivery Time
```

---

## Use Cases

```text
Large Enterprise Projects
```

---

# 5. Feature Branch Deployment

Modern Git-based strategy.

Each feature is developed independently.

```text
main

feature/account-api

feature/order-trigger

feature/lwc-dashboard
```

---

## Process

```text
Feature Branch
      ↓
Pull Request
      ↓
Review
      ↓
Merge
      ↓
Deploy
```

---

## Advantages

```text
Parallel Development

Safer Releases

Code Review Support
```

---

## Most Common Modern Approach

```text
Git + Feature Branches
```

---

# 6. Hotfix Deployment

Used for urgent production issues.

Process:

```text
Production Bug
       ↓
Hotfix Branch
       ↓
Deploy
```

---

## Example

```text
Critical Trigger Failure

Security Issue

Broken Integration
```

---

## Branch Example

```text
main

hotfix/account-trigger-fix
```

---

## Best Practice

After deployment:

```text
Merge Hotfix Back
Into Main And Develop
```

---

# 7. Blue-Green Deployment

Not commonly used in Salesforce but may appear in architecture discussions.

Concept:

```text
Blue Environment
(Current)

Green Environment
(New)
```

Switch traffic after validation.

---

## Salesforce Reality

Not directly supported.

Usually discussed conceptually.

---

# Sandbox Deployment Strategy

Recommended Flow:

```text
Developer Sandbox
         ↓
Integration Sandbox
         ↓
QA Sandbox
         ↓
UAT Sandbox
         ↓
Production
```

---

# Release Management Strategy

Typical Release Process:

```text
Development
      ↓
Code Review
      ↓
Testing
      ↓
UAT
      ↓
Production
```

---

# Rollback Strategy

Interview Question Favorite.

---

## Problem

Salesforce deployments do not provide automatic rollback.

---

## Common Rollback Methods

### Git Revert

```bash
git revert
```

---

### Redeploy Previous Version

```text
Deploy Last Stable Release
```

---

### Restore Metadata

Using:

```text
Git

Backup

Packages
```

---

# Deployment Windows

Enterprise organizations often deploy:

```text
Weekends

Low Usage Hours

Scheduled Release Windows
```

Reasons:

```text
Reduce Business Impact
```

---

# CI/CD Deployment Strategy

Modern Pipeline:

```text
Developer
      ↓
Feature Branch
      ↓
Pull Request
      ↓
Code Review
      ↓
Automated Tests
      ↓
QA
      ↓
UAT
      ↓
Production
```

---

# Production Deployment Checklist

Before deployment:

```text
Code Review Completed

Tests Passing

75% Coverage

Validation Successful

Backup Available

Stakeholders Informed
```

---

# Common Interview Questions

## What is a Deployment Strategy?

A structured approach used to safely move changes from development environments to production.

---

## Why Validate Before Deployment?

To detect failures before impacting production.

---

## What is Quick Deploy?

Deploying previously validated metadata without rerunning tests.

---

## Big Bang vs Phased Deployment?

| Big Bang | Phased |
|-----------|---------|
| All Changes Together | Incremental Releases |
| Higher Risk | Lower Risk |
| Faster Release | More Controlled |

---

## What is a Hotfix Deployment?

An emergency deployment used to fix critical production issues.

---

## How Do You Roll Back A Salesforce Deployment?

```text
Git Revert

Redeploy Previous Metadata

Restore Backup
```

---

## What Deployment Strategy Is Common In Modern Salesforce Projects?

```text
Feature Branch Strategy
+
CI/CD Pipeline
```

---

# Real Project Example

```text
Developer
      ↓
Feature Branch
      ↓
Commit
      ↓
Pull Request
      ↓
Review
      ↓
GitHub Actions
      ↓
QA Sandbox
      ↓
UAT
      ↓
Validation
      ↓
Quick Deploy
      ↓
Production
```

---

# Mid-Level Interview Focus

### Must Know

```text
Validate First

Quick Deploy

Feature Branch Deployment

Hotfix Deployment

Rollback Strategy

CI/CD Deployment Flow

Sandbox Promotion Strategy
```

---

# Interview Summary

```text
Most Common Strategies:

1. Validate First
2. Quick Deploy
3. Feature Branch Deployment
4. Hotfix Deployment
5. Phased Deployment

Important Concepts:

Validation

Quick Deploy

Rollback

Git Revert

Production Release Window

Modern Approach:

Git
+
Salesforce DX
+
CI/CD
+
Feature Branches
```