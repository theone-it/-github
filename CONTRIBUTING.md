# TheOne 开发规范
本文件定义 `theone-it` GitHub Organization 下所有 Repository 共同遵循的开发规范。

除非特定 Repository 有额外要求，否则所有 Developer 应遵循本规范。
特定 Repository 可以增加自己的开发规则，但不应降低本文件定义的 Security、Code Review 及 Production Safety 要求。

---

## Repository 命名规范
不同类型的 Repository 使用不同的命名规则。

### 普通项目
普通项目 Repository 使用以下格式：
`<project>-<platform>[-<component>]`

Repository 名称统一使用小写字母，并使用 `-` 分隔不同部分。
例如：
- `myboss-ui`
- `myboss-ui-api`
- `myboss-bo`
- `myboss-bo-api`
- `myboss-bo-worker`
- `safepay-ui`
- `safepay-ui-api`

常用 Platform / Component：
- `ui` — User Interface / User-facing Application
- `bo` — Back Office
- `api` — Web API / Backend API
- `worker` — Background Worker Service

新增 Platform 或 Component 缩写时，应确保命名清楚并在不同项目之间保持一致。

### 第三方 Integration Package
第三方 Integration Package Repository 使用以下格式：
`<CATEGORY>.<Brand>`

其中：
- `CATEGORY` 使用统一的大写分类缩写。
- `Brand` 使用第三方 Vendor / Provider 的正式或公司内部统一确认的品牌名称。

例如：
- `GP.AWC`
- `GP.EliteHubz`
- `PG.PayEssen`

目前使用的 Category：
- `GP` — Game Provider
- `PG` — Payment Gateway

新增 Category 时，应先确定统一的 Category 缩写，避免不同 Repository 对相同类型使用不同名称。

### Repository 与 NuGet Package
第三方 Integration Package 的 Repository Name 与发布的 NuGet Package ID 必须保持一致。

例如：
Repository：
`GP.AWC`

NuGet Package：
`GP.AWC`

Repository：
`PG.PayEssen`

NuGet Package：
`PG.PayEssen`

不应为同一个 Package 使用不同的 Repository Name 与 NuGet Package ID。

### General Rules
Repository 命名应：
- 简短并能够清楚表达用途。
- 遵循对应 Repository 类型的命名规范。
- 对相同 Platform、Component 及 Category 使用一致的名称。
- 避免使用 `new`、`latest`、`final`、`test2` 等临时名称。
- 避免在 Repository Name 中加入 Version Number，例如 `v2`、`v3`。
- 除非年份本身属于正式产品名称，否则避免加入年份。

已经投入使用的 Repository 不应随意 Rename。

Rename 前必须确认是否会影响：
- CI/CD Pipeline
- GitHub Actions
- NuGet Package
- Project Reference
- Git Submodule
- Deployment Configuration
- External Integration
- Documentation

---

## Semantic Versioning
所有正式发布的 Application、NuGet Package 及其他可发布组件统一采用 Semantic Versioning。

格式：
`MAJOR.MINOR.PATCH`

例如：
`1.4.2`

### MAJOR
当修改包含不向后兼容的 Breaking Change 时增加 MAJOR Version。

例如：
`1.4.2 → 2.0.0`

Breaking Change 包括但不限于：
- 删除或修改现有 Public API。
- 修改现有 API Contract，并导致旧 Consumer 无法继续使用。
- 修改 NuGet Package Public Interface，并导致现有调用方式失效。
- 其他需要 Consumer 修改现有代码才能继续使用的变更。

### MINOR
增加向后兼容的新功能时增加 MINOR Version。

例如：
`1.4.2 → 1.5.0`

例如：
- 增加新的 API。
- 增加新的 Package Function。
- 增加 Optional Parameter，并保持旧调用方式兼容。
- 增加现有 Consumer 不需要修改即可继续使用的新功能。

### PATCH
进行向后兼容的 Bug Fix 时增加 PATCH Version。

例如：
`1.4.2 → 1.4.3`

例如：
- 修复 Bug。
- 修复错误的 Provider Response Mapping。
- 修复 Timeout Handling。
- 修复不改变 Public Interface 的内部逻辑问题。

### Git Tag
正式 Release 的 Git Tag 使用以下格式：
`v<MAJOR>.<MINOR>.<PATCH>`

例如：
- `v1.0.0`
- `v1.4.2`
- `v2.0.0`

已经正式发布的 Version 不应被覆盖或重新发布不同内容。
需要修改时，应发布新的 Version。

---

## Branch 规范
`main` Branch 代表 Repository 当前稳定版本。
正常 Development 不应直接在 `main` Branch 上进行。
开发工作应建立独立 Branch，并通过 Pull Request Merge 回 `main`。

### Branch Naming
Branch 使用以下格式：
`<type>/<description>`

支持的 Type：
- `feature` — 新功能
- `fix` — Bug Fix
- `hotfix` — 需要紧急处理的 Production Fix
- `refactor` — 不改变预期行为的 Code Refactoring
- `chore` — Maintenance / 非功能性修改

例如：
- `feature/player-login`
- `feature/awc-settlement`
- `fix/token-validation`
- `fix/provider-timeout`
- `hotfix/wallet-balance`
- `refactor/settlement-service`
- `chore/update-dependencies`

Branch Name 应：
- 使用小写字母。
- 使用 `-` 分隔单词。
- 简短并能够说明修改目的。
- 避免使用 Developer Name 作为 Branch Name。
- 避免使用 `test`、`temp`、`new` 等无法说明实际用途的名称。

---

## Commit 规范
Commit Message 应能够清楚说明本次修改内容。

推荐格式：
`<type>: <description>`

支持的 Type：
- `feat` — 新功能
- `fix` — Bug Fix
- `refactor` — Code Refactoring
- `perf` — Performance Improvement
- `docs` — Documentation
- `test` — Test 相关修改
- `build` — Build / Dependency 相关修改
- `ci` — CI/CD 相关修改
- `chore` — Maintenance

例如：
`feat: add player token validation`
`fix: prevent duplicate settle transaction`
`fix: handle AWC callback timeout`
`refactor: simplify wallet transaction handling`
`docs: update AWC integration documentation`
`ci: add package publish workflow`

Commit Message 应描述：
**修改了什么**

而不是：
**谁修改了什么**

应避免没有明确意义的 Commit Message，例如：
`update`
`changes`
`fix`
`test`
`final`
`latest`

---

## Pull Request
对受保护 Branch 的修改应通过 Pull Request 进行。

Pull Request 应：
- 使用清楚的 Title。
- 说明修改目的。
- 说明主要 Changes。
- 标记可能受到影响的 Component。
- 说明是否包含 Breaking Change。
- 说明 Database / Configuration 是否需要修改。
- 说明 Testing 方法。
- 说明特殊 Deployment Requirement。
- 通过要求的 Automated Checks。
- 在需要时完成 Code Review 后才 Merge。

不相关的大量修改不应放入同一个 Pull Request。
如果一个 Pull Request 同时包含多个没有直接关系的功能，应考虑拆分成多个 Pull Request。

---

## Code Review
Reviewer 应根据修改内容检查以下项目：
- Correctness
- Security
- Error Handling
- Backward Compatibility
- API Compatibility
- Database Impact
- Performance Impact
- Maintainability
- Testing
- Deployment Impact

以下类型的修改需要特别谨慎 Review：
- Authentication
- Authorization / Permission
- Player Account
- Agent Account
- Financial Transaction
- Wallet
- Payment
- Bet
- Settle
- Third-party Callback
- Database Transaction
- Production Configuration
- Infrastructure
- Security-related Logic

Reviewer 不应只检查代码是否能够 Compile，还应确认修改是否可能影响现有 Business Flow 及 Production Behavior。

---

## Shared Package / Third-Party Integration
Shared Package 的目标是提供可重复使用的 Integration 能力。

第三方 Integration Package 可以负责：
- API Communication
- Authentication
- Request / Response Model
- Serialization / Deserialization
- Signature Generation
- Signature Validation
- Encryption / Decryption
- Provider Error Mapping
- Timeout Handling
- Retry Handling
- Provider-specific Protocol Handling

Shared Package 不应包含特定 Application 的 Business Logic。
例如：

允许：
`AWC Client → Settle API`

不建议：
`AWC Client → Update Myboss Player Wallet`
`Myboss`、`Platform` 或其他 Consumer 应自行负责自己的 Business Logic。

第三方 Integration Package 的 Public API 如发生 Breaking Change，必须增加 MAJOR Version。

---

## Release
Production Release 应具有明确且可以追踪的 Version。

例如：
`v2.3.1`

每一个正式 Release 必须能够通过对应 Git Tag 找回其 Source Code。

Release Artifact 应能够对应到明确的 Source Version。
例如：

Git Tag：
`v1.4.2`

NuGet Package：
`GP.AWC 1.4.2`

未来使用 Container Deployment 时：
`myboss-ui-api:1.4.2`

已经发布的 Artifact 不应在保持相同 Version 的情况下被不同内容覆盖。
如有修改，应建立新的 Version。

---

## Database 变更
涉及 Database 的修改应特别注意 Backward Compatibility 及 Deployment Order。

Database 变更包括但不限于：
- Table
- Column
- Index
- Constraint
- Stored Procedure
- Function
- View
- Data Migration

如果 Application 与 Database 无法同时完成 Deployment，应优先采用 Backward-compatible Migration。

例如：
1. 先增加新的 Database Structure。
2. 保持旧 Application 仍然能够正常运行。
3. Deployment 新 Application。
4. 确认新版本稳定。
5. 后续 Release 再移除 Deprecated Structure。

避免在同一次 Deployment 中直接删除仍被当前 Production Version 使用的 Database Structure。

Pull Request 必须说明 Database Change 及必要的 Deployment Order。

---

## Configuration
Application Configuration 不应直接包含 Sensitive Information。

不同 Environment 的 Configuration 应明确区分，例如：
- Development
- Staging
- Production

新增或修改以下 Configuration 时，应在 Pull Request 中说明：
- Environment Variable
- Connection Setting
- External Service Endpoint
- Feature Setting
- Provider Configuration
- Deployment Configuration

Sensitive Value 不应直接写入 Repository。

---

## Security
任何 Repository 都禁止 Commit Sensitive Information。

包括但不限于：
- Password
- API Key
- Access Token
- Refresh Token
- Private Key
- Certificate Private Key
- Database Credential
- Production Connection String
- Cloud Credential
- Provider Secret
- Encryption Secret
- Deployment Credential

Secret 应通过公司批准的 Secret Management 方式保存。

如果发现 Secret 被错误 Commit：
**不能只删除文件或建立新的 Commit。**

应立即：

1. 停止继续使用相关 Secret。
2. Rotate / Revoke 已泄露的 Credential。
3. 检查可能的影响范围。
4. 根据需要清理 Git History。
5. 确认新的 Secret 已通过正确方式配置。

---

## Repository-Specific Rules
个别 Repository 可以根据实际技术或业务需要增加自己的规则。

例如：
- Package-specific Release Process
- API-specific Testing Requirement
- Database Deployment Requirement
- Frontend Build Requirement
- Infrastructure Deployment Requirement

Repository-specific Rule 可以扩展本规范，但不应降低 Organization 对 Security、Code Review 及 Production Safety 的基本要求。

---

## 自动化
本规范中的部分要求目前可能通过人工方式执行。

随着 CI/CD 建立，将逐步通过 GitHub Rules、GitHub Actions 及其他 Automation 强制执行，包括但不限于：
- Pull Request Validation
- Build Validation
- Automated Testing
- Commit / Version Validation
- Package Build
- Package Publish
- Container Build
- Security Check
- Release
- Deployment

当 Automated Rule 与本文件规定不一致时，应先确认 Organization 最新规范，再修改 Automation 或本文件，使两者保持一致。
