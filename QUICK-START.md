# Quick Start Guide: Amazon AI Tools for SDE2

Get up and running with Amazon's AI tools in 30 minutes.

## Day 1: First Steps (30 minutes)

### Install Amazon Q Developer (10 minutes)

**VS Code:**
```bash
# Open VS Code
# Press Cmd/Ctrl+Shift+X (Extensions)
# Search "Amazon Q"
# Click Install on "Amazon Q Developer"

# OR via command line:
code --install-extension AmazonWebServices.amazon-q-vscode
```

**JetBrains (IntelliJ, PyCharm, etc.):**
1. Open IDE → Preferences/Settings
2. Plugins → Marketplace
3. Search "Amazon Q"
4. Install and restart

**Sign In:**
1. Click Amazon Q icon in your IDE
2. Choose "Sign in with AWS Builder ID" (free tier)
3. Create AWS Builder ID (if you don't have one)
4. Complete authentication in browser

### Your First Task (20 minutes)

**Try inline suggestions:**

1. Create a new Python file: `test_q.py`
2. Type a comment:
```python
# Function to validate email addresses using regex
```
3. Press Enter and wait
4. Amazon Q will suggest implementation
5. Press Tab to accept

**Try the chat:**

1. Open Amazon Q chat (icon in sidebar)
2. Ask:
```
@workspace What languages and frameworks does this project use?
```
3. Review the response
4. Ask follow-up questions

**Congrats! You're using Amazon Q! 🎉**

## Day 2: Real Work (2 hours)

### Complete a Small Task

Pick a simple feature or bug fix from your backlog (1-2 files max).

**Example: Add input validation to existing endpoint**

```
# In Amazon Q Chat
@workspace Review the user registration endpoint in [file path].

I need to add validation for:
- Email format (RFC 5322)
- Password strength (min 12 chars, uppercase, lowercase, number, special)
- Username (alphanumeric, 3-20 chars)

Please:
1. Show me the current code
2. Identify where validation should be added
3. Implement the validation
4. Add appropriate error messages
5. Include type hints
```

**Review the generated code:**
- Does it make sense?
- Does it follow your project's patterns?
- Are there edge cases missing?

**Modify if needed:**
```
The email validation looks good, but please use our existing
EmailValidator class from src/utils/validators.py instead of
implementing a new one.
```

**Test it:**
```
Generate pytest tests for this validation logic.
Include test cases for:
- Valid inputs
- Invalid email formats
- Weak passwords
- Edge cases (empty strings, None, very long inputs)
```

**Run security scan:**
```
Right-click on modified file → "Scan with Amazon Q"
```

**Commit:**
```bash
git add .
git commit -m "feat: add input validation to user registration

- Add email format validation (RFC 5322)
- Add password strength requirements
- Add username validation
- Include comprehensive error messages
- Add pytest tests with 95% coverage"
```

## Day 3: Go Deeper (2 hours)

### Use @workspace Effectively

**Explore your codebase:**

```
# Understanding
@workspace How does authentication work in this project?
@workspace What's the pattern for error handling?
@workspace Where are database models defined?

# Finding things
@workspace Find all API endpoints related to user management
@workspace Where do we log errors?
@workspace What external services does this project integrate with?

# Planning
@workspace What files would I need to modify to add [feature]?
@workspace How should I structure a new [module] to match existing patterns?
```

### Try Agentic Features

**Feature implementation:**

```
# In Amazon Q Chat
Implement a new API endpoint: GET /api/users/{id}/orders

Requirements:
- Fetch user's orders from database
- Include order items and total
- Paginate results (20 per page)
- Require authentication
- Return 404 if user not found
- Follow the pattern of existing endpoints in src/api/routes/

Implementation plan:
1. Create new route in src/api/routes/orders.py
2. Add service function in src/services/order_service.py
3. Add Pydantic models for response
4. Include error handling
5. Add tests

Please implement this step by step.
```

**Amazon Q will:**
1. Analyze your codebase
2. Generate implementation plan
3. Create/modify files
4. Show you diffs for review
5. Run tests (if configured)

### Run Your First Security Scan

**On a critical file:**

```
Right-click on authentication/payment/user-data file
→ "Scan with Amazon Q"

Review findings:
- High/Critical: Fix immediately
- Medium: Evaluate and fix
- Low: Consider fixing
```

## Day 4-5: Advanced Features (4 hours)

### Optimize Code

**Find a slow function in your codebase:**

```
# In Amazon Q Chat
Optimize this function to improve performance:

[paste function]

Current performance: ~800ms for 1000 records
Target: < 200ms

Approach:
- Identify bottlenecks
- Propose optimizations
- Implement the best solution
- Explain what you changed and why

Constraints:
- Must maintain current behavior (tests must still pass)
- Cannot change function signature
- Available resources: Redis cache, database indexes
```

### Generate Documentation

```
# In Amazon Q Chat
@workspace Generate comprehensive documentation for the authentication module.

Include:
1. Overview of how authentication works
2. Flow diagram (PlantUML)
3. API endpoints with examples
4. Security considerations
5. How to add a new auth method

Format as Markdown suitable for our wiki.
```

### Create Tests for Legacy Code

```
# In Amazon Q Chat
@workspace This module [path] has no tests.

Generate comprehensive test suite:
- Unit tests for all functions
- Integration tests for API endpoints
- Mock external dependencies
- Use pytest with fixtures from tests/conftest.py
- Aim for 80%+ coverage

Start with the most critical functions first.
```

## Week 2: Kiro (If Approved)

### Request Access

1. Visit https://kiro.dev
2. Sign up with your Amazon email
3. Wait for approval (typically 1-2 weeks)

### Write Your First Spec

**Pick a medium-complexity feature (3-5 files)**

Create `specs/my-feature.md`:
```markdown
# [Feature Name]

## Overview
[What and why]

## Functional Requirements
### FR-1: [Requirement]
**Input:** [details]
**Process:** [steps]
**Output:** [details]
**Error Handling:** [cases]

## Non-Functional Requirements
### NFR-1: Performance
[targets]

### NFR-2: Security
[requirements]

## Technical Constraints
- Python 3.12
- FastAPI 0.110+
- [other constraints]

## Success Criteria
- [ ] [criterion 1]
- [ ] [criterion 2]
```

**Use the full template from** `spec-driven-development.md`

### Generate and Review Plan

```
# In Kiro
Cmd/Ctrl + Shift + P → "Kiro: Generate Plan from Spec"
```

Review the plan, make edits if needed.

### Implement in Guided Mode

```
# In Kiro
Cmd/Ctrl + Shift + P → "Kiro: Implement Plan (Guided)"
```

Review each step, approve or modify.

### Set Up Agent Hooks

**Start with auto-documentation:**

```yaml
# .kiro/hooks/auto-doc.yaml
name: "Auto-Documentation"
trigger:
  event: "file.save"
  filter:
    - "src/**/*.py"

action:
  type: "agent"
  prompt: |
    Add Google-style docstrings to all functions/classes in {file_path}
    that are missing documentation.

  require_approval: true
```

## Keyboard Shortcuts to Memorize

**VS Code + Amazon Q:**
- `Ctrl/Cmd+Shift+Q`: Open Q Chat
- `Tab`: Accept suggestion
- `Esc`: Reject suggestion
- `Ctrl/Cmd+I`: Inline chat
- `Ctrl/Cmd+Shift+O`: Optimize code

**Kiro:**
- `Cmd/Ctrl+Shift+P`: Command palette
- `Cmd/Ctrl+K`: Kiro actions menu

## Common Issues

### Q suggestions seem off

**Fix:**
- Use `@workspace` to give more context
- Be more specific in your prompts
- Include tech stack versions

### Slow first @workspace query

**Expected:**
- First query indexes your codebase (30-60s for large projects)
- Subsequent queries are fast

### Security scan false positives

**Fix:**
- Review the finding
- If truly false positive, mark as such
- Q learns from your feedback

## Quick Wins to Try

### 1. Generate Boilerplate (2 min)
```
Generate a FastAPI endpoint boilerplate with:
- Request/response Pydantic models
- Error handling
- Type hints
- Docstrings
```

### 2. Explain Complex Code (1 min)
```
@workspace Explain what this function does: [file:line]
Provide step-by-step explanation.
```

### 3. Refactor Legacy Code (5 min)
```
Refactor this function to improve readability:
[paste code]

Requirements:
- Better variable names
- Reduce complexity
- Add type hints
- Same behavior (tests must pass)
```

### 4. Fix a Bug (5 min)
```
I'm getting this error: [paste error]

In file: [path]
Function: [name]

Please:
1. Identify root cause
2. Fix the bug
3. Explain the fix
4. Suggest a test to prevent regression
```

### 5. Write Tests (5 min)
```
Generate pytest tests for:
[paste function]

Include:
- Happy path
- Edge cases
- Error cases
- Use fixtures from tests/conftest.py
```

## Measuring Success

After 2 weeks, compare:

**Before (with Claude Code):**
- Time to complete typical feature: [X hours]
- Lines of code per day: [Y]
- Time on bug fixes: [Z hours/week]

**After (with Amazon Q + Kiro):**
- Time to complete typical feature: [X hours] (expect 20-40% reduction)
- Lines of code per day: [Y] (expect increase)
- Time on bug fixes: [Z hours/week] (expect 30% reduction)

**Track:**
- Code acceptance rate from Q
- Security issues found pre-commit
- Documentation completeness
- Test coverage

## Next Steps

- [ ] Complete Day 1 (install and first task)
- [ ] Complete Day 2 (real work)
- [ ] Complete Day 3-5 (advanced features)
- [ ] Request Kiro access
- [ ] Read full guides:
  - [ ] `amazon-q-guide.md` (comprehensive Q features)
  - [ ] `spec-driven-development.md` (understand the paradigm)
  - [ ] `prompt-engineering.md` (write better prompts)
  - [ ] `kiro-guide.md` (when Kiro access granted)

## Getting Help

**Internal Resources:**
- Amazon internal wiki (search "Amazon Q Developer")
- Your team's AI tools channel
- AWS re:Post (https://repost.aws/tags/amazon-q-developer)

**Official Resources:**
- AWS Amazon Q Docs: https://aws.amazon.com/q/developer/
- Kiro Docs: https://kiro.dev/docs

**Community:**
- AWS DevOps Blog (case studies, best practices)
- Internal Amazon Q users group (ask your team)

## Quick Reference Card

Print this or keep it handy:

```
┌─────────────────────────────────────────────────┐
│        AMAZON Q DEVELOPER QUICK REFERENCE        │
├─────────────────────────────────────────────────┤
│ INLINE SUGGESTIONS                              │
│  • Start typing → AI suggests                   │
│  • Tab to accept, Esc to reject                 │
│                                                  │
│ CHAT COMMANDS                                   │
│  • @workspace [question] → Codebase-aware       │
│  • Implement [feature] → Agentic coding         │
│  • Optimize [code] → Performance improvements   │
│  • Generate tests for [function] → Testing      │
│                                                  │
│ SECURITY                                        │
│  • Right-click → "Scan with Amazon Q"           │
│  • Fix vulnerabilities before commit            │
│                                                  │
│ BEST PRACTICES                                  │
│  • Be specific in prompts                       │
│  • Include tech stack versions                  │
│  • Use @workspace for context                   │
│  • Review all generated code                    │
│  • Run security scans on critical code          │
│                                                  │
│ FREE TIER: 50 agentic chats/month               │
│ PRO TIER: $19/month (unlimited)                 │
└─────────────────────────────────────────────────┘
```

---

**You're ready to supercharge your productivity! Start with Day 1 and build from there. 🚀**
