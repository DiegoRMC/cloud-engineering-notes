# ☁️ AWS Cloud Practitioner Essentials Study Notes

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
- Applications can be tightly coupled (if a component fails it can cause issues) or **loosely coupled** (by a message queue). The second seems to be the better one.
- Services for loose coupling:
    - **SQS (Simple Queue Service):** Message queue (Buffers messages).
    - **SNS (Simple Notification Service):** "Megaphone" (Sends messages to subscribers/emails).
