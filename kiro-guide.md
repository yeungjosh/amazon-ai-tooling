# Kiro IDE: Agentic Spec-Driven Development

Kiro is Amazon's AI-powered IDE that brings structure to AI coding, taking you from prototype to production with spec-driven development.

## What is Kiro?

Kiro emerged from a small team within AWS and launched in preview in July 2025. Unlike traditional AI coding assistants that work on prompts, Kiro uses **specifications as the source of truth**.

**Key Philosophy:**
> "Specifications, not prompts or code, are the fundamental unit of programming in the AI era."

### Core Differentiators

| Feature | Traditional AI Coding | Kiro Approach |
|---------|----------------------|---------------|
| **Input** | Vague prompts | Formal specifications |
| **Process** | Single-pass generation | Specify → Plan → Tasks → Implement |
| **Quality** | Variable (60-80%) | 95%+ accuracy on first pass |
| **Control** | Limited | Humans control what/how, AI does heavy lifting |
| **Production-Ready** | Requires heavy refactoring | Built for production from start |
| **Maintenance** | Specs get out of date | Specs are living documentation |

## Architecture

**Built on:**
- Code OSS (VS Code base)
- Claude Sonnet 4.0 (primary)
- Claude 3.7 Sonnet (fallback)
- AWS Bedrock for model access

**Unique Features:**
- Agent Hooks for event-driven automation
- Spec-driven workflow engine
- Built-in governance and security
- AWS MCP (Model Context Protocol) integration

## Getting Started

### Request Access

Kiro is currently in preview:
1. Visit [kiro.dev](https://kiro.dev)
2. Sign up for preview access
3. Wait for approval email (typically 1-2 weeks)

### Installation

```bash
# Download Kiro (once approved)
# macOS
curl -O https://kiro.dev/download/mac/latest

# Linux
curl -O https://kiro.dev/download/linux/latest

# Windows
# Download from https://kiro.dev/download/windows/latest

# Install and launch
chmod +x kiro-installer
./kiro-installer
```

### Initial Configuration

**1. AWS Credentials**
```bash
# Configure AWS CLI first
aws configure

# Kiro uses your AWS credentials for Bedrock access
# Ensure you have permissions for:
# - bedrock:InvokeModel
# - bedrock:InvokeModelWithResponseStream
```

**2. Project Setup**
```bash
# Initialize Kiro in your project
cd your-project
kiro init

# This creates:
# .kiro/
#   ├── config.yaml
#   ├── hooks/
#   ├── specs/
#   └── .gitignore
```

**3. Security Configuration**

Edit `.kiro/config.yaml`:
```yaml
security:
  # Never commit these files
  sensitive_files:
    - "**/.env*"
    - "**/*secret*"
    - "**/*credentials*"
    - "**/config/production.yaml"

  # Trusted shell commands (auto-accepted in Autopilot)
  trusted_commands:
    - "npm test"
    - "npm run lint"
    - "git status"
    - "git diff"

  # Auto-deny these commands
  denied_commands:
    - "rm -rf"
    - "sudo"
    - "curl | bash"

governance:
  # Require human approval for:
  require_approval:
    - file_changes_over: 10  # More than 10 files
    - lines_changed_over: 500  # More than 500 lines
    - files_matching: ["**/migration/**", "**/schema/**"]
```

## The Spec-Driven Workflow

### Step 1: Specify

**What:** Define WHAT you want to build in natural language.

**Example Spec** (`specs/user-authentication.md`):
```markdown
# User Authentication System

## Overview
Implement JWT-based authentication for our REST API with refresh tokens and role-based access control.

## Functional Requirements

### FR-1: User Registration
- Accept email and password
- Validate email format (RFC 5322)
- Password requirements: min 12 chars, 1 uppercase, 1 lowercase, 1 number, 1 special
- Hash passwords with bcrypt (cost factor: 12)
- Send verification email
- Return 201 with user ID (exclude password)

### FR-2: User Login
- Accept email and password
- Rate limit: 5 attempts per 15 minutes per IP
- Return access token (JWT, 15 min expiry) and refresh token (7 day expiry)
- Log successful/failed attempts

### FR-3: Token Refresh
- Accept refresh token
- Validate token signature and expiry
- Issue new access token
- Rotate refresh token (one-time use)

### FR-4: Authorization Middleware
- Verify JWT signature
- Check token expiry
- Extract user ID and roles
- Attach to request context
- Return 401 for invalid/expired tokens

## Non-Functional Requirements

### NFR-1: Security
- Store refresh tokens hashed in database
- Use environment variables for JWT secrets
- Implement HTTPS-only in production
- Add CORS configuration
- Rate limiting on all auth endpoints

### NFR-2: Performance
- Token verification < 10ms
- Login endpoint < 200ms p95
- Database queries optimized with indexes

### NFR-3: Reliability
- Handle database connection failures gracefully
- Retry email sending with exponential backoff
- Comprehensive error logging

## Technical Constraints

- Language: Python 3.12
- Framework: FastAPI 0.110+
- Database: PostgreSQL 16
- ORM: SQLAlchemy 2.x
- JWT Library: PyJWT 2.8+
- Password Hashing: bcrypt 4.x
- Email: AWS SES

## Architecture

### Components
1. Auth Router (FastAPI router)
2. Auth Service (business logic)
3. Token Service (JWT operations)
4. User Repository (database access)
5. Email Service (verification emails)
6. Auth Middleware (request validation)

### Database Schema
```sql
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    is_verified BOOLEAN DEFAULT FALSE,
    role VARCHAR(50) DEFAULT 'user',
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE refresh_tokens (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id) ON DELETE CASCADE,
    token_hash VARCHAR(255) UNIQUE NOT NULL,
    expires_at TIMESTAMP NOT NULL,
    created_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_refresh_tokens_user_id ON refresh_tokens(user_id);
CREATE INDEX idx_refresh_tokens_expires_at ON refresh_tokens(expires_at);
```

## Testing Requirements

- Unit tests for each service (80%+ coverage)
- Integration tests for API endpoints
- Test cases:
  - Valid registration
  - Duplicate email registration
  - Invalid password formats
  - Successful login
  - Failed login (wrong password)
  - Rate limiting trigger
  - Token refresh flow
  - Expired token handling
  - Invalid token handling

## Dependencies

- pyjwt==2.8.0
- bcrypt==4.1.2
- fastapi==0.110.0
- sqlalchemy==2.0.27
- psycopg2-binary==2.9.9
- pydantic==2.6.1
- python-multipart==0.0.9
- boto3==1.34.51 (for SES)

## Success Criteria

- All tests pass
- Code coverage ≥ 80%
- No security vulnerabilities (scan with Amazon Q)
- API documentation auto-generated (OpenAPI)
- Deployment-ready with Docker configuration
```

**Best Practices for Specs:**

✅ **DO:**
- Be specific about versions, formats, constraints
- Include success criteria and testing requirements
- Define error handling expectations
- Specify performance targets
- List technical constraints upfront
- Include database schemas if applicable
- Reference architectural patterns
- Add examples of inputs/outputs

❌ **DON'T:**
- Leave requirements vague or ambiguous
- Skip non-functional requirements
- Forget about error cases
- Omit security considerations
- Ignore performance requirements

### Step 2: Plan

**What:** Kiro analyzes your spec and creates a detailed implementation plan.

**How to trigger:**
```
# In Kiro IDE
Cmd/Ctrl + Shift + P → "Kiro: Generate Plan from Spec"
Select: specs/user-authentication.md
```

**Kiro generates:** (`plans/user-authentication-plan.md`)
```markdown
# Implementation Plan: User Authentication System

## Analysis
- Target: New feature in existing FastAPI application
- Estimated complexity: Medium
- Estimated files: 12 new, 3 modified
- Estimated time: 4-6 hours with AI assistance

## File Structure
```
src/
  auth/
    __init__.py
    router.py          # [NEW] FastAPI router
    service.py         # [NEW] Business logic
    models.py          # [NEW] Pydantic models
    dependencies.py    # [NEW] FastAPI dependencies
  tokens/
    __init__.py
    service.py         # [NEW] JWT operations
  database/
    models.py          # [MODIFY] Add User and RefreshToken models
    repositories/
      user_repository.py  # [NEW]
  middleware/
    auth_middleware.py # [NEW]
  services/
    email_service.py   # [NEW]
  config/
    settings.py        # [MODIFY] Add auth settings
tests/
  unit/
    test_auth_service.py
    test_token_service.py
  integration/
    test_auth_endpoints.py
migrations/
  001_create_users.sql  # [NEW]
  002_create_refresh_tokens.sql  # [NEW]
```

## Implementation Steps

### Phase 1: Database & Models (Tasks 1-3)
1. Create database migration for users table
2. Create database migration for refresh_tokens table
3. Create SQLAlchemy ORM models

### Phase 2: Core Services (Tasks 4-7)
4. Implement Token Service (JWT generation/validation)
5. Implement User Repository (CRUD operations)
6. Implement Email Service (SES integration)
7. Implement Auth Service (registration/login logic)

### Phase 3: API Layer (Tasks 8-10)
8. Create Pydantic request/response models
9. Implement Auth Router (API endpoints)
10. Create Auth Middleware

### Phase 4: Configuration & Dependencies (Tasks 11-12)
11. Update settings with auth configuration
12. Add dependencies to main.py

### Phase 5: Testing (Tasks 13-15)
13. Write unit tests for services
14. Write integration tests for endpoints
15. Test rate limiting and security measures

## Dependencies Between Tasks
- Task 3 depends on Tasks 1-2 (migrations before models)
- Tasks 4-7 can run in parallel
- Task 8 depends on Task 3 (models needed for Pydantic)
- Task 9 depends on Tasks 4-8
- Task 10 depends on Task 4
- Tasks 13-15 depend on all implementation tasks

## Risk Mitigation
- Rate limiting: Use Redis if in-memory solution insufficient
- Email delivery: Implement retry mechanism with exponential backoff
- Token storage: Ensure proper indexes for performance

## Architectural Decisions
- Pattern: Repository pattern for database access
- Error handling: Custom exceptions with FastAPI exception handlers
- Validation: Pydantic models for all requests/responses
- Testing: Pytest with fixtures for database and auth context
```

### Step 3: Tasks

**What:** Break the plan into discrete, executable tasks.

Kiro automatically generates tasks from the plan. You can review and modify:

```
# In Kiro IDE
View → Tasks Panel
```

**Example tasks:**
```
☐ Task 1: Create database migration for users table
  Scope: migrations/001_create_users.sql
  Estimated: 10 minutes

☐ Task 2: Create database migration for refresh_tokens table
  Scope: migrations/002_create_refresh_tokens.sql
  Estimated: 10 minutes

☐ Task 3: Create SQLAlchemy ORM models
  Scope: src/database/models.py
  Dependencies: Task 1, Task 2
  Estimated: 20 minutes

☐ Task 4: Implement Token Service
  Scope: src/tokens/service.py, tests/unit/test_token_service.py
  Estimated: 45 minutes

... (continues for all tasks)
```

### Step 4: Implement

**What:** Kiro executes tasks autonomously, one at a time, with your approval.

**Execution modes:**

1. **Guided Mode** (recommended for learning):
   - Kiro implements each task
   - Shows you the diff
   - Waits for your approval
   - You can modify or reject

2. **Autopilot Mode** (once confident):
   - Kiro executes all tasks
   - Only pauses for configured approvals
   - Faster but requires trust

**Example execution:**

```
# Start implementation
Cmd/Ctrl + Shift + P → "Kiro: Implement Plan"

# Kiro shows:
[Task 1/15] Create database migration for users table

Creating file: migrations/001_create_users.sql
+++ migrations/001_create_users.sql
@@ -0,0 +1,12 @@
+CREATE TABLE users (
+    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
+    email VARCHAR(255) UNIQUE NOT NULL,
+    password_hash VARCHAR(255) NOT NULL,
+    is_verified BOOLEAN DEFAULT FALSE,
+    role VARCHAR(50) DEFAULT 'user',
+    created_at TIMESTAMP DEFAULT NOW(),
+    updated_at TIMESTAMP DEFAULT NOW()
+);
+
+CREATE INDEX idx_users_email ON users(email);
+CREATE INDEX idx_users_created_at ON users(created_at);

[Approve] [Modify] [Skip] [Stop]
```

**You click [Approve], Kiro continues:**

```
✓ Task 1 complete

[Task 2/15] Create database migration for refresh_tokens table
...
```

## Agent Hooks: Automation on Steroids

Agent hooks are event-driven AI automations that run in the background.

### What Are Agent Hooks?

Hooks trigger AI agents on events like:
- File saves
- Git commits
- Test runs
- Builds
- Deployment

### Built-in Hooks

Kiro includes several default hooks in `.kiro/hooks/`:

**1. Auto-Documentation Hook**
```yaml
# .kiro/hooks/auto-doc.yaml
name: "Auto-Documentation"
trigger:
  event: "file.save"
  filter:
    - "src/**/*.py"
    - "!**/__init__.py"

action:
  type: "agent"
  prompt: |
    Analyze the just-saved file: {file_path}

    Generate or update:
    1. Docstrings for all functions/classes (Google style)
    2. Type hints for all parameters and returns
    3. Usage examples for public APIs

    Follow project conventions from docs/CONTRIBUTING.md

    Do NOT modify logic, only add documentation.

  auto_commit: false
  require_approval: true
```

**2. Test Generation Hook**
```yaml
# .kiro/hooks/auto-test.yaml
name: "Auto-Test Generation"
trigger:
  event: "file.save"
  filter:
    - "src/**/*.py"
    - "!src/**/test_*.py"

action:
  type: "agent"
  prompt: |
    Analyze: {file_path}

    Generate/update corresponding test file:
    - Location: tests/unit/test_{filename}
    - Framework: pytest
    - Coverage: Aim for 80%+ line coverage
    - Test cases: Happy path, edge cases, error conditions
    - Fixtures: Use existing fixtures from tests/conftest.py
    - Mocking: Use unittest.mock for external dependencies

    Match existing test style from tests/ directory.

  auto_commit: false
  require_approval: true
```

**3. Code Review Hook**
```yaml
# .kiro/hooks/pre-commit-review.yaml
name: "Pre-Commit Code Review"
trigger:
  event: "git.pre-commit"

action:
  type: "agent"
  prompt: |
    Review all staged changes.

    Check for:
    1. Code quality issues
    2. Security vulnerabilities
    3. Performance concerns
    4. Missing error handling
    5. Inconsistencies with project patterns (@workspace)
    6. Missing tests for new features
    7. Exposed secrets or credentials

    Provide actionable feedback.
    Block commit if critical issues found.

  blocking: true
  require_approval: false
```

**4. Dependency Update Hook**
```yaml
# .kiro/hooks/dependency-check.yaml
name: "Dependency Freshness Check"
trigger:
  event: "file.save"
  filter:
    - "package.json"
    - "requirements.txt"
    - "Cargo.toml"
    - "go.mod"

action:
  type: "agent"
  prompt: |
    Analyze: {file_path}

    Check for:
    1. Outdated dependencies (patch/minor/major)
    2. Security vulnerabilities (npm audit, safety, etc.)
    3. Deprecated packages

    Suggest updates with:
    - Version to upgrade to
    - Breaking changes (if major version)
    - Security fixes included

  auto_commit: false
  require_approval: true
```

### Creating Custom Hooks

**Example: Auto-Optimize on Save**

```yaml
# .kiro/hooks/auto-optimize.yaml
name: "Auto-Optimization"
trigger:
  event: "file.save"
  filter:
    - "src/**/*.py"

  # Only trigger if file is "large"
  condition: |
    file_size_kb > 10 or cyclomatic_complexity > 15

action:
  type: "agent"
  prompt: |
    Optimize: {file_path}

    Focus on:
    1. Reduce cyclomatic complexity (target: < 10 per function)
    2. Eliminate duplicate code
    3. Improve time/space complexity
    4. Add caching where beneficial
    5. Optimize database queries

    Maintain:
    - Existing functionality (must pass tests)
    - Code readability
    - Error handling

    Run tests after changes: npm test

  auto_commit: false
  require_approval: true
  run_tests: true
```

**Example: AWS Resource Tagging Enforcement**

```yaml
# .kiro/hooks/aws-tagging.yaml
name: "AWS Resource Tagging"
trigger:
  event: "file.save"
  filter:
    - "**/*.yaml"  # CloudFormation
    - "**/*.ts"    # CDK

action:
  type: "agent"
  prompt: |
    Check {file_path} for AWS resource definitions.

    Ensure ALL resources have required tags:
    - Environment (dev/staging/prod)
    - Owner (team name)
    - CostCenter (billing code)
    - Project (project name)
    - ManagedBy (terraform/cloudformation/cdk)

    Add missing tags following our tagging policy:
    https://wiki.internal/aws-tagging-policy

    Block commit if tags missing.

  blocking: true
  require_approval: false
```

### Hook Best Practices

✅ **DO:**
- Start with simple hooks (file-to-file relationships)
- Provide clear, detailed prompts with context
- Reference project documentation in hook prompts
- Set `require_approval: true` initially
- Use `blocking: true` for critical validations
- Share hooks with team via git
- Test hooks on non-critical files first

❌ **DON'T:**
- Create hooks that modify too many files at once
- Use `auto_commit: true` without thorough testing
- Skip approval requirements for destructive operations
- Create circular dependencies between hooks
- Forget to add filters (hooks triggering on every file)

## Best Practices for Kiro

### 1. Start with Small Specs

Your first spec should be a small, well-defined feature:
- 1-3 files maximum
- Clear inputs/outputs
- Testable in 1-2 hours

**Good first spec:**
- Add a new API endpoint
- Implement a utility function
- Create a data validation module

**Not a good first spec:**
- Rewrite entire authentication system
- Build complete microservice

### 2. Iterate on Specs

Specs are living documents:
```bash
# Version control your specs
git add specs/
git commit -m "docs: add authentication spec v1"

# Update as requirements change
# Kiro can diff specs and update implementation
```

### 3. Review Generated Plans

Don't blindly accept plans:
- Check file organization makes sense
- Verify dependencies are correct
- Ensure test coverage is adequate
- Add missing edge cases

Edit plans before implementation:
```markdown
# In generated plan, you can add:
## Additional Considerations
- Add rate limiting to prevent brute force
- Consider adding 2FA in future iteration
- Ensure GDPR compliance for user data
```

### 4. Use Autopilot Wisely

**Use Autopilot when:**
- Implementing well-defined specs
- Making repetitive changes
- Updating dependencies
- Generating boilerplate

**Use Guided Mode when:**
- Working on critical systems
- Learning new patterns
- Implementing complex business logic
- Unsure about requirements

### 5. Keep Humans in the Loop

**Governance checklist:**
```yaml
# .kiro/governance.yaml
require_human_approval:
  - file_patterns:
      - "**/migrations/**"
      - "**/schema/**"
      - "**/*production*"
      - "**/Dockerfile"
      - "**/.github/workflows/**"

  - change_types:
      - database_schema
      - security_config
      - ci_cd_pipeline
      - environment_variables

  - thresholds:
      files_changed: 10
      lines_added: 500
      complexity_increase: 20%
```

### 6. Leverage @workspace in Hook Prompts

Give agents full codebase context:
```yaml
action:
  prompt: |
    @workspace Review this change against our architectural patterns.

    File: {file_path}

    Check:
    1. Does it follow our error handling pattern?
    2. Does it match our logging conventions?
    3. Is it consistent with similar components?
    4. Are there better existing utilities we should reuse?
```

## Kiro + Amazon Q: The Power Combo

Use both tools strategically:

| Task | Tool | Why |
|------|------|-----|
| Exploratory coding | Amazon Q | Faster iteration, inline suggestions |
| Production feature development | Kiro | Spec-driven, better structure |
| Quick bug fixes | Amazon Q | Immediate, contextual |
| Refactoring large systems | Kiro | Plan-driven, safer |
| AWS-specific code | Amazon Q | AWS expertise |
| Multi-file features | Kiro | Better orchestration |
| Learning new codebase | Amazon Q @workspace | Fast exploration |
| Onboarding new features | Kiro specs | Documentation + code |

**Workflow example:**
1. Explore codebase with Amazon Q
2. Write spec for new feature
3. Implement with Kiro
4. Optimize specific functions with Amazon Q
5. Document with Kiro hooks

## Measuring Success with Kiro

Track these metrics:

**Quality Metrics:**
- First-pass implementation accuracy (target: 95%+)
- Test coverage (target: 80%+)
- Security vulnerabilities (target: 0 high/critical)
- Code review approval rate

**Productivity Metrics:**
- Time from spec to working code
- Number of manual edits needed
- Documentation completeness
- Spec reusability across features

**Team Metrics:**
- Onboarding time for new features
- Knowledge sharing (specs as documentation)
- Architectural consistency
- Technical debt reduction

## Troubleshooting

### Kiro generates incorrect code

**Likely cause:** Spec is ambiguous or missing context

**Fix:**
1. Review spec for vagueness
2. Add more technical constraints
3. Include examples
4. Reference existing code patterns

### Agent hooks not triggering

**Check:**
```bash
# View hook logs
kiro logs --hooks

# Verify hook syntax
kiro validate-hooks

# Test hook manually
kiro run-hook .kiro/hooks/auto-test.yaml --file src/test.py
```

### Plan seems wrong

**You can:**
1. Provide feedback: "Regenerate plan with X consideration"
2. Manually edit the plan file
3. Ask Kiro to explain the plan
4. Start over with better spec

### Performance is slow

**Optimization:**
- Reduce @workspace scope for hooks
- Use Claude 3.7 instead of 4.0 for simple tasks
- Increase Bedrock quotas
- Cache spec generations

## Next Steps

1. ✅ Request Kiro access at kiro.dev
2. ✅ Read spec-driven-development.md
3. ✅ Write your first small spec
4. ✅ Review and edit generated plan
5. ✅ Execute in Guided Mode
6. ✅ Set up 2-3 basic agent hooks
7. ✅ Graduate to Autopilot for trusted tasks

## Resources

- [Kiro Official Docs](https://kiro.dev/docs)
- [Spec-Driven Development Guide](./spec-driven-development.md)
- [Kiro Blog](https://kiro.dev/blog)
- [AWS re:Post - Kiro Discussions](https://repost.aws/tags/kiro)
- [GitHub Spec Kit](https://github.com/github/spec-kit) (compatible with Kiro)

---

**Remember:** Kiro is about bringing engineering discipline to AI coding. It's not faster for quick scripts, but it shines for production features that need to be maintainable, testable, and well-documented.
