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

تمام 👍 ده **Part 1 (Q1 → Q25)** بنفس الجدول الأول وبالإنجليزي فقط 👇

---

# AWS SAA-C03 Cheat Sheet

## Part 1 (Q1 → Q25)

| #  | Scenario                                                      | Solution                                          |
| -- | ------------------------------------------------------------- | ------------------------------------------------- |
| 1  | Aggregate large data from multiple continents quickly into S3 | S3 Transfer Acceleration + Multipart Upload       |
| 2  | Analyze JSON logs in S3 with minimal changes                  | Amazon Athena                                     |
| 3  | Restrict S3 access to accounts within organization            | aws:PrincipalOrgID in bucket policy               |
| 4  | EC2 access S3 without internet                                | S3 Gateway VPC Endpoint                           |
| 5  | Multiple EC2 instances need shared file access                | Amazon EFS                                        |
| 6  | Migrate 70TB data with minimal bandwidth usage                | AWS Snowball Edge                                 |
| 7  | High throughput messaging with multiple consumers             | SNS + SQS (fan-out pattern)                       |
| 8  | Handle variable workloads with scalable processing            | SQS + Auto Scaling EC2                            |
| 9  | Hybrid file storage with hot and cold data                    | S3 File Gateway + Lifecycle                       |
| 10 | Ensure ordered processing (e.g., orders)                      | SQS FIFO + Lambda                                 |
| 11 | Manage database credentials with low overhead                 | AWS Secrets Manager                               |
| 12 | Improve global latency (static + dynamic content)             | CloudFront + S3 + ALB                             |
| 13 | Rotate secrets across multiple regions                        | Secrets Manager (multi-region replication)        |
| 14 | Scale read-heavy database workloads                           | Aurora + Read Replicas                            |
| 15 | Inspect and filter VPC traffic                                | AWS Network Firewall                              |
| 16 | Data visualization and dashboards                             | Amazon QuickSight                                 |
| 17 | Secure EC2 access to S3                                       | IAM Role                                          |
| 18 | Event-driven image processing                                 | S3 → SQS → Lambda                                 |
| 19 | Traffic inspection using appliance                            | Gateway Load Balancer                             |
| 20 | Fast cloning of EBS data                                      | Snapshot + Fast Snapshot Restore                  |
| 21 | Highly scalable web application                               | S3 + CloudFront + API Gateway + Lambda + DynamoDB |
| 22 | Optimize storage for unknown access patterns                  | S3 Intelligent-Tiering                            |
| 23 | Archive data after 1 month                                    | Lifecycle → Glacier Deep Archive                  |
| 24 | Analyze EC2 costs                                             | Cost Explorer                                     |
| 25 | Improve Lambda scalability with DB                            | Decouple using SQS                                |

---
Got it 👍 — continuing in **English only** and keeping the same clean table format.

---

# AWS SAA-C03 Cheat Sheet (Table Format)

## Part 2 (Q26 → Q50)

| #  | Scenario                                      | Solution                                  |
| -- | --------------------------------------------- | ----------------------------------------- |
| 26 | Track S3 config changes                       | AWS Config                                |
| 27 | Share CloudWatch dashboard externally         | Share dashboard via email link            |
| 28 | SSO with on-prem AD across accounts           | AWS SSO + Directory Service               |
| 29 | Route users to lowest latency region (UDP)    | Global Accelerator + NLB                  |
| 30 | Reduce DB cost (monthly usage)                | Snapshot + terminate + restore            |
| 31 | Enforce resource tagging                      | AWS Config rules                          |
| 32 | Host static website cheapest way              | Amazon S3                                 |
| 33 | Real-time transaction processing              | Kinesis Data Streams + Lambda + DynamoDB  |
| 34 | Track config + API activity                   | AWS Config + CloudTrail                   |
| 35 | Protect against DDoS                          | AWS Shield Advanced                       |
| 36 | —                                             | —                                         |
| 37 | Secure EC2 remote access                      | SSM Session Manager                       |
| 38 | Reduce global latency (static site)           | CloudFront                                |
| 39 | Improve DB I/O performance                    | Provisioned IOPS (PIOPS)                  |
| 40 | Ingest streaming data + archive after 14 days | Kinesis Firehose + S3 + Lifecycle         |
| 41 | Improve SaaS → S3 performance                 | AWS AppFlow + SNS                         |
| 42 | Reduce S3 data transfer cost                  | S3 Gateway Endpoint                       |
| 43 | Reduce bandwidth usage to AWS                 | AWS Direct Connect                        |
| 44 | Prevent S3 accidental deletion                | Versioning + MFA Delete                   |
| 45 | Prevent data loss in ingestion workflow       | SNS → SQS → Lambda                        |
| 46 | Detect PII in uploads                         | Amazon Macie + SNS                        |
| 47 | Guarantee EC2 capacity                        | On-Demand Capacity Reservation            |
| 48 | Highly available file storage                 | Amazon EFS                                |
| 49 | Query recent data fast + archive old          | S3 Intelligent-Tiering + Glacier + Athena |
| 50 | Patch 1000 EC2 بسرعة                          | SSM Run Command                           |

---

# AWS SAA-C03 Cheat Sheet

## Part 3 (Q51 → Q75)

| #  | Scenario                          | Solution                         |
| -- | --------------------------------- | -------------------------------- |
| 51 | Scheduled report + email          | EventBridge + Lambda + SES       |
| 52 | Scalable file system              | EC2 + EFS                        |
| 53 | Store data 10 years (immutable)   | S3 Object Lock + Glacier         |
| 54 | Windows shared storage            | FSx for Windows                  |
| 55 | Restrict DB access from EC2       | Security Groups                  |
| 56 | Custom domain for API Gateway     | ACM + Route 53                   |
| 57 | Detect inappropriate images       | Rekognition                      |
| 58 | Run containers without infra mgmt | ECS Fargate                      |
| 59 | Process 30TB clickstream/day      | Kinesis + Firehose + Redshift    |
| 60 | Force HTTPS                       | ALB redirect rule                |
| 61 | Secure DB credentials             | Secrets Manager                  |
| 62 | External SSL cert with ALB        | ACM import + EventBridge alert   |
| 63 | Convert files on upload           | S3 + Lambda                      |
| 64 | Hybrid Windows file system        | FSx + File Gateway               |
| 65 | Extract PHI from documents        | Textract + Comprehend Medical    |
| 66 | Optimize S3 cost (hot → cold)     | Lifecycle → S3 IA                |
| 67 | Prevent duplicate processing      | Increase SQS visibility timeout  |
| 68 | Hybrid low latency connection     | Direct Connect + VPN backup      |
| 69 | High availability DB              | RDS Multi-AZ + Proxy             |
| 70 | Detect app health issues          | ALB + health checks              |
| 71 | DB recovery (RPO/RTO)             | DynamoDB PITR                    |
| 72 | Reduce S3 data transfer cost      | VPC Endpoint                     |
| 73 | Secure SSH via bastion            | SG rules (bastion → private EC2) |
| 74 | Secure web → DB access            | SG (443 public, DB private)      |
| 75 | Modern decoupled architecture     | API Gateway + Lambda + SQS       |

---

# AWS SAA-C03 Cheat Sheet

## Part 4 (Q76 → Q100)

| #   | Scenario                             | Solution                                  |
| --- | ------------------------------------ | ----------------------------------------- |
| 76  | Transfer large sensitive data        | DataSync + Direct Connect                 |
| 77  | Real-time ingestion pipeline         | API Gateway + Kinesis + Firehose + Lambda |
| 78  | Retain DynamoDB data 7 years         | AWS Backup                                |
| 79  | Unpredictable workload DB            | DynamoDB On-Demand                        |
| 80  | Share encrypted AMI                  | Share KMS key + AMI                       |
| 81  | Stateless scalable processing        | SQS + Auto Scaling EC2                    |
| 82  | Cert expiration alert                | EventBridge + Lambda + SNS                |
| 83  | Improve latency (global users)       | CloudFront                                |
| 84  | Optimize EC2 cost                    | Reserved (prod) + On-Demand (dev)         |
| 85  | Prevent file modification            | S3 Object Lock                            |
| 86  | Secure DB access + rotation          | Secrets Manager                           |
| 87  | Prevent data loss during DB downtime | SQS buffer                                |
| 88  | Reduce S3 transfer cost              | Requester Pays                            |
| 89  | Prevent S3 deletion                  | Versioning + MFA Delete                   |
| 90  | Improve DB read performance          | Read Replica                              |
| 91  | Private S3 access                    | VPC Endpoint                              |
| 92  | Secure EC2 → S3 access               | Endpoint + Bucket Policy                  |
| 93  | Reduce DB export latency             | Aurora + cloning                          |
| 94  | Event-driven file processing         | S3 → SQS → Lambda                         |
| 95  | Separate read/write DB traffic       | Read Replicas                             |
| 96  | Restrict EC2 actions by IP           | IAM policy condition                      |
| 97  | Windows shared storage (AD)          | FSx for Windows                           |
| 98  | Duplicate Lambda execution           | Increase visibility timeout               |
| 99  | HPC file system                      | FSx for Lustre                            |
| 100 | Secure certificate handling          | Secrets Manager / secure storage          |

---

## 🔥 Final High-Frequency Patterns

* **Decoupling → SQS**
* **Fan-out → SNS + SQS**
* **Event processing → S3 + Lambda**
* **Streaming → Kinesis**
* **Shared storage → EFS / FSx**
* **Credentials → Secrets Manager**
* **Global performance → CloudFront**
* **Private access → VPC Endpoint**
* **Cost optimization → Lifecycle policies**


---
**Exam Tip:** Always look for keywords like "most cost-effective" (Spot, S3 Standard-IA, Serverless), "least operational overhead" (Managed services like Fargate, Aurora, AppFlow), and "decouple" (SQS, SNS, EventBridge).
