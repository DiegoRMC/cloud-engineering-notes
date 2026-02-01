# ☁️ AWS Cloud Practitioner Essentials Notes

**Status:** In Progress

---

## Module 1: Introduction to AWS
- Cloud computing is the on-demand delivery of IT resources over the Internet with pay-as-you-go pricing. Basically you don't need to invest on an actual server, you simply have to pay based on the traffic you have.
- Deployment can be cloud based, on-premises or hybrid.
- An AWS customer still has a series of responsabilities when it comes to using the service.

## Module 2: Compute in the cloud
- Three types of ways to interact with the API:
  1. Windows-like (Management Console)
  2. Command line (CLI)
  3. Through external applications (SDKs: Java, Python, C++)
- How to launch EC2 and types of EC2 instances.
- AMI's (Amazon Machine Images) are needed to create an EC2 instance, they can be customized or even bought to third party creators.
- Pricing is mostly defined by commitment (On-Demand, Reserved, Spot, etc).
- The on-demand functions are handled by the ELB (Elastic Load Balancing) and Auto Scaling groups.
- Applications can be **tightly coupled** (if a component fails it can cause issues) or **loosely coupled** (by a message queue). The second seems to be the better one.
- Services for loose coupling:
    - **SQS (Simple Queue Service):** Message queue (Buffers messages).
    - **SNS (Simple Notification Service):** "Megaphone" (Sends messages to subscribers/emails).

## Module 3: Exploring Compute Services
- We can differenciate between 3 main service types:
  - Uunmanaged services (EC2)
  - Managed services (ELB, SNS and SQS)
  - Serverless (AWS Lambda)
- AWS Lambda:
    - Works with triggers and Lambda functions
    - Max duration of a Lambda function is 15m, useful for quick stuff
- Containers enssure the portability of programs by bundling the application with all of its dependencies and configuration, so it runs the same across different environments.
- Container orchestration services manage them. AWS has two main services for this:
  - Amazon Elastic Container Service (ECS)
  - Amazon Elastic Kubernetes Service (EKS)
- The difference is complexity and control, EKS giving you the bigger ammount of it.
- For these to work you need a container registry like ECR (Amazon Elastic Container Registry).
- You can run your containers on EC2 (unmanaged option) or AWS Fargate (serverless option).
- More compute services to be aware of:
  -  AWS Elastic Beanstalk (for easier EC2 configuration purposes and automation)
  -  AWS Batch (a fully managed service for batch computing workloads)
  -  Amazon Lightsail (simplified web application hosting)
  -  AWS Outposts Family (for consistency on hybrid infrastructures)

## Module 4: Going Global
- How to choose a Region:
  1. Compliance
  2. Proximity
  3. Feature avaibility
  4. Pricing
- Regions are made of various Availability Zones, having multi-AZs and Regions deployed provide redundancy.
- We also have Edge locations which are independent from Regions, these run various services like AWS Outposts, Amazon CloudFront (content delivery network), AWS Global Accelerator or Amazon Route 53 (DNS).
- To manage multiple resources across different Regions you use Infrastructure as Code (IaC) which work as blueprints for setups.
- AWS CloudFormation is an IaC tool we use for this.

## Module 5: Networking
- **VPC (Virtual Private Cloud):** An isolated section of the AWS Cloud where you can launch resources in a virtual network that you define.
- **Subnets:** Slicing your network into different parts (public and private) to control access.
- **Internet Gateway:** A component attached to your VPC to allow communication between your instances and the internet.
- **Connectivity Options:**
  - **AWS Client VPN:** Connects individual users (remote workers) to AWS or on-premises networks.
  - **AWS Site-to-Site VPN:** Connects your corporate data center to your VPC.
  - **AWS PrivateLink:** Simplified private connection between VPCs and services. Keeps traffic within the AWS network (doesn't traverse the public internet).
  - **AWS Direct Connect:** A dedicated physical fiber connection from your premises to AWS (increased bandwidth and consistency).
- **Network Access Control List (Network ACL):** Acts as a firewall for **subnets**. Controls traffic entering and leaving a network.
  - *Key Characteristic:* **Stateless** (It has no memory; you must explicitly allow return traffic).
- **Security Groups:** Acts as a firewall for **instances** (EC2).
  - *Key Characteristic:* **Stateful** (It has memory; if you allow an incoming request, the response is automatically allowed).
- **Basic Infrastructure:** A basic level structure includes subnets (private and public ones on different AZs), an internet gateway, and route tables.
- **Content Delivery & Routing:**
  - **Amazon Route 53:** AWS DNS service. Translates domain names into IP addresses.
  - **Amazon CloudFront:** A Content Delivery Network (CDN) that delivers data/video globally with low latency.
  - **AWS Global Accelerator:** Improves availability and performance using the AWS global network infrastructure.

## Module 6: Storage
- **Three types of data:**
  - **Block Storage:** Data divided into pieces (blocks). Fast, low latency.
  - **Object Storage:** Data + Unique ID + Metadata. Requires full rewrite to update. Organized using buckets. Best for files that don't change much ("write once, read many").
  - **File Storage:** Best for shared access across multiple instances.
- **Block Storage Options:**
  - **EC2 Instance Store:** Ephemeral storage physically attached to the host.
    - *Risk:* Data **does not persist** when the EC2 gets stopped or terminated. Good for buffers/cache.
  - **Amazon Elastic Block Store (Amazon EBS):** Persistent block storage volumes for EC2.
    - Data persists even if the instance stops.
    - **Snapshots:** Incremental backups (saves only what has changed since the last snapshot).
    - **Amazon Data Lifecycle Manager:** Automates the creation/retention of EBS snapshots.
- **Object Storage Options:**
  - **Amazon S3 (Simple Storage Service):**
    - Virtually unlimited amount of data.
    - Managed service.
    - Highly reliable (11 9s of durability).
    - **Security:** Private by default, supports bucket policies, time-limited URLs, audit logs, and encryption.
    - **Use cases:** Content distribution, hosting static websites, backups, and media files.
    - **Storage Classes:**
      - **S3 Standard:** For frequently accessed data (dynamic websites, cloud apps).
      - **S3 Standard-IA:** Infrequent access but rapid retrieval (backups).
      - **S3 Glacier:** For archiving. Very low cost, retrieval times vary from minutes to hours.
      - **S3 One Zone-IA:** Stores data in a single Availability Zone (cheaper, but risks data loss if that AZ fails).
    - **Management:**
      - **Lifecycle policies:** Move data between classes based on rules.
      - **S3 Intelligent-Tiering:** Automatically moves data to the most cost-effective tier based on access patterns.
- **File Storage Options:**
  - **Amazon EFS (Elastic File System):** Scalable file storage for **Linux** EC2 instances. Region-wide and fully managed.
  - **Amazon FSx:** File system for business applications. Use this if you need **Windows** File Server (SMB) or high-performance computing (Lustre).
- **Additional Storage Solutions:**
  - **AWS Storage Gateway:** Bridges on-premises environments with cloud storage. Types: S3 (File Gateway), Volume (Block Gateway), and Tape Gateway.
  - **AWS Elastic Disaster Recovery (DRS):** Minimizes downtime and data loss with fast recovery of on-premises/cloud-based applications.

# Module 7: Databases
- Types of AWS Services based on the ownership of administrative tasks. (applied to databases, tho it works pretty much the same in other areas)
  - Fully managed --> Fully managed AWS database services only require customers to be responsible for designing data structures and managing access controls.
  - Managed --> AWS handles routine tasks like backups, patching, and hardware provisioning.
  - Unmanaged --> With unmanaged databases, customers are responsible for installation, configuration, patching, maintenance tasks, database security, backups, high availability setup, and performance optimization.
- **Relational databases** (RDBMS)
  - Examples are MySQL, PostgreSQL, Microsoft SQL Server...
  - AWS DMS (Database Migration Service) to migrate from on-premises.
  - Amazon RDS is their relational database. (managed)
  - Amazon Aurora (MySQL and PostgreSQL compatible - Up to 5x faster than standard MySQL)
- **No SQL or non-relational databases**
  - Better performance and flexibility in some situations.
  - Amazon DynamoBD (fully managed serverless)
- **Caching in relational databases**
  - High traffic or complex queries performance affects performance, caching layers give a solution to this, storing frequently accessed data on quick access memory.
  - Redis OSS, Valkey and Memcached are common caching tools.
  - Amazon's is Elasticache.
- **Other DB related services**
  - For semi-structured databases you have Amazon DocumentDB (useful for content management or catalogs)
  - Amazon Neptune is a graph database, to store and manage interconnected data, pattern recognition and fraud detection.
  - Amazon Managed Blockchain.
  - Accelerators (DAX)
  - AWS Backup (works with hybrid environments too)

# Module 8: AI/ML and Data Analytics
- **AI:** A broad field focused on the development of intelligent computer systems capable of performing humanlike tasks.
- **ML:** A type of AI for training machines to perform complex tasks without explicit instructions.
- **Models** are trained on patterns and then used into new data.
- **Amazon's services can be divided into:** AI Services (already trained), ML Services (you can build, train and deploy), ML Frameworks and Infrastructures (for more specific tasks)
  - **Tier 1 (Pre-built AWS AI services) uses:**
    - Language services (Amazon Comprehend, Amazon Polly), Amazon Transcribe, Amazon Translate)
    - Computer vision and search services (Amazon Kendra, Amazon Rekognition, Amazon Textract)
    - Conversational AI and personalization services (Amazon Lex, Amazon Personalize)
  - **Tier 2: ML services**
    - Amazon SageMaker AI is the service here, it offers a fully managed infrastructure where you can build, train, and deploy your own ML models.
  - **Tier 3: ML frameworks and infrastructure**
    - ML frameworks: It's a software library or tool that provides experienced ML practitioners with pre-built, optimized components for building machine learning models. Supports PyTorch, Apache M-X Net, and TensorFlow.
    -  AWS ML infrastructure: ML-optimized Amazon Elastic Compute Cloud (Amazon EC2) instances, Amazon EMR, and Amazon Elastic Container Service (Amazon ECS), can support these custom solutions.
- If we add Deep Learning to Mahcine Learning we get Generative AI, which goes beyond, being capable of creating new content and adapt to new tasks.
  - Amazon SageMaker JumpStart (ML hub with FMs and pre-built ML solutions deployable with a few clicks)
  - Amazon Bedrock (fully managed service for adapting and deploying FMs from Amazon and other leading AI companies)
  - Amazon Q (interactive AI assistant that can be integrated with a company's information repositories) which is split into Business (for problem solving) and Developer (for coding)
- ETL is the process where you **E**xtract data, **T**ransform it into usable formats and **L**oad it into a destination.
- Data Pipelines automate this process.
  - Data is initially stored in either data lakes (S3, flexible) or data warehouses (Redshift, more structured)
  - Ingestion:
    - In real time and from multiple datesets (Amazon Kinesis Data Streams)
    - Amazon Data Firehose (near real time)
  - Catalogation and processing
    - AWS Glue and Glue Data Catalog (Catalog catalogs, just Glue analyzes)
    - Amazon EMR (the alternative, for people who know waht they're doing)
  - Data usage/analysis
    - Amazon Athena (fully managed serverless service)
    - Amazon Redshift (fully managed but suited for more complex and higher loads)
  - Visualization
    - Amazon QuickSight (dashboards and reports)
    - Amazon OpenSearch Service (monitoring and analytics)

# Module 9: Security
- Authentication: Verifying the identity of an user in a network.
- Authorization: Granting authenticated users access rights and pemissions.
  - Preventing unauthorized access:
    - Root user (owner of the account, cannot be restricted) obviously needs strong security to access, token, password, MFA. Not recommended for general use, can mess up easily.
    - AWS IAM: Create users, start with zero permissions and you give them permissions explicitly. IAM Groups and policies also exist. Roles are like users or groups but temporary.
    - The principle of least privilege dictates that you should only give people and systems access to what they need and nothing else.
    - AWS IAM Identity Center is the service for managing all of this.
  - Protecting Networks an Applications:
    - DDoS (Overwhelming a system with requests from multiple devices to deny a service, UDP Flood for example, where they send the answer to an UDP request to you, overwhelming you)
    - Low-level brute force attacks are repelled by default by AWS's infreastructure (Regions, Security Groups, ELB) and AWS Shield Standard.
    - Extra security services include AWS Shield (for most services=) and AWS WAF (for web)
  - Data protection
    - Encryption (you have the right key, you can access the data)
      - **At rest:** Data is idle (S3 buckets already have it when an object is added, same with DynamoDB, instance store...)
      - AWS Key Management Service (KMS) is the service used to encrypt and decrypt your data, disable keys, work with IAM...
      - **In transist:** While data is being transported by using SSL and TLS certificates to establish encrypted network connections.
      - AWS Certificate Manager (ACM) is the service used here.
  - Detecting and Responding to Security Incidents
    - Amazon Inspector (automatic security assesment, more like a security counselor)
    - Amazon GuardDuty (uses AI to pretty much do the same)
    - Amazon Detective (find the root cause for detected security issues)
    - AWS Security Hub (groups, monitors and overall provides a clear view of your security parameters coming from distinct services)

# Module 10: Monitoring, Compliance and Governance in the AWS Cloud
- General progression when it comes to it: Secure --> Monitor --> Audit --> Compliance
- Monitoring: Observing systems, collecting metrics and then using data to make decisions
- Amazon CloudWatch is the centralized service that takes care of monitoring
    - Metrics: Variables tied to your resources
    - CloudWatch alarms let you set thresholds tied to metrics and send notifications or make changes to resources when met.
    - A dashboard provides a quick view of your aplications, infrastructure and services
    - For going further (more specific or older data) you check the logs
- AWS CloudTrail is an audit log of the AWS API calls.
- Compliance in the EU responds to GDPR
- AWS pretty much does it for you, you just have to be aware of which compliances apply to you and make some research. With AWS Artifact you can check compliance reports.
- AWS Config helps you assess, audit and evaluate your resources so your solutions are in compliance with internal policies and regulations.
- AWS Audit Manager is a service that assesses your policies, manages reviews and build reports. (basically collects your data for evidence against inspections)
- AWS Organizations (account management service as you grow and scale)
- Governance:
  - Governance is a framework to manage your IT goals with policies, processess and structures to ensure adherence
  - AWS Control Tower: A service you can use to set up and govern a secure, compliant, multi-account AWS environment based on best practices
  - AWS Service Catalog: A service you can use to create, share, and organize AWS services and resources from a curated catalog that you define
  - AWS License Manager: A service that helps you manage your software licenses and fine-tune licensing costs
- AWS Health Dashboard (not for actual health, but for service events, troubleshooting...
- AWS Trusted Advisor (name pretty much says it)  works for cost optimization, security, performance...
- IAM Access Analyzer (like Trusted Advisor but for IAM

# Module 11: Pricing and Support
- Pricing
  - Pay as you go (you only pay for resources you consume)
  - Save whe nyou commit (classic, subscribe for a year rather than monthly and get a discount)
  - Pay less by using more (volume base discounts)
  - Main pricing drivers: Compute, storage and data transfer.
  - Services that help with billing include: AWS Organizations, AWS Billing and Cost Management Console Home, AWS Budgets, AWS Cost Explorer and AWS Pricing Calculator.
- Support:
  - Tiers:
    - Basic Support: automatic, no cost, documentation, forums, core Trusted advisor...
    - Developer Support: also direct email, for experimenting or getting started
    - Business Support: Phone access, 4 to 1h reponse depending on case
    - Enterpirse Support: Reduced response time, account managers, Technical Account Managers (TAMs)...
  - AWS Marketplace and AWS Partner Network (you can basically  buy software from individual vendors)
- Cost Optimization:
  - Rightsizing: Matching the needs of your workload
  - Lifecycling policies and compressing for storage...

# Module 12: Migrating to the AWS Cloud
- The phases of migration are: Assess, Mobilize, Migrate and modernize.
- Readiness, a lot of stakeholders and variables take action here. (AWS CAF is a framework that help prepare for migrations)
- There are 7 possible migration strategies depending on what you're moving:
  - Relocate (you simply change hosts, good if your applications are VMs or containers)
  - Rehost (change host without changing anything in your applications, lift and shift)
  - Replatform (same as rehost but you do make a few changes to your applications, lift, tinker and shift)
  - Refactor (this is not just tinkering a few things in your applications, this is actually changing them to sometimes work completely different)
  - Repurchase (this means changing software, for example you go from SQL Server with an AWS cloud based one)
  - Retain (you keep critical applications on-premises)
  - Retiring (you remove applications that are no longer needed)
- Migration services and tools:
  - AWS Application Discovery (literally what the name says, provides an snapshot of what you have, checking dependencies, inventory, map connections...)
  - AWS Application Migration Service (what actually makes the migration, some benefits include you can mantain normal bussiness while moving)
  - Migration Evaluator (helps understand potential savings, bugeting...)
  - AWS Migration Hub (unified view of all tasks and progress tracking)
- Database migrations:
  - AWS Database Migration Service (Works on SQL, non-SQL, warehouses...), works by replication and has high availavility even during the process.
  - If the migration includes changing the database engine you need the schema and use AWS Schema Conversion Tool, anything that's not automatically migrated needs to be done manually.
  - Online data transfers (from or to the cloud):
    - AWS DataSync (simply helps with moving and takes care of some extras like security)
    - AWS Transfer Family (fully managed and simple)
    - Direct Connect (for VPCs) also works
  - if you want offline data transfers you can use Snowball Edge Storage Optimized devices.

# Module 13: Well-Architected Solutions
- **AWS Well-Architected Framework** (helps evaluate the quality of your designs ensuring they align with best practices):
  - 6 Pillars:
    - Operational excellence (automation, event response...)
    - Security 
    - Reliability (recovery planning and ability to withstand failures)
    - Performance efficiency (efficient use of resources, making informed decisions)
    - Cost optimization
    - Sustainability
- Some other AWS services:
  - Development services: AWS CodeBuild, AWS CodePipeline, AWS X-Ray, AWS AppSync, AWS Amplify.
  - Business services: Amazon Connect, Amazon Simple Email Service.
  - End-user computing services: Amazon AppStream 2.0, Amazon Workspaces, Amazon WorkSpaces Web.
  - IoT services: AWS IoT Core
