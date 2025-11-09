# Quick Start Guide

Get productive with Amazon's AI tools in 30 minutes.

## 5-Minute Setup

### 1. Install Amazon Q Developer

**VS Code**:
```bash
code --install-extension amazonwebservices.amazon-q-vscode
```

**JetBrains**:
- Settings → Plugins → Search "Amazon Q" → Install

### 2. Authenticate
- Open Amazon Q panel
- Sign in with AWS Builder ID (or IAM Identity Center)

### 3. Try Your First AI Assistance

Open any code file and:
```python
# Function to validate email addresses and check domain MX records
```
Press Enter and watch Amazon Q suggest code!

---

## 15-Minute Optimization

### 4. Add Project Context

```bash
cd your-project
mkdir -p .amazonq/context
```

Create `.amazonq/context/project-introduction.md`:
```markdown
# Project: [Your Service]

## Tech Stack
- Language: Python 3.11
- Framework: FastAPI
- Database: PostgreSQL
- AWS: ECS, RDS, S3

## Important Files
- src/main.py - Entry point
- src/services/ - Business logic
```

### 5. Copy Prompt Library

```bash
mkdir -p ~/.aws/amazonq/prompts
cp amazon-q/prompts/* ~/.aws/amazonq/prompts/
```

Try it: In Amazon Q chat, type `/prompt code-review`

### 6. Add Rules

```bash
cp amazon-q/rules/* .amazonq/rules/
```

Now Amazon Q automatically enforces your team's standards!

---

## 30-Minute Mastery

### 7. Try Developer Agent

In Amazon Q chat:
```
"Implement a function to process payments with Stripe.
Include:
- Input validation
- Error handling
- Logging
- Tests"
```

Watch it generate complete implementation!

### 8. For ML Engineers: SageMaker Assistance

```
/prompt aws-sagemaker-ml

"Create SageMaker training code for PyTorch CNN image classifier.
Dataset in s3://my-bucket/data/.
Deploy as endpoint with auto-scaling."
```

### 9. Install Kiro (Optional - for Spec-Driven Development)

```bash
brew install --cask kiro
```

Use for greenfield projects and complex features.

---

## What's Next?

**Learn the full system**:
1. [Tools Comparison](docs/01-tools-comparison.md) - Understand all tools
2. [Amazon Q Guide](docs/02-amazon-q-guide.md) - Master Amazon Q
3. [Kiro Spec-Driven](docs/03-kiro-spec-driven.md) - Learn spec-driven development
4. [Onboarding Playbook](docs/04-onboarding-playbook.md) - Week-by-week ramp-up

**Customize for your team**:
- Edit `.amazonq/context/project-introduction.md` with your project details
- Customize prompts in `~/.aws/amazonq/prompts/`
- Add team-specific rules to `.amazonq/rules/`

**Measure success**:
- Track your Amazon Q acceptance rate (aim for 50%+)
- Log time saved weekly
- Share wins with your team

---

## Troubleshooting

**Suggestions are generic?**
→ Add more detail to `.amazonq/context/project-introduction.md`

**Not seeing inline suggestions?**
→ Check Amazon Q is enabled in IDE settings

**Rules not applying?**
→ Verify `.amazonq/rules/` directory exists and restart IDE

---

## Resources

- **This Repository**: Comprehensive guides and templates
- **Amazon Q Docs**: https://docs.aws.amazon.com/amazonq/
- **Kiro**: https://kiro.dev/
- **Issue Tracker**: https://github.com/yeungjosh/amazon-ai-tooling/issues

**Happy coding! 🚀**
