# Prompt Engineering for AI Coding Assistants

Master the art of crafting prompts that generate production-ready code. This guide covers techniques for Amazon Q, Kiro, Claude Code, and other AI assistants.

## The Anatomy of a Great Prompt

```
[CONTEXT] + [TASK] + [CONSTRAINTS] + [OUTPUT FORMAT] = Great Prompt
```

### 1. Context

**What:** Background information the AI needs to understand your request

**Examples:**
```
❌ Bad (no context):
"Add error handling"

✅ Good:
"In our FastAPI application, we currently have no error handling for database
connection failures. This causes 500 errors to be returned to users without
proper logging. Our team's error handling pattern uses custom exceptions
defined in src/utils/errors.py."
```

**What to include:**
- Current state of the code
- Technology stack (languages, frameworks, versions)
- Existing patterns in your codebase
- Business context (why this matters)
- Team conventions

### 2. Task

**What:** Precise description of what you want the AI to do

**Examples:**
```
❌ Bad (vague):
"Make it better"
"Fix the bug"
"Optimize this"

✅ Good:
"Implement retry logic with exponential backoff for database connection failures"
"Fix the race condition in the order processing where inventory can go negative"
"Optimize the user query to reduce response time from 800ms to under 200ms"
```

**Pro tip:** Use action verbs:
- Implement, Create, Build (for new code)
- Refactor, Optimize, Improve (for existing code)
- Fix, Resolve, Debug (for bugs)
- Generate, Write, Draft (for docs/tests)

### 3. Constraints

**What:** Requirements and limitations the solution must respect

**Examples:**
```
✅ Constraints to include:

Technical:
- "Must work with Python 3.12"
- "Use only standard library, no external dependencies"
- "Maintain backward compatibility with API v2 clients"
- "Follow PEP 8 style guide"

Performance:
- "Response time < 200ms at p95"
- "Memory usage < 512MB"
- "Support 1000 concurrent users"

Security:
- "Input must be sanitized to prevent SQL injection"
- "Use parameterized queries only"
- "Never log sensitive data (PII, passwords, tokens)"

Business:
- "Must handle timezone differences (UTC storage, local display)"
- "Support European GDPR requirements"
- "Graceful degradation if third-party API is down"
```

### 4. Output Format

**What:** How you want the AI to deliver the result

**Examples:**
```
"Provide the solution as a complete function with type hints and docstrings"

"Generate a git commit message following conventional commits format"

"Return a code diff showing only the changes needed"

"Provide step-by-step explanation followed by the implementation"

"Create a table comparing the three approaches with pros/cons"
```

## The STAR Prompt Framework

**S**ituation + **T**ask + **A**pproach + **R**equirements

### Example: Feature Implementation

```
[SITUATION]
I'm working on a Python FastAPI application for an e-commerce platform.
Currently, we don't have any caching for product data, which causes
slow response times (800ms average) when users browse products.

Current stack:
- FastAPI 0.110.0
- PostgreSQL 16
- SQLAlchemy 2.0 (async)
- Redis 7.x available but not configured

[TASK]
Implement a Redis-based caching layer for product data to reduce
database load and improve response times to under 200ms.

[APPROACH]
I want to:
1. Create a cache service abstraction (easy to swap implementations)
2. Cache individual products by ID (TTL: 1 hour)
3. Invalidate cache when products are updated
4. Add cache hit/miss metrics for monitoring

[REQUIREMENTS]
- Follow async/await pattern (our app is fully async)
- Use our existing Redis connection (src/database/redis.py)
- Add type hints for all functions
- Include error handling (continue if Redis is down)
- Write unit tests with pytest
- Add logging for cache operations
- Follow our project structure (see src/services/ for examples)

Please implement this solution across the necessary files.
```

## Prompt Patterns

### Pattern 1: @workspace for Codebase Context

**Use for:** Questions about existing code, refactoring, maintaining consistency

```
✅ Examples:

"@workspace How does authentication work in this project?"

"@workspace What's the pattern for adding a new API endpoint?
Follow the same pattern to add a DELETE /api/users/{id} endpoint."

"@workspace Find all places where we log user actions.
I need to add logging for password changes following the same format."

"@workspace Refactor the payment processing module to match
our error handling pattern used in the order processing module."
```

**Why it works:** AI ingests your entire codebase and provides context-aware responses

### Pattern 2: Progressive Refinement

**Use for:** Complex tasks where you're not sure of the exact solution

```
# Step 1: Broad exploration
"@workspace What are the different ways to implement caching in our
FastAPI application? Compare in-memory, Redis, and CDN approaches."

# Step 2: Choose direction
"Let's go with Redis. What's the best Redis client for async Python?
Compare aioredis and redis-py with async support."

# Step 3: Specific implementation
"Using redis-py with async support, implement a caching decorator
for our FastAPI endpoints. It should:
- Cache based on request path and query params
- TTL configurable per endpoint
- Skip caching for authenticated requests
- Handle Redis connection failures gracefully"

# Step 4: Refine
"Great! Now add monitoring: track cache hit/miss rates using
our existing Prometheus metrics in src/monitoring/metrics.py"
```

### Pattern 3: Example-Driven

**Use for:** When you want a specific style or format

```
Here's how we currently handle user registration:

[paste existing code]

Please implement a similar flow for password reset with these changes:
- Send email with reset token instead of verification token
- Token expires in 1 hour (not 24 hours)
- Add rate limiting (max 3 reset attempts per hour)
- Otherwise follow the same structure and error handling
```

**Why it works:** AI learns from your examples and maintains consistency

### Pattern 4: Test-Driven

**Use for:** When you know the behavior you want

```
I need a function that validates email addresses.

Here are the test cases it should pass:

```python
def test_valid_emails():
    assert validate_email("user@example.com") == True
    assert validate_email("user.name+tag@example.co.uk") == True

def test_invalid_emails():
    assert validate_email("notanemail") == False
    assert validate_email("@example.com") == False
    assert validate_email("user@") == False
    assert validate_email("user@.com") == False

def test_edge_cases():
    assert validate_email("") == False
    assert validate_email(None) == False
    assert validate_email("a@b.c") == True  # shortest valid email
```

Please implement the validate_email function that passes all these tests.
Use Python 3.12, type hints, and standard library only (no regex).
```

### Pattern 5: Constrained Refactoring

**Use for:** Improving code without changing behavior

```
Refactor this function to improve readability and performance:

[paste existing code]

Requirements:
- DO NOT change the function signature (inputs/outputs must stay the same)
- DO NOT change the behavior (all existing tests must still pass)
- DO improve: variable names, reduce complexity, add type hints
- DO optimize: reduce database queries, improve time complexity

Explain what you changed and why.
```

## Advanced Techniques

### 1. Chain of Thought Prompting

Ask AI to explain its reasoning before generating code:

```
Before implementing, please:
1. Analyze the requirements and identify potential edge cases
2. Propose an algorithm/approach
3. Identify any performance or security concerns
4. Then provide the implementation

Task: Implement a rate limiting decorator for our API endpoints
that supports both per-user and per-IP limits, stored in Redis.
```

**Result:** More thoughtful, robust solutions

### 2. Role-Based Prompting

Frame the AI as a specialist:

```
You are a senior security engineer reviewing this authentication code.

[paste code]

Perform a security audit and identify:
1. Vulnerabilities (OWASP Top 10)
2. Missing input validation
3. Insecure defaults
4. Potential injection attacks

Provide specific fixes for each issue.
```

### 3. Comparative Prompting

Ask for multiple solutions to choose from:

```
Provide three different approaches to implement user sessions in our
FastAPI app, comparing:

Approach 1: JWT tokens (stateless)
Approach 2: Server-side sessions (Redis)
Approach 3: Hybrid (JWT + Redis for revocation)

For each, provide:
- Pros and cons
- Security implications
- Performance characteristics
- Code complexity
- When to use it

Then recommend the best approach for our use case:
- 10,000 active users
- Need ability to revoke sessions
- Mobile app + web app clients
- Scalability is important
```

### 4. Incremental Building

Break complex tasks into steps:

```
Let's build a background job system for our FastAPI app in steps:

Step 1: Define the job interface
- Create a BaseJob class that all jobs inherit from
- Include execute(), retry logic, and error handling
- Show me this first, then we'll continue

[After AI responds...]

Step 2: Implement job queue using Redis
- Store jobs as JSON in Redis lists
- Include job priority
- Show me the queue implementation

[Continue step by step...]
```

**Why it works:** Easier to review, catch issues early, maintain control

### 5. Error-Driven Refinement

Use errors to improve the solution:

```
Initial prompt:
"Create a function to parse CSV files and return a list of dicts"

[AI generates code]

Follow-up:
"I got this error when running it: [paste error]
Please fix the error and also handle these edge cases:
- Empty CSV files
- Malformed CSV (missing columns)
- Very large files (> 1GB)"

[AI refines solution]
```

## Domain-Specific Prompts

### AWS / Cloud

```
Create a Lambda function that:
- Triggers on S3 file upload
- Processes CSV files (parse and validate)
- Stores results in DynamoDB
- Sends SNS notification on completion

Stack:
- Python 3.12 runtime
- boto3 for AWS SDK
- Use AWS Lambda Powertools for logging and tracing
- Handle large files with streaming (don't load entire file into memory)
- Include SAM template for deployment

Error handling:
- Retry with exponential backoff for DynamoDB throttling
- Dead letter queue for failed processing
- CloudWatch alarms for error rate > 1%
```

### Data Engineering

```
Optimize this Pandas data processing pipeline:

[paste code]

Current performance: 45 seconds for 1M rows

Goal: Under 10 seconds

Constraints:
- Must run on single machine (16GB RAM, 8 cores)
- Output must be identical (validation tests provided)

Approaches to consider:
- Vectorization (eliminate loops)
- Polars instead of Pandas
- Dask for parallel processing
- Optimize data types (reduce memory)

Show me the optimized version with benchmark comparisons.
```

### API Development

```
Design and implement a REST API for a todo application.

Requirements:
- FastAPI framework (latest version)
- PostgreSQL database
- JWT authentication
- CRUD operations for todos (create, read, update, delete, list)
- Filtering (by status, due date)
- Pagination (page-based)

Provide:
1. API schema (OpenAPI)
2. Database models (SQLAlchemy)
3. Pydantic models for request/response
4. Router implementation
5. Database migrations
6. Example requests/responses

Follow REST best practices:
- Proper HTTP methods and status codes
- Consistent error response format
- Idempotent PUT/DELETE
- HATEOAS links in responses
```

### Testing

```
Generate comprehensive tests for this function:

[paste function]

Include:
- Unit tests for all code paths
- Edge cases (empty inputs, None, large values)
- Error cases (exceptions, validation failures)
- Integration test with mocked dependencies

Use:
- pytest framework
- pytest fixtures for common setup
- pytest.mark.parametrize for multiple test cases
- unittest.mock for mocking

Target: 100% code coverage
```

## Anti-Patterns (What NOT to Do)

### ❌ Anti-Pattern 1: The Lazy Prompt

```
"fix this"
"make it work"
"add comments"
```

**Why bad:** AI has no context or guidance

**Fix:**
```
"Fix the race condition in the order processing that causes inventory
to go negative when two users checkout simultaneously. Use database
transactions with row-level locking (PostgreSQL)."
```

### ❌ Anti-Pattern 2: The Novel

```
[5 paragraphs of background story]
[3 paragraphs about team dynamics]
[2 paragraphs about business strategy]
[1 paragraph about what you actually want]
```

**Why bad:** Signal-to-noise ratio is too low

**Fix:** Be concise. Context is good, but keep it relevant and structured.

### ❌ Anti-Pattern 3: The Missing Constraints

```
"Build a user authentication system"
```

**Why bad:** AI will make assumptions (wrong language, wrong framework, insecure defaults)

**Fix:**
```
"Build a JWT-based user authentication system for our FastAPI application
using Python 3.12, PostgreSQL 16, and bcrypt for password hashing.
Must support refresh tokens and rate limiting."
```

### ❌ Anti-Pattern 4: The Trust Fall

```
[Accepts AI-generated code without reading it]
[Commits directly to production]
[Code breaks in production]
```

**Why bad:** AI makes mistakes. You're still responsible.

**Fix:** ALWAYS review, test, and understand AI-generated code.

### ❌ Anti-Pattern 5: The Kitchen Sink

```
"Build a complete e-commerce platform with user auth, product catalog,
shopping cart, payment processing, order management, admin dashboard,
email notifications, analytics, and mobile app support. Make it scalable,
secure, and production-ready."
```

**Why bad:** Too broad. AI will generate generic, incomplete code.

**Fix:** Break into smaller, focused tasks. Use spec-driven development for complex systems.

## Prompt Templates

### Template: Feature Request

```
# Feature: [Name]

## Context
- Project: [type and tech stack]
- Current state: [what exists now]
- Why needed: [business reason]

## Requirements
Implement [specific feature] that:
- [requirement 1]
- [requirement 2]
- [requirement 3]

## Technical Constraints
- Language: [X version]
- Framework: [Y version]
- Must integrate with: [existing systems]
- Must follow: [patterns/conventions]

## Success Criteria
- [ ] [specific outcome 1]
- [ ] [specific outcome 2]
- [ ] [tests pass]
```

### Template: Bug Fix

```
# Bug Report

## Current Behavior
[What happens now - be specific]

Steps to reproduce:
1. [Step 1]
2. [Step 2]
3. [Step 3]

## Expected Behavior
[What should happen]

## Error Message
```
[paste error/stack trace]
```

## Context
- File: [file path]
- Function: [function name]
- Environment: [Python 3.12, FastAPI 0.110, etc.]

## Please
1. Identify the root cause
2. Propose a fix
3. Explain why the fix works
4. Suggest tests to prevent regression
```

### Template: Code Review

```
Please review this code for:
1. Bugs and edge cases
2. Security vulnerabilities
3. Performance issues
4. Code quality (readability, maintainability)
5. Consistency with project patterns (@workspace)

Code:
```[language]
[paste code]
```

Context:
- Purpose: [what this code does]
- Stack: [tech stack]
- Concerns: [specific areas you're worried about]

Provide specific, actionable feedback.
```

### Template: Optimization

```
# Optimization Request

## Current Performance
- Metric: [response time, memory, CPU, etc.]
- Current value: [800ms, 2GB, 80%, etc.]
- Target value: [200ms, 512MB, 20%, etc.]

## Code to Optimize
```[language]
[paste code]
```

## Constraints
- Cannot change: [public API, database schema, etc.]
- Must maintain: [existing behavior, test compatibility]
- Available resources: [memory limit, CPU cores, etc.]

## Approaches to Consider
- [Algorithm optimization]
- [Caching strategy]
- [Database query optimization]
- [Parallel processing]

Please:
1. Profile the code (identify bottlenecks)
2. Propose optimizations with expected impact
3. Implement the best approach
4. Provide benchmark comparison
```

## Tool-Specific Tips

### Amazon Q Developer

**Leverage @workspace:**
```
@workspace [Your question/task]
```

**For AWS services:**
```
Generate a CloudFormation template for:
[detailed AWS infrastructure description]

Follow AWS Well-Architected Framework best practices.
```

**For security:**
```
Scan this code for security vulnerabilities:
[paste code]

Focus on:
- OWASP Top 10
- AWS-specific security issues
- Exposed credentials
- Input validation
```

### Kiro IDE

**Write specs, not prompts:**

Instead of:
```
"Build a user login system"
```

Create a spec:
```markdown
# specs/user-login.md

[Complete spec following the template]
```

Then let Kiro generate plan and implement.

### Claude Code

**Use conversation history:**
```
# Message 1
"Explain how authentication works in this codebase"

# Message 2 (after AI responds)
"Now add support for OAuth2 following the same patterns"
```

**Multi-step tasks:**
```
Let's build a feature in steps. After each step, wait for my approval
before continuing.

Step 1: Create database models for comments
Step 2: Create API endpoints
Step 3: Add tests
Step 4: Add documentation
```

## Measuring Prompt Quality

### Good prompts produce:
- ✅ Code that works on first try (90%+ of the time)
- ✅ Code that passes your tests
- ✅ Code that follows your conventions
- ✅ Code that handles edge cases
- ✅ Code that's secure and performant

### Bad prompts produce:
- ❌ Generic, boilerplate code
- ❌ Code that doesn't compile/run
- ❌ Code with security issues
- ❌ Code inconsistent with your codebase
- ❌ Code requiring extensive manual fixes

### Iteration Strategy

```
Round 1: Basic prompt → 60% correct
Round 2: Add context → 80% correct
Round 3: Add constraints → 95% correct
Round 4: Add examples → 99% correct
```

**Track your prompts:** Keep a log of prompts that work well. Build a library.

## Practice Exercises

### Exercise 1: Improve This Prompt

**Before:**
```
"add logging"
```

**Your improved version:**
```
[Write your improved prompt here]
```

<details>
<summary>Sample Answer</summary>

```
Add structured logging to the user authentication function in src/auth/service.py.

Requirements:
- Use Python's logging module
- Log at INFO level for successful logins
- Log at WARNING level for failed login attempts
- Log at ERROR level for exceptions
- Include: user_id, timestamp, IP address, user_agent
- Use JSON format for log messages (for log aggregation)
- Never log passwords or tokens

Follow the logging pattern used in src/orders/service.py

Example log output:
{"timestamp": "2025-01-15T10:30:00Z", "level": "INFO", "event": "user_login",
"user_id": "12345", "ip": "192.168.1.1", "user_agent": "Mozilla/5.0..."}
```
</details>

### Exercise 2: Write a Prompt for...

Write a complete prompt for implementing a rate-limiting middleware for a FastAPI application.

<details>
<summary>Sample Answer</summary>

```
Implement a rate-limiting middleware for our FastAPI application.

## Context
- FastAPI 0.110.0
- Redis 7.x available for distributed rate limiting
- Multiple application instances (horizontal scaling)
- Need to rate limit by both IP address and user ID

## Requirements

### Functional
- Limit: 100 requests per minute per IP
- Limit: 1000 requests per minute per authenticated user
- Return HTTP 429 (Too Many Requests) when limit exceeded
- Include Retry-After header indicating seconds until reset
- Exclude health check endpoint (/health) from rate limiting

### Technical
- Use Redis for distributed rate limiting (sliding window algorithm)
- Async implementation (our app is fully async)
- Graceful degradation if Redis is unavailable (allow requests through)
- Add middleware to FastAPI app with app.add_middleware()

### Monitoring
- Log rate limit violations
- Expose Prometheus metrics:
  - rate_limit_total (counter)
  - rate_limit_exceeded_total (counter)

## Implementation
Please provide:
1. Middleware class (src/middleware/rate_limit.py)
2. Redis client wrapper (src/database/redis_client.py)
3. Configuration (src/config.py additions)
4. Unit tests (tests/test_rate_limit.py)

## Success Criteria
- Rate limiting works across multiple app instances
- Does not impact performance (< 5ms overhead per request)
- Tests achieve 90%+ coverage
```
</details>

## Quick Reference

### Prompt Checklist

Before sending your prompt, check:
- [ ] Clear context provided
- [ ] Specific task described
- [ ] Constraints listed (tech stack, versions, patterns)
- [ ] Examples included (if applicable)
- [ ] Output format specified
- [ ] Success criteria defined
- [ ] Edge cases considered
- [ ] Security requirements mentioned

### Common Patterns Quick Reference

| Pattern | When to Use | Example |
|---------|-------------|---------|
| @workspace | Codebase-aware tasks | `@workspace How does X work?` |
| Example-driven | Maintain consistency | "Here's how we do X, do Y similarly" |
| Test-driven | Known behavior | "Pass these tests: [tests]" |
| Chain of thought | Complex logic | "First explain approach, then implement" |
| Incremental | Large tasks | "Step 1: [task]. Wait for approval." |
| Comparative | Exploring options | "Compare 3 approaches with pros/cons" |
| Role-based | Specialized review | "As a security expert, review this" |

## Resources

- [AWS Prescriptive Guidance: Best Practices for Code Generation](https://docs.aws.amazon.com/prescriptive-guidance/latest/best-practices-code-generation/)
- [Mastering Amazon Q Developer: Crafting Effective Prompts](https://aws.amazon.com/blogs/devops/mastering-amazon-q-developer-part-1-crafting-effective-prompts/)
- [Anthropic Prompt Engineering Guide](https://docs.anthropic.com/claude/docs/prompt-engineering)

---

**Remember:** Great prompts = great code. Invest time in crafting precise, well-structured prompts, and you'll get production-ready code with minimal iteration.
