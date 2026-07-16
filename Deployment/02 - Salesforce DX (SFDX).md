# Salesforce DX (SFDX)

## Overview

Salesforce DX (Developer Experience) is Salesforce's modern development model that supports source-driven development, version control, automation, and CI/CD.

Traditional Salesforce Development:

```text
Org → Source
```

Salesforce DX:

```text
Source → Org
```

This allows teams to use:

```text
Git
GitHub
CI/CD
Scratch Orgs
Automated Deployments
```

---

# Why Salesforce DX?

Before SFDX:

```text
Developer
    |
    v
Sandbox
    |
    v
Change Sets
    |
    v
Production
```

Problems:

- Manual Deployments
- No Version Control
- Difficult Team Collaboration

---

With Salesforce DX:

```text
Developer
    |
    v
Git Repository
    |
    v
CI/CD Pipeline
    |
    v
Salesforce Org
```

Benefits:

- Source Control
- Team Collaboration
- Automated Deployments
- Faster Releases

---

# What is SFDX?

SFDX stands for:

```text
Salesforce Developer Experience
```

It consists of:

```text
Salesforce DX

Salesforce CLI

Scratch Orgs

Source Tracking
```

---

# Key Concepts

## Source-Driven Development

Source code becomes the source of truth.

```text
Git Repository
      |
      v
Salesforce Org
```

Not:

```text
Salesforce Org
      |
      v
Source Code
```

---

## Salesforce CLI

Command-line tool for Salesforce development.

Current command:

```bash
sf
```

Older command:

```bash
sfdx
```

Interview Note:

```text
sf is the modern CLI.
```

---

# Scratch Orgs

## What is a Scratch Org?

A temporary Salesforce org created for development and testing.

Characteristics:

```text
Temporary

Source Driven

Easy To Create

Easy To Delete
```

---

## Benefits

```text
Isolated Development

Consistent Environments

Automated Testing
```

---

## Scratch Org Lifecycle

```text
Create Org
      |
      v
Develop
      |
      v
Test
      |
      v
Delete
```

---

# Salesforce DX Project Structure

Example:

```text
force-app/
│
├── main/
│   └── default/
│       ├── classes/
│       ├── lwc/
│       ├── triggers/
│       ├── objects/
│       └── flows/
│
├── sfdx-project.json
└── package.json
```

---

# Authorize an Org

Login to Salesforce.

```bash
sf org login web
```

Opens browser login page.

---

# View Authorized Orgs

```bash
sf org list
```

Output:

```text
Production
Sandbox
Scratch Org
```

---

# Retrieve Metadata

Retrieve metadata from org.

```bash
sf project retrieve start
```

Example:

```bash
sf project retrieve start \
--metadata ApexClass
```

---

# Deploy Metadata

Deploy source to org.

```bash
sf project deploy start
```

Example:

```bash
sf project deploy start \
--source-dir force-app
```

---

# Open Org

```bash
sf org open
```

Launches Salesforce org in browser.

---

# Create Scratch Org

```bash
sf org create scratch
```

Example:

```bash
sf org create scratch \
--definition-file config/project-scratch-def.json \
--alias DevOrg \
--duration-days 7
```

---

# Set Default Org

```bash
sf config set target-org DevOrg
```

---

# Delete Scratch Org

```bash
sf org delete scratch
```

---

# Source Tracking

Tracks metadata changes.

Example:

```text
Local Changes

Org Changes
```

Allows developers to identify differences.

---

# Version Control Integration

Salesforce DX works with:

```text
Git

GitHub

GitLab

Bitbucket
```

Typical Workflow:

```text
Developer
     |
     v
Git Commit
     |
     v
Pull Request
     |
     v
Deployment
```

---

# Package Development

Salesforce DX supports packages.

Types:

```text
Unlocked Packages

Managed Packages
```

---

# Unlocked Packages

Used for enterprise development.

Benefits:

```text
Versioning

Modular Development

Reusability
```

---

# CI/CD Integration

Salesforce DX is designed for CI/CD.

Pipeline Example:

```text
Developer
      |
      v
Git Commit
      |
      v
GitHub Actions
      |
      v
Run Tests
      |
      v
Deploy To Sandbox
      |
      v
Production
```

---

# Salesforce DX vs Change Sets

| Feature | Change Sets | Salesforce DX |
|----------|------------|---------------|
| Version Control | No | Yes |
| Git Integration | No | Yes |
| CI/CD | No | Yes |
| Automation | Limited | Full |
| Source Driven | No | Yes |
| Modern Development | No | Yes |

---

# Scratch Org vs Sandbox

| Scratch Org | Sandbox |
|------------|---------|
| Temporary | Permanent |
| Source Driven | Org Driven |
| Easy Creation | Refresh Required |
| Development Focused | Testing Focused |

---

# Common Commands

## Login

```bash
sf org login web
```

---

## List Orgs

```bash
sf org list
```

---

## Open Org

```bash
sf org open
```

---

## Retrieve Metadata

```bash
sf project retrieve start
```

---

## Deploy Metadata

```bash
sf project deploy start
```

---

## Run Tests

```bash
sf apex run test
```

---

## Create Scratch Org

```bash
sf org create scratch
```

---

## Delete Scratch Org

```bash
sf org delete scratch
```

---

# Common Interview Questions

## What is Salesforce DX?

A modern Salesforce development model that supports source-driven development, version control, automation, and CI/CD.

---

## What is SFDX?

Salesforce Developer Experience, including CLI tools, Scratch Orgs, source tracking, and modern development workflows.

---

## What is a Scratch Org?

A temporary Salesforce org used for development and testing.

---

## Why is Salesforce DX Better Than Change Sets?

```text
Git Integration

Version Control

CI/CD Support

Automation

Source Driven Development
```

---

## What is Source-Driven Development?

Source code in Git becomes the source of truth instead of the Salesforce org.

---

## Which Command Deploys Metadata?

```bash
sf project deploy start
```

---

## Which Command Retrieves Metadata?

```bash
sf project retrieve start
```

---

## Which Command Runs Apex Tests?

```bash
sf apex run test
```

---

## What is Source Tracking?

A feature that tracks differences between local source and Salesforce org metadata.

---

## Which Version Control Systems Work with Salesforce DX?

```text
Git
GitHub
GitLab
Bitbucket
```

---

# Mid-Level Interview Focus

### Must Know

```text
Salesforce DX

SFDX CLI

Source-Driven Development

Scratch Orgs

Git Integration

Metadata Deployments

Source Tracking

CI/CD

Unlocked Packages
```

---

# Interview Summary

```text
Salesforce DX
=
Modern Salesforce Development

Benefits:
- Git Integration
- Version Control
- CI/CD
- Automation

Key Concepts:
- Source Driven Development
- Scratch Orgs
- Source Tracking
- Salesforce CLI

Important Commands:

sf org login web

sf org list

sf org open

sf project retrieve start

sf project deploy start

sf apex run test

Scratch Org
=
Temporary Development Org

Salesforce DX
=
Modern Alternative To Change Sets
```