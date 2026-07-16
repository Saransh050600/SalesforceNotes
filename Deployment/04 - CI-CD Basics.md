# CI/CD Basics

## Overview

CI/CD is a modern software development practice that automates code integration, testing, validation, and deployment.

For Salesforce projects, CI/CD helps teams deploy changes faster, safer, and with fewer manual steps.

---

# What is CI/CD?

CI/CD stands for:

```text
CI  = Continuous Integration

CD  = Continuous Delivery
      or
      Continuous Deployment
```

---

# Why CI/CD?

Traditional Deployment:

```text
Developer
    ↓
Manual Testing
    ↓
Manual Deployment
    ↓
Production
```

Problems:

```text
Human Errors

Slow Releases

Deployment Failures

No Automation
```

---

CI/CD Approach:

```text
Developer
    ↓
Git
    ↓
Automated Testing
    ↓
Automated Deployment
    ↓
Production
```

Benefits:

```text
Faster Releases

Higher Quality

Automation

Reduced Risk
```

---

# Continuous Integration (CI)

## Definition

Developers frequently merge code into a shared repository.

Every code change automatically triggers:

```text
Validation

Build

Testing
```

---

## CI Flow

```text
Developer
     ↓
Commit Code
     ↓
Git Repository
     ↓
Automated Tests
     ↓
Validation
```

Goal:

```text
Detect Issues Early
```

---

# Continuous Delivery (CD)

## Definition

After successful testing, code is automatically prepared for deployment.

Deployment still requires approval.

---

## Flow

```text
Developer
     ↓
Commit
     ↓
Tests
     ↓
Validation
     ↓
Ready For Deployment
```

Human approval required.

---

# Continuous Deployment

## Definition

After successful testing, deployment happens automatically.

No manual approval.

---

## Flow

```text
Developer
     ↓
Commit
     ↓
Tests
     ↓
Validation
     ↓
Automatic Deployment
```

---

# CI vs CD

| Feature | Continuous Integration | Continuous Delivery |
|----------|----------|----------|
| Code Merge | Yes | Yes |
| Automated Tests | Yes | Yes |
| Deployment Ready | No | Yes |
| Production Deployment | No | Optional |

---

# Continuous Delivery vs Continuous Deployment

| Delivery | Deployment |
|-----------|------------|
| Manual Approval Required | Fully Automated |
| Common In Salesforce | Less Common |
| Safer | Faster |

---

# Salesforce CI/CD Architecture

```text
Developer
      ↓
Feature Branch
      ↓
GitHub
      ↓
Pull Request
      ↓
CI Pipeline
      ↓
Run Apex Tests
      ↓
Validate Metadata
      ↓
Deploy To Sandbox
      ↓
Deploy To Production
```

---

# Typical Salesforce Workflow

```text
Developer Sandbox
        ↓
Git Feature Branch
        ↓
Pull Request
        ↓
CI Validation
        ↓
QA/UAT
        ↓
Production
```

---

# CI/CD Components

## Source Control

Stores source code.

Examples:

```text
Git

GitHub

GitLab

Bitbucket
```

---

## Build Server

Runs automation.

Examples:

```text
GitHub Actions

Jenkins

Azure DevOps

GitLab CI
```

---

## Salesforce CLI

Used for deployments.

Commands:

```bash
sf project deploy start

sf project retrieve start

sf apex run test
```

---

# Salesforce Deployment Pipeline

## Step 1

Developer commits code.

```bash
git commit
```

---

## Step 2

Push code.

```bash
git push
```

---

## Step 3

Create Pull Request.

```text
Feature Branch
      ↓
Develop Branch
```

---

## Step 4

Pipeline starts.

Actions:

```text
Code Validation

PMD Scan

Unit Tests

Static Analysis
```

---

## Step 5

Deploy to Sandbox.

```text
QA Sandbox
```

---

## Step 6

User Acceptance Testing.

```text
UAT
```

---

## Step 7

Production Deployment.

```text
Production
```

---

# Common Salesforce CI/CD Tools

## GitHub Actions

Most popular.

```text
GitHub Native

Easy Setup

Free For Small Teams
```

---

## Jenkins

Traditional CI/CD tool.

```text
Highly Customizable
```

---

## Azure DevOps

Common in enterprise projects.

```text
Microsoft Ecosystem
```

---

## GitLab CI

Built into GitLab.

---

## DevOps Center

Salesforce-native DevOps solution.

Features:

```text
Git Integration

Work Items

Deployment Pipelines
```

---

# Automated Tests in CI/CD

Most Salesforce pipelines execute:

```text
Apex Tests

LWC Tests

Code Quality Checks
```

---

# Code Coverage

Production requirement:

```text
75% Coverage
```

Pipelines often fail when:

```text
Coverage < 75%
```

---

# Static Code Analysis

Checks code quality automatically.

Popular tools:

```text
PMD

CodeScan

SonarQube
```

Detects:

```text
SOQL In Loops

Security Issues

Bad Practices
```

---

# Benefits of CI/CD

## Faster Releases

Automation reduces deployment time.

---

## Better Quality

Tests run automatically.

---

## Fewer Production Issues

Problems detected earlier.

---

## Consistent Deployments

Same process every time.

---

## Easier Rollback

Git provides history.

---

# Common Interview Questions

## What is CI?

Continuous Integration.

Developers frequently merge code and run automated validations.

---

## What is CD?

Continuous Delivery or Continuous Deployment.

Automates deployment process.

---

## Difference Between Continuous Delivery and Continuous Deployment?

| Delivery | Deployment |
|-----------|------------|
| Manual Approval | Fully Automated |
| Common In Salesforce | Less Common |

---

## Why Use CI/CD?

```text
Automation

Faster Releases

Higher Quality

Reduced Risk
```

---

## What Happens In A Salesforce CI/CD Pipeline?

```text
Code Commit
      ↓
Pull Request
      ↓
Validation
      ↓
Run Tests
      ↓
Deploy
```

---

## Which Tools Are Commonly Used?

```text
GitHub Actions

Jenkins

Azure DevOps

GitLab CI

DevOps Center
```

---

## What Is The Role Of Git In CI/CD?

Git acts as the source of truth and triggers deployment pipelines.

---

## Why Is Salesforce DX Important For CI/CD?

Because it supports:

```text
Source-Driven Development

CLI Deployments

Git Integration
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
Push
      ↓
Pull Request
      ↓
GitHub Actions
      ↓
Run Apex Tests
      ↓
Deploy To QA
      ↓
UAT Approval
      ↓
Deploy To Production
```

---

# Mid-Level Interview Focus

### Must Know

```text
CI

CD

Git

Pull Requests

GitHub Actions

Salesforce DX

Automated Testing

75% Code Coverage

Deployment Pipeline
```

---

# Interview Summary

```text
CI/CD
=
Automated Build, Test, and Deployment

CI
=
Continuous Integration

CD
=
Continuous Delivery /
Continuous Deployment

Pipeline:

Developer
→ Git
→ Pull Request
→ Tests
→ Validation
→ Deployment

Popular Tools:

GitHub Actions

Jenkins

Azure DevOps

GitLab CI

DevOps Center

Salesforce DX
=
Foundation For Salesforce CI/CD
```