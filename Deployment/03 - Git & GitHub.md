
## Overview

Git is a distributed version control system used to track source code changes.

GitHub is a cloud platform that hosts Git repositories and enables team collaboration.

In Salesforce projects, Git is commonly used with:

```text
Salesforce DX (SFDX)
CI/CD Pipelines
GitHub Actions
Azure DevOps
Jenkins
```

---

# Why Git?

Without Git:

```text
Developer 1
    |
    v
Sandbox

Developer 2
    |
    v
Sandbox
```

Problems:

- No version history
- Code overwrites
- Difficult collaboration
- No rollback

---

With Git:

```text
Developer
    |
    v
Git Repository
    |
    v
GitHub
    |
    v
CI/CD
```

Benefits:

```text
Version Control
Collaboration
Code Reviews
Rollback Support
Deployment Automation
```

---

# Git vs GitHub

| Git | GitHub |
|------|--------|
| Version Control System | Repository Hosting Platform |
| Local Tool | Cloud Platform |
| Tracks Changes | Stores Repositories |
| Command Line Based | Web Interface |

---

# Common Git Terminology

## Repository (Repo)

A project stored in Git.

Example:

```text
salesforce-project
```

Contains:

```text
Apex
LWC
Flows
Objects
Profiles
```

---

## Clone

Downloads repository locally.

```bash
git clone https://github.com/company/project.git
```

---

## Commit

A snapshot of changes.

```bash
git commit -m "Added Account Trigger"
```

---

## Push

Uploads local changes to GitHub.

```bash
git push
```

---

## Pull

Downloads latest changes.

```bash
git pull
```

---

## Branch

Separate line of development.

Example:

```text
main

develop

feature/account-trigger
```

---

## Merge

Combines branches.

Example:

```text
feature/account-trigger
          ↓
        main
```

---

# Git Workflow

```text
Clone Repository
        ↓
Create Branch
        ↓
Make Changes
        ↓
Commit
        ↓
Push
        ↓
Pull Request
        ↓
Merge
```

---

# Most Common Commands

## Clone Repository

```bash
git clone https://github.com/company/project.git
```

---

## Check Status

```bash
git status
```

Shows:

```text
Modified Files

New Files

Deleted Files
```

---

## Add Files

Single file:

```bash
git add AccountTrigger.cls
```

All files:

```bash
git add .
```

---

## Commit Changes

```bash
git commit -m "Added Account Trigger"
```

---

## Push Changes

```bash
git push
```

---

## Pull Latest Changes

```bash
git pull
```

---

## Create Branch

```bash
git checkout -b feature/account-trigger
```

---

## Switch Branch

```bash
git checkout main
```

---

## View Branches

```bash
git branch
```

---

# Branching Strategy

Most common interview topic.

---

## Main Branch

```text
main
```

Contains:

```text
Production Ready Code
```

---

## Develop Branch

```text
develop
```

Contains:

```text
Ongoing Development
```

---

## Feature Branch

Example:

```text
feature/account-trigger

feature/order-api

feature/lwc-datatable
```

Used for:

```text
New Features
```

---

# Typical Salesforce Branch Structure

```text
main

develop

feature/*
```

Example:

```text
main

develop

feature/account-api

feature/opportunity-trigger
```

---

# Pull Request (PR)

## What is a Pull Request?

A request to merge code from one branch into another.

Example:

```text
feature/account-api
          ↓
        develop
```

Purpose:

```text
Code Review

Validation

Approval
```

---

# Pull Request Process

```text
Developer
      ↓
Push Code
      ↓
Create Pull Request
      ↓
Code Review
      ↓
Approval
      ↓
Merge
```

---

# Merge Conflict

## What is a Merge Conflict?

Occurs when two developers modify the same code and Git cannot automatically merge changes.

Example:

Developer A:

```apex
System.debug('A');
```

Developer B:

```apex
System.debug('B');
```

Git:

```text
Conflict Detected
```

Manual resolution required.

---

# GitHub

## What is GitHub?

A cloud platform for hosting Git repositories.

Features:

```text
Pull Requests

Code Reviews

Branch Protection

Actions

Issue Tracking
```

---

# GitHub Actions

GitHub's CI/CD service.

Example Pipeline:

```text
Developer
      ↓
Push Code
      ↓
GitHub Action
      ↓
Run Tests
      ↓
Deploy
```

---

# Salesforce CI/CD Example

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
Deploy To Sandbox
      ↓
Production
```

---

# Git in Salesforce Projects

Commonly Version Controlled:

```text
Apex Classes

Triggers

LWC

Flows

Objects

Permission Sets

Profiles

Layouts
```

Using:

```text
Salesforce DX
```

---

# Best Practices

## Pull Before Working

```bash
git pull
```

Always get latest changes.

---

## Use Feature Branches

Avoid direct commits to:

```text
main
```

---

## Small Commits

Good:

```text
Added Account Trigger
```

Bad:

```text
Fixed Everything
```

---

## Code Reviews

Use Pull Requests.

---

## Protect Main Branch

Require:

```text
Review

Approval

Successful Tests
```

Before merge.

---

# Common Interview Questions

## What is Git?

A distributed version control system used to track code changes.

---

## What is GitHub?

A cloud platform that hosts Git repositories and supports collaboration.

---

## Difference Between Git and GitHub?

| Git | GitHub |
|------|--------|
| Tool | Platform |
| Local | Cloud |
| Version Control | Repository Hosting |

---

## What is a Commit?

A snapshot of code changes.

---

## What is a Branch?

An isolated line of development.

---

## What is a Pull Request?

A request to merge code after review and approval.

---

## What is a Merge Conflict?

A situation where Git cannot automatically merge changes.

---

## Why Use Feature Branches?

To isolate development work and avoid impacting main code.

---

## What is GitHub Actions?

GitHub's built-in CI/CD automation service.

---

## How is Git Used in Salesforce?

Together with:

```text
Salesforce DX

CI/CD

Automated Deployments
```

---

# Mid-Level Interview Focus

### Must Know

```text
Git

GitHub

Repository

Clone

Commit

Push

Pull

Branch

Merge

Pull Request

Merge Conflict

Feature Branch

GitHub Actions
```

---

# Interview Summary

```text
Git
=
Version Control System

GitHub
=
Repository Hosting Platform

Workflow:

Clone
→ Branch
→ Commit
→ Push
→ Pull Request
→ Review
→ Merge

Most Important Concepts:

Repository

Commit

Push

Pull

Branch

Pull Request

Merge Conflict

GitHub Actions

Salesforce DX + Git
=
Modern Salesforce Development
```