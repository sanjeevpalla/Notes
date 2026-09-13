# ☁ AWS

> Notes from the **AWS Zero to Hero** series (instructor: **Abhishek**), a 30-day journey through AWS from a DevOps engineer's perspective — foundations, networking, CI/CD, containers, and production architecture.

## 📑 Contents

| # | 📅 Day | 📘 Note | 🧩 Topics Covered | 📌 Status |
|:-:|:-:|---|---|:-:|
| 1 | 1 | ☁️ [Introduction to AWS](<01. Introduction to AWS.md>) | Cloud computing · private vs. public cloud · why AWS won · cloud repatriation · AWS account setup | ✅ |
| 2 | 2 | 🔐 [IAM Deep Dive](<02. IAM Deep Dive.md>) | Why IAM exists (bank analogy) · users, policies, groups & roles · live IAM user practical | ✅ |
| 3 | 3 | 🖥️ [EC2 Deep Dive](<03. EC2 Deep Dive.md>) | Instance types · regions & AZs · launching & connecting to an instance · deploying Jenkins · Windows connection guide | ✅ |
| 4 | 4 | 🌐 [VPC Deep Dive](<04. VPC Deep Dive.md>) | VPC components via the "wise builder" analogy · subnets, route tables, gateways (foundations only) | ✅ |
| 5 | 5 | 🛡️ [Security Groups & NACLs](<05. Security Groups & NACLs.md>) | Custom VPC build · security groups vs. NACLs · live NACL rule-order demo | ✅ |
| 6 | 6 | 🌍 [DNS / Route 53](<06. DNS or Route53.md>) | DNS fundamentals · Route 53 high-level overview | ✅ |
| 7 | 7 | 🏗️ [A Complete Production-Grade Project](<07. A complete Production Grade Project.md>) | Public/private subnets · Auto Scaling Groups · Load Balancers · Bastion Hosts | ✅ |
| 8 | 9 | 🪣 [S3 Buckets Deep Dive](<08. AWS S3 Buckets Deep Dive.md>) | S3 core characteristics · storage classes · versioning · bucket policies · static website hosting | ✅ |
| 9 | 10 | 💻 [AWS CLI Deep Dive](<09. AWS CLI Deep Dive.md>) | API & abstraction layers · IaC categorization · installing/configuring the CLI · S3 & EC2 via CLI | ✅ |
| 10 | 11 | 🏗️ [IaC with AWS CFT](<10. Iac with AWS CFT.md>) | IaC principles · CloudFormation structure · drift detection · writing an EC2 template · CFT vs. Terraform | ✅ |
| 11 | 12 | 🔧 [AWS CodeCommit](<11. AWS CodeCommit.md>) | AWS-native CI/CD sub-series kickoff · CodeCommit mapped to a Jenkins workflow · repo creation & IAM setup | ✅ |
| 12 | 13 | 🔀 [AWS CodePipeline](<12. AWS CodePipeline.md>) | Jenkins-based CI/CD vs. AWS-native equivalent · why pay for CodePipeline over free Jenkins | ✅ |
| 13 | 14 | 🛠️ [AWS End-to-End CI](<13. AWS End-to-End CI.md>) | CodePipeline & CodeBuild · Flask app · `buildspec.yaml` · Parameter Store secrets · live debugging | ✅ |
| 14 | 15 | 🚀 [AWS Ultimate CI/CD Pipeline](<14. AWS Ultimate CICD Pipeline.md>) | Completing CD with CodeDeploy · EC2 & IAM setup · CodeDeploy agent · `appspec.yaml` · full pipeline demo | ✅ |
| 15 | 16 | 👁️ [AWS CloudWatch](<15. AWS CloudWatch.md>) | CloudWatch "gatekeeper" analogy · log groups · metrics & alarms · CPU-spike demo with email alert | ✅ |
| 16 | 17 | ⚡ [AWS Lambda](<16. AWS Lambda.md>) | EC2 vs. Lambda · serverless & cost/security benefits · creating & invoking a Lambda function | ✅ |
| 17 | 18 | 💰 [AWS Cloud Cost Optimization](<17. AWS Cloud Cost Optimization.md>) | Real Lambda + `boto3` cost-optimization project · live debugging (timeouts, permissions) · automated CloudWatch trigger | ✅ |
| 18 | 19 | 🌐 [AWS CloudFront](<18. AWS CloudFront.md>) | CDN fundamentals · hosting a static site on S3 · creating a CloudFront distribution (guest: Piyush) | ✅ |
| 19 | 20 | 📦 [AWS ECR](<19. AWS ECR.md>) | Elastic Container Registry vs. Docker Hub · build, tag & push a Docker image end to end | ✅ |
| 20 | 21 | 🐳 [AWS ECS](<20. AWS ECS.md>) | ECS vs. Kubernetes/EKS · cluster creation · deploying an app via a task definition | ✅ |
| 21 | 22 | ☸️ [AWS EKS](<21. AWS EKS.md>) | Why EKS matters · Ingress & load balancing · full "2048 game" deployment on EKS with public/private subnets | ✅ |
| 22 | 23 | 🔐 [Secret Management on AWS](<22. Secret Management on AWS.md>) | Systems Manager vs. Secrets Manager vs. HashiCorp Vault · sensitivity + cost decision framework | ✅ |
| 23 | 25 | 📋 [AWS Config](<23. AWS Config.md>) | Compliance monitoring · live Config rule demo · Lambda function code walkthrough | ✅ |
| 24 | 26 | ⚖️ [AWS Load Balancers](<24. AWS Load Balancers.md>) | OSI model recap · ALB vs. NLB vs. GWLB, compared with real examples | ✅ |
| 25 | 28 | 🚚 [Migrating Applications to AWS Cloud](<25. Migrating Applications to AWS Cloud.md>) | Real-world migration case studies · five-stage framework (prepare, plan, migrate, monitor, optimize) · the 7 R's in context | ✅ |
| 26 | 30 | 🏛️ [Three-Tier Architecture Implementation on AWS](<26. Three-Tier Architecture Implementation on AWS.md>) | Three-tier architecture on AWS · when to (and not to) mention it in interviews · series finale | ✅ |

---
