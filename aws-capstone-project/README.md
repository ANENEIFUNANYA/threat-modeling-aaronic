# AWS Integrated GRC Platform Capstone Project

## What I Was Solving

Modern cloud environments change continuously, a configuration that passes a compliance check at 9am can be modified by 9:05am. Traditional GRC approaches built around periodic audits cannot keep up. This capstone project addresses that directly: deploying a fully functional, automated GRC platform on AWS that monitors compliance continuously, logs every change with a full audit trail, and alerts the right people when controls start failing.

## Project Context

This project was completed as part of the capstone program at the International Cybersecurity and Digital Forensics Academy (ICDFA).

The capstone provided a reference architecture, infrastructure templates, application components, and testing requirements. My role was to deploy, configure, troubleshoot, and validate the solution within a personal AWS Free Tier environment, ensuring the multi-service cloud infrastructure functioned correctly end-to-end.

Through this project, I gained practical experience with cloud governance, compliance monitoring, infrastructure as code, AWS security services, and troubleshooting complex deployment issues within a simulated real-world environment.


## Frameworks Applied

| **Framework** | How it maps to this deployment |
|---|---|
| **ISO 27001:2022** | AWS CloudTrail enables audit logging and accountability, IAM enforces least-privilege access control, and AWS KMS supports encryption and data protection requirements |
| **NIST CSF** | AWS Config supports continuous monitoring (Detect), CloudWatch alarms enable alerting for anomalies (Detect), and EventBridge supports automated response actions (Respond) |
| **PCI DSS 3.2.1** | IAM access control, VPC network isolation, and KMS encryption support secure handling and protection of sensitive data environments|
| **HIPAA** | Encryption at rest and in transit, combined with audit logging and access controls, align with safeguards for protecting sensitive health information |
| **GDPR** | Centralized logging and audit trails support accountability, traceability, and evidence of data handling activities |
| **SOC 2** | Continuous monitoring through CloudWatch, automated compliance checks, and centralized logging support security, availability, and operational monitoring principles |


## Deployment Summary

Five structured phases plus four post-deployment components, across 13+ AWS services:

**Phases:**
- **Phase 1 — Network:** Deployed a VPC (10.0.0.0/16) with public/private subnets, NAT Gateway, and Application Load Balancer
- **Phase 2 — Database:** Deployed RDS MySQL, DynamoDB compliance table, KMS encryption keys, and S3 buckets
- **Phase 3 — Lambda:** Created a compliance monitoring function and IAM roles with least-privilege policies
- **Phase 4 — AWS Config:** Configured a recorder tracking 593+ resource types continuously
- **Phase 5 — Database Init:** Loaded sample GRC data via a custom Lambda function inside the VPC

**Post-deployment:** CloudTrail audit logging · CloudWatch compliance alarm (<80% threshold) · EventBridge hourly automation · Five purpose-specific S3 evidence buckets

**Results:** 5/5 phases complete · 13+ AWS services deployed · 22/22 automated tests passing, confirming full platform operation · Entire deployment within AWS Free Tier


## Challenges I Diagnosed and Solved

None of these appear in the deployment guide. Two were caused by bugs in the ICDFA-provided templates.

**Challenge 1 — CloudFormation ROLLBACK_COMPLETE**

The database stack failed immediately. I ran `describe-stack-events` and found the provided template contained `KMSMasterKeyID` instead of the correct `KMSMasterKeyId` (lowercase `d`) AWS property names are case-sensitive. Fixing that revealed a second bug: `BackupRetentionPeriod: 30` exceeded the Free Tier RDS limit. After multiple rounds of fixes each revealing a new template inconsistency, I made a deliberate decision to adopt a colleague's tested configuration, substituting my own environment-specific parameters. The stack returned `CREATE_COMPLETE` immediately. Knowing when to stop debugging a compounding problem is itself a practical skill.

**Challenge 2 — RDS Private Subnet: No External Connectivity**

Every attempt to connect to the MySQL database from outside AWS failed with errors 10060/2002. I worked through it systematically: confirmed the security group allowed port 3306, verified the RDS instance was publicly accessible, then examined the subnet route table. The problem was architectural, the private subnet routed all traffic through a NAT Gateway, not an Internet Gateway. External connections had no path in regardless of security group rules.

The solution was deploying a custom `grc-db-loader` Lambda function inside the same VPC. Lambda running within the VPC boundary could reach the private RDS endpoint directly, retrieved the SQL file from S3, and executed all statements internally,  status 200, database fully isolated, no security compromises.

**Challenge 3 — AWS Policy Names Had Changed**

The deployment guide instructed me to attach `ConfigUserAccess` and `AWSConfigRole` to service roles. Both had been renamed by AWS after the course material was written. I ran a CLI policy search to find the current names (`AWSConfigUserAccess` and `AWS_ConfigRole`), verified the ARNs, and attached them successfully.


## What I Learned

- **`describe-stack-events` was more useful than any troubleshooting guide.** Reading what AWS actually reported made the fix obvious, root cause analysis is the skill; the fix is just the result of it.
- **Private subnets are correct GRC practice, but require deliberate connectivity planning.** The challenge it created had a cloud-native solution that required no security compromises.
- **GRC automation changes the compliance model fundamentally.** A CloudWatch alarm catching a control failure within an hour is a different security posture than a quarterly audit catching it three months later.
- **AWS documentation goes out of date.** Policy names change, properties get renamed. Verifying current names via CLI is more reliable than trusting any fixed document.


## Skills Demonstrated

- Cloud infrastructure deployment (AWS, IaC)
- Compliance framework mapping (ISO 27001, NIST CSF, PCI DSS, HIPAA, GDPR, SOC 2)
- Root cause analysis and technical troubleshooting
- Least-privilege IAM design
- Continuous compliance monitoring and automated alerting
- Audit trail and evidence repository management

## Tools and Technologies

**AWS Services:** CloudFormation · VPC · RDS MySQL · Lambda · S3 · DynamoDB · KMS · AWS Config · CloudTrail · IAM · CloudWatch · SNS · EventBridge

**Deployment commands:** Command Prompt, PowerShell

**Course-provided components deployed:** Python (Lambda function) · SQL (database initialisation) · YAML (CloudFormation templates)

**Compliance Frameworks:** ISO 27001:2022 · NIST CSF · PCI DSS 3.2.1 · HIPAA · GDPR · SOC 2



## Artifacts

- `CapStone_Project_Technical_Report_pdf` Full technical report: all five phases, challenges, and test results
- `CapStone_Executive_Report_pptx` Executive summary presentation

