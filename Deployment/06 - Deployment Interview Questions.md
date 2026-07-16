
## Beginner Level Questions

### 1. What is Deployment in Salesforce?

Deployment is the process of moving metadata from one Salesforce org to another.

Example:

```text
Sandbox
    ↓
Production
```

Metadata includes:

```text
Apex Classes
Triggers
LWC
Flows
Objects
Fields
Permission Sets
```

---

### 2. What is a Change Set?

A Salesforce-native deployment mechanism used to move metadata between related Salesforce orgs.

Types:

```text
Outbound Change Set
Inbound Change Set
```

---

### 3. What Can Be Deployed Using Change Sets?

```text
Apex Classes
Triggers
LWC
Flows
Objects
Fields
Layouts
Validation Rules
Permission Sets
```

---

### 4. Can Change Sets Deploy Data?

```text
No
```

Change Sets deploy metadata only.

---

### 5. What is an Outbound Change Set?

A Change Set created in the source org.

Example:

```text
Developer Sandbox
```

---

### 6. What is an Inbound Change Set?

A Change Set received in the target org.

Example:

```text
Production
```

---

# Intermediate Level Questions

### 7. What is Validation Deployment?

Validation checks whether deployment will succeed without actually deploying metadata.

Benefits:

```text
Detect Errors Early

Validate Apex Tests

Check Dependencies
```

---

### 8. What is Quick Deploy?

Quick Deploy allows deployment of previously validated metadata without rerunning tests.

Interview Favorite.

---

### 9. What are Deployment Dependencies?

Components required by other components.

Example:

```text
Custom Object
      ↓
Custom Field
      ↓
Page Layout
```

---

### 10. What Happens During Production Deployment?

Salesforce:

```text
Validates Metadata

Runs Apex Tests

Checks Dependencies

Deploys Components
```

---

### 11. What is the Minimum Apex Code Coverage Required?

```text
75%
```

For production deployment.

---

### 12. Can Every Apex Class Have Less Than 75% Coverage?

```text
Yes
```

Requirement:

```text
Overall Org Coverage ≥ 75%
```

---

### 13. What Test Levels Are Available During Deployment?

### Run Local Tests

```text
Runs Local Apex Tests
```

---

### Run Specified Tests

```text
Runs Selected Tests
```

---

### Run All Tests

```text
Runs Managed Package Tests Also
```

---

### 14. Why Validate Before Deployment?

To identify issues before impacting production.

---

### 15. What Causes Deployment Failures?

Examples:

```text
Compilation Errors

Failed Tests

Missing Dependencies

Code Coverage Issues

Validation Rule Failures
```

---

# Salesforce DX Questions

### 16. What is Salesforce DX?

A modern development model supporting:

```text
Source Driven Development

Git

CI/CD

Scratch Orgs
```

---

### 17. What is Source-Driven Development?

Source code becomes the source of truth.

```text
Git
   ↓
Salesforce Org
```

---

### 18. What is a Scratch Org?

A temporary Salesforce org used for development and testing.

---

### 19. Why is Salesforce DX Better Than Change Sets?

| Change Sets | Salesforce DX |
|------------|---------------|
| Manual | Automated |
| No Git | Git Integrated |
| No CI/CD | CI/CD Friendly |
| Org Based | Source Based |

---

### 20. Which Command Deploys Metadata?

```bash
sf project deploy start
```

---

### 21. Which Command Retrieves Metadata?

```bash
sf project retrieve start
```

---

# Git & GitHub Questions

### 22. What is Git?

A distributed version control system.

---

### 23. What is GitHub?

A cloud platform for hosting Git repositories.

---

### 24. What is a Branch?

An isolated line of development.

Example:

```text
main

develop

feature/account-api
```

---

### 25. What is a Pull Request?

A request to merge code into another branch after review.

---

### 26. What is a Merge Conflict?

Occurs when Git cannot automatically merge changes.

---

### 27. Why Use Feature Branches?

```text
Isolation

Code Reviews

Safer Releases
```

---

# CI/CD Questions

### 28. What is CI?

Continuous Integration.

Developers merge code frequently and automated validation runs.

---

### 29. What is CD?

Continuous Delivery or Continuous Deployment.

Automates deployment process.

---

### 30. Difference Between Continuous Delivery and Continuous Deployment?

| Delivery | Deployment |
|-----------|------------|
| Manual Approval | Fully Automated |
| Common in Salesforce | Less Common |

---

### 31. What Happens In A Salesforce CI/CD Pipeline?

```text
Commit Code
      ↓
Pull Request
      ↓
Run Tests
      ↓
Validate
      ↓
Deploy
```

---

### 32. Which CI/CD Tools Are Commonly Used?

```text
GitHub Actions

Jenkins

Azure DevOps

GitLab CI

DevOps Center
```

---

# Scenario-Based Questions

### 33. Your Deployment Failed Due To Missing Components. What Would You Check?

Answer:

```text
Dependencies

Custom Fields

Objects

Permission Sets

Profiles
```

---

### 34. A Deployment Passed Validation Yesterday. How Can You Deploy Faster Today?

Answer:

```text
Quick Deploy
```

---

### 35. A Critical Production Bug Needs Immediate Fix. What Deployment Strategy Would You Use?

Answer:

```text
Hotfix Deployment
```

---

### 36. How Would You Roll Back A Salesforce Deployment?

Answer:

```text
Git Revert

Redeploy Previous Version

Restore Backup
```

Salesforce does not provide automatic rollback.

---

### 37. A Team Has Multiple Developers Working Simultaneously. Which Deployment Strategy Would You Recommend?

Answer:

```text
Git Feature Branches
+
Pull Requests
+
CI/CD Pipeline
```

---

### 38. When Would You Use Change Sets Instead Of Salesforce DX?

Answer:

```text
Small Projects

Simple Deployments

Teams Without CI/CD
```

---

### 39. What Is The Recommended Modern Salesforce Deployment Approach?

Answer:

```text
Git
+
Salesforce DX
+
CI/CD
+
Feature Branches
```

---

### 40. Describe Your Deployment Process In Your Current Project.

Sample Answer:

```text
We use Git-based development.

Developers work on feature branches.

Code is reviewed using Pull Requests.

GitHub Actions runs validation and Apex tests.

Changes are deployed to QA and UAT.

After business approval, deployment is performed to Production using CI/CD pipelines.
```

---

# Top 10 Questions Most Frequently Asked

### 1. What is a Change Set?

### 2. What is Salesforce DX?

### 3. What is a Scratch Org?

### 4. What is Git?

### 5. What is a Pull Request?

### 6. What is CI/CD?

### 7. What is Quick Deploy?

### 8. What is the Minimum Code Coverage Required?

```text
75%
```

### 9. How Do You Roll Back A Deployment?

### 10. Describe Your Deployment Process.

---

# Interview Cheat Sheet

```text
Change Sets
→ Native Deployment

Salesforce DX
→ Modern Development

Scratch Org
→ Temporary Org

Git
→ Version Control

Pull Request
→ Code Review

CI
→ Continuous Integration

CD
→ Continuous Delivery

Quick Deploy
→ Deploy Without Rerunning Tests

Code Coverage
→ 75%

Modern Deployment
→ Git + SFDX + CI/CD
```