# OpsZ Platform Overview

**Last updated:** February 2026

## What is OpsZ?

OpsZ is an enterprise IT automation platform that combines real-time infrastructure awareness, workflow orchestration, and edge-based AI into a single deployable stack. It gives operations teams a continuous, queryable connection to every system they manage — on-premises, cloud, or hybrid — and the ability to automate actions across all of them with built-in governance and audit controls.

The platform is built around a lightweight agent architecture that maintains persistent, low-overhead connections to managed nodes. This is fundamentally different from SSH-based tools or API-polling approaches — OpsZ agents publish state continuously and respond to commands in sub-second timeframes at scale.

## Who is it for?

- **IT operations teams** managing hybrid infrastructure across cloud and on-premises environments
- **Organizations that need automation** but can't justify building custom tooling
- **Teams running multiple tools** (ServiceNow, PagerDuty, Grafana, Terraform, Ansible) that need an orchestration layer to connect them

## The Four Pillars

### 1. Communication Substrate

An agent-based publish-subscribe messaging layer that provides always-on connectivity to your infrastructure.

**Agent Design**
- Single binary deployment, one outbound connection, no listening ports
- No inbound SSH or firewall rules required
- Runs on Linux, Windows, containers, Kubernetes, bare metal

**Performance**
- Sub-second fleet-wide queries across tens of thousands of nodes
- A single management server scales to enterprise fleet sizes
- High-availability replication across clusters

**Provisioning**
- Agents self-enroll with secure certificates and receive automatic updates
- No manual configuration per node
- Rapid enrollment for large fleets

**Security**
- Certificate-based authentication with short-lived tokens
- TLS encryption for all communication
- Zero-trust model: agents only make outbound connections
- No static credentials stored on managed systems

### 2. Real-Time Asset Inventory

Continuous infrastructure discovery that builds and maintains a live inventory without relying on stale databases.

**On-Premises Discovery**
- Automated hardware fact collection: CPU, memory, network interfaces, storage, system information
- NUMA topology and detailed system architecture
- Network configuration (interfaces, addresses, link speeds)

**Cloud Metadata Integration**
- AWS, GCP, and Azure instance metadata
- Automated cloud provider detection
- Unified view across on-premises and cloud resources

**Data Pipeline**
- Real-time streaming architecture with sub-second latency
- Exactly-once processing semantics
- REST API access to inventory data with flexible querying

**Flexible Inventory**
- Hierarchical grouping based on metadata attributes
- Custom metadata collection via configuration
- Regular refresh intervals with on-demand queries

### 3. Governance & Access Management

Identity, access control, and audit capabilities built into every platform operation — not added as an afterthought.

**Authentication**
- OAuth 2.0 / SAML (Okta, Azure AD, and other identity providers)
- LDAP / Active Directory integration
- Multi-factor authentication support
- Token-based security with local validation

**Authorization**
- Fine-grained role-based access control (RBAC)
- Action-level permissions (execute, approve, read, write)
- Role and group hierarchies with inheritance
- Temporary access grants with automatic expiration

**Audit & Compliance**
- Comprehensive logging of all user and system activity
- Every API call, approval decision, and workflow execution is recorded
- Searchable audit trail for compliance reporting

**Approval Workflows**
- Multi-step approval chains for sensitive operations
- Configurable approver groups and escalation rules
- Bulk user operations for enterprise-scale management

### 4. AI & Analytics at the Edge

Intelligence that runs where the work happens, not just in a central cloud.

**Semantic Workflow Search**
- AI-powered template discovery using vector embeddings
- Find workflows by describing what you want to accomplish
- Fast, relevant search results across your automation library

**Automatic Template Vectorization**
- Background processing generates intelligent embeddings
- Incremental updates as templates change
- No manual tagging or categorization required

**Edge Intelligence (Planned)**
- Local AI model deployment on managed nodes
- Autonomous log analysis and troubleshooting
- Federated intelligence model: local decision-making coordinated with central oversight

## Key Capabilities

### Workflow Orchestration

Workflows are defined as directed acyclic graphs (DAGs), not linear scripts. This means parallel execution paths, conditional branching based on step outcomes, and the ability to re-enter a failed workflow at the point of failure.

- **Visual workflow designer** generates version-controlled YAML
- **Cross-platform execution** — a single workflow can target nodes across multiple environments
- **Template gallery** with AI-powered semantic search
- **Configurable approval workflows** before execution
- **Conditional logic** and error handling

### Job Execution & Tracking

Complete job lifecycle management from creation through execution and monitoring.

- Parent-child job hierarchies with multi-target support
- DAG-based action dependencies — parallel, sequential, and conditional execution
- GitHub integration — PRs, issues, and workflow events automatically trigger jobs
- Real-time status and log collection
- Analytics dashboard with success rates and performance trends
- Cron-based scheduling for recurring jobs

### Event Streaming

Real-time data pipeline for infrastructure events and metadata:

```
Managed Systems → Agent Events → Message Bus → Stream Processing → Storage → API
```

- Exactly-once processing semantics
- Automatic payload handling and normalization
- Connection pooling with retry logic and backoff
- Asynchronous processing for high throughput

### API-First Design

Every platform capability is available via REST API. The UI is a consumer of the same APIs available to customers.

- Four API domains: metadata collection, user management, workflow execution, asset management
- External system integration — ServiceNow, PagerDuty, Grafana, CloudWatch via webhooks and REST
- Headless operation — the entire platform can be driven without the UI
- Comprehensive API documentation

## Deployment

OpsZ deploys on a single VM running Kubernetes, or scales to a full cluster for high availability.

**Installation**
- Fast deployment with single-command installation
- Automated setup with minimal configuration
- Self-contained package including all components and dependencies

**Architecture**
- Container-based deployment with namespace isolation
- Distributed services for scalability
- Modern infrastructure stack (streaming, analytics, storage)

**Delivery Options**
- Docker Compose for development and testing
- Kubernetes/Helm for production deployments
- Customer on-premises installation packages
- Cloud provider deployment options

## Key Differentiators

1. **End-to-end lifecycle** — Incident detection through auto-remediation to post-mortem analysis, in one platform. Not just alerting, not just execution.

2. **Bi-directional SOP-workflow sync** — Keeps standard operating procedures and automation in sync automatically. No other product does this.

3. **No rip-and-replace** — API-first design integrates with existing tools. OpsZ orchestrates ServiceNow, PagerDuty, Ansible — it doesn't replace them.

4. **Federated intelligence** — AI models deployed at the edge for autonomous decision-making, not just centralized analysis.

5. **Built-in governance** — RBAC, approval chains, and audit trails are native to the platform, not bolted on.

6. **Scale without complexity** — Enterprise-scale fleet management from a single server with optional clustering for high availability.

## Use Cases

**Incident Response Automation**
- Detect infrastructure issues in real time
- Automatically execute remediation workflows
- Built-in approval gates for sensitive operations
- Complete audit trail from detection to resolution

**Compliance Enforcement**
- Continuous infrastructure scanning
- Automated reporting and remediation
- Policy-as-code enforcement
- Audit-ready compliance documentation

**Patch Management at Scale**
- Orchestrate patching across thousands of systems
- Phased rollouts with validation gates
- Automatic rollback on failure
- Real-time status tracking

**Cloud Cost Optimization**
- Identify underutilized resources
- Automated rightsizing recommendations
- Execution workflows with approval chains
- Cost tracking and reporting

---

**Learn More:**
- [Architecture Details](architecture.md)
- [Complete Feature List](features.md)
- [Integration Capabilities](integrations.md)
- [Security Model](security.md)
