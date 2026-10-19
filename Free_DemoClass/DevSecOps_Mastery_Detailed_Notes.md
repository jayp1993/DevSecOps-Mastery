# DevSecOps Mastery — Detailed Notes

> Expanded notes based on the uploaded **DevSecOps Mastery – Free Demo Class** PDF. The original diagram covers application architecture, HLD/LLD, cloud infrastructure, IaC, Git, CI/CD, security, and operations. Explanations and examples below expand those topics for learning and demo-class delivery.

## Contents

1. Software Development Lifecycle
2. DevOps and DevSecOps
3. Application Architecture: 1-Tier, 2-Tier, 3-Tier
4. Microservices Architecture
5. HLD vs. LLD
6. On-Premises vs. Cloud
7. IaaS, PaaS, and SaaS
8. Manual Provisioning vs. Automation
9. Infrastructure as Code (IaC)
10. Terraform Fundamentals
11. Git and Source Code Management
12. CI/CD Pipelines
13. DevSecOps Security Practices
14. Observability and Troubleshooting
15. Practical Azure Project
16. Interview Questions
17. Suggested Learning Roadmap

---

## 1. Software Development Lifecycle

A software application usually passes through these stages:

1. **Requirement and planning:** Understand customer needs, scope, constraints, and success criteria.
2. **Architecture and design:** Decide application components, data flow, security boundaries, and infrastructure.
3. **Development:** Frontend and backend developers implement application functionality; database engineers design data storage.
4. **Build and test:** Compile or package code and run automated tests.
5. **Security validation:** Check source code, dependencies, secrets, containers, and infrastructure configuration as appropriate.
6. **Infrastructure and deployment:** Provision infrastructure and deploy the application.
7. **Monitoring and maintenance:** Observe performance, troubleshoot incidents, patch systems, and improve reliability.

### Why DevOps?

When development and operations teams work in silos, releases can be slow, environments can differ, and deployment issues can be difficult to diagnose. DevOps promotes collaboration, automation, repeatable releases, and fast feedback.

**Example:** An e-commerce team pushes a new search feature to a Git repository. A pipeline tests and packages the code, validates infrastructure changes, deploys the feature, and monitors the result.

## 2. DevOps and DevSecOps

### DevOps

DevOps combines development and operations practices to improve collaboration and the reliability and speed of software delivery.

### DevSecOps

DevSecOps integrates security into the software delivery lifecycle instead of waiting until the end for a security review.

| Lifecycle activity | Example security practice |
|---|---|
| Coding | Identify insecure coding patterns |
| Source control | Detect committed secrets |
| Build | Scan dependencies |
| Container creation | Scan container images |
| Infrastructure provisioning | Scan Terraform configuration |
| Deployment | Enforce identity, access, and approval controls |
| Production | Monitor suspicious activity and vulnerabilities |

**Key point:** DevSecOps is not just one security tool in a pipeline. It is a set of practices, controls, and shared responsibilities throughout development and operations.

## 3. Application Architecture

### 3.1 One-tier application

In a simple one-tier design, the user interface, business logic, and data storage reside within one system or application environment.

**Example:** A desktop inventory application that stores records in a local database on the same computer.

**Advantages**
- Easy to set up and understand.
- Suitable for small local applications.
- Can work offline if designed for it.

**Limitations**
- Scaling and centralized management can be difficult.
- Data sharing can be challenging.
- A host failure can affect the whole application.

**Interview note:** One server does not automatically mean one tier. Tier classification depends on logical architecture, not only physical machine count.

### 3.2 Two-tier application

A common two-tier design separates a client from a database server. In a classic design, the client may also contain business logic.

**Request flow**
1. The user acts in the client.
2. The client sends a query or request to the database server.
3. The database processes it.
4. The result returns to the client.

**Advantages**
- Centralized data storage.
- Easier sharing than isolated local databases.
- Relatively straightforward for smaller systems.

**Limitations**
- Direct client-to-database access can create security concerns.
- Many clients can increase database load.
- Updating client-side logic may be difficult.

### 3.3 Three-tier application

A three-tier application separates presentation, application logic, and data responsibilities.

| Tier | Responsibility | Examples |
|---|---|---|
| Presentation | Displays information and accepts user input | React, Angular, HTML/CSS |
| Application | Executes business rules and processes requests | Java, Python, .NET, Go |
| Data | Stores and retrieves information | SQL Server, PostgreSQL, MySQL |

**Example: product search**
1. The browser displays the search page.
2. The frontend sends an HTTP request to a backend API.
3. The backend validates the request and queries the database.
4. The database returns matching products.
5. The backend sends a response.
6. The frontend displays the results.

```text
User / Browser
      |
      v
Frontend / UI
      |
      v
Backend / API
      |
      v
Database
```

**Benefits**
- Layers can be maintained independently.
- Backend logic can enforce authorization and business rules.
- Database access can be isolated from the public internet.
- Different layers can be scaled separately.
- Teams can work on layers in parallel.

The tiers are logical responsibilities; they do not necessarily require three separate physical machines.

## 4. Microservices Architecture

Microservices divide an application into smaller, independently deployable services.

An e-commerce application might have:
- User service
- Product catalogue service
- Search service
- Shopping cart service
- Payment service
- Order service
- Notification service

A request may enter through an API gateway and be routed to one or more services. Services may communicate synchronously through APIs or asynchronously through messaging.

### Monolith vs. microservices

| Monolithic application | Microservices |
|---|---|
| One main deployable application | Multiple independently deployable services |
| Often simpler to start | More operational complexity |
| Scaling may involve the whole application | Individual services can be scaled independently |
| Internal calls are common | Network communication is common |
| Often simpler deployment and monitoring | Requires strong distributed observability and automation |

Microservices are not always better. For small applications, a well-designed monolith can be simpler and less expensive.

## 5. HLD vs. LLD

### High-Level Design (HLD)

HLD describes the overall architecture and major components.

For an Azure application, it might show:
- Users accessing the application over HTTPS.
- Application Gateway routing incoming requests.
- Frontend and backend components.
- Azure SQL Database.
- Virtual Network and subnet boundaries.
- Identity, security, monitoring, and backup services.

HLD answers:
- Which platform and major components will be used?
- How will users access the application?
- Where will data be stored?
- How will availability, security, and scaling be addressed?

### Low-Level Design (LLD)

LLD explains implementation details, such as:
- Resource names and naming conventions.
- IP ranges and subnet CIDRs.
- NSG rules and permitted ports.
- Routes and network connectivity.
- VM sizes or application service plans.
- Managed identities and RBAC assignments.
- Database connectivity and secret handling.
- Health probes and diagnostic settings.
- Terraform module structure, variables, and dependencies.

| Area | HLD | LLD |
|---|---|---|
| Purpose | Overall architecture | Implementation details |
| Network | Network and subnet architecture | CIDRs, routes, rules, configuration |
| Compute | Select a compute approach | Sizes, scaling, configuration |
| Security | Security architecture | Exact roles, policies, and rules |
| Deliverable | Architecture diagram and decisions | Detailed configuration and implementation plan |

**Interview approach:** Explain the HLD first, then describe LLD details such as address spaces, subnets, NSGs, identities, compute sizing, database access, and Terraform implementation.

## 6. On-Premises vs. Cloud

### On-premises

The organization owns or manages physical infrastructure in its own data centre or a colocation facility.

Responsibilities may include:
- Physical servers and storage.
- Network switches, routers, and firewalls.
- Power, cooling, and hardware replacement.
- Virtualization and operating systems.
- Backup, disaster recovery, and monitoring.

### Cloud

Cloud providers supply infrastructure and managed services through a platform. Examples include Microsoft Azure, Amazon Web Services (AWS), and Google Cloud Platform (GCP).

| On-premises | Cloud |
|---|---|
| Organization manages physical hardware | Provider manages underlying physical infrastructure |
| Expansion may require hardware procurement | Capacity can often be provisioned on demand |
| May require significant capital expenditure | Costs generally follow service and consumption models |
| Physical maintenance is the organization's responsibility | Responsibilities are shared according to service model |

Cloud customers still need to manage appropriate identities, configurations, data protection, application security, and service-specific controls.

## 7. IaaS, PaaS, and SaaS

### IaaS — Infrastructure as a Service
You manage the operating system and applications while the provider manages underlying physical infrastructure.

**Example:** Azure Virtual Machines.

### PaaS — Platform as a Service
The provider manages more of the runtime and platform, allowing you to focus on the application.

**Example:** Azure App Service.

### SaaS — Software as a Service
You consume a ready-to-use application operated by the provider.

**Example:** Microsoft 365.

## 8. Manual Provisioning vs. Automation

### Manual provisioning

An engineer creates resources through a portal or individual commands:
1. Create a resource group.
2. Create a virtual network.
3. Create subnets and NSGs.
4. Create a virtual machine.
5. Configure access and monitoring.

This can be useful for learning, but becomes difficult to repeat consistently across environments.

### Automated provisioning

Infrastructure is defined through scripts or configuration and deployed through repeatable processes.

Benefits:
- Consistent environments.
- Faster environment creation.
- Less repetitive manual work.
- Version-controlled infrastructure changes.
- Easier review, auditing, and recovery.

Automation does not eliminate mistakes. Poorly designed automation can repeat the same mistake widely, so testing and review are important.

## 9. Infrastructure as Code (IaC)

Infrastructure as Code means defining and managing infrastructure through machine-readable configuration rather than relying entirely on manual provisioning.

### Imperative vs. declarative

**Imperative:** Describe the steps or commands to execute.

**Declarative:** Describe the desired infrastructure state; the tool determines the changes required to reach it.

| Tool | Typical approach | Main use |
|---|---|---|
| Azure CLI | Imperative commands | Automate Azure operations |
| Azure PowerShell | Commands and scripts | Automate Azure management |
| ARM templates | Declarative | Define Azure resources |
| Bicep | Declarative | Define Azure resources with a concise language |
| Terraform | Declarative, desired-state-oriented workflow | Provision resources across multiple providers |

The distinction concerns the programming model and workflow. Imperative commands can also be used within larger declarative automation systems.

## 10. Terraform Fundamentals

Terraform is an Infrastructure as Code tool that uses configuration files to describe resources and manage infrastructure changes.

### Example: Azure resource group

```hcl
terraform {
  required_providers {
    azurerm = {
      source = "hashicorp/azurerm"
    }
  }
}

provider "azurerm" {
  features {}
}

resource "azurerm_resource_group" "demo" {
  name     = "rg-devsecops-demo"
  location = "Central India"
}
```

This example assumes the Azure provider is installed and Azure authentication is configured. Production configurations should define provider version constraints and appropriate security settings.

### Common commands

```bash
terraform init
terraform fmt
terraform validate
terraform plan
terraform apply
terraform destroy
```

- `init`: Initializes the working directory and downloads required providers/modules.
- `fmt`: Formats configuration files.
- `validate`: Checks syntax and internal consistency.
- `plan`: Previews proposed changes.
- `apply`: Executes infrastructure changes.
- `destroy`: Removes managed infrastructure; use carefully.

### Terraform state

Terraform state tracks managed resources and maps configuration to real infrastructure.

Best practices:
- Use a secured remote backend for shared state.
- Use locking where supported to reduce conflicting operations.
- Restrict access because state may contain sensitive values.
- Configure appropriate backup, recovery, and auditing.
- Avoid manual state-file editing unless following a well-understood recovery procedure.

### Why Terraform for multi-cloud?

Terraform supports providers for platforms including Azure, AWS, and GCP, as well as supported on-premises infrastructure. It offers a common workflow, but each provider still has its own resources, APIs, permissions, and implementation details.

## 11. Git and Source Code Management

SCM stands for Source Code Management. It helps teams track changes, collaborate, review code, and preserve version history.

| Concept | Meaning |
|---|---|
| Repository | Stores project files and version history |
| Commit | Records a set of changes |
| Branch | Independent line of development |
| Merge | Combines changes from branches |
| Pull request | Requests review and merging |
| Tag | Marks a point such as a release |
| Remote | Repository hosted on another system |

Common platforms:
- GitHub
- Azure Repos
- Bitbucket

### Example Git workflow

```bash
git clone <repository-url>
git checkout -b feature/add-network
git add .
git commit -m "Add network configuration"
git push origin feature/add-network
```

The developer then creates a pull request, obtains required review, and merges approved changes.

**Why Git for IaC?** If a Terraform change accidentally opens a sensitive port, version control helps the team review the change, identify its history, and compare it with the previous configuration. Git alone does not prevent insecure changes; reviews, branch protection, scans, and deployment controls are also important.

## 12. CI/CD Pipelines

CI/CD automates code validation, packaging, security checks, and delivery.

### CI — Continuous Integration
Automates validation as developers contribute code:
- Compile or package application code.
- Run automated tests.
- Check code quality.
- Scan dependencies and secrets.
- Produce build artifacts.

### Continuous Delivery vs. Continuous Deployment

- **Continuous Delivery:** Changes are kept ready for release, often with manual approval before production.
- **Continuous Deployment:** Changes that pass required automated checks are deployed to production automatically.

### Typical pipeline flow

1. Developer pushes code.
2. CI builds and tests it.
3. Security tools scan code and dependencies.
4. Application artifacts or container images are packaged.
5. IaC is validated and a Terraform plan is reviewed.
6. Required approvals and policy checks run.
7. Authorized deployment occurs.
8. Health checks and monitoring verify the result.

### Illustrative Azure DevOps YAML

```yaml
trigger:
  branches:
    include:
      - main
      - feature/*

pool:
  vmImage: ubuntu-latest

stages:
  - stage: Validate
    jobs:
      - job: TerraformValidate
        steps:
          - script: terraform fmt -check
            displayName: Check Terraform formatting

          - script: terraform init -backend=false
            displayName: Initialize Terraform

          - script: terraform validate
            displayName: Validate configuration
```

This is a basic example, not a complete production deployment. It assumes Terraform is available on the agent. It does not implement cloud authentication, security scanning, a remote-state plan, or a controlled apply stage. Production pipelines should separate validation, scanning, plan, and apply stages and protect production environments with appropriate permissions and approvals.

## 13. DevSecOps Security Practices

| Practice | Purpose |
|---|---|
| SAST | Detect potential weaknesses in source code |
| SCA | Identify vulnerable third-party dependencies |
| Secret scanning | Detect exposed credentials and tokens |
| IaC scanning | Detect insecure infrastructure configurations |
| Container scanning | Identify known vulnerabilities in images |
| DAST | Test a running application for security weaknesses |
| RBAC | Restrict access according to roles and permissions |
| Vulnerability management | Prioritize, remediate, and verify findings |

### Example: insecure Terraform rule

Suppose a Terraform configuration permits inbound traffic from anywhere to a management port. An IaC scanner may flag the rule. The engineer assesses the finding, restricts the source to an approved network or removes the rule, validates the change, and obtains required review before deployment.

A scanner finding does not automatically prove an exploitable vulnerability. Context, severity, exposure, and compensating controls must be assessed.

### Security gates

A security gate defines when a pipeline can proceed. Examples:
- Block deployment when a critical vulnerability exceeds the accepted threshold.
- Fail a scan if a prohibited secret is detected.
- Require review for production infrastructure changes.
- Prevent deployment from unauthorized branches.

Define exception and remediation processes so teams do not bypass security controls under release pressure.

## 14. Observability and Troubleshooting

Observability helps teams understand application behaviour after deployment.

### Three pillars

**Metrics:** Numerical measurements such as CPU utilization, memory usage, latency, error rate, and throughput.

**Logs:** Records of errors, authentication events, and application operations.

**Traces:** Show how requests move across components and where time is spent.

Azure examples include Azure Monitor, Log Analytics, Application Insights, Azure Monitor Agent, and Data Collection Rules, depending on workload and configuration.

### Troubleshooting example: website is slow

1. Confirm affected users, endpoints, and start time.
2. Check availability and HTTP error rates.
3. Inspect CPU, memory, and network metrics.
4. Review application logs for exceptions.
5. Trace slow requests across frontend, backend, and database.
6. Check database query performance and connection limits.
7. Identify the root cause and implement a fix.
8. Verify recovery and document the incident.

Monitoring also supports security investigations, incident response, capacity planning, and continuous improvement.

## 15. Practical Project: Secure Three-Tier Application on Azure

### Objective

Deploy a three-tier web application with controlled network access, repeatable infrastructure provisioning, automated testing, security checks, and monitoring.

### Example architecture components

| Component | Responsibility |
|---|---|
| Azure Application Gateway | Entry point and HTTP/HTTPS routing |
| Frontend | Displays the web interface |
| Backend API | Processes business logic |
| Azure SQL Database | Stores application data |
| Virtual Network | Provides network isolation |
| Network Security Groups | Restrict permitted traffic |
| Microsoft Entra ID / managed identity | Supports identity and resource access |
| Terraform | Provisions infrastructure |
| GitHub or Azure Repos | Stores source code and IaC |
| CI/CD pipeline | Validates, scans, and deploys changes |
| Azure Monitor | Supports operational monitoring |

This is one possible architecture. A real implementation should select hosting services, database connectivity, availability design, and network topology according to application requirements.

### Implementation phases

1. **Design:** Create HLD, LLD, request-flow diagram, network design, and security requirements.
2. **Source control:** Create repositories, define a branch strategy, and configure pull-request reviews.
3. **Infrastructure as Code:** Develop Terraform modules for networking, application hosting, database, and monitoring.
4. **Security:** Add secret scanning, IaC scanning, dependency checks, and container scanning if containers are used.
5. **CI/CD:** Validate configuration, generate a Terraform plan, review changes, and deploy with controlled permissions.
6. **Verification:** Test application functionality, connectivity, access controls, and security rules.
7. **Monitoring:** Configure metrics, logs, alerts, dashboards, and incident procedures.

### Student deliverables

Students should be able to:
- Explain why the application uses three tiers.
- Draw HLD and LLD diagrams.
- Provision infrastructure through Terraform.
- Explain Git and pull requests.
- Demonstrate a pipeline that validates and scans changes.
- Show how production deployment is controlled.
- Diagnose a sample application or networking issue.
- Explain monitoring and secret protection.

## 16. Interview Questions and Answers

### Q1. What is the difference between DevOps and DevSecOps?
DevOps improves collaboration and delivery through automation and feedback. DevSecOps integrates security into those workflows from coding through production operations.

### Q2. What is the difference between HLD and LLD?
HLD defines overall architecture and major components. LLD describes the detailed configuration needed to implement them.

### Q3. Why use three-tier architecture?
It separates presentation, business logic, and data responsibilities, supporting access control, maintenance, and independent scaling.

### Q4. What is Infrastructure as Code?
IaC defines infrastructure in machine-readable files so it can be reviewed, version-controlled, validated, and provisioned consistently.

### Q5. What is Terraform state?
Terraform state tracks resources managed by Terraform and maps configuration to real infrastructure. It must be protected and managed carefully.

### Q6. What is the difference between CI and CD?
CI automates integration and validation of changes. CD can mean Continuous Delivery, which prepares changes for release, or Continuous Deployment, which deploys qualifying changes automatically.

### Q7. Why scan Terraform files?
IaC scanning helps identify insecure configurations before infrastructure is deployed, reducing the risk of exposing services or granting excessive access.

### Q8. What is observability?
Observability uses telemetry such as metrics, logs, and traces to understand system behaviour and investigate failures.

## 17. Suggested Learning Roadmap

### Module 1 — Application Architecture
Software lifecycle, one-tier/two-tier/three-tier architecture, microservices, HLD, and LLD.

### Module 2 — Cloud and Infrastructure
On-premises vs. cloud, Azure/AWS/GCP fundamentals, networking, compute, storage, and identity.

### Module 3 — IaC and Terraform
Imperative vs. declarative approaches, Terraform configuration, providers, resources, state, and reusable modules.

### Module 4 — Git and CI/CD
Repositories, branching, pull requests, build pipelines, testing, deployment, and approvals.

### Module 5 — DevSecOps
Security scanning, vulnerability remediation, access control, secrets management, and secure deployment.

### Module 6 — Production Operations
Monitoring, observability, troubleshooting, incident response, reliability, and interview preparation.

---

## Suggested Demo-Class Teaching Flow

1. Start with a real-world application example.
2. Explain one-tier, two-tier, and three-tier architecture.
3. Show HLD and LLD and explain the difference.
4. Introduce cloud infrastructure and IaC.
5. Demonstrate how Terraform describes infrastructure.
6. Explain Git, pull requests, and CI/CD.
7. Show where security checks fit in the pipeline.
8. Close with monitoring, troubleshooting, and the end-to-end project.

**Core takeaway:** DevSecOps brings application development, infrastructure automation, security, delivery, and operations into one repeatable workflow.
