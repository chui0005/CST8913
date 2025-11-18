#  Zero Trust Landing Zone for CloudMed Solutions

## 1. Company Overview
### CloudMed Solutions Inc. – Business and Services

CloudMed Solutions Inc. is a multinational healthcare technology provider specializing in secure, cloud-based digital health platforms. Its flagship product, **MedConnect**, delivers a unified telemedicine and patient-management experience for hospitals, clinics, and healthcare networks across Canada, the United States, and Europe. 

Core services include:

- Secure telehealth consultations between clinicians and patients.

- Electronic medical record (EMR) management with strict health-data protections.

- AI-driven diagnostic and health-analytics tooling to support clinical decision-making.

CloudMed operates globally and delivers its SaaS-based healthcare services from multiple Azure regions, primarily **Canada Central** and **West Europe**, to ensure data locality, compliance, and low-latency user experiences.

### Why CloudMed Must Adopt a Zero Trust Architecture in Azure

Because CloudMed processes highly sensitive health information, it faces elevated exposure to cyber threats and regulatory scrutiny. 

A Zero Trust architecture is essential for:

- Protecting PHI/PII in a hostile threat landscape where healthcare is a top target for ransomware and data breaches.

- Ensuring secure remote access for physicians, nurses, and administrators connecting from hospitals, home offices, and external clinics.

- Establishing strong identity-driven access controls for a diverse user base that includes internal teams, partner hospitals, and automated services.

- Enforcing continuous verification of identity, device posture, and session risk across all interaction points.

Zero Trust allows CloudMed to mitigate breach impact, maintain service availability, and ensure end-to-end protection across its telemedicine and data-management ecosystem.

### Key Compliance and Operational Drivers

CloudMed must align its cloud environment with multiple healthcare and privacy regulatory frameworks:

#### Regulatory Compliance Requirements

- HIPAA (USA): strict controls around PHI storage, transmission, auditability, and access management.

- GDPR (EU): data residency, consent management, encryption requirements, and data-minimization principles.

- PIPEDA (Canada): mandates for safeguarding personal health information and ensuring breach transparency.

These laws require robust identity controls, network isolation, encryption, audit logging, and continuous monitoring, all of which are core Zero Trust capabilities.

#### Operational and Architectural Drivers

- Multi-region operations (Canada Central, West Europe) require standardized governance, policies, and secure connectivity.

- Centralized governance and management to maintain consistency across environments, subscriptions, and workloads.

- Enhanced visibility through monitoring, logging, and compliance tracking for regulated healthcare workloads.

- Cost governance to control spending across global deployments.

- Secure workload isolation to support tenant-specific or region-specific healthcare partners.

Together, these drivers make a Zero Trust Azure Landing Zone mandatory for CloudMed’s cloud strategy, ensuring security, compliance, scalability, and operational resilience for healthcare workloads.


## 2. Governance and Identity

To support a secure, compliant, and well-organized Azure environment, CloudMed Solutions adopts a governance and identity model that follows Zero Trust principles from the foundation upward. This section outlines how CloudMed structures its management groups, enforces policies, and secures identity through Azure Entra ID and Conditional Access.

### Management Group Hierarchy

CloudMed uses a tiered management group structure to organize its Azure resources and apply governance consistently across all environments. The hierarchy is shown below:

```
Root
└── CloudMed
    ├── Platform
    ├── Production
    └── Development
```

#### Explanation of Hierarchy:

##### Root
This is the top-level tenant group where global governance guardrails are applied. High-level security and compliance baselines start here.

##### CloudMed
This group contains all CloudMed management groups and subscriptions. Policies related to tagging, allowed regions (Canada Central and West Europe), and healthcare compliance are enforced here.

##### Platform
Only CloudMed’s platform/infra team has the ability to modify these resources.
Dedicated to shared services and foundational infrastructure, such as:
- firewalls, and routing
- Centralized monitoring (Log Analytics)
- Identity and security services

##### Production
Holds subscriptions that power the MedConnect application in production. This includes:
- App Tier (web and mobile interfaces)
- API Tier (backend services)
- Data Tier (SQL, storage, analytics)

Stronger restrictions apply here because this environment handles real patient data.

##### Development
Contains the dev/test workloads used by engineers. Policies mirror production where possible, but allow limited flexibility for testing new features safely.

This structure ensures that CloudMed can scale globally while applying consistent security and compliance controls across all environments.


### Governance Model and RBAC (Role-Based Access Control)

CloudMed applies governance through a combination of RBAC, management groups, and Azure Policies. RBAC is used to enforce least-privilege access.

#### Key role assignments include:

##### Admins / Platform Team
 - Manage foundational services in the Platform group

##### DevOps / Engineering Teams
- Assigned Contributor or more specific roles like Web Site Contributor or SQL DB Contributor
- Scoped to specific subscriptions or resource groups in Production or Development

##### Finance / Cost Management
- Granted Cost Management Reader or Billing Reader
- Can view usage and spending but cannot make resource changes

By limiting access at the lowest appropriate scope, CloudMed ensures resources remain protected while maintaining operational efficiency.

### Azure Policies and Governance Controls

Azure Policies help CloudMed enforce consistency, maintain compliance, and follow Zero Trust requirements:

#### Tagging Policies
Require tags like Environment, Owner, CostCenter, and DataClassification on every resource.

#### Allowed Regions
Enforce deployment only in Canada Central and West Europe to support residency requirements.

#### Security Policies
- Mandatory diagnostic logging
- Mandatory encryption at rest
- Mandatory private endpoints for databases and storage
- Deny public IP creation on sensitive workloads

#### Cost Management Policies
Restrict resource SKUs and VM sizes to control expenses and maintain predictable costs.

These policies standardize CloudMed’s Azure landscape and prevent misconfiguration that could lead to security or compliance issues.

### Identity Security with Azure Entra ID and Conditional Access

Identity is the core of CloudMed’s Zero Trust strategy. All users, services, and workloads authenticate through Azure Entra ID, which acts as the central identity provider.

#### Key identity features implemented:

##### Centralized authentication
All CloudMed users, developers, and administrators authenticate through Entra ID.

##### Conditional Access Policies
Enforce Zero Trust by evaluating:
- User identity
- Device compliance
- IP/location risk (Blocking risky sign-ins based on real-time detection)
- Session behavior
- MFA requirements (MFA enforced for all admin roles)

##### Managed Identities
Used for communication between Azure services, removing the need for hard-coded credentials.

##### Least Privilege Role Assignments
Users and services receive only the minimum permissions necessary to perform their tasks.

With Entra ID at the center, CloudMed ensures explicit verification for every access request and reduces the attack surface dramatically.


## 3. Network Architecture


![CloudMed Network Architecture.png](CloudMed%20Network%20Architecture.png)


CloudMed’s network architecture follows a modern Hub-and-Spoke topology designed to support Zero Trust requirements for workload isolation, secure connectivity, and centralized security governance. This model ensures that all traffic is inspected, controlled, and logged, while preventing unnecessary lateral movement between workloads.

### Hub-and-Spoke Design and Zero Trust Alignment

CloudMed deploys a centralized Hub Virtual Network that provides shared security and connectivity services. Individual workloads—such as the App Tier, API Tier, and Database Tier are deployed into separate Spoke Virtual Networks. This structure supports Zero Trust by enforcing strict segmentation and requiring all traffic to be explicitly authorized and inspected.

Key ways this design reflects Zero Trust principles:

#### Verify Explicitly:
Traffic flowing between spokes does not route directly. All cross-workload communication is inspected by the hub firewall and follows defined security rules.

#### Least Privilege Network Access:
Workloads can only connect to the resources they are explicitly allowed to reach. For example, the App Tier cannot directly reach the Data Tier without going through approved API endpoints.


This model scales well with CloudMed’s global footprint, allowing multiple regional hubs while preserving consistent governance.

### Hub Components

The Hub VNet contains all shared security, management, and connectivity services. These services operate as the control plane for CloudMed’s cloud network.

Hub Components Include:

#### Azure Firewall 
- Acts as the primary security inspection point
- Enforces inbound, outbound, and east–west traffic filtering
- Supports TLS inspection for healthcare data protection requirements

#### Azure Bastion
- Provides secure, browser-based RDP/SSH access to resources
- Eliminates public IP exposure on virtual machines
- Private DNS Zones
- Used to resolve private endpoints for PaaS services (e.g., SQL, Storage, Key Vault)
- Ensures all PaaS traffic stays within Microsoft’s private network

#### Azure Monitor / Log Analytics Workspace
- Centralized logging for firewall, traffic flows, NSGs, VMs, and platform logs
- Required for auditability under HIPAA, GDPR, and PIPEDA


By keeping these services in the hub, CloudMed ensures uniform visibility and enforcement across every spoke network.

### Workload Isolation in Spokes (App, API, Data)

Each workload tier in MedConnect is deployed into its own Spoke VNet, ensuring strict isolation between application layers:

#### App Spoke
- Hosts front-end web and mobile application components
- Public access is routed through Azure Front Door or Application Gateway
- No direct connectivity to Data Spoke

#### API Spoke
- Hosts backend microservices accessed by the App Tier
- Only exposes private API endpoints
- Connects to the Data Spoke through allowed service paths

#### Data Spoke
- Contains data services such as Azure SQL, Storage Accounts, and analytics workloads
- Accessible only via private endpoints

All subnets are locked down with strict NSG and Firewall policies

This layered design prevents the App Tier from interacting directly with the Data Tier, enforcing strong separation of duties at the network level.

### Subnet Separation and East–West Traffic Control

CloudMed applies further segmentation inside each spoke by separating resources into multiple subnets.

#### App Spoke
- Subnet: app-web
- Subnet: app-services

#### API Spoke
- Subnet: api-core
- Subnet: api-functions

#### Data Spoke
- Subnet: sql-private-endpoints
- Subnet: storage-private-endpoints


### East–West Traffic Policies:

Communication between spokes does not occur directly. All east–west traffic flows through Azure Firewall in the hub.

Hub Firewall policies specify exactly:
- Which subnets can talk to which services
- Which ports are allowed
- Which protocols are required

This ensures CloudMed can inspect and control all workload-to-workload communication, meeting Zero Trust’s “Assume Breach” requirement.

### Private Endpoints and Secure PaaS Access

CloudMed uses Private Endpoints to securely connect workloads to PaaS services (SQL, Storage, Key Vault, Event Grid, etc.). This ensures:
- No public internet exposure
- All data stays inside Azure’s private backbone
- Policies can enforce that only private endpoints are allowed for sensitive services


## 4. Zero Trust Controls

### Verify Explicitly

CloudMed ensures that every access request, whether from a user, device, service, or workload, is authenticated and authorized before granting access to cloud resources. Verification is based on identity, device state, location, and risk scoring.

#### Key Controls:

- Azure Entra ID authentication for all users and workloads
  - All cloud services authenticate through Entra ID, ensuring centralized identity governance.

- Multi-Factor Authentication (MFA) enforcement
  - Mandatory for all privileged roles and for any access to production-tier workloads.

- Conditional Access Policies
  - Access decisions consider user location, device compliance, and sign-in risk.


### Least Privilege Access

Least privilege ensures that users and services receive the minimum access required to perform their responsibilities. This approach reduces the risk of accidental or malicious misuse of permissions across CloudMed’s global infrastructure.

#### Key Controls:

- Role-Based Access Control (RBAC) applied at the smallest necessary scope


### Assume Breach

CloudMed designs its network and workloads assuming that intrusions can occur. Controls focus on containing attacks, preventing lateral movement, and ensuring full visibility for threat detection and incident response.

#### Key Controls:

- Hub-and-Spoke network with strict segmentation

- App, API, and Data tiers run in isolated spokes

- No direct spoke-to-spoke connectivity

- Encryption of data at rest and in transit



### Design Examples

1) Azure Bastion for secure admin access. This eliminates the need for public IPs on VMs and enforces authenticated, logged RDP/SSH.

2) Azure Private Link for Azure SQL and Storage. This ensures all PHI remains on the private Azure backbone with no public exposure.

3) Azure Policy denying public IP creation in Production. This prevents accidental creation of internet-exposed workloads.
