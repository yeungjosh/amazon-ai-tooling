# Amazon Q Developer Comprehensive Guide

Master Amazon Q Developer to maximize your productivity as an SDE2/ML engineer at Amazon.

## Table of Contents
1. [Setup & Installation](#setup--installation)
2. [Core Features](#core-features)
3. [Context Management System](#context-management-system)
4. [Prompt Library](#prompt-library)
5. [Rules Engine](#rules-engine)
6. [Advanced Techniques](#advanced-techniques)
7. [AWS Integration](#aws-integration)
8. [Customization for Teams](#customization-for-teams)

---

## Setup & Installation

### IDE Installation

**VS Code**:
```bash
# Install from marketplace
code --install-extension amazonwebservices.amazon-q-vscode
```

**JetBrains**:
1. Settings → Plugins → Marketplace
2. Search "Amazon Q"
3. Install and restart

**AWS Console**: Already integrated (no installation needed)

### Authentication

```bash
# AWS Builder ID (recommended for developers)
# Follow prompts in IDE after installation

# OR IAM Identity Center
# Configure through AWS organization
```

### Initial Configuration

After installation, configure these settings in your IDE:

**VS Code** (`settings.json`):
```json
{
  "amazonQ.telemetry": true,
  "amazonQ.shareCodeWhispererContentWithAWS": true,
  "amazonQ.experiments": {
    "contextSize": "100k"
  }
}
```

---

## Core Features

### 1. Inline Code Suggestions

**How it works**:
- Type code and Amazon Q suggests completions (snippets to full functions)
- Uses workspace context, not just open files
- 50% acceptance rate (industry-leading)

**Best practices**:
- Write descriptive comments before functions
- Use clear variable names
- Keep files organized (helps context awareness)

**Example**:
```python
# Write this comment:
# Function to validate email addresses using regex and check domain MX records

# Amazon Q suggests:
import re
import dns.resolver

def validate_email(email: str) -> bool:
    """Validate email address format and verify domain has MX records."""
    # Regex validation
    pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
    if not re.match(pattern, email):
        return False

    # Check MX records
    try:
        domain = email.split('@')[1]
        dns.resolver.resolve(domain, 'MX')
        return True
    except (dns.resolver.NXDOMAIN, dns.resolver.NoAnswer):
        return False
```

### 2. Chat Interface

**Access**:
- VS Code: `Cmd+I` (Mac) / `Ctrl+I` (Windows)
- Sidebar panel: Amazon Q icon

**Capabilities**:
- Explain code
- Generate functions
- Debug issues
- Write tests
- Refactor code
- Answer AWS questions

**Context injection**:
```
# Include specific symbols
@ClassName
@functionName
@globalVariable

# Include files
@filename.py

# The context window is 100K characters (expanded April 2025)
```

**Conversation history**:
- Persists between sessions (added April 2025)
- Search conversation history
- Export as markdown

### 3. Developer Agent (Agentic Mode)

**Capabilities**:
- Autonomous multi-file editing
- Implements features, fixes bugs, writes tests
- Runs shell commands
- Generates code diffs
- Provides real-time updates

**How to use**:
1. Open chat
2. Describe feature: "Implement user authentication with JWT tokens"
3. Agent analyzes codebase
4. Generates implementation plan
5. Executes changes across multiple files
6. Provides diffs for review

**Agent architecture**:
- **Memory management agent**: Analyzes previous iteration results
- **Critic agent**: Provides feedback to debugger
- **Debugger agent**: Modifies plan to fix errors
- **Tools**: Browse code, edit files, trigger builds, add dependencies

### 4. Code Transformation Agent

**Use case**: Large-scale refactoring (Java upgrades, framework migrations)

**Capabilities**:
- 85% success rate on 100K+ LOC codebases
- Multi-file solution with inter-iteration memory
- Handles dependencies, build configs, tests

**Example workflow**:
```bash
# In IDE, select code transformation
# Choose: "Upgrade Java 8 to Java 17"
# Agent analyzes codebase
# Generates transformation plan
# Executes across all files
# Validates with build and tests
```

**Free tier**: 1000 lines of code transformation/month

### 5. Security Scanning

**Features**:
- Outperforms leading tools on detection
- Suggests remediations
- Instant vulnerability fixes

**Scan types**:
- Injection vulnerabilities (SQL, XSS, Command)
- Authentication/authorization flaws
- Sensitive data exposure
- Insecure dependencies

**Usage**:
```
Right-click → Amazon Q → Security Scan
```

### 6. CLI Integration

**Install**:
```bash
npm install -g @aws/amazon-q-developer-cli
```

**Features**:
- Natural language to bash translation
- Command completions
- Contextual suggestions

**Example**:
```bash
q "list all running ec2 instances in us-west-2"
# Suggests: aws ec2 describe-instances --region us-west-2 --filters "Name=instance-state-name,Values=running"

q "find large log files older than 30 days"
# Suggests: find /var/log -name "*.log" -type f -size +100M -mtime +30
```

---

## Context Management System

Amazon Q's power comes from its context awareness. Optimize it:

### Project Directory Structure

```
your-project/
├── .amazonq/
│   ├── context/
│   │   ├── project-introduction.md
│   │   └── architecture-overview.md
│   ├── rules/
│   │   ├── coding-standards.rule.md
│   │   ├── security-checks.rule.md
│   │   └── aws-best-practices.rule.md
│   └── docs/
│       ├── api-reference.md
│       ├── database-schema.md
│       └── deployment-guide.md
├── ~/.aws/amazonq/prompts/
│   ├── code-review.md
│   ├── refactoring.md
│   ├── testing.md
│   └── documentation.md
```

### Context Files

**`.amazonq/context/project-introduction.md`**:
```markdown
# Project: Customer Order Service

## Overview
Microservice handling customer orders for e-commerce platform.

## Tech Stack
- **Language**: Python 3.11
- **Framework**: FastAPI
- **Database**: PostgreSQL 14 + Redis cache
- **AWS Services**: ECS, RDS, ElastiCache, SQS, S3
- **Testing**: pytest, moto (AWS mocking)

## Architecture
- REST API with async/await patterns
- Event-driven using SQS for order processing
- CQRS pattern: separate read/write models
- Repository pattern for data access

## Key Patterns
- Dependency injection via FastAPI dependencies
- Pydantic models for validation
- Structured logging with correlation IDs
- Circuit breaker for external APIs

## Important Files
- `app/api/routes/orders.py` - Order endpoints
- `app/services/order_service.py` - Business logic
- `app/repositories/order_repo.py` - Data access
- `app/models/order.py` - Pydantic models
```

**Benefits**:
- Every chat starts with this context
- No need to repeat project info
- Consistent suggestions aligned with architecture

### Context Hooks (CLI)

**Setup** (`~/.aws/amazonq/config.yml`):
```yaml
hooks:
  pre_conversation:
    - command: "git status --short"
      description: "Show current git changes"
    - command: "cat .amazonq/context/current-sprint.md"
      description: "Load current sprint context"
```

**Use case**: Auto-inject recent changes and current work context

---

## Prompt Library

Save frequently used prompts for reusability.

**Location**: `~/.aws/amazonq/prompts/`

### Example Prompts

**`code-review.md`**:
```markdown
# Code Review Checklist

Please review this code for:

## Functionality
- [ ] Correct implementation of requirements
- [ ] Edge cases handled
- [ ] Error handling comprehensive

## Code Quality
- [ ] Follows project coding standards
- [ ] DRY principle (no duplication)
- [ ] Clear naming conventions
- [ ] Appropriate abstractions

## Testing
- [ ] Unit tests cover main paths
- [ ] Edge cases tested
- [ ] Mocks used appropriately

## Security
- [ ] Input validation
- [ ] No SQL injection vulnerabilities
- [ ] No XSS vulnerabilities
- [ ] Secrets not hardcoded
- [ ] Proper authentication/authorization

## Performance
- [ ] No N+1 queries
- [ ] Appropriate indexing
- [ ] Caching considered
- [ ] Async operations where beneficial

## AWS Best Practices
- [ ] Least privilege IAM
- [ ] Proper resource tagging
- [ ] Cost-optimized resource selection
- [ ] Observability (logging, metrics, tracing)

Provide specific feedback with code examples.
```

**`refactoring.md`**:
```markdown
# Refactoring Request

Refactor this code to:

1. **Improve readability**
   - Extract complex logic into named functions
   - Reduce nesting depth
   - Use descriptive variable names

2. **Enhance maintainability**
   - Apply SOLID principles
   - Remove code duplication
   - Add type hints (Python) / types (TypeScript)

3. **Optimize performance**
   - Identify bottlenecks
   - Suggest caching opportunities
   - Reduce database queries

4. **Add error handling**
   - Validate inputs
   - Handle edge cases
   - Add logging

Show before/after with explanation of changes.
```

**`testing.md`**:
```markdown
# Test Generation

Generate comprehensive tests for this code:

## Unit Tests
- Happy path scenarios
- Edge cases
- Error conditions
- Boundary values

## Test Structure
- Arrange-Act-Assert pattern
- Descriptive test names
- Appropriate mocking
- Test fixtures where needed

## Coverage
- Aim for 80%+ coverage
- Focus on critical paths
- Include integration tests for complex workflows

Use project's testing framework: [pytest/jest/JUnit]
```

**`documentation.md`**:
```markdown
# Documentation Request

Generate documentation for this code:

## Function/Class Documentation
- Purpose and behavior
- Parameters with types
- Return values
- Exceptions raised
- Usage examples

## Code Comments
- Complex logic explanation
- Rationale for non-obvious decisions
- TODOs for future improvements

## README Updates
- New feature description
- Setup/configuration steps
- Example usage
- Breaking changes (if any)

Follow project documentation style: [Google/NumPy/JSDoc]
```

### Using Prompts

**In chat**:
```
/prompt code-review

[Amazon Q loads the prompt template and applies to current context]
```

**Via CLI**:
```bash
q prompt code-review --context "src/services/payment.py"
```

---

## Rules Engine

Define project-wide rules that automatically apply to all AI interactions.

### Rules File Format

**`.amazonq/rules/coding-standards.rule.md`**:
```markdown
---
name: Coding Standards
description: Enforce project coding conventions
priority: high
---

# Coding Standards

## Python Code Style
- Use Black formatter (line length: 100)
- Type hints required for all function signatures
- Docstrings required for all public functions (Google style)
- No wildcard imports
- Use f-strings for string formatting (not % or .format())

## Error Handling
- Use specific exception types, not bare `except:`
- Always log exceptions with context
- Raise custom exceptions for domain errors

## Naming Conventions
- Classes: PascalCase
- Functions/variables: snake_case
- Constants: UPPER_SNAKE_CASE
- Private members: _leading_underscore

## File Organization
```
module/
├── __init__.py
├── models.py       # Data models
├── services.py     # Business logic
├── repositories.py # Data access
├── schemas.py      # Pydantic/validation
└── tests/
    └── test_services.py
```

## Import Order
1. Standard library
2. Third-party packages
3. Local application imports

Separated by blank lines, sorted alphabetically within each group.
```

**`.amazonq/rules/security-checks.rule.md`**:
```markdown
---
name: Security Checks
description: Enforce security best practices
priority: critical
---

# Security Requirements

## Input Validation
- Validate all user inputs
- Use Pydantic models for API request validation
- Sanitize data before database queries
- Validate file uploads (type, size, content)

## Authentication & Authorization
- Never store passwords in plain text
- Use bcrypt/argon2 for password hashing
- Implement proper session management
- Check authorization for all sensitive operations
- Use AWS IAM roles, not hardcoded credentials

## Data Protection
- Encrypt sensitive data at rest
- Use TLS for data in transit
- Never log sensitive information (passwords, tokens, PII)
- Use AWS Secrets Manager for secrets
- Implement data retention policies

## AWS Security
- Least privilege IAM policies
- Enable VPC Flow Logs
- Use Security Groups restrictively
- Enable CloudTrail for audit logs
- Encrypt S3 buckets and RDS databases

## SQL Injection Prevention
- Use parameterized queries (SQLAlchemy)
- Never concatenate user input into SQL
- Use ORM safely (avoid raw SQL strings)

## XSS Prevention
- Escape output in templates
- Set Content-Security-Policy headers
- Validate and sanitize rich text input
```

**`.amazonq/rules/aws-best-practices.rule.md`**:
```markdown
---
name: AWS Best Practices
description: AWS architecture and implementation guidelines
priority: high
---

# AWS Best Practices

## Resource Tagging
All AWS resources must include these tags:
```
Environment: [dev|staging|prod]
Service: [service-name]
Team: [team-name]
CostCenter: [cost-center-id]
ManagedBy: terraform
```

## High Availability
- Multi-AZ deployment for production
- Auto-scaling groups for EC2/ECS
- RDS Multi-AZ with automated backups
- S3 cross-region replication for critical data

## Cost Optimization
- Use appropriate instance types (consider Graviton)
- Implement auto-scaling
- Use S3 lifecycle policies
- Right-size RDS instances
- Use Reserved Instances for predictable workloads
- Delete unused resources (EBS snapshots, old AMIs)

## Observability
- CloudWatch Logs for all services
- Custom CloudWatch Metrics for business KPIs
- X-Ray tracing for distributed systems
- CloudWatch Alarms for critical metrics
- Structured logging with correlation IDs

## Infrastructure as Code
- Use Terraform/CloudFormation
- Store state in S3 with locking (DynamoDB)
- Version control all infrastructure code
- Use modules/nested stacks for reusability
- Apply changes via CI/CD, not console

## Networking
- Use VPC with private subnets for services
- NAT Gateway for outbound internet access
- VPC Endpoints for AWS services
- Network ACLs and Security Groups
- Use AWS PrivateLink for service-to-service

## Data Backup
- Automated RDS backups (7-day retention minimum)
- EBS snapshot lifecycle management
- S3 versioning for critical buckets
- Test disaster recovery procedures

## Performance
- Use CloudFront for static content
- ElastiCache for session storage and caching
- RDS Read Replicas for read-heavy workloads
- SQS for asynchronous processing
- Use appropriate RDS instance storage (Provisioned IOPS for high-performance)
```

### How Rules Work

1. **Automatic Application**: Rules in `.amazonq/rules/` are automatically loaded
2. **Priority Levels**: `critical` > `high` > `medium` > `low`
3. **Team-Wide**: Rules apply to all team members using the repository
4. **Enforcement**: Amazon Q checks suggestions against rules before presenting

### Testing Rules

```bash
# In Amazon Q chat:
"Generate a function to hash user passwords"

# Amazon Q will automatically:
# 1. Check security-checks.rule.md
# 2. Use bcrypt/argon2 (not plain text or weak hashing)
# 3. Include salt generation
# 4. Add proper error handling
```

---

## Advanced Techniques

### 1. Multi-File Refactoring

**Scenario**: Rename service across 20+ files

**Approach**:
```
In Amazon Q chat:
"Refactor UserService to UserManagementService across the entire codebase.
Update:
- Class name
- File name
- All imports
- All instantiations
- Test files
- Configuration files

Provide a summary of changes before applying."
```

**Amazon Q will**:
1. Analyze all files mentioning UserService
2. Generate plan showing affected files
3. Create diffs for each change
4. Execute after approval
5. Validate with build

### 2. Feature Development Workflow

**Step 1**: Create context for the feature
```markdown
# .amazonq/context/feature-payment-integration.md

Feature: Stripe Payment Integration

## Requirements
- Process credit card payments
- Handle webhooks for payment events
- Store payment metadata
- Retry failed payments

## Acceptance Criteria
- PCI compliance (use Stripe.js, don't store card data)
- Idempotent payment processing
- Comprehensive error handling
- Integration tests with Stripe test mode
```

**Step 2**: Generate implementation plan
```
"Based on feature-payment-integration.md context, create implementation plan:
1. Data models
2. API endpoints
3. Service layer
4. Stripe SDK integration
5. Webhook handler
6. Tests
7. Documentation"
```

**Step 3**: Implement incrementally
```
"Implement step 1: Create payment data models following project structure"
[Review and approve]

"Implement step 2: Create API endpoints with validation"
[Review and approve]
...
```

### 3. Test-Driven Development

**Approach**:
```
"Generate failing tests for a function that validates credit card numbers using Luhn algorithm.

Then implement the function to make tests pass."
```

**Amazon Q generates**:
1. Comprehensive test cases first
2. Implementation that satisfies tests
3. Edge cases covered

### 4. Code Review Automation

**Pre-commit hook** (`.git/hooks/pre-commit`):
```bash
#!/bin/bash

# Get changed files
CHANGED_FILES=$(git diff --cached --name-only --diff-filter=ACM | grep '\.py$')

if [ -n "$CHANGED_FILES" ]; then
    echo "Running Amazon Q code review..."
    for FILE in $CHANGED_FILES; do
        q chat --prompt code-review --context "$FILE" > "/tmp/review-$FILE.txt"
    done

    echo "Reviews saved to /tmp/review-*.txt"
    echo "Review feedback before committing."
fi
```

### 5. Documentation Generation

**API documentation**:
```
"Generate OpenAPI 3.0 specification for all endpoints in app/api/routes/orders.py

Include:
- Request/response schemas
- Error responses
- Authentication requirements
- Example requests/responses"
```

**Architecture diagrams**:
```
"Generate Mermaid diagram showing:
1. Service architecture
2. AWS resources and connections
3. Data flow for order processing
4. External API integrations"
```

---

## AWS Integration

### Console Integration

**Access**: AWS Console → any service → Amazon Q icon

**Capabilities**:
- Cost optimization recommendations
- Architecture best practices
- Troubleshooting operational issues
- Network diagnostics
- Security posture analysis

**Example queries**:
```
"Why is my Lambda function timing out?"
"How can I reduce my RDS costs?"
"Diagnose why my EC2 instance can't reach the internet"
"What's the best way to implement VPC peering between these accounts?"
```

### Infrastructure as Code

**Generate Terraform**:
```
"Create Terraform configuration for:
- VPC with public and private subnets across 3 AZs
- ECS Fargate cluster
- Application Load Balancer
- RDS PostgreSQL Multi-AZ
- ElastiCache Redis cluster
- S3 bucket with versioning and encryption
- CloudWatch Log Groups
- IAM roles following least privilege

Include:
- Variables file
- Outputs
- Backend configuration (S3 + DynamoDB)"
```

**Generate CloudFormation**:
```
"Convert this Terraform to CloudFormation with best practices"
```

### AWS SDK Code Generation

**Python Boto3 example**:
```
"Generate a function to:
1. Upload file to S3
2. Set appropriate Content-Type
3. Enable server-side encryption
4. Add metadata tags
5. Generate pre-signed URL (1 hour expiry)
6. Handle errors (bucket doesn't exist, permission denied, etc.)
7. Add logging
8. Include type hints and docstring"
```

**Amazon Q generates**:
```python
import boto3
import logging
from botocore.exceptions import ClientError
from typing import Optional

logger = logging.getLogger(__name__)

def upload_file_to_s3(
    file_path: str,
    bucket_name: str,
    object_key: str,
    content_type: str,
    metadata: dict[str, str]
) -> Optional[str]:
    """
    Upload a file to S3 with encryption and generate a pre-signed URL.

    Args:
        file_path: Local path to the file to upload
        bucket_name: Name of the S3 bucket
        object_key: S3 object key (path in bucket)
        content_type: MIME type of the file
        metadata: Dictionary of metadata tags

    Returns:
        Pre-signed URL (1 hour expiry) if successful, None otherwise

    Raises:
        ClientError: If AWS API call fails
    """
    s3_client = boto3.client('s3')

    try:
        # Upload file with encryption and metadata
        s3_client.upload_file(
            file_path,
            bucket_name,
            object_key,
            ExtraArgs={
                'ContentType': content_type,
                'ServerSideEncryption': 'AES256',
                'Metadata': metadata
            }
        )

        logger.info(f"Successfully uploaded {file_path} to s3://{bucket_name}/{object_key}")

        # Generate pre-signed URL (1 hour expiry)
        presigned_url = s3_client.generate_presigned_url(
            'get_object',
            Params={
                'Bucket': bucket_name,
                'Key': object_key
            },
            ExpiresIn=3600
        )

        return presigned_url

    except ClientError as e:
        error_code = e.response['Error']['Code']

        if error_code == 'NoSuchBucket':
            logger.error(f"Bucket {bucket_name} does not exist")
        elif error_code == 'AccessDenied':
            logger.error(f"Permission denied to upload to s3://{bucket_name}/{object_key}")
        else:
            logger.error(f"Failed to upload file: {e}")

        raise
    except FileNotFoundError:
        logger.error(f"File not found: {file_path}")
        raise
```

---

## Customization for Teams

Enterprise feature for consistent team-wide suggestions.

### Setup Customization

**Requirements**:
- Amazon Q Business tier ($25/user/month)
- S3 bucket with codebase samples
- Administrator access

**Supported languages** (as of April 2025):
- Python, Java, JavaScript, TypeScript, C#, C++
- Dart, Go, Kotlin, PHP, Ruby, Rust, Scala
- Bash, PowerShell, CloudFormation, Terraform

### Process

1. **Collect code samples**: Representative examples of your team's code
2. **Upload to S3**: Organized by project/module
3. **Configure customization**: AWS Console → Amazon Q → Customizations
4. **Train model**: Takes 1-4 hours
5. **Deploy**: Makes available to team members

**Benefits**:
- Suggestions aligned with internal APIs
- Follows team coding patterns
- Uses company-specific libraries
- Reduces onboarding time for new engineers

---

## Productivity Metrics

Track your improvement:

### Built-in Metrics

Amazon Q tracks:
- Acceptance rate (% of suggestions accepted)
- Characters saved (suggested vs typed)
- Time saved estimates
- Most productive times/days

**Access**: IDE → Amazon Q → Metrics Dashboard

### Custom Tracking

Create `.amazonq/metrics.md` to track:
```markdown
# My Amazon Q Productivity

## Week of 2025-11-09

### Features Developed
- [x] Payment integration (3 hours saved with Agent mode)
- [x] User authentication refactor (2 hours saved with multi-file edit)

### Code Reviews
- 5 reviews accelerated with code-review prompt

### Bugs Fixed
- 8 bugs (security scan found 3 vulnerabilities)

### Learning
- AWS Cost optimization patterns from Q console integration
- Lambda best practices from Q suggestions

### Acceptance Rate: 52%
### Estimated Time Saved: 8 hours
```

---

## Troubleshooting

### Common Issues

**Suggestions seem generic**:
- Add project context files
- Use more specific comments
- Include type hints/interfaces
- Load relevant code with `@` mentions

**Wrong AWS region suggested**:
- Set default region in AWS config
- Specify region in prompts
- Add AWS_REGION to context file

**Slow responses**:
- Check network connection
- Reduce context size (remove unnecessary files)
- Use specific prompts instead of vague requests

**Rules not applying**:
- Verify `.amazonq/rules/` directory structure
- Check YAML frontmatter syntax
- Ensure priority is set
- Restart IDE after adding rules

---

## Next Steps

1. ✅ Install Amazon Q in your IDE
2. ✅ Create `.amazonq/context/project-introduction.md` for your project
3. ✅ Set up prompt library with templates from this guide
4. ✅ Define team rules in `.amazonq/rules/`
5. ✅ Try Developer Agent for a feature
6. ✅ Integrate CLI for terminal productivity
7. ✅ Explore AWS Console integration

**Continue to**: [Kiro Spec-Driven Development](03-kiro-spec-driven.md)
