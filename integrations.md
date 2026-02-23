# OpsZ Integration Capabilities

**Last updated:** February 2026

OpsZ is designed to work with your existing tools, not replace them. Our API-first architecture enables seamless integration with the platforms you already use.

## Integration Philosophy

OpsZ acts as an **orchestration layer** that connects your existing tools:

```
Monitoring → OpsZ → Execution
Ticketing ← OpsZ → Configuration Management
Collaboration ← OpsZ → Infrastructure
```

The platform doesn't require rip-and-replace — it enhances and automates workflows across your current stack.

---

## Current Integrations

### GitHub

**Status:** Beta
**Capabilities:**
- Webhook integration for pull requests, issues, and workflow events
- Automatic job triggers based on repository activity
- HMAC-SHA256 signature verification for security
- Bidirectional integration (trigger workflows from GitHub, update GitHub from OpsZ)

**Use Cases:**
- Run automated tests when PRs are created
- Deploy infrastructure changes after PR merge
- Create incidents when builds fail
- Update issue status based on workflow outcomes

### Identity Providers (Okta, Azure AD, LDAP)

**Status:** GA
**Capabilities:**
- OAuth 2.0 / SAML single sign-on
- LDAP / Active Directory authentication
- Multi-factor authentication support
- Automatic user provisioning and deprovisioning
- Group-based access control

**Use Cases:**
- Centralized authentication with corporate identity provider
- Sync organizational structure for access control
- Enforce MFA for sensitive operations
- Audit user access across platforms

### Streaming & Analytics (Kafka, Flink)

**Status:** GA
**Capabilities:**
- Real-time event streaming
- Integration with existing Kafka infrastructure
- Custom Flink jobs for data processing
- Message bus connectivity

**Use Cases:**
- Feed infrastructure events to data lake
- Integrate with enterprise analytics platform
- Custom event processing pipelines
- Real-time dashboard updates

---

## Planned Integrations

### ServiceNow

**Target:** Q1 2026
**Capabilities:**
- Bi-directional incident management
- Automated incident creation from OpsZ alerts
- Workflow execution triggered by ServiceNow incidents
- Change management integration
- CMDB synchronization

**Use Cases:**
- Auto-create ServiceNow incidents when infrastructure issues detected
- Execute remediation workflows directly from ServiceNow tickets
- Update incident status automatically as workflows complete
- Keep CMDB in sync with live infrastructure

### PagerDuty

**Target:** Q1 2026
**Capabilities:**
- Alert-triggered workflow execution
- Automatic acknowledgment and resolution
- On-call schedule integration
- Incident response automation

**Use Cases:**
- Automatically execute runbooks when alerts fire
- Route incidents based on on-call schedules
- Auto-remediate common issues before paging engineers
- Update PagerDuty status from workflow outcomes

### Grafana

**Target:** Q2 2026
**Capabilities:**
- Dashboard integration
- Alert rule integration
- Annotation of workflows on dashboards
- Metrics export from OpsZ to Grafana

**Use Cases:**
- Trigger workflows from Grafana alerts
- Visualize OpsZ metrics alongside infrastructure metrics
- Annotate dashboards with workflow execution events
- Track automation impact on system performance

### Slack

**Target:** Q1 2026
**Capabilities:**
- Workflow notifications
- Interactive approvals via Slack
- Command execution from Slack
- Incident collaboration

**Use Cases:**
- Notify teams when workflows complete
- Approve sensitive operations without leaving Slack
- Run workflows via slash commands
- Collaborate on incidents in Slack channels

### Jira

**Target:** Q1 2026
**Capabilities:**
- Issue-triggered workflows
- Automated issue updates
- Sprint and project integration
- Custom field mapping

**Use Cases:**
- Automate deployment when tickets move to "Ready for Deploy"
- Create Jira issues from workflow failures
- Update issue status based on automation progress
- Track automation work alongside development

### AWS CloudWatch

**Target:** Q2 2026
**Capabilities:**
- CloudWatch Events integration
- Metric-based triggers
- Log ingestion
- Alarm-based workflow execution

**Use Cases:**
- React to AWS service events
- Execute workflows when CloudWatch alarms fire
- Collect and analyze CloudWatch logs
- Automate AWS resource management

### Azure Monitor

**Target:** Q2 2026
**Capabilities:**
- Azure Monitor alerts integration
- Metric collection
- Activity log integration
- Resource health monitoring

**Use Cases:**
- Trigger workflows from Azure Monitor alerts
- Automate Azure resource scaling
- Respond to resource health changes
- Collect and analyze Azure metrics

### Terraform

**Target:** Q2 2026
**Capabilities:**
- Terraform workflow orchestration
- State file monitoring
- Plan and apply automation
- Drift detection and remediation

**Use Cases:**
- Orchestrate multi-environment Terraform deployments
- Automatically remediate infrastructure drift
- Approval workflows for Terraform changes
- Integrate Terraform with broader automation

### Ansible

**Target:** Q2 2026
**Capabilities:**
- Use Ansible playbooks as workflow steps
- Ansible inventory integration
- Playbook execution via OpsZ
- Result collection and logging

**Use Cases:**
- Orchestrate Ansible playbooks across environments
- Add approval gates to Ansible execution
- Combine Ansible with other automation tools
- Centralized logging and audit for Ansible runs

---

## Integration Patterns

### Webhooks (Inbound)

OpsZ can receive webhooks from external systems to trigger workflows:

- Configurable webhook endpoints per integration
- HMAC signature verification for security
- Flexible payload mapping to workflow parameters
- Automatic retries and error handling

**Supported Events:**
- Repository events (commits, PRs, releases)
- Alert and monitoring events
- Ticketing system updates
- Custom application events

### Webhooks (Outbound)

OpsZ can send webhooks to external systems:

- Configurable webhook destinations
- Event-based or workflow-based triggers
- Custom payload templates
- Retry logic and failure handling

**Event Types:**
- Workflow started/completed/failed
- Approval requests
- Infrastructure changes
- Custom events

### REST APIs

All OpsZ capabilities are available via REST API:

- Comprehensive API documentation
- Token-based authentication
- Rate limiting and throttling
- Consistent error handling

**Integration Use Cases:**
- Custom dashboards and reporting
- Integration with internal tools
- Automation from CI/CD pipelines
- Data synchronization

### Message Bus Integration

Connect directly to OpsZ's event streaming infrastructure:

- Kafka topic subscriptions
- Custom Flink stream processing jobs
- Real-time event consumption
- Exactly-once processing semantics

**Use Cases:**
- Feed events to data warehouses
- Custom analytics pipelines
- Real-time dashboards
- Integration with enterprise message bus

---

## Custom Integrations

### API-First Design

Every OpsZ capability is available via API, making custom integrations straightforward:

1. **Authenticate** with OAuth 2.0 or API tokens
2. **Discover** available endpoints via API documentation
3. **Integrate** using standard REST conventions
4. **Monitor** integration health via metrics and logs

### Integration Support

- **Documentation:** Comprehensive API docs with examples
- **SDKs:** Python and Go client libraries
- **Support:** Integration assistance for enterprise customers
- **Professional Services:** Custom integration development available

---

## Integration Roadmap

| Quarter | Integrations |
|---------|-------------|
| Q1 2026 | ServiceNow, PagerDuty, Slack, Jira |
| Q2 2026 | Grafana, AWS CloudWatch, Azure Monitor, Terraform, Ansible |
| Q3 2026 | Datadog, Splunk, Elasticsearch, Jenkins |
| Q4 2026 | Google Cloud Monitoring, Kubernetes Operators, Custom webhook library |

---

## Request an Integration

Don't see the integration you need? [Let us know](mailto:integrations@opsz.io)

Enterprise customers can influence integration priorities — [schedule a call](mailto:product@opsz.io) to discuss your needs.
