# 🚀 Static Website Deployment with Secure CI/CD

### Production-Style Static Website Deployment with AWS CloudFormation, S3, CloudFront & Automated CI/CD

This project is a production-style static website deployment solution built on AWS to demonstrate Infrastructure as Code, secure content delivery, CI/CD automation, controlled production deployment, event-driven notifications, and AWS security best practices.

The solution uses AWS CloudFormation to provision the infrastructure and integrates GitHub, AWS CodePipeline, AWS CodeBuild, Amazon S3, Amazon CloudFront, EventBridge, Lambda, SNS, KMS, IAM, and CloudWatch.

---

# 🌟 Key Highlights

✅ Static Website Deployment

✅ Amazon S3 Private Website Bucket

✅ Amazon CloudFront Global Content Delivery

✅ CloudFront Origin Access Control (OAC)

✅ HTTPS Website Delivery

✅ AWS CloudFormation Infrastructure as Code

✅ GitHub Source Code Integration

✅ AWS CodePipeline CI/CD

✅ AWS CodeBuild Validation & Packaging

✅ Automatic Main Branch Pipeline Trigger

✅ Manual Production Approval

✅ Deployment Approval Email Notification

✅ Deployment Approved / Denied Email Notifications

✅ CloudFront Cache Invalidation

✅ Deployment Successful Email Notification

✅ Amazon EventBridge Event-Driven Notifications

✅ AWS Lambda Notification Processing

✅ Amazon SNS Email Notifications

✅ AWS KMS Encryption

✅ IAM Least-Privilege Access

✅ CloudWatch Logging

---

# 🏗 Solution Architecture

        Developer
            │
            ▼
        GitHub Repository
            │
            │ Push to main
            ▼
        AWS CodePipeline
            │
            ▼
        AWS CodeBuild
            │
            ▼
        Manual Approval
            │
            │ Approved
            ▼
        Private Amazon S3
            │
            │ Origin Access Control
            ▼
        Amazon CloudFront
            │
            ▼
        End Users

---

# 🔔 Notification Architecture

                    AWS CodePipeline
                           │
             ┌─────────────┴─────────────┐
             │                           │
             ▼                           ▼
      Approval Status             Deployment Success
       EventBridge                   EventBridge
             │                           │
             └─────────────┬─────────────┘
                           ▼
                 Notification Lambda
                           │
                           ▼
                 SNS Deployment Status
                           │
                           ▼
                     Email Alert

---

# 🔄 Complete Deployment Flow

        GitHub Push
            │
            ▼
        CodePipeline Trigger
            │
            ▼
        CodeBuild
            │
            ▼
        Manual Approval
            │
            ├──────────────► Approval Email
            │
            ├── DENIED ────► Deployment Denied Email
            │
            ▼
        APPROVED
            │
            ├──────────────► Deployment Approved Email
            │
            ▼
        S3 Deployment
            │
            ▼
        CloudFront Cache Invalidation
            │
            ▼
        Deployment Success Event
            │
            ▼
        Deployment Successful Email
            │
            ▼
        CloudFront Website

---

# 🎯 Project Objectives

The project was designed to achieve the following objectives:

- Deploy a static website on AWS.
- Serve the website through Amazon CloudFront.
- Keep the Amazon S3 origin private.
- Provision the infrastructure using AWS CloudFormation.
- Store source code in GitHub.
- Implement an automated CI/CD pipeline.
- Automatically trigger deployment when changes are pushed to the `main` branch.
- Introduce manual production approval before deployment.
- Send deployment approval and status notifications through email.
- Deploy the latest approved website version to Amazon S3.
- Invalidate CloudFront cache after deployment.
- Ensure users receive the latest deployed website content.
- Follow AWS security best practices and the principle of least privilege.

---

# ☁️ AWS Services Used

| AWS Service        | Purpose                                               |
| ------------------ | ----------------------------------------------------- |
| AWS CloudFormation | Infrastructure as Code                                |
| Amazon S3          | Private website storage and pipeline artifact storage |
| Amazon CloudFront  | Global HTTPS content delivery                         |
| CloudFront OAC     | Secure CloudFront access to private S3                |
| AWS CodePipeline   | CI/CD orchestration                                   |
| AWS CodeBuild      | Website validation and artifact creation              |
| Amazon EventBridge | Pipeline event detection                              |
| AWS Lambda         | Notification processing                               |
| Amazon SNS         | Email notification delivery                           |
| AWS KMS            | Encryption and key management                         |
| AWS IAM            | Access control and least privilege                    |
| Amazon CloudWatch  | Logging and monitoring                                |
| GitHub             | Source code repository                                |


---

# 🗄 Amazon S3

The static website is stored in a private Amazon S3 bucket.

## Security Features

- Block Public Access enabled.
- No direct public access to website objects.
- CloudFront used as the public delivery layer.
- CloudFront Origin Access Control used to access the S3 origin securely.

Architecture:

        Internet
            │
            ▼
        CloudFront
            │
            │ OAC
            ▼
        Private S3 Bucket

The S3 bucket is therefore not exposed directly to the internet.

---

# 🌎 Amazon CloudFront

Amazon CloudFront provides the public delivery layer for the website.

## Features

- HTTPS content delivery.
- Global content distribution.
- Private S3 origin.
- Origin Access Control.
- CloudFront cache management.
- Automated cache invalidation after deployment.

The CloudFront distribution serves the latest successfully deployed website version.

---

# 🔐 CloudFront Origin Access Control

CloudFront Origin Access Control (OAC) is used to securely access the private S3 bucket.

        CloudFront
            │
            │ Authenticated Origin Request
            ▼
        Private S3

Users access the website through CloudFront rather than directly accessing S3.

---

# 🚀 Infrastructure as Code

The entire AWS infrastructure is provisioned using:

```
AWS CloudFormation
```

Primary template:

```
cloudformation/main.yml
```

The CloudFormation template provisions the required infrastructure including:

- S3 buckets.
- CloudFront distribution.
- CloudFront Origin Access Control.
- CodePipeline.
- CodeBuild.
- IAM roles and policies.
- Lambda notification function.
- SNS topic.
- EventBridge rules.
- Lambda invocation permissions.
- KMS key.
- CloudWatch logging resources.

---

# 📋 CloudFormation Parameters

| Parameter             | Purpose                                         |
| --------------------- | ----------------------------------------------- |
| `GitHubConnectionArn` | Existing GitHub CodeConnections connection ARN  |
| `NotificationEmail`   | Email address used for deployment notifications |
| `Owner`               | Project owner                                   |
| `ProjectName`         | Project name                                    |
| `Environment`         | Deployment environment                          |
| `DevelopedBy`         | Developer/project attribution                   |
| `GitHubBranch`        | Source branch                                   |
| `GitHubRepository`    | GitHub repository                               |


---

# 🔄 CI/CD Pipeline

AWS CodePipeline provides the complete CI/CD workflow.

## Pipeline Stages

        Source
        │
        ▼
        Build
        │
        ▼
        Manual Approval
        │
        ▼
        Deploy Website
        │
        ▼
        CloudFront Cache Invalidation

---

## Source Stage

GitHub is used as the source repository.

The pipeline monitors the:

```
main
```

branch.

A new push to the branch automatically starts the pipeline.

---

## Build Stage

AWS CodeBuild validates the frontend project and prepares the deployment artifact.

The required frontend structure is:

        Frontend/
        ├── index.html
        ├── style.css
        └── script.js

The `Frontend` directory is packaged as the deployment artifact.

---

# ✋ Manual Production Approval

Before production deployment, the pipeline pauses at the manual approval stage.

The approval workflow provides deployment information through email.

The reviewer can choose:

```
APPROVE
```

or

```
DENY
```

This prevents an unreviewed pipeline execution from reaching the production deployment stage.

---

# 📧 Deployment Notifications

The notification system is implemented using:

        Amazon EventBridge
                │
                ▼
        AWS Lambda
                │
                ▼
        Amazon SNS
                │
                ▼
        Email

A single notification Lambda processes the different CodePipeline events.

---

## Deployment Approval Email

When the pipeline reaches the manual approval stage, an approval notification is generated.

The email provides deployment information and access to the pipeline approval workflow.

---

## Deployment Approved Email

Immediately after the approval decision:

```
Status:APPROVED
```

A Deployment Approved notification is sent.

The notification contains relevant information such as:

- Pipeline name.
- Stage.
- Action.
- Region.
- Approval status.
- Approver.
- Approval comment.
- Execution ID.
- Event time.

---

## Deployment Denied Email

If the deployment is denied:

```
Status:DENIED
```

A Deployment Denied notification is sent.

The notification includes relevant execution and decision information.

---

## Deployment Successful Email

After the deployment is approved and the CloudFront cache invalidation action succeeds, the final deployment notification is sent.

The notification contains:

- Pipeline name.
- Region.
- Deployment status.
- Execution ID.
- Event time.
- CloudFront website URL.

---

# ⚡ Amazon EventBridge

Two EventBridge rules are used.

## Approval Status Rule

The approval-status rule handles the CodePipeline manual approval lifecycle.

It processes the relevant approval states including:

```
IN_PROGRESSSUCCEEDEDFAILED
```

These events are passed to the notification Lambda.

---

## Deployment Success Rule

The deployment-success rule monitors the CloudFront cache invalidation action.

When the cache invalidation action reaches:

```
SUCCEEDED
```

the notification Lambda sends the final Deployment Successful email.

---

# 🐍 AWS Lambda

A single Lambda function is used for deployment notifications.

The Lambda:

- Receives CodePipeline events from EventBridge.
- Extracts pipeline information.
- Determines the deployment state.
- Generates formatted email content.
- Publishes the message to SNS.
- Provides CloudFront URL information for successful deployments.
- Logs received events for troubleshooting.

---

# 📢 Amazon SNS

Amazon SNS is used as the notification delivery service.

The project uses a deployment-status SNS topic.

        CodePipeline Event
            │
            ▼
        EventBridge
            │
            ▼
        Lambda
            │
            ▼
        SNS Topic
            │
            ▼
        Confirmed Email Subscription

The SNS topic is encrypted using AWS KMS.

---

# 🔑 AWS KMS

A customer-managed AWS KMS key is used for encryption-related operations.

The notification Lambda requires:

```
kms:Decryptkms:GenerateDataKey
```

permissions for the project KMS key.

KMS provides encryption support for protected resources within the deployment notification architecture.

---

# 👤 IAM Security

Dedicated IAM roles are used for AWS services.

Major roles include:

- CodePipeline service role.
- CodeBuild service role.
- Notification Lambda execution role.

The project follows the principle of least privilege by granting services only the permissions required to perform their respective tasks.

---

# 📜 CloudWatch Logging

CloudWatch Logs are used for monitoring and troubleshooting.

The project includes logging for:

- CodePipeline.
- CodeBuild.
- Lambda.

The required logging permissions include:

```
logs:CreateLogGrouplogs:CreateLogStreamlogs:PutLogEvents
```

---

# 📦 Repository Structure

        Secure-Static-Website-Deployment/
        │
        ├── Frontend/
        │   ├── index.html
        │   ├── style.css
        │   └── script.js
        │
        ├── cloudformation/
        │   └── main.yml
        │
        ├── buildspec.yml
        │
        └── README.md

---

# 🧪 Testing & Validation

The final solution was validated through multiple deployment and infrastructure tests.

## CI/CD Testing

- GitHub push successfully triggers CodePipeline.
- CodeBuild completes successfully.
- Manual approval correctly pauses the pipeline.
- Approved deployment continues automatically.
- Denied deployment stops the production deployment.

## Notification Testing

- Deployment approval email received.
- Deployment approved email received.
- Deployment denied email received.
- Deployment successful email received.
- Successful deployment email contains the CloudFront URL.

## CloudFront Testing

- CloudFront distribution is enabled.
- CloudFront successfully accesses the private S3 origin.
- S3 remains private.
- Cache invalidation succeeds.
- Updated website content is available through CloudFront.

## Infrastructure Testing

CloudFormation drift detection was performed to verify that the deployed infrastructure matches the CloudFormation template.

Final state:

        CloudFormation Stack
                │
                ▼
            IN_SYNC

---

# 🛠 Troubleshooting

## CloudFront / S3 Access

**Problem:** CloudFront could not correctly retrieve content from the private S3 origin.

**Resolution:** Configured CloudFront Origin Access Control and the required S3 bucket policy.

---

## CodePipeline CloudWatch Logging

**Problem:** The CodePipeline Commands action required additional CloudWatch Logs permissions.

**Resolution:** Configured the required:

```
logs:CreateLogGrouplogs:CreateLogStreamlogs:PutLogEvents
```

permissions.

---

## KMS / SNS Notification

**Problem:** The notification Lambda required KMS permissions to publish to the encrypted SNS topic.

**Resolution:** Configured:

```
kms:Decryptkms:GenerateDataKey
```

permissions for the Lambda execution role.

---

## EventBridge Approval Notification

**Problem:** The initial approval event pattern did not correctly capture the required CodePipeline approval lifecycle.

**Resolution:** Updated the EventBridge approval-status rule to handle the appropriate approval action states.

---

## CloudFormation Drift

**Problem:** CloudFormation detected a modified Notification Lambda IAM role.

**Cause:** An additional SNS/KMS policy existed on the role.

**Resolution:** Removed the duplicate policy and reran drift detection.

Final result:

```
IN_SYNC
```

---

# 📊 Final AWS Resource Inventory

        CloudFormation Stack
                │
                ├── S3 Website Bucket
                ├── S3 Artifact Bucket
                ├── CloudFront Distribution
                ├── CloudFront OAC
                ├── CodePipeline
                ├── CodeBuild
                ├── Notification Lambda
                ├── SNS Topic
                ├── EventBridge Rules
                ├── KMS Key
                ├── IAM Roles & Policies
                └── CloudWatch Logs

---

# 📈 Project Status

    Static Website Deployment       ✅ Completed
    Private S3 Origin               ✅ Completed
    CloudFront Distribution         ✅ Completed
    CloudFront OAC                  ✅ Completed
    CloudFormation IaC              ✅ Completed
    GitHub Integration              ✅ Completed
    CodePipeline CI/CD              ✅ Completed
    CodeBuild Validation            ✅ Completed
    Manual Production Approval      ✅ Completed
    Approval Notification           ✅ Completed
    Approved Notification           ✅ Completed
    Denied Notification             ✅ Completed
    CloudFront Invalidation         ✅ Completed
    Deployment Success Notification ✅ Completed
    EventBridge Integration         ✅ Completed
    Lambda Notification System      ✅ Completed
    SNS Email Notifications         ✅ Completed
    KMS Encryption                  ✅ Completed
    IAM Least Privilege             ✅ Completed
    CloudWatch Logging              ✅ Completed
    Infrastructure Drift Validation ✅ Completed

---

# 🎓 Concepts Demonstrated

## Cloud Computing

- Amazon S3
- Amazon CloudFront
- CloudFront Origin Access Control
- AWS IAM
- AWS KMS
- Amazon CloudWatch

## Infrastructure as Code

- AWS CloudFormation
- CloudFormation Parameters
- CloudFormation Resources
- CloudFormation IAM Configuration
- CloudFormation Drift Detection

## CI/CD

- GitHub
- AWS CodePipeline
- AWS CodeBuild
- Automated Source Trigger
- Manual Production Approval
- Deployment Automation
- CloudFront Cache Invalidation

## Event-Driven Architecture

- Amazon EventBridge
- AWS Lambda
- Amazon SNS
- CodePipeline Event Handling
- Automated Email Notifications

## Security

- Private S3 Bucket
- S3 Block Public Access
- CloudFront OAC
- HTTPS
- IAM Least Privilege
- KMS Encryption
- Encrypted SNS
- Manual Production Approval

---

# 📦 Project Deliverables

The project includes:

### Source Code Repository

GitHub repository containing the static website source code.

### Infrastructure as Code

```
cloudformation/main.yml
```

### CI/CD Configuration

AWS CodePipeline and CodeBuild configuration.

### Architecture Diagram

Final AWS architecture and deployment flow diagram.

### README

Project architecture, deployment workflow, security implementation, and operational documentation.

### Final Project Report

Complete technical documentation covering the implementation, architecture, security, testing, troubleshooting, and project outcomes.

---

# 🚀 Deployment Flow

        Developer
            │
            │ git push
            ▼
        GitHub
            │
            ▼
        AWS CodePipeline
            │
            ▼
        AWS CodeBuild
            │
            ▼
        Manual Approval
            │
            ├──────────────► EventBridge
            │                     │
            │                     ▼
            │              Notification Lambda
            │                     │
            │                     ▼
            │                    SNS
            │                     │
            │                     ▼
            │              Approval Email
            │
            │
            ▼
        Approved
            │
            ├──────────────► Deployment Approved Email
            │
            ▼
        Private S3
            │
            ▼
        CloudFront Cache Invalidation
            │
            ▼
        EventBridge
            │
            ▼
        Notification Lambda
            │
            ▼
        SNS
            │
            ▼
        Deployment Successful Email
            │
            ▼
        CloudFront
            │
            ▼
        End Users

---

# 📌 Conclusion

The Static Website Deployment with Secure CI/CD project provides a secure, automated, and repeatable AWS deployment solution for a static website.

The architecture combines GitHub, AWS CodePipeline, AWS CodeBuild, Amazon S3, Amazon CloudFront, Amazon EventBridge, AWS Lambda, Amazon SNS, AWS KMS, AWS IAM, Amazon CloudWatch, and AWS CloudFormation.

The private S3 origin prevents direct public access while CloudFront provides secure HTTPS content delivery. The CI/CD pipeline automatically validates and deploys website changes while the manual approval stage provides production control.

EventBridge, Lambda, and SNS provide automated deployment visibility through approval, denial, and successful deployment notifications. CloudFront cache invalidation ensures users receive the latest deployed website content.

The infrastructure is managed using AWS CloudFormation and the final implementation has been validated through end-to-end deployment testing, notification testing, cache invalidation testing, and CloudFormation drift detection.

The resulting solution demonstrates secure AWS architecture, Infrastructure as Code, CI/CD automation, event-driven architecture, least-privilege access control, and production-oriented deployment practices.

---

# 👨‍💻 Author

**Bharath**

Cloud & DevOps Enthusiast | Backend Engineering | AWS | CI/CD

GitHub: https://github.com/Bharath77Bharath