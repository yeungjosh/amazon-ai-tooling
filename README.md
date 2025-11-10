# Amazon AI Tooling Supercharge Guide

A comprehensive guide for SDE2/ML engineers joining Amazon to maximize productivity with Amazon's AI tools including Amazon Q Developer, Kiro, and related technologies.

## Overview

This repository provides battle-tested strategies, configurations, and workflows to help you:
- **Onboard quickly** to Amazon's AI development ecosystem
- **Supercharge productivity** using Amazon Q Developer, Kiro, and complementary tools
- **Adopt spec-driven development** practices for maintainable, production-ready code
- **Bridge your experience** from tools like Claude Code to Amazon's internal AI tooling

## Repository Structure

```
amazon-ai-tools/
├── README.md                          # This file
├── docs/
│   ├── 01-tools-comparison.md         # Detailed comparison of AI coding tools
│   ├── 02-amazon-q-guide.md           # Amazon Q Developer comprehensive guide
│   ├── 03-kiro-spec-driven.md         # Kiro and spec-driven development
│   ├── 04-onboarding-playbook.md      # SDE2/ML engineer onboarding guide
│   └── 05-spec-kit-vs-kiro.md         # GitHub Spec Kit vs Kiro deep comparison
├── amazon-q/
│   ├── context/                       # Amazon Q context files
│   │   ├── project-introduction.md    # Project context template
│   │   └── examples/                  # Example context files
│   ├── prompts/                       # Reusable prompt library
│   │   ├── code-review.md
│   │   ├── refactoring.md
│   │   ├── testing.md
│   │   └── documentation.md
│   └── rules/                         # Amazon Q rules (.rule.md files)
│       ├── coding-standards.rule.md
│       ├── security-checks.rule.md
│       └── aws-best-practices.rule.md
├── kiro/
│   ├── templates/                     # Kiro spec-driven templates
│   │   ├── requirements-template.md
│   │   ├── design-template.md
│   │   └── tasks-template.md
│   └── examples/                      # Example Kiro projects
│       └── sample-microservice/
├── workflows/
│   ├── feature-development.md         # End-to-end feature development
│   ├── code-review-process.md         # AI-assisted code review
│   ├── testing-strategy.md            # Testing with AI assistance
│   └── debugging-techniques.md        # Advanced debugging workflows
└── integrations/
    ├── cline-setup.md                 # Cline + AWS MCP servers
    ├── claude-to-amazon-q.md          # Transitioning from Claude Code
    └── vscode-extensions.md           # Essential VS Code extensions
```

## Quick Start

### For New Amazon Engineers

1. **Start here**: Read [Onboarding Playbook](docs/04-onboarding-playbook.md)
2. **Set up Amazon Q**: Follow [Amazon Q Guide](docs/02-amazon-q-guide.md)
3. **Learn Kiro**: Explore [Kiro Spec-Driven Development](docs/03-kiro-spec-driven.md)
4. **Configure your environment**: Use templates from `amazon-q/` directory

### For Engineers Coming from Claude Code

1. **Compare tools**: Read [Tools Comparison](docs/01-tools-comparison.md)
2. **Migration guide**: See [Claude to Amazon Q](integrations/claude-to-amazon-q.md)
3. **Leverage both**: Learn how to use Cline with AWS in [Cline Setup](integrations/cline-setup.md)

## Key Features

### Amazon Q Optimization
- **Context files** for project-aware AI assistance
- **Prompt library** with battle-tested prompts for common tasks
- **Rules engine** for enforcing coding standards and best practices
- **100K character context** for deep codebase understanding

### Kiro Spec-Driven Development
- **Requirements templates** for user stories and acceptance criteria
- **Design templates** for technical architecture documentation
- **Task breakdown** for trackable implementation
- **Vibe-to-viable** transformation for production-ready code

### Advanced Workflows
- **Multi-file refactoring** strategies
- **AI-assisted code review** checklists
- **Test-driven development** with AI pair programming
- **Documentation generation** automation

## Research Highlights

Based on comprehensive research (as of 2025):

### Amazon Q Developer
- **Highest code acceptance rates** in the industry (50% at National Australia Bank)
- **#1 on SWE-Bench** leaderboard for agentic capabilities
- **100K character context** window (expanded April 2025)
- **Multi-file solution** capability with inter-iteration memory
- **AWS-native expertise** for cloud development

### Kiro
- **Spec-driven approach** addressing "vibe coding chaos"
- **Claude-powered** VS Code fork for specification-first development
- **Structured outputs**: Requirements → Design → Tasks
- **Maintainability focus** with auto-updating documentation

### Comparison with Claude Code
- **Claude Code**: Terminal-based, whole-project awareness, excellent for cross-cutting changes
- **Amazon Q**: IDE-integrated, AWS-expert, highest acceptance rates, agentic multi-file editing
- **Kiro**: Spec-first workflow, documentation-centric, production-ready code generation

## Contributing

This is a living document. As you discover new workflows and optimizations:
1. Document what works
2. Share your configurations
3. Submit PRs with improvements

## Resources

- [Amazon Q Developer Documentation](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/)
- [Kiro Official Site](https://kiro.dev/)
- [AWS MCP Servers for Cline](https://cline.bot/blog/aws-cline-35-new-mcp-servers-to-manage-your-cloud-with-ai)
- [Claude Code Documentation](https://docs.claude.com/en/docs/claude-code)

## License

MIT License - Feel free to use and adapt for your team

---

**Built for engineers, by engineers** | Last updated: 2025-11-09
