# AWS Certified Solutions Architect Associate (SAA-C03) Cheat Sheet

This cheat sheet is generated from an analysis of high-frequency exam patterns, grouped by the four main AWS SAA domains. 

## Domain 1: Design Resilient Architectures

| Scenario | Solution |
|---------|----------|
| **Decouple variable workloads** (e.g., sudden spikes of 100,000 messages/sec). | Use **Amazon SQS** with an Auto Scaling group of EC2 workers. SQS buffers the traffic, preventing overload. |
| **Ensure strict ordering of transactions** (e.g., ecommerce orders). | Use **Amazon SQS FIFO** queues to guarantee exactly-once processing and preserve order. |
| **Highly available shared file storage** across multiple EC2 instances in different AZs. | Use **Amazon EFS** (Elastic File System). It scales automatically and supports concurrent access across AZs. |
| **Recover database to a specific minute** (RPO of 15 minutes, RTO 1 hour). | Enable **Point-in-Time Recovery (PITR)** on **Amazon DynamoDB** or **Amazon RDS/Aurora** and restore to the desired time. |
| **Migrate on-premises Windows shared file storage** (SMB) to AWS. | Use **Amazon FSx for Windows File Server** with a Multi-AZ deployment. |
| **Process events from an S3 bucket redundantly** without dropping messages. | Configure S3 Event Notifications to send messages to an **Amazon SQS** queue, then process with Lambda. |
| **Decouple pub/sub architecture** with multiple consumer applications. | Publish messages to an **Amazon SNS** topic with multiple **SQS** queues subscribed to it (Fan-out pattern). |
| **Guarantee EC2 capacity** for an upcoming event in a specific AZ. | Create an **On-Demand Capacity Reservation** specifying the Region and AZ. |
| **Migrate large data files offline** (e.g., 50 TB+) when internet bandwidth is limited. | Use **AWS Snowball Edge**. (Transfer takes days but requires 0 network bandwidth). |

## Domain 2: Design High-Performing Architectures

| Scenario | Solution |
|---------|----------|
| **Serve static and dynamic web content globally** with minimal latency. | Use **Amazon CloudFront** as a CDN with an S3 bucket origin (static) and an **Application Load Balancer (ALB)** origin (dynamic). |
| **Analyze S3 log files directly** without loading them into a database. | Use **Amazon Athena** to run ad-hoc SQL queries directly against the JSON/CSV logs in S3. |
| **Ingest and buffer high-velocity streaming data** (e.g., IoT alerts, clickstreams). | Use **Amazon Kinesis Data Firehose** to capture the stream and automatically deliver to S3. |
| **Scale read-heavy database workloads** automatically. | Use **Amazon Aurora** (Multi-AZ) with **Aurora Auto Scaling** to add Aurora Read Replicas based on CPU/connection load. |
| **Provide low-latency hybrid cloud file access** (cache frequently used files on-prem). | Use **Amazon S3 File Gateway** (Storage Gateway) to back up to S3 while keeping a local cache. |
| **Accelerate global data uploads to a central S3 bucket.** | Enable **S3 Transfer Acceleration** on the destination bucket to use CloudFront Edge Locations for fast uploads. |
| **Run high-performance computing (HPC) workloads** requiring Lustre. | Use **Amazon FSx for Lustre**. Link it to an S3 bucket if needed. |
| **Route users to the closest AWS Region** using UDP traffic. | Use **AWS Global Accelerator** with a Network Load Balancer (NLB) to ensure the lowest latency and automated failover. |
| **Run containers without managing underlying EC2 infrastructure.** | Use **Amazon Elastic Container Service (ECS)** with **AWS Fargate** (Serverless compute for containers). |

## Domain 3: Design Secure Applications and Architectures

| Scenario | Solution |
|---------|----------|
| **Prevent accidental deletion of critical S3 objects.** | Enable **S3 Versioning** and **MFA Delete** on the bucket. |
| **Protect against large-scale DDoS attacks** across thousands of IP addresses. | Enable **AWS Shield Advanced** and attach it to your ALB or CloudFront distribution. |
| **Protect REST APIs from SQL injection and Cross-Site Scripting (XSS).** | Deploy **AWS WAF** (Web Application Firewall) on Amazon API Gateway or CloudFront. |
| **Securely manage and automatically rotate database credentials.** | Use **AWS Secrets Manager**. Attach an IAM role to instances allowing them to fetch the secret. |
| **Enforce WORM (Write-Once-Read-Many)** to prevent documents from being modified (Regulatory compliance). | Store documents in an S3 bucket with **S3 Object Lock** enabled (Compliance mode). |
| **Provide private access from EC2 to S3/DynamoDB** without going over the public internet. | Create a **VPC Gateway Endpoint** for S3 or DynamoDB and update the route table. |
| **Track AWS resource configuration changes** and API calls for auditing. | Use **AWS Config** (for configuration history) and **AWS CloudTrail** (for API activity). |
| **Inspect all VPC traffic using a third-party firewall appliance.** | Deploy a **Gateway Load Balancer (GWLB)** to route traffic through your security appliances. |
| **Discover and protect Personally Identifiable Information (PII)** in S3. | Enable **Amazon Macie** to scan S3 buckets and trigger SNS alerts if PII is detected. |

## Domain 4: Design Cost-Optimized Architectures

| Scenario | Solution |
|---------|----------|
| **Store objects with unpredictable or changing access patterns.** | Use **Amazon S3 Intelligent-Tiering**. It automatically moves data to cost-effective tiers without retrieval fees. |
| **Archive data indefinitely that is rarely accessed** (retrieval time of hours is fine). | Use an S3 Lifecycle policy to move the data to **S3 Glacier Deep Archive**. |
| **Run stateless, interruptible batch processing jobs** (e.g., image processing). | Use **Amazon EC2 Spot Instances** to get up to 90% discount compared to On-Demand instances. |
| **Run predictable, 24/7 production workloads** (e.g., DBs, core APIs). | Purchase **EC2 Reserved Instances (RIs)** or **Savings Plans** for 1- or 3-year terms. |
| **Host a static website** (HTML, CSS, JS, Images) cost-effectively. | Host it in an **Amazon S3 Bucket** configured for Static Website Hosting. Avoid EC2. |
| **Stop incurring charges for a test DB instance** that is only used 48 hours a month. | Take a snapshot of the DB, terminate the instance, and restore the snapshot when needed. |
| **Transfer data between a SaaS application (e.g., Salesforce) and S3** without custom code. | Use **Amazon AppFlow** to fully manage and automate the data transfer securely. |
| **Identify root causes of cost spikes** grouped by EC2 instance types. | Use **AWS Cost Explorer** with granular filtering by Instance Type. |
| **Avoid NAT Gateway data transfer costs** when accessing AWS services. | Use **VPC Endpoints** (Gateway for S3/DDB, Interface for others) to keep traffic on the AWS private network. |

---
**Exam Tip:** Always look for keywords like "most cost-effective" (Spot, S3 Standard-IA, Serverless), "least operational overhead" (Managed services like Fargate, Aurora, AppFlow), and "decouple" (SQS, SNS, EventBridge).
