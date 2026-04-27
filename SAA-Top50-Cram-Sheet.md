# Top 50 SAA repeated scenarios — one-page cram sheet

Globally ranked **1–50** (most exam‑common first). Format: short scenario → best AWS answer.

| Rank | Domain | Scenario | Solution |
|-----:|--------|----------|----------|
| 1 | Resilient | Relational DB must survive AZ outage with automatic failover | **RDS Multi‑AZ** or **Aurora** with Multi‑AZ / managed failover |
| 2 | Performant | Global users see slow page/API latency | **CloudFront** in front of **S3** and/or **ALB** |
| 3 | Secure | App needs AWS API access without long‑lived secrets on servers | **IAM role** attached to compute (**EC2/Lambda/ECS**); avoid access keys |
| 4 | Cost‑optimized | Objects hot briefly, then archived rarely accessed | **S3 Lifecycle** → **Standard‑IA/Glacier/Deep Archive** |
| 5 | Resilient | Web tier single point of failure | **ALB + Auto Scaling** across **≥2 AZs** |
| 6 | Performant | DB overloaded by **reads** | **Read Replicas** / **Aurora Replicas** (plus route reads correctly) |
| 7 | Resilient | Producer spikes crash consumers / tight coupling | **SQS** (often **DLQ**) between tiers |
| 8 | Secure | Private subnets must reach **S3/DynamoDB** without internet | **VPC Gateway Endpoints** |
| 9 | Secure | Encryption with auditable key policy control | **SSE‑KMS** (customer managed CMKs where required) |
| 10 | Performant | DynamoDB hot repeated reads need microsecond cache | **DAX** |
| 11 | Performant | Too many DB connections from many Lambdas/containers | **RDS Proxy** |
| 12 | Resilient | Regional disaster recovery for Aurora‑style DB | **Aurora Global Database** |
| 13 | Secure | Block common web exploits / rate abuse at layer 7 | **AWS WAF** on **CloudFront/ALB/API Gateway** |
| 14 | Cost‑optimized | Steady baseline + bursty overflow acceptable | **Savings Plans/RIs** for baseline + **Spot** for burst |
| 15 | Resilient | Multiple instances need shared POSIX files | **EFS** |
| 16 | Performant | Multi‑Region routing wants AWS backbone + static anycast entry | **Global Accelerator** |
| 17 | Secure | Org‑wide “cannot do X in any account” guardrails | **AWS Organizations SCPs** |
| 18 | Cost‑optimized | DynamoDB traffic wildly unpredictable | **On‑Demand** capacity |
| 19 | Performant | Ad‑hoc SQL over logs/data **in S3** | **Athena** |
| 20 | Resilient | Fan‑out events to many subscribers reliably | **SNS → multiple SQS subscribers** (fan‑out pattern) |
| 21 | Secure | Rotate DB/app secrets safely | **Secrets Manager** (rotation) or **SSM Parameter Store** (secrets tier) |
| 22 | Performant | Session/offload cache for DB/web reads | **ElastiCache (Redis/Memcached)** |
| 23 | Cost‑optimized | Unknown/changing access patterns in S3 | **S3 Intelligent‑Tiering** |
| 24 | Resilient | DNS failover between stacks/regions | **Route 53** routing policies + health checks |
| 25 | Secure | Strong DDoS protection + support posture | **Shield Advanced** (where justified) |
| 26 | Performant | REST APIs at scale with throttling/auth integration | **API Gateway** + **Lambda** (or HTTP integrations) |
| 27 | Cost‑optimized | Spiky intermittent workloads | Prefer **Lambda/Fargate**/managed services vs idle EC2 |
| 28 | Resilient | Long‑running workflows with retries/branching | **Step Functions** |
| 29 | Secure | Private connectivity to partner/VPC endpoint services | **VPC Interface Endpoint / PrivateLink** |
| 30 | Performant | Restrict CloudFront delivery to authorized viewers | **Signed URLs / Signed Cookies** |
| 31 | Resilient | Streaming ingestion + replay | **Kinesis Data Streams** (replay use‑cases) |
| 32 | Cost‑optimized | Large recurring data transfer out from **S3** | Reduce egress via **CloudFront caching**/architecture changes |
| 33 | Secure | Central audit trail across accounts | **Organization CloudTrail** trail + secure storage |
| 34 | Performant | Large uploads into **S3** across unstable/long‑haul networks | **S3 Transfer Acceleration** |
| 35 | Resilient | Predictable traffic ramps (known schedule) | **Scheduled Scaling** on ASG |
| 36 | Secure | Federated web/mobile sign‑in | **Cognito User Pools** |
| 37 | Cost‑optimized | Predictable steady DynamoDB workload | **Provisioned** + **Auto Scaling** / tuning |
| 38 | Performant | Full‑text search beyond Dynamo/SQL | **Amazon OpenSearch Service** |
| 39 | Resilient | Backup immutability / ransomware‑aware backups | **AWS Backup** + vault locks / restricted deletes (design‑dependent) |
| 40 | Secure | Find unintended external resource access | **IAM Access Analyzer** |
| 41 | Cost‑optimized | Right‑size block storage cost/perf | **gp3** baseline tuning vs provisioned IOPS where needed |
| 42 | Performant | Heavy SMB/NFS features beyond generic POSIX shared files | Choose **FSx** variant matching workload |
| 43 | Resilient | Active/passive multi‑Region app traffic shift | **Route 53** failover / weighted routing + replicated data tier |
| 44 | Secure | Break‑glass admin restrictions across OU accounts | **SCPs** + separation of duties + minimal privilege roles |
| 45 | Cost‑optimized | Infrequent jobs don’t justify always‑on containers | **EventBridge** schedules + **Lambda**/batch workers |
| 46 | Performant | Origin unhealthy—still serve cache/failover behavior | **CloudFront origin failover groups** (two origins) |
| 47 | Resilient | Message duplicates possible—workers must be idempotent | Design consumers **idempotent** + dedupe keys / conditional writes |
| 48 | Secure | HTTPS enforcement at the bucket edge | **Bucket policy** deny insecure transport (`aws:SecureTransport`) |
| 49 | Cost‑optimized | Snapshots piling up across Regions | **AWS Backup** copy rules / lifecycle + delete unused AMIs/snapshots |
| 50 | Performant | Repeated identical database queries from app tier | Cache layer (**ElastiCache**) or **DynamoDB DAX** for Dynamo |
