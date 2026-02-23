# OpsZ Feature Catalog

**Last updated:** February 2026

This catalog lists all OpsZ capabilities with their current availability status.

**Status Definitions:**
- **GA** (Generally Available) — Production-ready, fully supported
- **Beta** — Functional and available, may have minor limitations
- **Planned** — On the roadmap, actively being designed or developed

---

## Infrastructure Management

### Agent Deployment & Management

| Feature | Status | Description |
|---------|--------|-------------|
| Lightweight agent deployment | GA | Single binary agent with minimal footprint. Supports Linux, Windows, containers, Kubernetes, and bare metal. |
| Zero-trust security model | GA | Agents use outbound-only connections with no listening ports. No inbound firewall rules required. |
| Automatic agent provisioning | GA | Agents self-enroll using secure certificates. Rapid deployment across large fleets. |
| Automatic agent updates | GA | Centralized update distribution. Agents receive new versions automatically without manual intervention. |
| Multi-platform support | GA | Runs on all major operating systems and deployment environments. |

### Infrastructure Discovery

| Feature | Status | Description |
|---------|--------|-------------|
| Hardware inventory | GA | Automatic discovery of CPU, memory, network, storage, and system information. |
| NUMA topology mapping | GA | Detailed CPU and memory topology for performance optimization. |
| Network interface discovery | GA | MAC addresses, IP addresses, link speeds, and interface details. |
| On-demand inventory refresh | GA | Real-time queries for current system state. |
| Custom metadata collection | GA | Configurable data collection via YAML templates. |
| Cloud instance metadata | Beta | AWS EC2 instance information including instance type, region, and tags. |
| Multi-cloud discovery | Planned | Standardized discovery for GCP and Azure instances. |
| External inventory integration | Planned | API for importing data from third-party CMDB and discovery tools. |

### Fleet Management

| Feature | Status | Description |
|---------|--------|-------------|
| Fleet-wide queries | GA | Sub-second response times for queries across thousands of nodes. |
| Hierarchical grouping | GA | Organize infrastructure by metadata attributes (environment, datacenter, application). |
| Real-time fleet monitoring | GA | Continuous awareness of agent status, versions, and health. |
| Cross-cluster replication | GA | High-availability support for multi-site deployments. |
| Fleet metrics | GA | Prometheus-compatible metrics for monitoring and alerting. |

---

## Workflow Automation

### Workflow Design

| Feature | Status | Description |
|---------|--------|-------------|
| DAG-based workflows | Beta | Directed acyclic graph execution with parallel paths and conditional branching. |
| Visual workflow designer | Beta | Drag-and-drop interface generates version-controlled YAML. |
| Workflow templates | GA | Reusable template library with metadata and search capabilities. |
| Cross-platform execution | Beta | Single workflow targets nodes across multiple environments. |
| Version control integration | Beta | YAML workflows integrate with Git for versioning and collaboration. |
| Conditional execution | Beta | Branch workflow paths based on step outcomes. |
| Error handling | Beta | Automatic retry, rollback, and recovery options. |

### Workflow Discovery

| Feature | Status | Description |
|---------|--------|-------------|
| AI-powered template search | GA | Semantic search finds workflows by describing what you want to accomplish. |
| Keyword search | GA | Traditional text-based search across workflow names and descriptions. |
| Template recommendations | GA | Intelligent suggestions based on your infrastructure and use patterns. |
| Template analytics | GA | Usage statistics and search performance metrics. |

### Execution & Orchestration

| Feature | Status | Description |
|---------|--------|-------------|
| Multi-target execution | GA | Run workflows across selected nodes or groups. |
| Parallel execution | Beta | Execute independent workflow steps simultaneously. |
| Sequential execution | Beta | Run steps in order with dependency tracking. |
| Real-time status tracking | Beta | Live updates as workflows execute. |
| Execution logging | Beta | Detailed logs for debugging and audit. |
| Restart from failure | Beta | Resume failed workflows without restarting from the beginning. |

---

## Job Management

### Job Lifecycle

| Feature | Status | Description |
|---------|--------|-------------|
| Job creation and scheduling | GA | Create one-time or recurring jobs via API or UI. |
| Job hierarchies | GA | Parent-child relationships for complex multi-stage operations. |
| Status tracking | GA | Track job state: pending, running, success, failed, cancelled, paused. |
| Cron scheduling | Beta | Schedule recurring jobs with standard cron expressions. |
| Job cancellation | GA | Stop running jobs with proper cleanup. |
| Job history | GA | Complete execution history with logs and metrics. |

### Job Execution

| Feature | Status | Description |
|---------|--------|-------------|
| Task routing | GA | Intelligent dispatch to target nodes via messaging infrastructure. |
| Dependency management | Beta | Ensure prerequisites complete before starting dependent tasks. |
| Result collection | Beta | Gather output and logs from distributed execution. |
| Execution analytics | Beta | Success rates, duration trends, and performance metrics. |

### Integration & Triggers

| Feature | Status | Description |
|---------|--------|-------------|
| GitHub webhooks | Beta | Trigger jobs from pull requests, issues, and workflow events. |
| API triggers | GA | Start jobs via REST API calls from external systems. |
| Event-based triggers | Beta | React to infrastructure events and alerts. |
| PagerDuty integration | Planned | Alert-triggered workflow execution. |

---

## Governance & Security

### Identity Management

| Feature | Status | Description |
|---------|--------|-------------|
| User and group management | GA | Full user lifecycle management with hierarchical groups. |
| OAuth 2.0 / SAML SSO | GA | Single sign-on with Okta, Azure AD, and other identity providers. |
| LDAP / Active Directory | GA | Enterprise directory integration. |
| Multi-factor authentication | GA | Additional security for sensitive operations. |
| Bulk user operations | GA | Import and manage users at scale via CSV or API. |

### Access Control

| Feature | Status | Description |
|---------|--------|-------------|
| Role-based access control | GA | Fine-grained permissions at the action and resource level. |
| Role hierarchies | GA | Inherit permissions through role and group structures. |
| Temporary access grants | GA | Time-limited permissions with automatic expiration. |
| Custom roles | GA | Define roles tailored to your organization's needs. |
| Authorization policies | GA | Centralized policy management and enforcement. |

### Approval Workflows

| Feature | Status | Description |
|---------|--------|-------------|
| Multi-step approvals | Beta | Configure approval chains for sensitive operations. |
| Approval routing | Beta | Route approval requests to appropriate groups and managers. |
| Approval tracking | Beta | Complete history of who approved what and when. |
| Approval escalation | Beta | Automatic escalation for overdue approvals. |
| Access requests | Beta | Self-service access request workflows. |

### Audit & Compliance

| Feature | Status | Description |
|---------|--------|-------------|
| Comprehensive audit logging | GA | Record all user and system activity. |
| Audit trail search | GA | Query audit logs for compliance reporting. |
| Execution history | GA | Complete record of workflow and job execution. |
| Approval history | Beta | Track all approval decisions and outcomes. |
| Retention policies | GA | Configurable data retention for regulatory compliance. |

---

## Intelligence & Analytics

### AI-Powered Search

| Feature | Status | Description |
|---------|--------|-------------|
| Semantic workflow search | GA | Find workflows by describing intent in natural language. |
| Multi-stage search pipeline | GA | Combines keyword, vector, and step-level matching for optimal results. |
| Automatic embedding generation | GA | AI models generate intelligent representations of workflows. |
| Search result ranking | GA | Relevance-based ranking with score fusion. |
| Query result caching | GA | Fast responses for frequently searched terms. |

### Analytics & Reporting

| Feature | Status | Description |
|---------|--------|-------------|
| Job success metrics | Beta | Track success and failure rates over time. |
| Execution duration trends | Beta | Identify performance patterns and anomalies. |
| Fleet health dashboard | GA | Real-time view of infrastructure state. |
| Search analytics | GA | Understand how users discover and use workflows. |
| Custom dashboards | Planned | Build custom views of platform metrics. |

### Advanced AI (Planned)

| Feature | Status | Description |
|---------|--------|-------------|
| Edge-based LLMs | Planned | Deploy AI models on managed nodes for local intelligence. |
| Autonomous log analysis | Planned | AI-powered troubleshooting at the edge. |
| Incident pattern matching | Planned | Identify similar incidents and suggest remediation. |
| Natural language workflow creation | Planned | Generate workflows from plain-English descriptions. |
| Remediation recommendations | Planned | AI-suggested fixes based on historical data. |

---

## Platform & Deployment

### Deployment Options

| Feature | Status | Description |
|---------|--------|-------------|
| Kubernetes deployment | GA | Helm-based installation for production environments. |
| Docker Compose deployment | GA | Single-command setup for development and testing. |
| Customer on-premises packaging | GA | Self-contained installation for air-gapped environments. |
| Cloud provider deployment | GA | Automated deployment to AWS, with GCP and Azure support planned. |
| High availability configuration | GA | Multi-replica deployment with failover. |

### Management & Operations

| Feature | Status | Description |
|---------|--------|-------------|
| Single-command installation | GA | Automated setup with minimal configuration. |
| Namespace isolation | GA | Logical separation of platform components. |
| Resource management | GA | Configurable CPU and memory limits. |
| TLS certificate management | GA | Automatic certificate provisioning and rotation. |
| Health checks | GA | Automatic service monitoring and restart. |
| Platform upgrades | GA | In-place upgrades with minimal downtime. |

### APIs & Integration

| Feature | Status | Description |
|---------|--------|-------------|
| REST APIs | GA | Complete API coverage for all platform capabilities. |
| API documentation | GA | Interactive API documentation with examples. |
| Webhook support | GA | Receive events from external systems. |
| Token-based authentication | GA | Secure API access with short-lived tokens. |
| Rate limiting | GA | Protect APIs from abuse and overload. |
| Headless operation | GA | Full platform capability without UI. |

### User Interface

| Feature | Status | Description |
|---------|--------|-------------|
| Web dashboard | Beta | Modern, responsive UI for all platform functions. |
| RBAC-based rendering | Beta | UI adapts to user permissions. |
| Workflow editor | Beta | Visual workflow designer with Monaco editor. |
| Real-time updates | Beta | Live status and log streaming in the UI. |
| Job analytics dashboard | Beta | Visualize job metrics and trends. |
| Inventory browser | Beta | Explore infrastructure topology and metadata. |

---

## Planned Capabilities

### Integration Roadmap

| Feature | Target | Description |
|---------|--------|-------------|
| PagerDuty | Q1 2026 | Alert-triggered workflow execution. |
| Grafana | Q2 2026 | Dashboard and alerting integration. |
| Slack | Q1 2026 | Notifications and command execution via Slack. |
| Jira | Q1 2026 | Issue-driven workflow triggers. |
| AWS CloudWatch | Q2 2026 | AWS monitoring event ingestion. |
| Azure Monitor | Q2 2026 | Azure monitoring integration. |
| Terraform integration | Q2 2026 | Orchestrate Terraform operations. |
| Ansible integration | Q2 2026 | Use Ansible as an execution backend. |

### Advanced Features

| Feature | Target | Description |
|---------|--------|-------------|
| SOP-workflow sync | Q2 2026 | Bi-directional sync between documentation and automation. |
| Natural language workflows | Q3 2026 | Generate workflows from plain-English descriptions. |
| Edge AI models | Q3 2026 | Deploy AI on managed nodes for autonomous decision-making. |
| MCP integration | Q3 2026 | Model Context Protocol for extended AI capabilities. |
| Incident analysis | Q4 2026 | AI-powered incident correlation and remediation suggestions. |

---

**Questions about a specific feature?** [Contact us](mailto:info@opsz.io)

**Want to influence the roadmap?** [Schedule a call](mailto:info@opsz.io)
