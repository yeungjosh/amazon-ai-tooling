# Amazon Q CLI: Complete Guide & Comparison

Everything you need to know about Amazon Q Developer's command-line interface and how it fits into your AI coding workflow.

## Table of Contents
1. [Overview](#overview)
2. [What is Amazon Q CLI?](#what-is-amazon-q-cli)
3. [Key Features](#key-features)
4. [Commands Reference](#commands-reference)
5. [Q CLI vs IDE Plugin](#q-cli-vs-ide-plugin)
6. [Q CLI vs Cline](#q-cli-vs-cline)
7. [When to Use Q CLI](#when-to-use-q-cli)
8. [Setup & Configuration](#setup--configuration)
9. [Real-World Workflows](#real-world-workflows)
10. [Limitations & Issues](#limitations--issues)
11. [Integration Strategies](#integration-strategies)

---

## Overview

Amazon Q CLI is a **command-line interface** for Amazon Q Developer, separate from the IDE plugin. It brings AI assistance directly to your terminal with agentic capabilities powered by Claude 3.7 Sonnet.

**Released**: Originally as CodeWhisperer CLI, enhanced significantly in March 2025

**Key Innovation**: Agentic terminal experience that can read/write files, execute commands, and query AWS resources.

---

## What is Amazon Q CLI?

### Two Main Modes

**1. Translation Mode (`q translate`)**
- Converts natural language → bash commands
- Quick syntax lookup
- One-shot command generation

**2. Chat Mode (`q chat`)**
- Interactive AI conversations in terminal
- Multi-turn discussions with context
- Agentic capabilities (file editing, command execution)
- Powered by Claude 3.7 Sonnet (March 2025+)

### Architecture

```
Your Terminal
    ↓
Amazon Q CLI (local client)
    ↓
AWS Bedrock (Claude 3.7 Sonnet)
    ↓
Your Files / AWS Resources / System Tools
```

**Important**: Q CLI and Q IDE plugin are **completely separate** systems. They don't share conversation history or context.

---

## Key Features

### 🚀 Enhanced Agentic Agent (March 2025)

**Capabilities**:
- **Read/write files locally** - Can create, edit, delete files
- **Execute system commands** - Compilers, package managers, AWS CLI
- **Query AWS resources** - EC2, S3, Lambda, etc.
- **Multi-turn conversations** - Context-aware discussions
- **Step-by-step reasoning** - Claude 3.7 Sonnet's CoT

**Example**:
```bash
q chat
> "Create a FastAPI app with a health check endpoint, add tests, run them"

Q will:
1. Create app.py with FastAPI code
2. Create test_app.py with pytest tests
3. Install dependencies (pip install fastapi pytest)
4. Run tests
5. Report results
```

### 💬 Conversation Management

**Persistent Conversations**:
```bash
q chat --resume          # Continue previous conversation in this directory
q chat --save my-session # Save current conversation
q chat --load my-session # Load saved conversation
```

**In-Session Commands**:
- `/save` - Save current conversation
- `/load` - Load saved conversation
- `/clear` - Clear conversation history
- `/compact` - Summarize history (avoid token limits)
- `/usage` - Show context window usage
- `/quit` - Exit session

**Git-Aware File Selection**:
- Fuzzy finder respects .gitignore
- Easier to include relevant files from large repos

### 🔧 Tool Access

Q CLI can use tools installed on your system:
- **Compilers**: gcc, javac, rustc
- **Package managers**: npm, pip, cargo
- **AWS CLI**: aws s3, aws ec2, aws lambda
- **Git**: All git commands
- **Build tools**: make, gradle, maven

### 🔌 Model Context Protocol (MCP)

**Background MCP server loading** (improved startup time):
```bash
/tools              # Manage tool permissions
/tools allow s3     # Allow S3 MCP tool
/tools deny lambda  # Deny Lambda MCP tool
```

**Integrate custom MCP servers** for internal tools.

### 📝 Natural Language Command Translation

```bash
q translate "list all files modified in the last week"
# Output: find . -type f -mtime -7

q translate "compress all log files older than 30 days"
# Output: find . -name "*.log" -mtime +30 -exec gzip {} \;

q translate "kill all Python processes"
# Output: pkill -9 python
```

**Use cases**:
- Forgot command syntax
- Complex find/grep/awk commands
- AWS CLI commands with many flags

### 🔄 Context Hooks

**Auto-include context based on conversation**:
```bash
# .q/hooks.yml
context_hooks:
  - type: git_status
    when: discussing_changes
  - type: package_json
    when: discussing_dependencies
  - type: aws_resources
    when: discussing_deployment
```

---

## Commands Reference

### Core Commands

| Command | Purpose | Example |
|---------|---------|---------|
| `q chat` | Interactive AI conversation | `q chat` |
| `q chat --resume` | Resume previous conversation | `q chat --resume` |
| `q translate` | Natural language → bash | `q translate "find large files"` |
| `q generate` | Generate/refactor code | `q generate "API endpoint for users"` |
| `q explain` | Explain existing code | `q explain src/auth.py` |
| `q test` | Write/improve tests | `q test src/payment.py` |
| `q run` | Execute code | `q run my_script.py` |
| `q fix` | Debug/resolve errors | `q fix "TypeError in line 42"` |
| `q ask` | General Q&A | `q ask "how to use boto3 for S3?"` |

### AWS-Specific Commands

```bash
# AWS CLI translation
q translate "list all EC2 instances in us-east-1"
# aws ec2 describe-instances --region us-east-1

# IAM policy help
q ask "write IAM policy for read-only S3 access to my-bucket"

# CloudFormation assistance
q generate "CloudFormation template for VPC with public/private subnets"
```

### In-Chat Commands

While in `q chat`:

| Command | Purpose |
|---------|---------|
| `/save [name]` | Save conversation |
| `/load [name]` | Load conversation |
| `/clear` | Clear history |
| `/compact` | Summarize history |
| `/usage` | Show token usage |
| `/tools` | Manage MCP tools |
| `/quit` | Exit chat |

### Context References

```bash
# In q chat:
@history        # Reference conversation history
@file:path.py   # Include specific file
@git:diff       # Include git diff
```

---

## Q CLI vs IDE Plugin

### Amazon Q CLI

**Architecture**: Terminal-based AI agent

**Pros** ✅:
- **No IDE required** - Works in any terminal
- **Agentic capabilities** - Autonomous file/command execution
- **Parallel sessions** - Multiple terminals simultaneously
- **Remote-friendly** - SSH, containers, cloud shells
- **Scriptable** - Can be automated in bash scripts
- **CI/CD integration** - Use in pipelines
- **Lightweight** - No IDE overhead
- **System tool access** - Uses compilers, package managers

**Cons** ❌:
- **No IDE integration** - Can't see warnings, errors, symbols
- **No inline suggestions** - No autocomplete while typing
- **Separate from IDE plugin** - No shared context
- **Terminal-only** - Less visual, more text-based

**Best For**:
- DevOps/infrastructure work
- Terminal-native workflows
- Scripting and automation
- Remote development
- CI/CD pipelines
- Quick command generation

### Amazon Q IDE Plugin

**Architecture**: IDE extension (VS Code, JetBrains, etc.)

**Pros** ✅:
- **Deep IDE integration** - Warnings, errors, symbols
- **Inline suggestions** - Autocomplete as you type
- **Visual feedback** - See diffs, highlights, annotations
- **Workspace awareness** - Understands project structure
- **Security scanning** - Real-time vulnerability detection
- **Refactoring tools** - Multi-file changes with IDE support

**Cons** ❌:
- **Requires IDE** - Must have IDE open
- **Limited autonomy** - Less agentic than CLI
- **One session per IDE** - Can't run parallel easily
- **Heavyweight** - IDE resource usage

**Best For**:
- Daily coding in IDE
- Feature development
- Code review and refactoring
- Security scanning
- AWS service integration (console)

### Side-by-Side Comparison

| Feature | Q CLI | Q IDE Plugin |
|---------|-------|--------------|
| **Inline Suggestions** | ❌ | ✅ 50% acceptance rate |
| **Chat** | ✅ Claude 3.7 | ✅ Multi-model |
| **File Editing** | ✅ Agentic | ✅ With approval |
| **Command Execution** | ✅ Direct | ❌ Manual |
| **AWS Integration** | ✅ via CLI | ✅ via Console |
| **Security Scanning** | ❌ | ✅ Real-time |
| **Multi-file Refactoring** | ✅ | ✅ Agent mode |
| **Parallel Sessions** | ✅ Easy | ❌ Difficult |
| **CI/CD Integration** | ✅ Native | ❌ Not practical |
| **Requires IDE** | ❌ | ✅ |
| **Context Sharing** | ❌ Separate | ❌ Separate |

**Key Insight**: They're complementary tools, not alternatives. Use both!

---

## Q CLI vs Cline

### Amazon Q CLI

**Provider**: Amazon (AWS)
**Model**: Claude 3.7 Sonnet (via Bedrock)
**Cost**: $19/month (or AWS Bedrock pricing)
**Platform**: Terminal

**Strengths** ✅:
- **AWS native** - Deep AWS service integration
- **Bedrock routing** - Enterprise security/compliance
- **Command translation** - Natural language → bash
- **Session persistence** - Resume conversations by directory
- **Git-aware** - Smart file selection
- **Conversation management** - Save/load/compact

**Limitations** ❌:
- **AWS-centric** - Less flexible for non-AWS work
- **Bedrock latency** - Slower than direct API
- **Limited model choice** - Claude 3.7 only
- **Less autonomous** - More guided than Cline
- **No MCP marketplace** - Manual MCP server setup

**Best For**:
- AWS-heavy development
- Enterprise environments (compliance)
- AWS CLI command generation
- IAM policy creation
- CloudFormation/Terraform assistance

### Cline

**Provider**: Open source community
**Model**: Any (Claude, GPT-4, Gemini, DeepSeek, local)
**Cost**: Free + your API costs (~$3-5/day)
**Platform**: VS Code extension (also terminal-like)

**Strengths** ✅:
- **Highly autonomous** - Executes, tests, iterates independently
- **Multi-model** - Choose best model for task
- **MCP marketplace** - One-click AWS integration (35+ servers)
- **Faster** - Direct API calls (no Bedrock routing)
- **Cost-effective** - Use cheap models (DeepSeek $0.27/M tokens)
- **Open source** - Community-driven, transparent
- **Flexible** - Works with any cloud/service

**Limitations** ❌:
- **Setup complexity** - API keys, MCP configuration
- **Less polished** - Community tool vs AWS product
- **No enterprise support** - DIY troubleshooting
- **IDE-bound** - Requires VS Code open

**Best For**:
- General development (any stack)
- Cost-conscious teams
- Multi-cloud environments
- Autonomous coding workflows
- Power users wanting customization

### Head-to-Head

| Feature | Amazon Q CLI | Cline |
|---------|--------------|-------|
| **Autonomy** | Moderate | High |
| **AWS Integration** | Native (CLI) | MCP Servers |
| **Model Choice** | Claude 3.7 only | 10+ models |
| **Speed** | Slower (Bedrock) | Faster (direct) |
| **Cost** | $19/month | $3-5/day (API) |
| **Enterprise** | ✅ Compliant | ❌ DIY |
| **MCP Support** | Manual | Marketplace |
| **Multi-cloud** | AWS-focused | Agnostic |
| **Open Source** | ❌ | ✅ |
| **Session Mgmt** | ✅ Excellent | ⚠️ Basic |
| **Command Translation** | ✅ `q translate` | ❌ |

### Which to Choose?

**Choose Amazon Q CLI if**:
- Working primarily on AWS
- Need enterprise compliance
- Want integrated AWS CLI translation
- Company pays for AWS services
- Prefer supported product over community tool

**Choose Cline if**:
- Multi-cloud or non-AWS development
- Want maximum autonomy
- Need flexibility (model choice, customization)
- Budget-conscious (pay for usage only)
- Comfortable with open source tools

**Use Both** (Recommended):
```
AWS tasks: Q CLI
  - "Create S3 bucket with versioning"
  - "Generate IAM policy"
  - "Deploy CloudFormation stack"

General coding: Cline
  - "Implement user authentication"
  - "Refactor database layer"
  - "Write comprehensive tests"
```

---

## When to Use Q CLI

### ✅ Excellent Use Cases

**1. AWS Command Generation**
```bash
q translate "copy all files from s3://my-bucket to local, only modified today"
q translate "create EC2 instance with Ubuntu 22.04 in us-west-2"
q ask "write IAM policy for Lambda to read DynamoDB"
```

**2. Terminal-Native Workflows**
```bash
# In SSH session
q chat
> "Analyze nginx logs for 5xx errors in last hour"
> "Show top 10 IPs by request count"
> "Generate iptables rule to block them"
```

**3. Complex Bash Commands**
```bash
q translate "find all Python files, exclude venv, search for TODO, show with line numbers"
# Output: find . -name "*.py" -not -path "*/venv/*" -exec grep -n "TODO" {} +

q translate "archive all logs older than 30 days, compress, move to s3://backups"
```

**4. CI/CD Pipelines**
```yaml
# .github/workflows/deploy.yml
- name: Generate deployment script
  run: |
    q generate "deployment script for ECS blue-green" > deploy.sh
    bash deploy.sh
```

**5. Infrastructure Troubleshooting**
```bash
q chat
> "Why is my Lambda timing out? Here's the code: $(cat lambda.py)"
> [Q analyzes and suggests fixes]
> "Implement the fix"
> [Q updates lambda.py]
```

**6. Documentation & Learning**
```bash
q ask "explain boto3 pagination with examples"
q ask "best practices for DynamoDB partition keys"
q explain "this Terraform module" @file:main.tf
```

**7. Quick Scripting**
```bash
q generate "Python script to backup DynamoDB table to S3"
q generate "Bash script to rotate CloudWatch logs"
```

**8. Remote Development**
```bash
# In EC2 instance via SSH
q chat --resume
> "Continue debugging the API performance issue"
```

### ⚠️ Moderate Use Cases

**Code Development** - Better in IDE with Cline/Q Plugin
- Q CLI can do it, but IDE gives better experience
- No inline suggestions, no visual diffs
- Use for quick scripts, not full applications

**Multi-file Refactoring** - Better in IDE
- Q CLI can edit multiple files
- But harder to review changes without IDE
- No refactoring tools (rename symbol, extract method)

**Testing** - Better in IDE
- Q CLI can write tests
- But can't run with IDE test runner
- No visual test results

### ❌ Poor Use Cases

**Daily Feature Development**
- Use IDE plugin instead
- Better autocomplete, better UX

**Security Scanning**
- Q CLI doesn't have security scanner
- Use IDE plugin for vulnerability detection

**Visual Debugging**
- No breakpoints, no watches
- Use IDE debugger

**Large Multi-file Changes**
- Reviewing diffs in terminal is painful
- Use Cline or Q IDE plugin

---

## Setup & Configuration

### Installation

**macOS/Linux**:
```bash
# Via npm (recommended)
npm install -g @aws/amazon-q-developer-cli

# Via Homebrew
brew install amazon-q

# Verify installation
q --version
```

**Windows** (WSL required):
```bash
# In WSL terminal
npm install -g @aws/amazon-q-developer-cli
```

### Authentication

**Option 1: AWS Builder ID** (Recommended for individuals)
```bash
q configure
# Follow prompts to authenticate with AWS Builder ID
```

**Option 2: IAM Identity Center** (For organizations)
```bash
q configure --sso
# Enter your organization's SSO portal URL
```

### Configuration Files

**Location**: `~/.q/`

**Main config**: `~/.q/config.json`
```json
{
  "auth": {
    "method": "builder-id",
    "region": "us-east-1"
  },
  "defaults": {
    "model": "claude-3-7-sonnet",
    "max_tokens": 4096
  },
  "features": {
    "mcp_enabled": true,
    "git_aware": true,
    "auto_save_sessions": true
  }
}
```

**MCP servers**: `~/.q/mcp_settings.json`
```json
{
  "mcpServers": {
    "aws-s3": {
      "command": "npx",
      "args": ["-y", "@aws/mcp-server-s3"]
    },
    "aws-ec2": {
      "command": "npx",
      "args": ["-y", "@aws/mcp-server-ec2"]
    }
  }
}
```

### Project-Level Configuration

**`.q/hooks.yml`** (in project root):
```yaml
context_hooks:
  - type: git_status
    when: always
  - type: package_manager
    when: discussing_dependencies
  - type: aws_profile
    when: discussing_deployment

auto_include:
  - .env.example
  - README.md
  - architecture.md
```

---

## Real-World Workflows

### Workflow 1: AWS Resource Management

**Scenario**: Create S3 bucket with proper configuration

```bash
q chat
> "Create S3 bucket 'my-app-data-prod' with:
- Versioning enabled
- Encryption with KMS
- Lifecycle policy: IA after 30 days, Glacier after 90 days
- Block public access
- Tags: Environment=prod, Team=platform"

Q generates and executes:
aws s3api create-bucket --bucket my-app-data-prod --region us-east-1
aws s3api put-bucket-versioning --bucket my-app-data-prod --versioning-configuration Status=Enabled
aws s3api put-bucket-encryption --bucket my-app-data-prod --server-side-encryption-configuration ...
[etc.]

> "Now create lifecycle policy JSON"
> [Q creates lifecycle.json]
> "Apply it"
> [Q executes aws s3api put-bucket-lifecycle-configuration]
```

### Workflow 2: Debugging Production Issues

**Scenario**: Lambda function timing out

```bash
q chat --resume  # Continue from last session
> "My Lambda is timing out. Here's the error from CloudWatch:"
> [Paste error]
> "And here's the code:" @file:lambda.py

Q analyzes:
"I see three issues:
1. Synchronous S3 calls in a loop (N+1 problem)
2. No connection pooling for database
3. 30s timeout but processing can take 45s"

> "Fix issue 1 and 2"

Q edits lambda.py:
- Changes to async S3 batch operations
- Adds connection pooling

> "Run local tests"

Q executes:
pytest test_lambda.py -v

> "All pass! Deploy to staging"

Q generates deployment script and executes.
```

### Workflow 3: Infrastructure as Code

**Scenario**: Generate Terraform for microservice

```bash
q generate "Terraform module for:
- ECS Fargate service (2-10 tasks, auto-scaling)
- Application Load Balancer
- RDS PostgreSQL Multi-AZ
- ElastiCache Redis
- S3 bucket for uploads
- CloudWatch dashboards and alarms
- All with proper security (VPC, security groups, IAM)"

Q creates:
main.tf
variables.tf
outputs.tf
ecs.tf
rds.tf
elasticache.tf
s3.tf
monitoring.tf

> "Add comments explaining each resource"
> [Q adds detailed comments]

> "Generate terraform.tfvars for dev environment"
> [Q creates tfvars file]

> "Run terraform plan"
> [Q executes: terraform init && terraform plan]
```

### Workflow 4: Command-Line Productivity

**Daily tasks**:

```bash
# Morning: Check what's changed
q translate "show git diff for files modified yesterday"
# git log --since="yesterday" --name-only --pretty=format: | sort -u | xargs git diff HEAD~1

# Deploy
q translate "deploy to ECS service my-app-prod, wait for stable"
# aws ecs update-service --cluster my-cluster --service my-app-prod --force-new-deployment && aws ecs wait services-stable --cluster my-cluster --services my-app-prod

# Check logs
q translate "tail CloudWatch logs for /aws/lambda/my-function, last 10 minutes"
# aws logs tail /aws/lambda/my-function --since 10m --follow

# Database backup
q translate "create RDS snapshot of my-db with timestamp"
# aws rds create-db-snapshot --db-instance-identifier my-db --db-snapshot-identifier my-db-$(date +%Y%m%d-%H%M%S)
```

### Workflow 5: Learning & Documentation

**Learning new AWS services**:

```bash
q ask "explain Amazon EventBridge with code examples"
q ask "EventBridge vs SNS vs SQS - when to use which?"
q ask "show me a complete EventBridge rule for S3 to Lambda"
q generate "EventBridge rule that triggers Lambda on S3 upload to 'invoices/'"
```

---

## Limitations & Issues

### Known Limitations

**1. Separate from IDE Plugin**
- ❌ No shared context between Q CLI and Q IDE
- ❌ Can't reference IDE workspace from CLI
- ❌ Conversations don't sync

**2. No Inline Autocomplete**
- ❌ Can't suggest as you type in files
- ❌ No IDE-style refactoring tools
- Use Q IDE plugin for this

**3. Visual Limitations**
- ❌ No visual diffs (terminal only)
- ❌ Harder to review multi-file changes
- ❌ No syntax highlighting in output

**4. Resource Constraints (Sandbox)**
- ⚠️ 2 vCPUs, 4GB RAM limit
- ⚠️ Commands timeout after 5 minutes
- ⚠️ Not suitable for heavy compilation/builds

**5. Model Selection**
- ❌ Can't choose specific model (auto-routed)
- ❌ Stuck with Claude 3.7 Sonnet
- Cline offers more model flexibility

**6. Workspace Indexing Required**
- ⚠️ Must explicitly index workspace
- ⚠️ No automatic project understanding
- Q IDE plugin does this automatically

### Recent Security Issues (Patched)

**July 2025**: Prompt injection vulnerabilities
- Could execute commands without human confirmation
- Required malicious file access
- **Fixed**: July 17 & 29, 2025
- **Mitigation**: Updated Language Server requires HITL confirmation

**Recommendation**: Keep Q CLI updated to latest version.

### Cost & Limits

**Free Tier**:
- 50 chat interactions/month
- 1000 lines code transformation/month

**Pro ($19/month)**:
- Unlimited chat
- 4000 lines transformation/month
- $0.003 per additional line

**Bedrock Overage**:
- Additional costs if exceeding included usage
- Monitor via AWS Cost Explorer

---

## Integration Strategies

### Strategy 1: Q CLI + Cline Combo

**Use Q CLI for**:
- AWS command generation: `q translate`
- IAM policy creation: `q ask`
- CloudFormation templates: `q generate`

**Use Cline for**:
- Application code development
- Multi-file refactoring
- Test generation
- General coding tasks

**Workflow**:
```bash
# Terminal 1: Q CLI for AWS
q chat
> "Generate Terraform for ECS infrastructure"

# VS Code with Cline: Application code
Cline: "Implement order processing service with FastAPI"

# Terminal 1: Deploy with Q CLI
> "Deploy to staging ECS cluster"
```

### Strategy 2: Q CLI + Q IDE Plugin

**Use Q CLI for**:
- Terminal work (SSH, containers)
- Automation scripts
- CI/CD integration

**Use Q IDE Plugin for**:
- Daily feature development
- Inline autocomplete
- Security scanning
- Multi-file agent mode

**Workflow**:
```
Morning (IDE):
- Code with Q inline suggestions
- Agent mode for complex features

Afternoon (CLI):
- Deploy: q chat "deploy to production"
- Monitor: q translate "check CloudWatch alarms"
- Troubleshoot: q chat --resume
```

### Strategy 3: Full Stack (All Tools)

**Modern AI Coding Stack**:
1. **Cline** - Primary coding assistant (VS Code)
2. **Amazon Q IDE** - AWS tasks, security scanning
3. **Amazon Q CLI** - Terminal work, deployments
4. **GitHub Copilot** - Quick inline autocomplete
5. **Kiro** - Complex feature planning (spec-driven)

**When to use each**:
```
Feature Planning: Kiro (requirements → design → tasks)
Implementation: Cline (autonomous, multi-file)
AWS Tasks: Q IDE (inline) or Q CLI (terminal)
Quick Autocomplete: Copilot (fastest inline)
Deployment: Q CLI (terminal automation)
```

---

## Summary

### Amazon Q CLI at a Glance

**What it is**: Terminal-based AI assistant powered by Claude 3.7 Sonnet

**Key Strengths**:
- ✅ AWS-native integration
- ✅ Command translation (`q translate`)
- ✅ Agentic terminal experience
- ✅ Session management (save/resume)
- ✅ CI/CD integration
- ✅ Remote-friendly

**Key Weaknesses**:
- ❌ No inline IDE autocomplete
- ❌ Separate from Q IDE plugin
- ❌ Visual limitations (terminal-only)
- ❌ Less autonomous than Cline

**Best Use Cases**:
1. AWS CLI command generation
2. IAM policy/CloudFormation creation
3. Terminal-native workflows
4. SSH/remote development
5. CI/CD automation
6. Quick bash scripting

**Avoid For**:
1. Daily feature development (use IDE)
2. Multi-file refactoring (use Cline/Q IDE)
3. Security scanning (use Q IDE)

### Recommendations for Amazon Engineers

**Essential Setup**:
1. Install Q CLI for terminal work
2. Install Q IDE plugin for daily coding
3. Install Cline for advanced workflows

**Strategic Usage**:
```
80% of coding: Cline or Q IDE plugin (in editor)
15% of work: Q CLI (terminal, AWS, deployments)
5% of work: Kiro (complex feature planning)
```

**Cost-Effective Approach**:
- Q CLI: $19/month (Pro)
- Cline: $3-5/day (Claude API)
- Total: ~$40-60/month for maximum productivity

**Bottom Line**: Q CLI is a specialized tool for terminal-based AI assistance, especially strong for AWS work. It complements (not replaces) IDE-based tools like Cline and Q IDE plugin. Use all three strategically for maximum productivity.

---

## Resources

- **Q CLI Documentation**: https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/command-line.html
- **Q CLI GitHub**: https://github.com/aws/amazon-q-developer-cli
- **AWS DevOps Blog**: https://aws.amazon.com/blogs/devops/
- **Cline**: https://github.com/cline/cline
- **This Repository**: https://github.com/yeungjosh/amazon-ai-tooling

**Next**: [Advanced Workflows](07-advanced-workflows.md) (coming soon)
