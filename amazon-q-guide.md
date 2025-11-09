# Amazon Q Developer: The Complete Guide

Amazon Q Developer is your AI pair programmer that knows AWS inside and out. This guide covers everything from basics to advanced techniques.

## What Makes Amazon Q Special

### Multi-Model Intelligence
Unlike single-model assistants, Q dynamically selects the optimal model for each task:
- **Claude 3** for complex reasoning and architecture decisions
- **Amazon Titan** for efficient code search
- **Llama** for lightweight code completions
- Models from AI21, Cohere, and Meta for specialized tasks

### Industry-Leading Performance
- **37-50% code acceptance rate** (highest in the industry)
- **95%+ spec implementation accuracy** on first attempt
- **SWE-Bench Leaderboard leader** for agentic coding
- **20-40% developer productivity boost** (early adopters)

## Installation & Setup

### IDE Extensions
```bash
# VS Code
code --install-extension AmazonWebServices.amazon-q-vscode

# JetBrains (search in marketplace)
# IntelliJ IDEA, PyCharm, WebStorm, etc.
```

### AWS Console Integration
Q is built directly into the AWS Management Console - no installation needed.

### CLI Integration
```bash
# Install AWS CLI if not already installed
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

# Q is included in latest AWS CLI
aws --version  # Should be 2.x+
```

## Core Features

### 1. Inline Code Suggestions

**How it works:**
- Type comments or start writing code
- Q generates real-time suggestions (snippets to full functions)
- Accept with Tab, reject by continuing to type

**Best practices:**
```python
# ❌ Vague comment
# process data

# ✅ Specific comment with context
# Process user data from DynamoDB, filter active users from last 30 days,
# return as list of dicts with fields: user_id, email, last_active_date
```

**Pro tip:** Include library versions and AWS service names for better suggestions.

### 2. @workspace Annotation - Your Secret Weapon

This is THE most powerful feature for codebase-aware AI.

**What it does:**
- Ingests and indexes ALL code files in your workspace
- Understands project structure, patterns, and conventions
- Provides context-aware responses across your entire codebase

**When to use:**
```
@workspace How does authentication work in this project?
@workspace Find all API endpoints related to user management
@workspace What's the pattern for adding a new service?
@workspace Refactor the payment module to match our error handling patterns
```

**Best practices:**
- Use for architecture questions
- Use before refactoring across multiple files
- Use to understand legacy code quickly
- Always use when asking "how does X work?"

**Warning:** The first @workspace query indexes your project (may take 30-60 seconds for large codebases).

### 3. Agentic Capabilities - The Game Changer

Q can autonomously perform multi-step tasks:

**Feature Implementation:**
```
Implement a caching layer for our API using Redis.
Requirements:
- Add Redis client configuration
- Create cache middleware
- Add cache keys for user data endpoints
- Include cache invalidation on updates
- Add monitoring metrics
- Write unit tests
```

Q will:
1. Analyze your codebase structure
2. Create a multi-file implementation plan
3. Generate code diffs across files
4. Run tests and iterate on failures
5. Provide real-time progress updates

**Code Transformation:**
```
Upgrade this Lambda function from Python 3.8 to Python 3.12.
Update all deprecated AWS SDK calls.
```

### 4. Security Scanning

**Automatic detection of:**
- Exposed credentials and API keys
- SQL injection vulnerabilities
- Log injection flaws
- OWASP Top 10 vulnerabilities
- AWS-specific security issues

**Usage:**
- Right-click on file → "Scan with Amazon Q"
- Q provides vulnerability report + auto-fix suggestions
- One-click remediation for most issues

**Best practice:** Run security scan before every commit on sensitive code.

### 5. Code Optimization

**Access via:**
- Right-click → "Optimize with Amazon Q"
- Keyboard: `Ctrl+Shift+O` (customize in settings)

**What it optimizes:**
- Algorithm efficiency (time/space complexity)
- AWS service usage (cost optimization)
- Memory management
- Database query performance
- API call patterns

**Example workflow:**
```python
# Your code
def get_user_orders(user_ids):
    orders = []
    for user_id in user_ids:
        response = dynamodb.get_item(Key={'user_id': user_id})
        orders.append(response['Item'])
    return orders

# Q's optimization: batch reads
def get_user_orders(user_ids):
    response = dynamodb.batch_get_item(
        RequestItems={
            'orders': {
                'Keys': [{'user_id': uid} for uid in user_ids]
            }
        }
    )
    return response['Responses']['orders']
```

### 6. Documentation Generation

**Generates:**
- Function/class docstrings
- README files
- Architecture documentation
- Data flow diagrams
- API documentation

**Usage:**
```
@workspace Generate comprehensive documentation for the authentication module including data flow diagrams
```

**Pro tip:** Q creates PlantUML diagrams that you can render in most IDEs.

## Advanced Techniques

### Breaking Down Large Problems

Amazon Q works best with focused, scoped tasks.

**❌ Too broad:**
```
Build a complete e-commerce backend
```

**✅ Well-scoped:**
```
Phase 1: Implement product catalog service
Requirements:
- DynamoDB table schema for products
- CRUD API endpoints using API Gateway + Lambda
- Product search using DynamoDB GSI
- Input validation and error handling
- Unit tests with 80%+ coverage
```

### Providing Context for Better Results

**Structure your prompts:**
```
[Context]
Our app is a serverless e-commerce platform using:
- AWS Lambda (Python 3.12)
- DynamoDB for data
- API Gateway for REST APIs
- Cognito for auth

[Current State]
File: src/handlers/products.py
We have basic CRUD but no caching

[Goal]
Add Redis caching layer for product reads

[Requirements]
- ElastiCache Redis cluster
- Cache TTL: 1 hour
- Invalidate on product updates
- Add CloudWatch metrics for cache hit/miss
- Maintain existing API contract

[Constraints]
- Must work within VPC
- Follow our existing error handling pattern (see src/utils/errors.py)
```

### Version-Specific Queries

Always specify versions for better results:

```
Generate AWS Lambda handler using:
- Python 3.12
- boto3 1.34.x
- AWS Lambda Powertools 2.x
- Follow typing best practices with mypy
```

### Customizations for Team Patterns

Amazon Q Pro supports customizations:

**What you can customize:**
- Import statement patterns
- Error handling styles
- Logging formats
- Testing frameworks
- Code organization conventions

**How to set up:**
1. Go to Amazon Q settings in your IDE
2. Add customization rules (Admin role required)
3. Supported languages: Python, Java, JavaScript, TypeScript, Go, Kotlin, PHP, Ruby, Rust, Scala, and more

**Example customization:**
```yaml
# .amazon-q-customizations.yaml
language: python
patterns:
  - name: "import-style"
    rule: "Always use absolute imports, never relative"

  - name: "error-handling"
    rule: "Use our custom ErrorHandler class from src.utils.errors"

  - name: "logging"
    rule: "Use AWS Lambda Powertools Logger, never print() statements"

  - name: "testing"
    rule: "Use pytest with fixtures from tests/conftest.py"
```

## AWS-Specific Superpowers

### 1. Infrastructure as Code

Q understands CloudFormation, CDK, and Terraform:

```
Create a CDK stack (TypeScript) for:
- VPC with public/private subnets across 3 AZs
- ECS Fargate cluster
- Application Load Balancer
- RDS PostgreSQL in private subnets
- All following AWS Well-Architected Framework
```

### 2. AWS Console Integration

In the AWS Console, ask Q:
- "How do I reduce costs for my EC2 instances?"
- "What's causing high latency in my API Gateway?"
- "Troubleshoot this Lambda timeout error: [paste error]"
- "Best practices for securing this S3 bucket?"

### 3. Operational Intelligence

Q can help with live debugging:
```
@workspace The Lambda function in src/handlers/payment.py is timing out.
CloudWatch logs show: [paste logs]
Diagnose the issue and provide a fix.
```

## Keyboard Shortcuts (VS Code)

| Action | Shortcut | Customizable |
|--------|----------|--------------|
| Open Q Chat | `Ctrl+Shift+Q` | Yes |
| Accept Suggestion | `Tab` | Yes |
| Reject Suggestion | `Esc` | Yes |
| Optimize Code | `Ctrl+Shift+O` | Yes |
| Security Scan | Right-click menu | - |
| Inline Chat | `Ctrl+I` | Yes |

## Best Practices Summary

### DO:
✅ Use `@workspace` for codebase-aware queries
✅ Break large tasks into focused subtasks
✅ Provide version numbers and tech stack details
✅ Include constraints and requirements explicitly
✅ Review generated code for security and logic
✅ Run security scans before committing
✅ Leverage Q's AWS knowledge for cloud-native patterns
✅ Use specific, technical language in prompts
✅ Reference existing files for pattern matching

### DON'T:
❌ Accept code blindly without review
❌ Ask Q to "build everything" at once
❌ Forget to specify language/framework versions
❌ Skip security scanning on sensitive code
❌ Use vague prompts without context
❌ Ignore Q's security recommendations
❌ Assume Q knows your internal business logic without context

## Pricing & Limits

### Free Tier (Perpetual)
- 50 agentic chat interactions per month
- Unlimited inline code suggestions
- Security scanning included
- No credit card required

### Pro Tier ($19/month)
- Unlimited agentic chats
- Advanced customizations
- Priority support
- Agent customizations for team patterns
- Extended context window
- Multi-repository awareness

## Common Issues & Solutions

### Q suggestions seem off-context
**Solution:** Use `@workspace` to give Q full codebase context.

### Slow first @workspace query
**Expected behavior:** Indexing takes 30-60s for large repos. Subsequent queries are fast.

### Security scan false positives
**Solution:** Review and mark as false positive. Q learns from your feedback.

### Generated code doesn't follow team patterns
**Solution:** Set up customizations in Pro tier, or include pattern examples in your prompt.

### Q doesn't know recent AWS features
**Solution:** Provide documentation links or describe the feature. Q's training includes updates through early 2025.

## Integration with Other Tools

### Works with:
- **Git/GitHub**: Q understands version history and can help with PRs
- **AWS CodeCommit**: Native integration
- **Jira/Linear**: Copy requirements directly into Q prompts
- **Slack/Teams**: Amazon Q Business can integrate (separate product)

### Complementary tools:
- **Kiro IDE**: Use for spec-driven development
- **AWS CodeGuru**: Deeper performance analysis
- **AWS CodePipeline**: CI/CD integration

## Measuring Your Productivity Gains

Track these metrics:

**Week 1 Baseline:**
- Time to complete typical feature
- Lines of code written per day
- Bug fix time
- Documentation time

**After 1 Month:**
- Code acceptance rate from Q
- Time saved on boilerplate
- Vulnerabilities caught by security scan
- Documentation completeness improvement

**Target Gains:**
- 20-40% productivity increase
- 30% reduction in bug-related time
- 50% faster documentation
- Higher code review approval rate

## Next Steps

1. ✅ Install Amazon Q in your IDE
2. ✅ Try inline suggestions on a small task
3. ✅ Use `@workspace` to explore your codebase
4. ✅ Complete your first agentic feature implementation
5. ✅ Run security scan on a critical file
6. ✅ Set up customizations (Pro tier)
7. ✅ Document your productivity wins

## Additional Resources

- [AWS Prescriptive Guidance](https://docs.aws.amazon.com/prescriptive-guidance/latest/best-practices-code-generation/)
- [Amazon Q Developer Blog](https://aws.amazon.com/blogs/devops/)
- [Mastering Amazon Q Series](https://aws.amazon.com/blogs/devops/mastering-amazon-q-developer-part-1-crafting-effective-prompts/)
- Internal wiki: Search for "Amazon Q best practices" on your team's wiki

---

**Remember:** Amazon Q is a powerful tool, but you're still the engineer. Always review, test, and understand the code Q generates. Use it to amplify your skills, not replace them.
