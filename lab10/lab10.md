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


## Governance and Identity

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