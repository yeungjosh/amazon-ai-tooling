---
name: AWS Best Practices
description: AWS architecture and implementation guidelines for all services
priority: high
---

# AWS Best Practices

## Resource Tagging
All AWS resources MUST include these tags:
- `Environment`: [dev | staging | prod]
- `Service`: [service-name]
- `Team`: [team-name]
- `CostCenter`: [cost-center-id]
- `ManagedBy`: [terraform | cloudformation | manual]

## High Availability

### Multi-AZ Deployment
- Production workloads MUST be Multi-AZ
- Use Auto Scaling Groups with instances in multiple AZs
- RDS: Enable Multi-AZ for production databases
- ElastiCache: Use replication across AZs
- S3: Use Standard storage class (already multi-AZ) or enable Cross-Region Replication for critical data

### Load Balancing
- Use Application Load Balancer for HTTP/HTTPS traffic
- Use Network Load Balancer for TCP/UDP traffic
- Configure health checks with appropriate thresholds
- Enable connection draining (deregistration delay)

## Cost Optimization

### Compute
- Use appropriate instance types:
  - Consider Graviton instances for 20-40% cost savings
  - Use Spot Instances for fault-tolerant workloads (up to 90% savings)
  - Use Reserved Instances or Savings Plans for predictable workloads
- Implement auto-scaling to match demand
- Stop/terminate unused EC2 instances
- Right-size instances based on CloudWatch metrics

### Storage
- Use S3 lifecycle policies:
  - Transition to Infrequent Access after 30-90 days
  - Transition to Glacier for archival
  - Expire old versions and incomplete multipart uploads
- Use S3 Intelligent-Tiering for unknown access patterns
- Delete unused EBS volumes and snapshots
- Use EBS gp3 instead of gp2 (cheaper, better performance)

### Database
- Right-size RDS instances (monitor CPU/memory)
- Use Aurora Serverless for variable workloads
- Enable automated backups with appropriate retention (7 days minimum)
- Use Read Replicas to offload read traffic

### Monitoring
- Set up AWS Cost Explorer and Budget alerts
- Tag resources for cost allocation
- Review AWS Trusted Advisor recommendations
- Use AWS Compute Optimizer for rightsizing recommendations

## Security

### IAM
- Follow least privilege principle
- Use IAM roles for EC2/ECS/Lambda (not access keys)
- Rotate access keys regularly (max 90 days)
- Enable MFA for all IAM users
- Use IAM policies, not inline policies
- Never commit AWS access keys to version control

### Network Security
- Use VPCs with public and private subnets
- Bastion hosts or Systems Manager Session Manager (no direct SSH)
- Security Groups: whitelist only required ports
- Network ACLs: additional defense layer
- Use VPC endpoints for AWS services (avoid internet gateway traffic)
- Enable VPC Flow Logs for network monitoring

### Data Protection
- Enable encryption at rest:
  - S3: Server-side encryption (SSE-S3 or SSE-KMS)
  - RDS: Storage encryption
  - EBS: Volume encryption
  - DynamoDB: Encryption at rest
- Enable encryption in transit:
  - Use TLS/SSL for all API communication
  - Enable HTTPS on load balancers
  - RDS: Enforce SSL connections
- Use AWS Secrets Manager for secrets (not environment variables)
- Enable versioning on S3 buckets for critical data

### Compliance
- Enable AWS CloudTrail for audit logging (all regions)
- Enable AWS Config for compliance monitoring
- Use AWS GuardDuty for threat detection
- Enable S3 bucket logging for access logs
- Implement log retention policies (minimum 90 days)

## Observability

### Logging
- Send all application logs to CloudWatch Logs
- Use structured logging (JSON format)
- Include correlation IDs for request tracing
- Set up log retention policies (avoid indefinite retention)
- Create CloudWatch Log Insights queries for common searches

### Metrics
- Emit custom CloudWatch Metrics for business KPIs
- Use CloudWatch dashboards for visualization
- Standard metrics to track:
  - Request count and latency (p50, p95, p99)
  - Error rate and error types
  - Resource utilization (CPU, memory, disk)
  - Queue depth (SQS, Kinesis)

### Alarms
- Set up CloudWatch Alarms for critical metrics:
  - High error rate (> 1%)
  - High latency (p99 > threshold)
  - Low health check success rate
  - High CPU/memory utilization (> 80%)
  - Database connection pool exhaustion
- Use SNS for alarm notifications
- Integrate with PagerDuty/Opsgenie for on-call

### Tracing
- Use AWS X-Ray for distributed tracing
- Trace critical paths end-to-end
- Include custom subsegments for business logic
- Track external API calls

## Infrastructure as Code

### Version Control
- All infrastructure as code (Terraform or CloudFormation)
- Store state in S3 with locking (DynamoDB for Terraform)
- Use workspaces/stacks for different environments
- Never create resources manually in AWS Console

### Modules & Reusability
- Use Terraform modules or CloudFormation nested stacks
- Create reusable components (VPC module, RDS module, etc.)
- Version your modules
- Share modules across teams

### CI/CD for Infrastructure
- Apply changes via CI/CD pipeline, not locally
- Use terraform plan / CloudFormation change sets for review
- Require approval for production changes
- Enable drift detection

## Networking

### VPC Design
- Use private subnets for application servers and databases
- Use public subnets only for load balancers and NAT gateways
- Use NAT Gateway for outbound internet access from private subnets
- One NAT Gateway per AZ for high availability
- Use VPC peering or Transit Gateway for VPC-to-VPC communication

### DNS
- Use Route 53 for DNS management
- Enable health checks for failover routing
- Use ALIAS records for AWS resources (no charge)

## Serverless Best Practices

### Lambda
- Set appropriate memory and timeout values
- Use environment variables for configuration
- Use Lambda Layers for shared code/dependencies
- Enable X-Ray tracing
- Use Lambda reserved concurrency for critical functions
- Set up Dead Letter Queues (DLQ) for failed invocations
- Keep functions small and single-purpose
- Minimize cold start time:
  - Keep deployment package small
  - Use Provisioned Concurrency for latency-sensitive functions

### API Gateway
- Enable caching to reduce backend load
- Set up request/response validation
- Use API keys and usage plans for rate limiting
- Enable CloudWatch logging (execution and access logs)
- Use stage variables for environment-specific configuration

## Database Best Practices

### RDS
- Use parameter groups and option groups
- Enable automated backups (7-35 day retention)
- Enable Performance Insights for query monitoring
- Use Enhanced Monitoring for OS-level metrics
- Enable deletion protection for production databases
- Use encryption at rest (KMS)

### DynamoDB
- Use on-demand billing for unpredictable workloads
- Use provisioned capacity with auto-scaling for predictable workloads
- Enable point-in-time recovery for critical tables
- Use DynamoDB Streams for change data capture
- Enable encryption at rest
- Use Global Tables for multi-region replication
- Design partition keys to avoid hot partitions

## Container Best Practices (ECS/EKS)

### ECS
- Use Fargate for serverless containers (avoid EC2 management)
- Use task roles for AWS permissions (not container credentials)
- Enable CloudWatch Container Insights
- Use secrets in AWS Secrets Manager (not environment variables)
- Implement health checks (ELB target group health checks)

### EKS
- Use managed node groups or Fargate
- Enable control plane logging
- Use IAM roles for service accounts (IRSA)
- Implement pod security policies
- Use Kubernetes Horizontal Pod Autoscaler
- Use Cluster Autoscaler for node scaling

### ECR
- Scan images for vulnerabilities (enable scan on push)
- Use immutable tags
- Implement lifecycle policies to clean up old images
- Replicate critical images across regions

## Disaster Recovery

### Backup Strategy
- Automated RDS backups + manual snapshots before major changes
- EBS snapshot lifecycle policies
- S3 versioning for critical buckets
- DynamoDB point-in-time recovery or on-demand backups
- Test restore procedures regularly

### Multi-Region
- Critical systems: Active-active or active-passive multi-region
- Use Route 53 failover routing
- Replicate data: S3 CRR, DynamoDB Global Tables, RDS cross-region replicas
- Document and test failover procedures

## Performance Optimization

### Caching
- Use CloudFront for static content and API caching
- Use ElastiCache (Redis/Memcached) for session storage and query results
- Enable RDS query cache where applicable
- Use DAX for DynamoDB query caching

### Database Optimization
- Create appropriate indexes (avoid full table scans)
- Use RDS Read Replicas to offload read traffic
- Use connection pooling
- Implement query caching
- Monitor slow query logs

### Asynchronous Processing
- Use SQS for decoupling services
- Use SNS for pub/sub messaging
- Use Step Functions for complex workflows
- Use EventBridge for event-driven architectures

## Code Examples

### Tagging Resources (Terraform)
```hcl
resource "aws_instance" "app_server" {
  # ... other configuration ...

  tags = {
    Name        = "app-server-${var.environment}"
    Environment = var.environment
    Service     = "order-service"
    Team        = "platform"
    CostCenter  = "engineering"
    ManagedBy   = "terraform"
  }
}
```

### IAM Role for Lambda
```python
# Use boto3 with IAM role (no hardcoded credentials)
import boto3

# SDK automatically uses Lambda execution role
s3 = boto3.client('s3')
dynamodb = boto3.resource('dynamodb')
```

### CloudWatch Metric Emission
```python
import boto3
from datetime import datetime

cloudwatch = boto3.client('cloudwatch')

def emit_metric(metric_name, value, unit='Count'):
    """Emit custom metric to CloudWatch"""
    cloudwatch.put_metric_data(
        Namespace='MyApp/Orders',
        MetricData=[{
            'MetricName': metric_name,
            'Value': value,
            'Unit': unit,
            'Timestamp': datetime.utcnow(),
            'Dimensions': [
                {'Name': 'Environment', 'Value': 'production'},
                {'Name': 'Service', 'Value': 'order-service'}
            ]
        }]
    )
```

### Structured Logging
```python
import json
import logging

logger = logging.getLogger()

def log_structured(level, message, **kwargs):
    """Log in structured JSON format"""
    log_entry = {
        'timestamp': datetime.utcnow().isoformat(),
        'level': level,
        'message': message,
        **kwargs
    }
    logger.log(getattr(logging, level), json.dumps(log_entry))

# Usage
log_structured('INFO', 'Order created', order_id=order.id, user_id=user.id)
```

## References
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [AWS Security Best Practices](https://docs.aws.amazon.com/security/latest/userguide/best-practices.html)
- [AWS Cost Optimization](https://aws.amazon.com/pricing/cost-optimization/)
