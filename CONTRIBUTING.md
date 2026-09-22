# TheOne Development Standards

This document defines the common development standards for repositories under the `theone-it` GitHub organization.

These standards apply to all developers unless a repository defines additional project-specific requirements.

---

## 1. Versioning

All production applications, reusable packages, and released components should follow Semantic Versioning.

Format:

MAJOR.MINOR.PATCH

Example:

1.4.2

### MAJOR

Increment the MAJOR version when introducing incompatible or breaking changes.

Example:

1.4.2 → 2.0.0

### MINOR

Increment the MINOR version when adding backward-compatible functionality.

Example:

1.4.2 → 1.5.0

### PATCH

Increment the PATCH version when making backward-compatible bug fixes.

Example:

1.4.2 → 1.4.3

Git tags should use the following format:

v1.4.2

---

## 2. Branches

The `main` branch represents the stable version of the repository.

Development work should normally be performed in a separate branch and merged through a Pull Request.

Recommended branch naming:

feature/<description>
fix/<description>
hotfix/<description>
refactor/<description>
chore/<description>

Examples:

feature/player-login
feature/awc-settlement
fix/token-validation
hotfix/wallet-balance
refactor/settlement-service

Branch names should:

- Use lowercase letters.
- Use hyphens to separate words.
- Be short and descriptive.
- Avoid developer names in branch names.

---

## 3. Commits

Commit messages should clearly describe the change being made.

Recommended format:

<type>: <description>

Supported types:

- feat: New functionality
- fix: Bug fix
- refactor: Code restructuring without changing expected behavior
- perf: Performance improvement
- docs: Documentation change
- test: Test-related change
- build: Build or dependency change
- ci: CI/CD configuration change
- chore: Maintenance work

Examples:

feat: add player token validation

fix: prevent duplicate settle transaction

refactor: simplify wallet transaction handling

docs: update AWC integration documentation

Commit messages should describe the change rather than the developer performing the change.

---

## 4. Pull Requests

Changes to protected branches should be submitted through a Pull Request.

A Pull Request should:

- Have a clear title.
- Explain the purpose of the change.
- Identify important behavior changes.
- Identify breaking changes when applicable.
- Pass required automated checks.
- Be reviewed before merging when review is required.

Large unrelated changes should not be combined into a single Pull Request.

---

## 5. Code Review

Reviewers should check:

- Correctness
- Security
- Error handling
- Backward compatibility
- Database impact
- API compatibility
- Performance impact
- Maintainability

Changes affecting financial transactions, wallet operations, authentication, permissions, or production infrastructure require additional care during review.

---

## 6. Releases

Production releases should have a clearly identifiable version.

Example:

v2.3.1

A released version should be reproducible from its corresponding Git tag.

Published packages and deployment artifacts should not silently replace an existing released version.

A new version should be created instead.

---

## 7. Reusable Packages

Shared packages should remain independent from application-specific business logic.

Packages responsible for third-party integrations should contain reusable integration functionality such as:

- API communication
- Authentication
- Request and response models
- Serialization
- Signature generation and validation
- Encryption and decryption
- Provider error mapping
- Retry and timeout handling

Application-specific business rules should remain within the consuming application.

Breaking changes to public package APIs require a MAJOR version increment.

---

## 8. Security

Never commit sensitive information to a repository.

This includes:

- Passwords
- API keys
- Access tokens
- Private keys
- Database credentials
- Production connection strings
- Cloud credentials
- Provider secrets

Secrets must be stored using approved secret-management mechanisms.

---

## 9. Repository-Specific Rules

Individual repositories may define additional requirements when necessary.

Repository-specific requirements may extend these organization standards but should not weaken security or production-safety requirements.
