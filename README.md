# Amazon AI Tooling Productivity System

**Welcome SDE2/ML Engineer!** This guide will help you supercharge your productivity at Amazon using cutting-edge AI tools.

## Quick Start

Coming from Claude Code? Great! Amazon's AI tools share similar agentic capabilities but with AWS-specific superpowers.

**Your AI Arsenal at Amazon:**
- **Amazon Q Developer** - Your primary AI coding assistant (like Claude Code but AWS-native)
- **Kiro IDE** - Agentic spec-driven development environment (preview)
- **Amazon CodeWhisperer** - Part of Q Developer for inline suggestions

## Key Differences from Claude Code

| Feature | Claude Code | Amazon Q Developer | Kiro IDE |
|---------|-------------|-------------------|----------|
| **Base Model** | Claude 3.7 Sonnet / Opus 4 | Multi-model (Claude 3, Titan, Llama) | Claude Sonnet 4.0 |
| **Code Generation** | Agentic, full codebase context | Agentic with AWS context | Spec-driven agentic |
| **Acceptance Rate** | ~40-50% | 37-50% (industry leading) | N/A (different workflow) |
| **Best For** | General development | AWS-native development | Prototype to production |
| **Context** | 100k+ tokens | 100k characters in chat | Full codebase + specs |
| **Workflow** | Conversational prompting | Conversational + autonomous | Spec-driven + agent hooks |
| **Security Scanning** | Basic | Hard-to-detect vulnerabilities | Built-in governance |
| **Price (Free Tier)** | Limited | 50 agentic chats/month | Preview (check pricing) |

## The Productivity System

### Phase 1: First Week (Days 1-5)
**Goal: Get comfortable with Amazon Q Developer**

1. **Day 1-2:** Install & Configure
   - Set up Amazon Q in VS Code/JetBrains
   - Learn keyboard shortcuts
   - Review `amazon-q-guide.md` for best practices

2. **Day 3-4:** Master Core Features
   - Practice @workspace for codebase context
   - Use inline suggestions with AWS libraries
   - Try security scanning on existing code
   - Review `prompt-engineering.md`

3. **Day 5:** Autonomous Features
   - Test agentic capabilities on small tasks
   - Document code with Q's auto-documentation
   - Run your first optimization workflow

### Phase 2: Week 2 (Advanced Patterns)
**Goal: Adopt spec-driven development**

1. **Days 6-8:** Spec-Driven Mindset
   - Read `spec-driven-development.md`
   - Convert 2-3 feature requests into specs
   - Practice Specify → Plan → Tasks → Implement

2. **Days 9-10:** Request Kiro Access
   - Apply for Kiro preview access
   - Set up Kiro IDE (if approved)
   - Review `kiro-guide.md`
   - Configure your first agent hooks

### Phase 3: Week 3+ (Mastery)
**Goal: Achieve 20-40% productivity boost**

1. **Create Personal Workflows**
   - Build custom agent hooks (Kiro)
   - Set up Q customizations for your team's patterns
   - Document your productivity wins

2. **Team Integration**
   - Share specs and hooks with team
   - Establish code review practices with AI
   - Contribute to team's AI best practices

## Quick Reference Guides

### Core Documents
- [Amazon Q Developer Guide](./amazon-q-guide.md) - Detailed Q features and best practices
- [Kiro IDE Guide](./kiro-guide.md) - Spec-driven development with Kiro
- [Spec-Driven Development](./spec-driven-development.md) - The new paradigm for AI coding
- [Prompt Engineering](./prompt-engineering.md) - Craft better prompts for better code
- [Tools Comparison](./tools-comparison.md) - Deep dive into Amazon tools vs alternatives

### Workflows
- [Feature Development Workflow](./workflows/feature-development.md)
- [Bug Fixing Workflow](./workflows/bug-fixing.md)
- [Code Review Workflow](./workflows/code-review.md)
- [Documentation Workflow](./workflows/documentation.md)

## Success Metrics

Track your productivity gains:
- Code acceptance rate (target: 40%+)
- Time saved on documentation (target: 50%+)
- Vulnerabilities caught pre-commit (measure improvement)
- Feature delivery velocity (compare sprint to sprint)

## Resources

### Official Documentation
- [AWS Amazon Q Developer](https://aws.amazon.com/q/developer/)
- [Kiro Official Site](https://kiro.dev/)
- [AWS Prescriptive Guidance](https://docs.aws.amazon.com/prescriptive-guidance/latest/best-practices-code-generation/)

### Community
- AWS re:Post - Q Developer discussions
- Internal Amazon wiki (check your team's resources)
- Kiro community blog

## FAQ

**Q: Should I use Amazon Q or Kiro?**
A: Use both! Q for daily coding and AWS tasks, Kiro for complex feature work requiring specs.

**Q: How does this compare to my Claude Code workflow?**
A: Very similar agentic experience, but Q knows AWS deeply and Kiro enforces better structure.

**Q: Can I use Claude Code at Amazon?**
A: Check your team's policies. Many teams use multiple tools where appropriate.

**Q: What about Cline and Roo Code?**
A: These are NOT Amazon tools - they're open-source community projects. Kiro is Amazon's IDE.

## Next Steps

1. Read `amazon-q-guide.md` thoroughly
2. Install Amazon Q Developer extension
3. Complete your first agentic task
4. Review `spec-driven-development.md` before your first feature
5. Join your team's AI tooling channel

**Let's ship great code faster! 🚀**
