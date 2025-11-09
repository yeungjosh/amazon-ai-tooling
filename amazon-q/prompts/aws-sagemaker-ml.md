# AWS SageMaker ML Development

Specialized prompt for ML engineers working with AWS SageMaker and related ML services.

## Context
I'm developing a machine learning solution using AWS SageMaker and related AWS ML services.

## Task
[Describe your specific ML task - model training, endpoint deployment, feature engineering, etc.]

## Requirements

### Model Development
If developing/training a model:
- **Framework**: [e.g., PyTorch, TensorFlow, Scikit-learn, XGBoost]
- **Model type**: [e.g., CNN for image classification, LSTM for time series, etc.]
- **Input data**: [Description of input features and format]
- **Output**: [What the model predicts]
- **Performance target**: [e.g., >90% accuracy, <100ms latency]

### AWS Services to Use
- [ ] **SageMaker Training Jobs** - For model training
- [ ] **SageMaker Endpoints** - For real-time inference
- [ ] **SageMaker Batch Transform** - For batch predictions
- [ ] **SageMaker Pipelines** - For ML workflows
- [ ] **SageMaker Feature Store** - For feature management
- [ ] **S3** - For data storage
- [ ] **Lambda** - For serverless inference
- [ ] **Step Functions** - For workflow orchestration
- [ ] **CloudWatch** - For monitoring and logging
- [ ] **ECR** - For custom container images

## Code Generation Requirements

### 1. SageMaker Training Code
Generate code that includes:
- Data loading from S3
- Data preprocessing and feature engineering
- Model definition (using specified framework)
- Training loop with validation
- Model checkpointing
- Hyperparameter configuration
- Metrics logging to CloudWatch
- Model artifact saving to S3

### 2. SageMaker Deployment Code
Generate code that includes:
- Model packaging for deployment
- Endpoint configuration (instance type, count, auto-scaling)
- Inference script with preprocessing and postprocessing
- Error handling for inference
- Logging inference requests
- A/B testing configuration (if needed)

### 3. Inference Code
Generate code for invoking the model:
- **Real-time**: Lambda function or API Gateway → SageMaker Endpoint
- **Batch**: SageMaker Batch Transform job
- Input validation
- Error handling and retries
- Response formatting

### 4. Infrastructure as Code
Generate Terraform or CloudFormation for:
- S3 buckets for data and models (with encryption)
- IAM roles with least privilege (SageMaker execution role)
- SageMaker training job configuration
- SageMaker endpoint configuration
- CloudWatch alarms for endpoint metrics
- VPC configuration (if needed)

## Best Practices to Follow

### Data Handling
- ✅ Use S3 for training data with proper prefix structure
- ✅ Enable S3 encryption at rest
- ✅ Use S3 lifecycle policies for data retention
- ✅ Implement data versioning (e.g., timestamped prefixes)

### Training
- ✅ Use SageMaker managed spot training for cost savings
- ✅ Enable SageMaker Experiments for tracking
- ✅ Use SageMaker Debugger for monitoring training
- ✅ Implement checkpointing for long training jobs
- ✅ Log metrics to CloudWatch for monitoring
- ✅ Use appropriate instance types (GPU for deep learning)

### Deployment
- ✅ Use SageMaker Multi-Model Endpoints if serving multiple models
- ✅ Enable auto-scaling based on invocations or latency
- ✅ Use appropriate instance types (cost vs latency tradeoff)
- ✅ Implement A/B testing for model updates
- ✅ Enable data capture for monitoring and retraining

### Monitoring & MLOps
- ✅ Set up CloudWatch alarms for:
  - Model latency (p50, p95, p99)
  - Error rates
  - Model drift (if using SageMaker Model Monitor)
- ✅ Implement logging for all inference requests
- ✅ Use SageMaker Model Monitor for data quality and drift detection
- ✅ Set up retraining pipelines with SageMaker Pipelines

### Security
- ✅ Use VPC for SageMaker training and endpoints (if handling sensitive data)
- ✅ Enable encryption for training volumes and endpoints
- ✅ Use IAM roles with least privilege
- ✅ Don't log sensitive data
- ✅ Enable VPC endpoints for S3 and SageMaker (avoid public internet)

### Cost Optimization
- ✅ Use Spot instances for training when appropriate
- ✅ Right-size instances (don't over-provision)
- ✅ Use SageMaker Serverless Inference for infrequent traffic
- ✅ Delete unused endpoints
- ✅ Use S3 Intelligent-Tiering for data storage

## Code Structure

### Training Script (train.py)
```python
# Expected structure:
import boto3
import sagemaker
from sagemaker import get_execution_role

# 1. Hyperparameters
# 2. Data loading from S3
# 3. Preprocessing
# 4. Model definition
# 5. Training loop
# 6. Evaluation
# 7. Save model to /opt/ml/model/
```

### Inference Script (inference.py)
```python
# Expected structure:
# 1. model_fn() - Load model
# 2. input_fn() - Preprocess input
# 3. predict_fn() - Run inference
# 4. output_fn() - Format output
```

### Deployment Script
```python
# Expected structure:
# 1. Load model artifact from S3
# 2. Create SageMaker Model
# 3. Create Endpoint Configuration
# 4. Create/Update Endpoint
# 5. Test endpoint
```

## Example Scenarios

### Scenario 1: Train PyTorch model on custom dataset
"Generate SageMaker training code for a PyTorch ResNet model for image classification.
Dataset is in S3 at s3://my-bucket/training-data/.
Use ml.p3.2xlarge instance.
Save model to s3://my-bucket/models/.
Log accuracy and loss to CloudWatch."

### Scenario 2: Deploy model for real-time inference
"Create SageMaker endpoint deployment code for a trained XGBoost model.
Model artifact: s3://my-bucket/models/model.tar.gz
Use ml.m5.xlarge with auto-scaling (min=1, max=10).
Scale based on invocations per instance (target=1000).
Create Lambda function to invoke endpoint."

### Scenario 3: Batch prediction pipeline
"Generate code for SageMaker Batch Transform job.
Input data: s3://my-bucket/input/ (CSV files)
Model: s3://my-bucket/models/model.tar.gz
Output: s3://my-bucket/predictions/
Use ml.m5.2xlarge instance.
Split input into 50MB chunks."

### Scenario 4: MLOps pipeline with SageMaker Pipelines
"Create SageMaker Pipeline for:
1. Data preprocessing step (Processing Job)
2. Model training step (Training Job)
3. Model evaluation step (Processing Job)
4. Conditional deployment (if metrics > threshold)
5. Model registration to Model Registry"

## Testing Requirements
- Include code for testing endpoint locally (before deployment)
- Include code for A/B testing different model versions
- Generate sample test data for inference
- Create integration tests with moto (AWS mocking)

## Documentation
- Add docstrings to all functions
- Include README with setup instructions
- Document hyperparameters and their defaults
- Provide example inference requests/responses

## Output Format
Please provide:
1. **Training code** (complete, ready to run)
2. **Inference code** (model_fn, input_fn, predict_fn, output_fn)
3. **Deployment script** (creating endpoint)
4. **Infrastructure code** (Terraform or CloudFormation)
5. **Testing code** (integration tests)
6. **README** (setup and usage instructions)

Organize as:
```
project/
├── src/
│   ├── train.py
│   ├── inference.py
│   └── utils.py
├── scripts/
│   ├── deploy.py
│   └── test_endpoint.py
├── infrastructure/
│   └── sagemaker.tf
├── tests/
│   └── test_model.py
├── requirements.txt
└── README.md
```
