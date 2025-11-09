# Project: [Your Service Name]

## Overview
[Brief 2-3 sentence description of what this service/project does and its role in the larger system]

## Tech Stack
- **Language**: [e.g., Python 3.11, Java 17, TypeScript 5.0]
- **Framework**: [e.g., FastAPI, Spring Boot, Express, React]
- **Database**: [e.g., PostgreSQL 14, DynamoDB, MongoDB]
- **Cache**: [e.g., Redis, ElastiCache, Memcached]
- **Message Queue**: [e.g., SQS, SNS, Kafka]
- **AWS Services**: [List primary AWS services - ECS, Lambda, S3, etc.]
- **Testing**: [e.g., pytest, JUnit, Jest]
- **Build/Deploy**: [e.g., Docker, Terraform, CloudFormation]

## Architecture Pattern
[e.g., Microservices, Event-Driven, CQRS, Serverless, etc.]

### High-Level Architecture
```
[Simple text diagram or description]
Example:
API Gateway → Lambda Functions → DynamoDB
                ↓
              SQS Queue → Processing Lambda → S3
```

## Key Design Patterns
- [e.g., Repository pattern for data access]
- [e.g., Dependency injection via framework]
- [e.g., Circuit breaker for external APIs]
- [e.g., Async/await for I/O operations]

## Project Structure
```
project-root/
├── src/
│   ├── api/         # API endpoints/routes
│   ├── services/    # Business logic
│   ├── models/      # Data models
│   ├── repositories/ # Data access layer
│   └── utils/       # Shared utilities
├── tests/
│   ├── unit/
│   └── integration/
├── infrastructure/  # IaC (Terraform/CloudFormation)
└── docs/
```

## Important Files & Directories
- **Entry point**: [e.g., `src/main.py`, `src/app.ts`]
- **Core business logic**: [e.g., `src/services/order_service.py`]
- **Data models**: [e.g., `src/models/`]
- **API routes**: [e.g., `src/api/routes/`]
- **Configuration**: [e.g., `src/config.py`, `.env.example`]
- **Tests**: [e.g., `tests/`]

## Team Conventions

### Code Style
- [e.g., Follow PEP 8 for Python, use Black formatter]
- [e.g., ESLint + Prettier for TypeScript]
- [e.g., Line length: 100 characters]

### Naming Conventions
- **Files**: [e.g., snake_case for Python, camelCase for TypeScript]
- **Classes**: [e.g., PascalCase]
- **Functions**: [e.g., snake_case for Python, camelCase for TypeScript]
- **Constants**: [e.g., UPPER_SNAKE_CASE]
- **Private members**: [e.g., _leading_underscore]

### Testing Requirements
- Minimum coverage: [e.g., 80%]
- Required tests: [e.g., Unit tests for all service methods, integration tests for API endpoints]
- Mocking strategy: [e.g., Use moto for AWS services, pytest fixtures for database]

### Git Workflow
- Branch naming: [e.g., `feature/`, `bugfix/`, `hotfix/`]
- Commit message format: [e.g., Conventional Commits]
- PR requirements: [e.g., All tests pass, code coverage maintained, 1+ approval]

## AWS Services Used

### Compute
- [e.g., ECS Fargate for container orchestration]
- [e.g., Lambda for event processing]

### Storage
- [e.g., S3 for file storage]
- [e.g., RDS PostgreSQL for relational data]
- [e.g., DynamoDB for key-value storage]

### Networking
- [e.g., VPC with public/private subnets]
- [e.g., Application Load Balancer]

### Monitoring & Logging
- [e.g., CloudWatch Logs for application logs]
- [e.g., CloudWatch Metrics for monitoring]
- [e.g., X-Ray for distributed tracing]

## Common Tasks

### Local Development
```bash
# Setup
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# Run locally
python src/main.py

# Run tests
pytest tests/
```

### AWS CLI Commands
```bash
# Deploy to dev
aws cloudformation deploy --template-file infra/template.yaml --stack-name my-service-dev

# View logs
aws logs tail /aws/ecs/my-service --follow

# Invoke Lambda
aws lambda invoke --function-name my-function output.json
```

## Environment Variables
- `AWS_REGION`: AWS region (default: us-west-2)
- `ENVIRONMENT`: Environment name (dev/staging/prod)
- `DATABASE_URL`: Database connection string
- `LOG_LEVEL`: Logging level (INFO/DEBUG/ERROR)
[Add more as needed]

## External Dependencies
- **External APIs**: [e.g., Stripe API for payments]
- **Third-party services**: [e.g., SendGrid for emails]
- **Internal services**: [e.g., User Service at https://user-api.internal]

## Documentation Links
- Architecture Decision Records: [Link or path]
- API Documentation: [Link to Swagger/OpenAPI docs]
- Runbook: [Link to operational runbook]
- Dashboards: [Link to monitoring dashboards]

## Contact & Support
- **Team**: [Team name]
- **Slack Channel**: [#team-channel]
- **On-call**: [PagerDuty link or rotation schedule]
- **Tech Lead**: [Name]
