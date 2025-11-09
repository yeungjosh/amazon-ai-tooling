# Amazon SDE2/ML Engineer Onboarding Playbook

Your comprehensive guide to ramping up quickly with Amazon's AI tooling ecosystem.

## Table of Contents
1. [Day 1: Getting Started](#day-1-getting-started)
2. [Week 1: Foundation Setup](#week-1-foundation-setup)
3. [Week 2-4: Mastering the Tools](#week-2-4-mastering-the-tools)
4. [Month 2-3: Advanced Productivity](#month-2-3-advanced-productivity)
5. [Ongoing: Continuous Improvement](#ongoing-continuous-improvement)

---

## Day 1: Getting Started

### Morning: Access & Installation

**1. Get AWS Access** (Corporate IT/Manager)
- AWS Builder ID or IAM Identity Center access
- Verify access to AWS Console
- Note your AWS region (typically `us-east-1` or `us-west-2`)

**2. Install Amazon Q Developer**
```bash
# VS Code
code --install-extension amazonwebservices.amazon-q-vscode

# Or download for JetBrains from AWS Toolkit page
```

**3. Install Kiro** (Optional but recommended)
```bash
# Mac
brew install --cask kiro

# Or download from https://kiro.dev
```

**4. Set Up Cline** (Optional - for AWS MCP integration)
```bash
# In VS Code
code --install-extension saoudrizwan.claude-dev
```

### Afternoon: First Explorations

**Try Amazon Q Inline Suggestions**:
1. Open any Python/Java file in your codebase
2. Write a comment: `# Function to validate email addresses`
3. Press Enter and wait for suggestion
4. Accept with Tab if helpful

**Try Amazon Q Chat**:
1. Open Amazon Q panel (sidebar)
2. Ask: "Explain the authentication flow in this codebase"
3. Ask: "What AWS services are we using?"
4. Ask: "Show me an example of how to query DynamoDB"

**Browse Documentation**:
- [Amazon Q Developer Docs](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/)
- [This repository's guides](../README.md)

---

## Week 1: Foundation Setup

### Day 2: Context Configuration

**Create Project Context**:
```bash
cd your-project
mkdir -p .amazonq/context
```

**Write `.amazonq/context/project-introduction.md`**:
```markdown
# Project: [Your Service Name]

## Overview
[Brief description of what this service does]

## Tech Stack
- **Language**: [Python 3.11 / Java 17 / etc.]
- **Framework**: [FastAPI / Spring Boot / etc.]
- **Database**: [PostgreSQL / DynamoDB / etc.]
- **AWS Services**: [List main AWS services used]

## Architecture
[Key architectural patterns - microservices, event-driven, etc.]

## Important Files
- [Path to main entry point]
- [Path to core business logic]
- [Path to tests]

## Team Conventions
- [Coding standards]
- [Testing requirements]
- [Deployment process]
```

**Test it**:
Ask Amazon Q: "What is this project about?"
It should reference your context file.

### Day 3: Prompt Library Setup

**Create prompt library**:
```bash
mkdir -p ~/.aws/amazonq/prompts
```

**Copy templates** from this repo:
```bash
cp -r ../amazon-q/prompts/* ~/.aws/amazonq/prompts/
```

**Customize for your team**:
Edit prompts to match your team's standards, frameworks, and practices.

**Try a prompt**:
In Amazon Q chat: `/prompt code-review`

### Day 4: Rules Engine Setup

**Create rules directory**:
```bash
mkdir -p .amazonq/rules
```

**Add coding standards rule**:
Copy from `../amazon-q/rules/coding-standards.rule.md` and customize for your team.

**Add security rule**:
Copy from `../amazon-q/rules/security-checks.rule.md`.

**Add AWS best practices rule**:
Copy from `../amazon-q/rules/aws-best-practices.rule.md`.

**Test rules**:
Ask Amazon Q to generate a function that stores a password.
It should automatically use secure hashing (bcrypt/argon2) due to security rules.

### Day 5: First Real Task

**Pick a small task** (bug fix or minor feature).

**Use Amazon Q Developer Agent**:
1. Open Amazon Q chat
2. Describe the task: "Fix bug where email validation fails for .co.uk domains"
3. Let agent analyze codebase
4. Review proposed changes
5. Apply and test

**Document what worked**:
Start a personal log of productivity wins.

---

## Week 2-4: Mastering the Tools

### Week 2: Amazon Q Advanced Features

**Monday: Multi-File Refactoring**
- Task: Rename a class/service across codebase
- Use Amazon Q Agent mode
- Document time saved vs manual find-replace

**Tuesday: Code Transformation**
- If applicable: Upgrade Java version or migrate framework
- Use Amazon Q Code Transformation Agent
- Observe multi-file coordination

**Wednesday: Security Scanning**
- Run security scan on a module
- Review findings
- Use Amazon Q to fix vulnerabilities
- Learn common security patterns

**Thursday: AWS Console Integration**
- Open AWS Console → any service
- Click Amazon Q icon
- Ask: "How can I optimize costs for this RDS instance?"
- Ask: "Diagnose why my Lambda function is slow"
- Explore AWS-specific Q capabilities

**Friday: CLI Productivity**
```bash
# Install Amazon Q CLI
npm install -g @aws/amazon-q-developer-cli

# Try natural language commands
q "list all S3 buckets in us-west-2"
q "find all EC2 instances tagged Environment=production"
q "show recent CloudWatch errors for MyLambdaFunction"
```

### Week 3: Kiro Spec-Driven Development

**Monday: Learn Spec-Driven Workflow**
- Read [Kiro Guide](03-kiro-spec-driven.md)
- Watch Kiro tutorial video (https://kiro.dev/tutorials)

**Tuesday-Thursday: Build a Feature with Kiro**
- Pick a medium-sized feature (2-3 days of work)
- Open Kiro
- Describe feature in natural language
- Review generated requirements.md → edit as needed
- Review generated design.md → refine architecture
- Review generated tasks.md → adjust task breakdown
- Implement tasks incrementally
- Observe bidirectional sync

**Friday: Retrospective**
- Compare with your usual process
- What took longer? (Spec creation)
- What was easier? (Implementation, staying on track)
- When would you use Kiro vs vibe coding?

### Week 4: Cline + AWS MCP Servers

**Monday: Set Up Cline**
- Install Cline extension in VS Code
- Configure with AWS Bedrock or Claude API
- Connect to 35 AWS MCP servers (one-click in marketplace)

**Tuesday: Infrastructure Management**
Try these with Cline:
```
"List all my EC2 instances and their costs"
"Create an S3 bucket with encryption and versioning"
"Show me CloudWatch logs for errors in the last hour"
"Describe my VPC configuration"
```

**Wednesday: Combined Workflow**
- Use Amazon Q for coding
- Use Cline for AWS ops
- Switch seamlessly between tools

**Thursday: Automation Scripts**
Ask Cline to generate:
- Script to backup RDS databases
- Script to clean up old CloudWatch Logs
- Script to analyze S3 costs by bucket

**Friday: Week 4 Review**
- You now have: Amazon Q, Kiro, and Cline set up
- You understand when to use each tool
- You've built real features with AI assistance
- Time to measure productivity impact

---

## Month 2-3: Advanced Productivity

### Advanced Technique 1: Test-Driven Development with AI

**Workflow**:
1. Write test descriptions in natural language
2. Amazon Q generates failing tests
3. Amazon Q implements code to pass tests
4. You review and refine

**Example**:
```
In Amazon Q chat:
"Generate pytest tests for a function that:
1. Validates credit card numbers using Luhn algorithm
2. Handles None input gracefully
3. Rejects invalid formats
4. Accepts valid Visa, Mastercard, Amex numbers

Then implement the function to make tests pass."
```

**Practice this weekly** until it's second nature.

### Advanced Technique 2: Architecture-Level Refactoring

**Workflow**:
1. Use Kiro to document current architecture
2. Design target architecture in design.md
3. Generate migration plan in tasks.md
4. Use Amazon Q Agent to execute tasks
5. Test incrementally

**Example**: Migrate from monolith to microservices
- Kiro: Plan service boundaries
- Amazon Q: Extract service code
- Cline: Provision AWS infrastructure

### Advanced Technique 3: Documentation-Driven Development

**Workflow**:
1. Write API documentation first (OpenAPI spec)
2. Generate code from spec
3. Implementation matches contract
4. Frontend can work in parallel

**Tools**:
- Kiro for OpenAPI spec generation
- Amazon Q for endpoint implementation
- Auto-generated API docs always in sync

### Advanced Technique 4: PR Review Automation

**Set up pre-commit hook**:
```bash
# .git/hooks/pre-commit
#!/bin/bash
CHANGED_FILES=$(git diff --cached --name-only --diff-filter=ACM)

for FILE in $CHANGED_FILES; do
    echo "Reviewing $FILE..."
    # Use Amazon Q CLI to review
    q chat --prompt code-review --context "$FILE"
done
```

**Benefits**:
- Catch issues before PR submission
- Learn from AI feedback
- Improve code quality

### Advanced Technique 5: Custom Kiro Templates

**Create team-specific templates**:
```markdown
# .kiro/templates/microservice-template.md

## Requirements Template
[User stories following your team's format]

## Design Template
- Must use company's standard tech stack
- Must follow company's API conventions
- Must include observability (metrics, logs, traces)
- Must include runbook

## Tasks Template
- Always include Terraform infrastructure tasks
- Always include CI/CD pipeline tasks
- Always include security review task
```

**Benefits**:
- Consistent architecture across team
- New services follow established patterns
- Onboarding new engineers easier

---

## Month 2-3: Domain-Specific Learning

### For ML Engineers

**Amazon Q for ML workflows**:
```
"Generate SageMaker training job code for a PyTorch CNN model"
"Create Lambda function to invoke SageMaker endpoint"
"Write code to preprocess images for model training"
"Generate CloudFormation for MLOps pipeline"
```

**Kiro for ML projects**:
- Requirements: Model performance targets, data requirements
- Design: Training pipeline, feature engineering, model architecture
- Tasks: Data collection, preprocessing, training, evaluation, deployment

**Cline for ML infrastructure**:
```
"Set up SageMaker notebook instance with these packages"
"Create S3 bucket for training data with lifecycle rules"
"Show me CloudWatch metrics for my SageMaker endpoint"
```

### For Backend Engineers

**Amazon Q for backend development**:
```
"Generate REST API for user management with FastAPI"
"Create database migration for new tables"
"Implement circuit breaker for external API calls"
"Write integration tests for API endpoints"
```

**Focus areas**:
- API design and implementation
- Database schema and queries
- Microservices communication
- Error handling and resilience

### For Full-Stack Engineers

**Amazon Q for frontend + backend**:
```
"Generate React component with TypeScript for user profile"
"Create corresponding backend API endpoint"
"Add form validation on both frontend and backend"
"Write E2E tests with Playwright"
```

**Kiro for full-stack features**:
- Requirements: User stories with UI/UX details
- Design: API contracts, component hierarchy, state management
- Tasks: Backend implementation, frontend implementation, integration

---

## Ongoing: Continuous Improvement

### Weekly Practices

**Monday: Plan Week with Kiro**
- Review sprint tasks
- Create specs for complex features
- Break down into daily goals

**Wednesday: Mid-Week Checkpoint**
- Review Amazon Q acceptance rate (aim for 50%+)
- Refine prompts that didn't work well
- Update context files if project evolved

**Friday: Retrospective**
- Log productivity wins
- Note AI limitations encountered
- Share learnings with team

### Monthly Practices

**Review Metrics**:
- Time saved (estimate)
- Code quality (bugs, review comments)
- Velocity (story points completed)

**Update Configurations**:
- Refine `.amazonq/rules/` based on recent issues
- Update `.amazonq/context/` with new architecture
- Add new prompts to library

**Share with Team**:
- Demo a productivity win
- Share a useful prompt
- Contribute to team knowledge base

### Quarterly Practices

**Deep Skill Building**:
- Master one advanced technique fully
- Create a case study of AI-assisted project
- Mentor new team member on AI tools

**Tool Evaluation**:
- Review new AI tool releases
- Experiment with alternatives
- Recommend updates to team tooling

---

## Measuring Success

### Quantitative Metrics

Track these in a personal log:

**Productivity**:
- Hours saved per week (estimate)
- Story points completed per sprint (compare to pre-AI)
- Code review turnaround time

**Quality**:
- Bugs in production (compare to baseline)
- Security vulnerabilities found/fixed
- Test coverage percentage

**Acceptance Rates**:
- Amazon Q suggestion acceptance rate
- Kiro-generated spec accuracy
- AI-generated test effectiveness

### Qualitative Indicators

**You're succeeding if**:
- ✅ You reach for AI tools instinctively
- ✅ You can explain when to use each tool
- ✅ You catch and correct AI mistakes quickly
- ✅ You feel more productive than before
- ✅ Your code reviews mention fewer issues
- ✅ You can onboard teammates to AI tools

**Warning signs**:
- ❌ Blindly accepting all AI suggestions
- ❌ Slower than before (after 4+ weeks)
- ❌ More bugs in production
- ❌ Teammates confused by AI-generated code
- ❌ Specs and code out of sync (Kiro)

**If seeing warning signs**:
- Slow down and review AI outputs carefully
- Pair program with experienced engineer
- Focus on one tool at a time
- Re-read guides and best practices

---

## Transitioning from Claude Code

If you're coming from Claude Code experience:

### Similarities (Easy Transition)
- Both use Claude models (similar reasoning quality)
- Both understand multi-file context
- Both can autonomously edit files
- Chat-based interaction feels familiar

### Differences (Learn These)
- **Amazon Q**: IDE-integrated (not terminal-based)
- **Amazon Q**: AWS-expert (specialized knowledge)
- **Amazon Q**: Rules engine (team standards enforcement)
- **Kiro**: Spec-driven (more structured than Claude Code)

### Migration Strategy
Week 1-2: Use Amazon Q for AWS-specific work, Claude Code for other tasks
Week 3-4: Gradually shift all work to Amazon Q, use Claude Code as fallback
Month 2+: Primarily Amazon Q + Kiro, Claude Code for special cases

### What to Keep from Claude Code
- Whole-project thinking
- Iterative refinement approach
- Test-first mindset
- Clear, specific prompts

### What to Add for Amazon Q
- Context file configuration
- Rules definition
- Prompt library usage
- AWS Console integration

---

## Common Challenges & Solutions

### Challenge 1: "AI suggestions are too generic"

**Solutions**:
- ✅ Add detailed context files
- ✅ Use `@` mentions for specific symbols
- ✅ Write more descriptive comments
- ✅ Define rules for your tech stack

### Challenge 2: "I'm slower with AI than without"

**Solutions**:
- ✅ Start with small tasks, build confidence
- ✅ Focus on acceptance rate, not speed initially
- ✅ Review and learn from suggestions (builds intuition)
- ✅ Give it 4-6 weeks (learning curve is real)

### Challenge 3: "AI makes mistakes I don't catch"

**Solutions**:
- ✅ Always review AI-generated code line by line
- ✅ Write tests first (TDD)
- ✅ Use security scanning
- ✅ Code review with human teammate

### Challenge 4: "Specs get out of sync with code (Kiro)"

**Solutions**:
- ✅ Enable auto-sync in Kiro settings
- ✅ Review spec diffs after coding sessions
- ✅ Update spec first, then code (preferred workflow)
- ✅ Make spec review part of PR checklist

### Challenge 5: "Team isn't using AI tools consistently"

**Solutions**:
- ✅ Demo productivity wins in team meetings
- ✅ Share configs (context files, prompts, rules)
- ✅ Pair program with AI to show workflow
- ✅ Create team guidelines for AI usage

---

## Resources Checklist

By end of Month 1, you should have:
- [ ] Amazon Q installed and configured
- [ ] Kiro installed (if doing greenfield work)
- [ ] Cline installed and connected to AWS MCP
- [ ] Project context files created
- [ ] Prompt library set up
- [ ] Rules engine configured
- [ ] First feature shipped with AI assistance
- [ ] Personal productivity log started

By end of Month 3, you should have:
- [ ] 50%+ suggestion acceptance rate
- [ ] Used all three tools (Amazon Q, Kiro, Cline) in real work
- [ ] Created custom prompts for common tasks
- [ ] Contributed to team AI tool standards
- [ ] Mentored another engineer on AI tools
- [ ] Measurable productivity improvement

---

## Getting Help

**Amazon Q Issues**:
- Check [AWS Developer Forums](https://forums.aws.amazon.com/)
- AWS Support (if you have enterprise support)
- This repository's [issue tracker](https://github.com/yeungjosh/amazon-ai-tooling/issues)

**Kiro Issues**:
- [Kiro Documentation](https://kiro.dev/docs)
- [Kiro Community Forum](https://community.kiro.dev)

**Cline Issues**:
- [Cline GitHub Repository](https://github.com/clinebot/cline)
- [Cline Discord Community](https://discord.gg/cline)

**General AI Coding Questions**:
- Share in your team Slack/chat
- Pair program with experienced AI tool user
- Review examples in this repository

---

## Next Steps

You've completed onboarding! Now:

1. **Customize this playbook** for your specific team/domain
2. **Share learnings** with teammates
3. **Contribute back** to this repository with your discoveries
4. **Keep learning** - AI tools evolve rapidly

**Continue to**: [Advanced Workflows](05-advanced-workflows.md)
