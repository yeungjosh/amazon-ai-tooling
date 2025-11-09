# Code Review Checklist

Please perform a comprehensive code review of the selected code or current file.

## Functionality
- [ ] **Correctness**: Does the code correctly implement the requirements?
- [ ] **Edge cases**: Are edge cases and boundary conditions handled?
- [ ] **Error handling**: Is error handling comprehensive and appropriate?
- [ ] **Logic**: Is the business logic sound and free of bugs?

## Code Quality
- [ ] **Readability**: Is the code easy to read and understand?
- [ ] **Naming**: Are variables, functions, and classes named clearly and descriptively?
- [ ] **DRY principle**: Is there any code duplication that should be extracted?
- [ ] **Abstraction**: Are abstractions appropriate and not over-engineered?
- [ ] **Complexity**: Is the cyclomatic complexity reasonable? (Max 10 per function)
- [ ] **Comments**: Are complex sections explained? Are comments up-to-date?
- [ ] **Magic numbers**: Are magic numbers replaced with named constants?

## Testing
- [ ] **Unit tests**: Are there unit tests covering main paths?
- [ ] **Edge case tests**: Are edge cases tested?
- [ ] **Mocking**: Are external dependencies mocked appropriately?
- [ ] **Test quality**: Are tests readable, maintainable, and valuable?
- [ ] **Coverage**: Does this maintain or improve code coverage?

## Security
- [ ] **Input validation**: Is all user input validated and sanitized?
- [ ] **SQL injection**: Are parameterized queries used (no string concatenation)?
- [ ] **XSS prevention**: Is output properly escaped in templates?
- [ ] **Authentication**: Is authentication required where appropriate?
- [ ] **Authorization**: Are authorization checks in place (RBAC)?
- [ ] **Secrets**: Are there any hardcoded secrets, API keys, or passwords?
- [ ] **Sensitive data**: Is sensitive data (PII, credentials) logged or exposed?
- [ ] **Dependencies**: Are dependencies up-to-date and free of known vulnerabilities?

## Performance
- [ ] **N+1 queries**: Are there any N+1 database query issues?
- [ ] **Indexing**: Are appropriate database indexes in place?
- [ ] **Caching**: Should caching be used to improve performance?
- [ ] **Async operations**: Are async patterns used where beneficial (I/O operations)?
- [ ] **Resource cleanup**: Are resources (connections, files) properly closed?
- [ ] **Pagination**: Are large result sets paginated?

## AWS Best Practices (if applicable)
- [ ] **IAM permissions**: Does this follow least privilege principle?
- [ ] **Resource tagging**: Are AWS resources properly tagged?
- [ ] **Cost optimization**: Is the most cost-effective AWS service/tier used?
- [ ] **Logging**: Are appropriate logs sent to CloudWatch?
- [ ] **Metrics**: Are custom CloudWatch metrics emitted for KPIs?
- [ ] **Error handling**: Are AWS API errors handled gracefully (retries, exponential backoff)?
- [ ] **Service limits**: Are AWS service limits considered (rate limits, quotas)?

## Maintainability
- [ ] **Documentation**: Is the code self-documenting or well-commented?
- [ ] **Type hints**: Are type hints used (Python) or strong typing (TypeScript/Java)?
- [ ] **API documentation**: Are public APIs documented (docstrings, JSDoc)?
- [ ] **Configuration**: Are configuration values externalized (not hardcoded)?
- [ ] **Logging**: Is there appropriate logging for debugging and monitoring?
- [ ] **Error messages**: Are error messages clear and actionable?

## Architecture & Design
- [ ] **SOLID principles**: Does this follow SOLID principles where applicable?
- [ ] **Separation of concerns**: Are concerns properly separated (API, business logic, data access)?
- [ ] **Dependency injection**: Are dependencies injected (not hardcoded)?
- [ ] **Testability**: Is the code easily testable?
- [ ] **Scalability**: Will this scale under increased load?

## Specific Feedback

Please provide specific, actionable feedback with:
1. **Location**: File and line numbers
2. **Issue description**: What's wrong or could be improved
3. **Code example**: Show the improved version
4. **Rationale**: Explain why the change is beneficial
5. **Severity**: Critical / Important / Suggestion

Format:
```
📍 file.py:42
🔴 Critical: SQL injection vulnerability
❌ Current code concatenates user input into SQL query
✅ Suggested fix:
[code example]
💡 Rationale: Prevents SQL injection attacks
```
