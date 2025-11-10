# Engineer's Practical Quick Start

Battle-tested setup for maximum productivity with AI coding tools. Based on real-world usage patterns.

## The Modern AI Coding Stack (2025)

Based on what actually works in production:

```
Daily Coding:     Cline (or Roo Code) in VS Code
AWS Work:         Amazon Q Developer
Complex Features: Kiro (spec-driven)
Quick Tasks:      GitHub Copilot inline suggestions
```

---

## 10-Minute Essential Setup

### 1. Install Cline (Recommended for Most Engineers)

**Why Cline?**
- Free and open source (3.8M+ developers)
- MCP integration for AWS (35+ MCP servers)
- Works with any AI model (Claude, GPT-4, etc.)
- Simple, reliable, battle-tested

```bash
# In VS Code
code --install-extension saoudrizwan.claude-dev
```

**Alternative: Roo Code** (if you want advanced customization)
```bash
code --install-extension RooVeterinaryInc.roo-cline
```

**Cline vs Roo Code**:
- **Cline**: Simpler, more reliable, larger community
- **Roo Code**: More features (modes, profiles), faster for large projects
- **Verdict**: Start with Cline, switch to Roo if you need specialized modes

### 2. Configure Cline with Claude (Best Performance)

**Option A: Claude API (Recommended)**
1. Get API key: https://console.anthropic.com/
2. In Cline settings → Add API Key
3. Model: `claude-sonnet-4-5-20250929` (best for coding)

**Option B: AWS Bedrock (If at Amazon)**
1. Cline settings → Provider: AWS Bedrock
2. Use your AWS credentials
3. Model: Claude 3.5 Sonnet

**Cost**: ~$3-5/day for heavy usage with Claude API

### 3. Install AWS MCP Servers (Critical for AWS Work)

```bash
# Cline makes this one-click:
# Open Cline → MCP Marketplace → Search "AWS" → Install

# Or manually configure ~/.cline/mcp_settings.json
```

**Essential AWS MCP Servers**:
- `@aws/ec2` - Manage EC2 instances
- `@aws/s3` - S3 operations
- `@aws/lambda` - Lambda functions
- `@aws/cloudwatch` - Logs and metrics
- `@aws/dynamodb` - DynamoDB operations

### 4. Install Amazon Q Developer (For AWS-Specific Work)

```bash
code --install-extension amazonwebservices.amazon-q-vscode
```

Authenticate with AWS Builder ID.

---

## Your First Hour: Practical Workflows

### Workflow 1: Use Cline for Feature Implementation

**Scenario**: Build a REST API endpoint

```
In Cline chat:
"Create a FastAPI endpoint POST /api/orders that:
- Accepts order data (items, shipping address)
- Validates input with Pydantic
- Saves to PostgreSQL using SQLAlchemy
- Returns order ID and status
- Includes error handling
- Add unit tests with pytest"

Cline will:
1. Create/edit multiple files
2. Show you diffs before applying
3. Run tests
4. Report results
```

**Key**: Be specific. The more detail, the better the output.

### Workflow 2: Use Amazon Q for AWS Tasks

**Scenario**: Deploy to AWS

```
In Amazon Q chat:
"Generate Terraform code to:
- Create ECS Fargate cluster
- Application Load Balancer
- RDS PostgreSQL Multi-AZ
- S3 bucket for uploads
- CloudWatch logs and alarms
- Follow least privilege IAM"

Amazon Q will generate complete IaC with AWS best practices.
```

**Why Amazon Q for AWS**: It's trained on AWS docs and knows best practices.

### Workflow 3: Use Cline + AWS MCP for Cloud Ops

**Scenario**: Debug production issue

```
In Cline chat:
"Check CloudWatch logs for errors in my-lambda-function
in the last hour. Show me the stack traces."

With AWS MCP installed, Cline:
1. Fetches logs from CloudWatch
2. Parses errors
3. Shows you stack traces
4. Can suggest fixes
```

**Powerful Use Cases**:
```
"List all EC2 instances and their costs this month"
"Create S3 bucket with encryption and versioning"
"Show DynamoDB table scan for user_id='123'"
"Analyze Lambda cold start issues"
```

---

## Real-World Workflows

### Daily Coding Workflow

**Morning**:
```
1. Open VS Code
2. Cline: "Review my git diff and suggest improvements"
3. Cline: "Add unit tests for yesterday's changes"
4. Amazon Q inline: Accepts suggestions while coding
```

**Feature Development**:
```
1. Cline: "Implement [feature] with [requirements]"
2. Review diffs carefully (Cline shows before/after)
3. Accept/reject/modify changes
4. Cline: "Run tests and fix failures"
5. Cline: "Update documentation"
```

**Debugging**:
```
1. Cline: "This function is failing with [error]. Debug it."
2. Cline analyzes code, suggests fixes
3. Apply fix, test
4. Cline: "Add test case to prevent regression"
```

### AWS-Heavy Workflow

**Infrastructure**:
```
1. Amazon Q: "Generate Terraform for [infrastructure]"
2. Review generated code
3. Cline: "Apply this Terraform and handle any errors"
4. Cline: "Create CloudWatch dashboard for these resources"
```

**Troubleshooting**:
```
1. Cline + AWS MCP: "Why is my Lambda timing out?"
2. Cline fetches logs, metrics, configuration
3. Analyzes and suggests optimizations
4. Cline: "Implement the fix"
```

### ML Engineering Workflow

**Model Development**:
```
1. Amazon Q: "Generate SageMaker training code for [model type]"
2. Cline: "Set up data preprocessing pipeline"
3. Cline: "Create training script with hyperparameter tuning"
4. Amazon Q: "Deploy as SageMaker endpoint with auto-scaling"
```

**Experimentation**:
```
1. Cline: "Run experiment comparing [approach A] vs [approach B]"
2. Cline implements both, runs training
3. Cline: "Visualize results and suggest which is better"
```

---

## Advanced: Cline + Roo Code Combo

### When to Use Roo Code's Specialized Modes

**Roo Code Modes**:
- **Ask Mode**: ChatGPT-like, for questions and guidance
- **Code Mode**: Autonomous coding (like Cline)
- **Architect Mode**: High-level planning and design
- **Debug Mode**: Specialized debugging (NEW in v3.3)

**Custom Modes** (Roo Code's killer feature):
Create specialized AI agents:
```
Security Expert Mode:
"Review this code for OWASP Top 10 vulnerabilities.
Focus on: SQL injection, XSS, auth issues, secrets in code."

Performance Optimizer Mode:
"Analyze this code for performance bottlenecks.
Check: N+1 queries, missing indexes, sync vs async."

Documentation Writer Mode:
"Generate comprehensive docs with examples and diagrams."
```

**Workflow: Roo Code for Planning, Cline for Implementation**
```
1. Roo Code (Architect Mode): Plan feature architecture
2. Review and refine plan
3. Switch to Cline: Implement the plan step-by-step
4. Roo Code (Debug Mode): Fix any issues
5. Roo Code (Custom "Security" Mode): Security review
```

**Why Both?**
- Cline: More reliable, simpler, better for straightforward coding
- Roo Code: More flexible, specialized modes, better for complex workflows

**Recommendation**: Start with Cline. Add Roo Code if you find yourself needing specialized modes.

---

## Cost Optimization

### Claude API Usage (~$3-5/day for heavy use)

**Tips to reduce costs**:
```
1. Use Claude Sonnet (not Opus) for coding - $3 per 1M input tokens
2. Clear Cline's context periodically (Tools → Clear History)
3. Be specific in prompts (avoid back-and-forth)
4. Use cheaper models for simple tasks (GPT-4o-mini via OpenRouter)
```

**OpenRouter Setup** (Access to 100+ models):
```
1. Get API key: https://openrouter.ai/
2. Cline → Provider: OpenAI Compatible
3. Base URL: https://openrouter.ai/api/v1
4. Choose model: Claude, GPT-4, Llama, DeepSeek, etc.
```

**Budget-Friendly Models**:
- DeepSeek Coder V2: $0.27 per 1M tokens (99% cheaper!)
- GPT-4o-mini: $0.15 per 1M input tokens
- Gemini 1.5 Flash: $0.075 per 1M tokens

### AWS Bedrock (If at Amazon)

**Pros**:
- Enterprise billing
- No personal API keys
- AWS compliance

**Cons**:
- Higher latency than direct API
- Fewer model options

---

## Essential Cline Configurations

### Settings to Enable

```json
{
  "cline.alwaysApproveReads": true,          // Faster workflow
  "cline.alwaysApproveWrites": false,        // Safety: review changes
  "cline.maxFileLineCount": 10000,           // Handle large files
  "cline.showDiffBeforeApply": true,         // Review before applying
  "cline.mcpEnabled": true,                  // Enable MCP servers
  "cline.experimentalFeatures": true         // Latest features
}
```

### Custom Instructions (Critical)

Add to Cline's custom instructions:
```
Project Context:
- Language: Python 3.11
- Framework: FastAPI
- Database: PostgreSQL + SQLAlchemy
- AWS Services: ECS, RDS, S3, SQS
- Testing: pytest with 80%+ coverage required

Code Standards:
- Type hints for all functions
- Docstrings (Google style)
- Error handling with logging
- Security: validate inputs, use parameterized queries

AWS Best Practices:
- Least privilege IAM
- Enable encryption (S3, RDS)
- Tag all resources
- CloudWatch logging required
```

---

## Integration with Other Tools

### Cline + GitHub Copilot

**Strategy**: Use both together
```
- Copilot: Inline suggestions while typing (autocomplete)
- Cline: Complex multi-file changes (agentic tasks)
```

**Workflow**:
```
1. Start typing → Copilot suggests → Accept with Tab
2. Complex task? → Ask Cline
3. Cline implements across multiple files
4. Back to typing → Copilot assists
```

### Cline + Amazon Q

**Strategy**: Specialized tools
```
- Cline: General coding, refactoring, testing
- Amazon Q: AWS-specific tasks, IaC, cloud architecture
```

**Example**:
```
1. Amazon Q: Generate Terraform for infrastructure
2. Cline: Implement application code
3. Amazon Q: Create deployment pipeline
4. Cline: Write integration tests
5. Amazon Q: Set up monitoring and alarms
```

### Cline + Kiro

**Strategy**: Spec-driven development
```
- Kiro: Plan and design (requirements → design → tasks)
- Cline: Implement tasks from Kiro's task list
```

**Workflow**:
```
1. Kiro: Generate comprehensive specs
2. Export tasks from Kiro
3. Cline: Implement each task
4. Kiro: Update specs based on implementation
```

---

## Productivity Hacks

### 1. Context Files

Create `.clinerules` in your project root:
```markdown
# Project Rules

## Code Style
- Use Black formatter (line length 100)
- Type hints required
- Docstrings for all public functions

## Testing
- pytest for all tests
- Minimum 80% coverage
- Use fixtures for database tests

## Security
- No hardcoded secrets
- Validate all inputs
- Use parameterized SQL queries

## AWS
- Tag all resources
- Enable encryption
- Use least privilege IAM
```

Cline reads this automatically!

### 2. Pre-Made Prompts

Save common prompts:

**`review.prompt`**:
```
Review this code for:
1. Security vulnerabilities
2. Performance issues
3. Code quality and readability
4. Test coverage gaps
5. AWS best practices (if applicable)

Provide specific, actionable feedback with code examples.
```

**`refactor.prompt`**:
```
Refactor this code to:
1. Improve readability (reduce complexity)
2. Enhance maintainability (SOLID principles)
3. Optimize performance
4. Add comprehensive error handling
5. Include type hints and docstrings
```

**Usage**: Copy/paste into Cline when needed.

### 3. Task Chaining

Break complex tasks into steps:
```
Step 1: "Create database models for orders, order_items, customers"
[Review and approve]

Step 2: "Create repository layer with CRUD operations"
[Review and approve]

Step 3: "Create service layer with business logic"
[Review and approve]

Step 4: "Create API endpoints with validation"
[Review and approve]

Step 5: "Add comprehensive tests for all layers"
[Review and approve]
```

**Why**: Easier to review, catch errors early, maintain quality.

### 4. Git Integration

```
"Review my staged changes and suggest improvements"
"Create a descriptive commit message for these changes"
"Revert the changes in src/services/payment.py"
"Show me what changed in the last commit"
```

Cline can read git state and help with version control!

### 5. AWS MCP Power Moves

```
"Deploy my Lambda function and show me the logs"
"What's causing high CPU on my EC2 instances?"
"Compare S3 costs between buckets this month"
"Create CloudWatch alarm for Lambda errors > 5/min"
"Show me all resources without proper tags"
```

With MCP, Cline becomes your AWS CLI on steroids.

---

## Troubleshooting

### Cline Not Responding
```
1. Check API key is valid
2. Check rate limits (Anthropic: 50 requests/min)
3. Reduce context size (clear history)
4. Restart VS Code
```

### MCP Servers Not Working
```
1. Verify ~/.cline/mcp_settings.json exists
2. Check AWS credentials configured
3. Restart Cline extension
4. Check MCP server logs (View → Output → Cline MCP)
```

### Poor Code Quality
```
1. Add detailed custom instructions
2. Create .clinerules file
3. Be more specific in prompts
4. Review and reject bad suggestions
5. Iterate: "Improve this by..."
```

### High API Costs
```
1. Switch to cheaper model (DeepSeek, GPT-4o-mini)
2. Clear context frequently
3. Use Copilot for simple autocomplete
4. Be specific to avoid back-and-forth
```

---

## Week 1 Challenge

**Day 1**: Set up Cline + Claude API → Build hello world API
**Day 2**: Add AWS MCP servers → List your EC2 instances via Cline
**Day 3**: Build a CRUD API with Cline's help
**Day 4**: Deploy to AWS using Cline + Amazon Q
**Day 5**: Use Cline to write tests and refactor code

**Goal**: Ship a small feature end-to-end using AI assistance.

---

## Measuring Success

Track your productivity:
```
Before AI Tools:
- Feature time: [X days]
- Bugs found in review: [Y]
- Test coverage: [Z%]

After 2 Weeks:
- Feature time: [X - 30%?]
- Bugs found in review: [Y - 20%?]
- Test coverage: [Z + 10%?]
```

**Good Indicators**:
- Shipping features faster
- Fewer bugs in code review
- Better test coverage
- More time for architecture/design
- Less time on boilerplate

**Warning Signs**:
- Blindly accepting all suggestions
- Not understanding generated code
- More bugs reaching production
- Slower than manual coding

If you see warning signs → slow down, review carefully, iterate.

---

## Next Steps

**After Quick Start**:
1. Read [Tools Comparison](docs/01-tools-comparison.md) for deep dive
2. Set up [Amazon Q optimization](docs/02-amazon-q-guide.md)
3. Learn [Kiro for complex features](docs/03-kiro-spec-driven.md)
4. Follow [Onboarding Playbook](docs/04-onboarding-playbook.md) for systematic ramp-up

**Advanced**:
- Create custom Roo Code modes for your domain
- Build MCP servers for internal tools
- Set up team shared prompts and rules
- Integrate AI tools into CI/CD

---

## Essential Links

- **This Repo**: https://github.com/yeungjosh/amazon-ai-tooling
- **Cline**: https://github.com/cline/cline
- **Roo Code**: https://roocode.com/
- **Amazon Q**: https://aws.amazon.com/q/developer/
- **Anthropic API**: https://console.anthropic.com/
- **OpenRouter** (multi-model): https://openrouter.ai/

---

**Remember**: AI tools are force multipliers, not replacements. Your engineering judgment, code review, and testing are still critical. Use AI to move faster, but always understand what you're shipping.

**Happy coding! 🚀**
