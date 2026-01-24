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
    


