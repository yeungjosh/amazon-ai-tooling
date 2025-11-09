# Feature Development Workflow

A step-by-step guide for developing new features using Amazon Q Developer and Kiro.

## When to Use Each Tool

| Complexity | Scope | Tool | Reason |
|------------|-------|------|--------|
| Low | 1-2 files | Amazon Q | Fast, conversational |
| Medium | 3-5 files | Amazon Q or Kiro | Q for speed, Kiro for structure |
| High | 6+ files | Kiro | Spec-driven, safer |
| Critical | Any | Kiro | Need governance, docs |

## Amazon Q Workflow (Simple Features)

### Step 1: Understand the Requirement

```
# In Amazon Q Chat
@workspace Explain how [related feature] currently works.

Example:
@workspace Explain how user authentication currently works in this project.
```

**Goal:** Understand existing patterns before adding new code.

### Step 2: Plan the Implementation

```
# In Amazon Q Chat
I need to implement [feature description].

Context:
- Current state: [what exists]
- Requirements: [what's needed]
- Tech stack: [versions]

Please:
1. Analyze the existing codebase
2. Propose an implementation approach
3. Identify files that need changes
4. List any new files needed
5. Highlight potential issues

Example:
I need to add password reset functionality to our FastAPI app.

Context:
- We have user registration and login (src/auth/)
- We use JWT tokens for authentication
- Email service configured (AWS SES in src/services/email_service.py)

Please provide an implementation plan.
```

**Review the plan carefully before proceeding.**

### Step 3: Implement

**Option A: Autonomous Implementation**
```
# After reviewing the plan
Implement this feature autonomously:
1. Create/modify files as needed
2. Follow existing code patterns
3. Include error handling
4. Add type hints and docstrings
5. Run tests after each change

Stop if tests fail and explain the issue.
```

**Option B: Step-by-Step**
```
Let's implement this step by step. Wait for my approval after each step.

Step 1: Create the password reset endpoint
[After Q generates code, review and approve]

Step 2: Add token generation logic
[Review and approve]

... continue for each step
```

### Step 4: Security Scan

```
# Right-click on new/modified files
→ "Scan with Amazon Q"

OR

# In Q Chat
Scan the files I just modified for security vulnerabilities:
- [list files]

Focus on:
- Input validation
- Token security
- SQL injection
- Exposed credentials
```

### Step 5: Test

```
# In Q Chat
Generate comprehensive tests for the password reset feature.

Include:
- Unit tests for services
- Integration tests for API endpoints
- Edge cases (expired tokens, invalid emails, etc.)

Use our testing framework: pytest with fixtures from tests/conftest.py
```

### Step 6: Document

```
# In Q Chat
Generate documentation for the password reset feature:

1. Update API documentation (OpenAPI spec)
2. Add function docstrings (Google style)
3. Update README with new endpoint
4. Add architecture notes if significant design decisions
```

### Step 7: Review and Commit

**Self-review checklist:**
- [ ] Code follows project patterns
- [ ] Tests pass
- [ ] Security scan clean
- [ ] Documentation updated
- [ ] No hardcoded values
- [ ] Error handling included

**Commit:**
```bash
git add .
git commit -m "feat: add password reset functionality

- Add POST /auth/reset-password endpoint
- Implement reset token generation (1-hour expiry)
- Add email notification via SES
- Include rate limiting (3 attempts per hour)
- Add comprehensive tests (90% coverage)"
```

## Kiro Workflow (Complex Features)

### Step 1: Write the Specification

Create `specs/feature-name.md` using the spec template.

**Example:** `specs/payment-processing.md`

```markdown
# Payment Processing Feature

## Overview
Implement payment processing for orders using Stripe integration.

## Functional Requirements

### FR-1: Create Payment Intent
**Input:**
- order_id: UUID
- payment_method_id: String (Stripe payment method)

**Process:**
1. Fetch order details from database
2. Calculate total amount (including tax)
3. Create Stripe payment intent
4. Store payment intent ID with order

**Output:**
- HTTP 200: { payment_intent_id, client_secret, amount }
- HTTP 404: Order not found
- HTTP 400: Order already paid

**Error Handling:**
- Stripe API failure → Retry 3 times with exponential backoff
- Network timeout → Return 503 Service Unavailable
- Invalid payment method → Return 400 with clear message

### FR-2: Confirm Payment
[... continue with detailed requirements]

## Non-Functional Requirements

### NFR-1: Security
- All Stripe communication over HTTPS
- API keys in environment variables (never hardcoded)
- Validate webhook signatures
- PCI compliance (never store card details)

### NFR-2: Performance
- Payment intent creation: < 500ms p95
- Webhook processing: < 200ms
- Idempotent operations (safe to retry)

### NFR-3: Reliability
- Handle Stripe downtime gracefully
- Webhook retry with exponential backoff
- Dead letter queue for failed payments
- Alert on payment failures > 5%

## Technical Constraints
- Python 3.12
- FastAPI 0.110+
- Stripe SDK 8.x
- PostgreSQL for order storage
- Redis for idempotency keys

## Architecture
[... detailed architecture]

## Testing Requirements
[... comprehensive test cases]

## Success Criteria
- [ ] All functional requirements implemented
- [ ] Payment success rate > 99%
- [ ] Tests pass with 85%+ coverage
- [ ] Security scan clean
- [ ] Stripe webhooks properly verified
- [ ] Load tested (100 concurrent payments)
```

**Commit the spec:**
```bash
git add specs/payment-processing.md
git commit -m "docs: add payment processing specification"
```

### Step 2: Review Spec (Team)

- Share spec with team for review
- Get feedback on requirements
- Adjust before implementation
- Final approval from tech lead

### Step 3: Generate Plan in Kiro

```
# In Kiro IDE
Cmd/Ctrl + Shift + P → "Kiro: Generate Plan from Spec"
Select: specs/payment-processing.md
```

Kiro generates: `plans/payment-processing-plan.md`

**Review the plan:**
- File organization makes sense?
- Dependencies correct?
- Phases logical?
- Any missing considerations?

**Edit if needed:** You can manually edit the plan file.

### Step 4: Approve Task Breakdown

```
# In Kiro
View → Tasks Panel
```

Review tasks:
- Each task scoped appropriately?
- Dependencies correct?
- Estimated time reasonable?

**Modify tasks if needed.**

### Step 5: Implement (Guided or Autopilot)

**Guided Mode (recommended for first time):**
```
# In Kiro
Cmd/Ctrl + Shift + P → "Kiro: Implement Plan (Guided)"
```

Kiro implements each task and waits for approval.

**Autopilot Mode (for trusted specs):**
```
# In Kiro
Cmd/Ctrl + Shift + P → "Kiro: Implement Plan (Autopilot)"
```

Kiro implements all tasks, only pausing for configured approvals.

### Step 6: Agent Hooks (Automatic)

If you have hooks configured, they run automatically:

**After file save:**
- Auto-documentation hook adds docstrings
- Auto-test hook generates tests
- Auto-optimize hook improves performance

**Before commit:**
- Pre-commit review hook checks for issues
- Security scan hook validates no vulnerabilities

**Review hook results and approve/modify as needed.**

### Step 7: Verify Against Spec

Go through spec success criteria:

```
## Success Criteria
✓ All functional requirements implemented
✓ Payment success rate > 99% (load tested)
✓ Tests pass with 85%+ coverage (87% achieved)
✓ Security scan clean
✓ Stripe webhooks properly verified
✓ Load tested (100 concurrent payments - passed)
```

### Step 8: Commit

```bash
git add .
git commit -m "feat: implement payment processing with Stripe

Implements specs/payment-processing.md

- Add payment intent creation endpoint
- Add payment confirmation endpoint
- Implement Stripe webhook handling
- Add idempotency for retries
- Include comprehensive error handling
- 87% test coverage
- Load tested for 100 concurrent payments

Closes #123"
```

## Hybrid Workflow (Best of Both)

For medium-complexity features:

### Step 1: Quick Spec with Amazon Q

```
# In Amazon Q
Generate a specification for [feature] following this template:
[paste spec template]

Feature: [description]
Requirements: [bullet points]
Constraints: [tech stack]
```

**Review and refine the generated spec.**

### Step 2: Implement with Amazon Q

```
# In Amazon Q
@workspace Implement the following spec:

[paste spec]

Approach:
1. Generate an implementation plan
2. Implement step-by-step
3. Run tests after each step
4. Generate final documentation
```

### Step 3: Security and Quality with Kiro

- Move implementation to Kiro
- Set up agent hooks for quality gates
- Run pre-commit review
- Ensure consistency with codebase

## Common Patterns

### Pattern 1: Iterative Refinement

**For uncertain requirements:**

```
# Amazon Q - Round 1
Implement basic [feature] with core functionality only.

# Test and gather feedback

# Amazon Q - Round 2
Enhance [feature] with:
- [enhancement 1]
- [enhancement 2]

# Refine based on usage

# Kiro - Final
Write comprehensive spec based on current implementation.
Use Kiro to refactor for production quality.
```

### Pattern 2: Research Then Implement

**For unfamiliar domains:**

```
# Amazon Q - Research
@workspace How do we currently handle [similar functionality]?
What are the patterns and best practices in our codebase?

# External research
[Search docs, ask team, review examples]

# Write spec
[Create comprehensive spec based on research]

# Kiro - Implement
[Use Kiro for structured implementation]
```

### Pattern 3: Prototype Then Productionize

**For innovative features:**

```
# Amazon Q - Quick prototype
Build a quick prototype of [feature] to validate the approach.
Don't worry about perfection, focus on proving the concept.

# Validate prototype
[Test, demo, gather feedback]

# Kiro - Production version
[Write spec based on validated prototype]
[Implement production-ready version with Kiro]
```

## Anti-Patterns to Avoid

### ❌ Don't: Skip the Planning Phase

**Bad:**
```
# Immediately start coding without understanding existing patterns
"Add password reset feature" → [generates code]
```

**Good:**
```
@workspace Explain current auth patterns
[Review response]
[Create plan or spec]
[Then implement]
```

### ❌ Don't: Ignore Security Scans

**Bad:**
```
[Implement feature]
[Commit without security scan]
```

**Good:**
```
[Implement feature]
[Run security scan]
[Fix vulnerabilities]
[Commit]
```

### ❌ Don't: Over-Automate Complex Features

**Bad:**
```
# In Kiro Autopilot
[Run entire complex feature implementation without review]
```

**Good:**
```
# In Kiro Guided Mode
[Review each step]
[Approve incrementally]
[Understand what's being built]
```

## Troubleshooting

### AI generated incorrect code

**Cause:** Insufficient context or vague requirements

**Fix:**
1. Use @workspace to provide codebase context
2. Include specific examples
3. Reference existing patterns explicitly
4. Be more precise in requirements

### Tests are failing

**Cause:** Generated code doesn't match test expectations

**Fix:**
```
# In Amazon Q or Kiro
The tests are failing with this error:
[paste error]

Please:
1. Analyze the root cause
2. Fix the implementation
3. Ensure all tests pass
4. Explain what was wrong
```

### Code doesn't follow project patterns

**Cause:** AI doesn't know your conventions

**Fix (Amazon Q):**
- Use @workspace more extensively
- Provide examples of correct patterns
- Set up customizations (Pro tier)

**Fix (Kiro):**
- Include pattern requirements in spec
- Reference example files explicitly
- Configure governance rules

## Metrics to Track

For each feature, track:

**Development Time:**
- Time to spec/plan: [X hours]
- Time to implement: [Y hours]
- Time to test: [Z hours]
- Total: [X+Y+Z hours]

**Quality Metrics:**
- Code acceptance rate: [% of AI code kept as-is]
- Test coverage: [%]
- Security issues: [count]
- Code review iterations: [count]

**Productivity Gains:**
- Compare with previous manual implementation
- Target: 20-40% time savings

## Next Steps

- [ ] Choose a small feature to practice (1-2 files)
- [ ] Follow Amazon Q workflow end-to-end
- [ ] Request Kiro access for future complex features
- [ ] Track your productivity gains
- [ ] Refine your personal workflow based on what works

---

**Remember:** Start simple, build confidence, then tackle more complex features. The AI tools are here to amplify your skills, not replace your judgment.
