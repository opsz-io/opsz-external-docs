# Getting Started with OpsZ

**Last updated:** February 2026

This guide will help you get up and running with OpsZ quickly. For detailed documentation, see the complete [product documentation](README.md).

---

## Overview

OpsZ connects four key capabilities:

1. **Infrastructure Awareness** — Continuous discovery of your systems
2. **Workflow Orchestration** — Automate operations across your fleet
3. **Access Control** — Govern who can do what
4. **Intelligence** — AI-powered automation and insights

This guide walks through basic usage of each capability.

---

## Prerequisites

Before you begin, ensure:

- OpsZ is installed and accessible (see [Deployment Guide](deployment.md))
- You have administrator credentials
- You understand your use case (see [Use Cases](use-cases.md))

---

## Step 1: Access the Platform

### Log In

1. Navigate to your OpsZ URL (e.g., `https://opsz.example.com`)
2. Log in with your credentials
3. If using SSO, click "Sign in with [Provider]"

### First Login

On first login, you'll see:
- Dashboard with platform overview
- Quick start checklist
- Navigation menu for main features

**Recommended:** Change your password immediately if using username/password authentication.

---

## Step 2: Deploy Agents

Agents provide connectivity to your managed systems.

### Install Agent on Linux

```bash
# Download and run installer
curl -O https://packages.opsz.io/install-agent.sh
sudo bash install-agent.sh

# Agent auto-provisions and registers
# Check status
sudo systemctl status opsz-agent
```

### Install Agent on Windows

```powershell
# Download installer
Invoke-WebRequest -Uri https://packages.opsz.io/opsz-agent-windows.exe -OutFile opsz-agent.exe

# Run installer
.\opsz-agent.exe install

# Check status
Get-Service opsz-agent
```

### Verify Agents

1. Go to **Infrastructure → Hosts** in the UI
2. You should see your systems appearing (within 5 minutes)
3. Click on a host to see detailed information

**Tip:** Start with 2-3 test systems before deploying to production fleet.

---

## Step 3: Explore Infrastructure

### View Your Inventory

1. Navigate to **Infrastructure → Hosts**
2. See all discovered systems with:
   - Hostname and IP addresses
   - Operating system and version
   - Hardware details (CPU, memory, network)
   - Agent status and version

### Search and Filter

- **Search:** Type hostname or IP address
- **Filter by metadata:** Environment, datacenter, application
- **Group by attribute:** Organize by custom attributes

### View Host Details

Click on any host to see:
- Complete hardware topology
- Network interfaces and addresses
- Storage devices
- NUMA architecture (for performance tuning)
- Custom metadata

**Try it:** Find your test system and explore its details.

---

## Step 4: Create Your First Workflow

### Option A: Use a Template (Recommended)

1. Go to **Workflows → Templates**
2. Browse the template library
3. Search for "ping test" or use AI search: "check if system is reachable"
4. Click on a template to preview
5. Click **Create Workflow from Template**
6. Customize parameters if needed
7. Save your workflow

### Option B: Create from Scratch

1. Go to **Workflows → Create New**
2. Give it a name: "My First Workflow"
3. Add a step:
   - Type: Shell command
   - Command: `hostname && uptime`
4. Click **Save**

### Workflow Structure

Workflows are defined as steps in a DAG (directed acyclic graph):

```yaml
name: System Check
steps:
  - name: check_disk
    command: df -h

  - name: check_memory
    command: free -m
    depends_on: [check_disk]

  - name: report
    command: echo "Health check complete"
    depends_on: [check_disk, check_memory]
```

Steps can run in parallel or sequence based on dependencies.

---

## Step 5: Execute a Job

### Create a Job

1. Go to **Jobs → Create Job**
2. Select your workflow
3. Choose target systems:
   - Select specific hosts
   - Or use a group/tag to target multiple systems
4. Review and click **Execute**

### Monitor Execution

Watch real-time progress:
- **Status:** Pending → Running → Success/Failed
- **Logs:** See output from each step
- **Timeline:** View execution duration
- **Results:** Collect output from all targets

### Review Results

After completion:
- View success/failure per target
- Download logs
- View execution metrics
- Analyze any failures

**Try it:** Run your first workflow on your test systems.

---

## Step 6: Set Up Access Control

### Create Users

1. Go to **Access → Users**
2. Click **Create User**
3. Enter details (or import from SSO/LDAP)
4. Assign to groups

### Create Roles

1. Go to **Access → Roles**
2. Click **Create Role**
3. Define permissions:
   - View infrastructure: `read:host`
   - Execute workflows: `execute:job_request`
   - Approve jobs: `approve:job_action`
4. Save role

### Assign Roles

1. Go to user or group
2. Click **Assign Role**
3. Select role
4. Optionally set expiration (temporary access)

**Tip:** Start with built-in roles (Viewer, Operator, Administrator) and customize as needed.

---

## Step 7: Configure Approval Workflows

For sensitive operations, add approval gates:

### Create Approval Rule

1. Go to **Workflows → [Your Workflow]**
2. Enable **Require Approval**
3. Configure:
   - Who can approve (role or specific users)
   - How many approvals needed
   - Timeout (how long before expiry)
4. Save

### Approval Flow

When a user executes a workflow with approval required:

1. Job is created in **Pending Approval** state
2. Approvers receive notification
3. Approver reviews and approves/denies
4. If approved, job executes automatically
5. If denied, job is cancelled with reason

**Try it:** Create a workflow that requires your approval, then approve it.

---

## Step 8: Schedule Recurring Jobs

Automate operations with cron scheduling:

### Create Scheduled Job

1. Go to **Jobs → Schedules**
2. Click **Create Schedule**
3. Select workflow
4. Choose targets
5. Set schedule:
   - Cron expression: `0 2 * * *` (daily at 2 AM)
   - Or use helpers: Daily, Weekly, Monthly
6. Save

### Manage Schedules

- **View upcoming runs:** See next execution time
- **Pause/resume:** Temporarily disable schedule
- **View history:** See past executions
- **Edit schedule:** Change timing or targets

**Example schedules:**
- Daily health checks
- Weekly patch deployments
- Monthly compliance scans
- Hourly monitoring and alerting

---

## Step 9: Integrate External Systems

### GitHub Webhooks

1. Go to **Integrations → GitHub**
2. Click **Add Webhook**
3. Generate secret key
4. Configure in GitHub:
   - Webhook URL: `https://opsz.example.com/api/v1/integrations/github/webhook`
   - Secret: [paste generated key]
   - Events: Select events to trigger workflows
5. Test webhook

Now GitHub events (PRs, merges, releases) can trigger workflows automatically.

### Other Integrations

See [Integrations Guide](integrations.md) for:
- ServiceNow incidents
- PagerDuty alerts
- Slack notifications
- Custom webhooks

---

## Step 10: Use AI-Powered Search

Find workflows quickly with natural language:

### Semantic Search

1. Go to **Workflows → Templates**
2. Click in search box
3. Instead of keywords, describe what you want:
   - "restart a service and check if it's running"
   - "check disk space and alert if low"
   - "patch systems and verify success"
4. Review ranked results
5. Click to preview and use

### How It Works

- AI understands intent, not just keywords
- Searches workflow names, descriptions, and individual steps
- Ranks results by relevance
- Learns from usage patterns

**Try it:** Search for "check if a port is open" and explore results.

---

## Best Practices

### Security

- **Enable SSO:** Use Okta, Azure AD, or LDAP
- **Enforce MFA:** Especially for administrators
- **Use approval workflows:** For production changes
- **Review audit logs:** Regularly check for anomalies
- **Rotate credentials:** Periodically update passwords and tokens

### Workflow Design

- **Start simple:** Begin with single-step workflows
- **Add error handling:** Anticipate failures
- **Use templates:** Don't reinvent the wheel
- **Test first:** Always test on non-production systems
- **Document:** Add descriptions and comments

### Agent Management

- **Gradual rollout:** Deploy agents incrementally
- **Monitor health:** Check agent status regularly
- **Keep updated:** Enable automatic agent updates
- **Group strategically:** Use metadata for logical grouping

### Governance

- **Principle of least privilege:** Grant minimum necessary permissions
- **Temporary access:** Use time-limited grants when possible
- **Regular access reviews:** Remove unnecessary permissions
- **Approval chains:** Multi-level approval for sensitive ops

---

## Common Use Cases

### 1. Incident Response

**Scenario:** Server becomes unresponsive

**Workflow:**
1. Check if system is reachable (ping)
2. Check critical services status
3. Restart failed services
4. Verify services are running
5. Notify team of resolution

**Benefits:** Faster resolution, consistent response, audit trail

### 2. Patch Management

**Scenario:** Deploy monthly security patches

**Workflow:**
1. Check current patch level
2. Download and stage patches
3. Apply patches with approval gate
4. Verify successful installation
5. Reboot if required
6. Confirm system health
7. Report on patch status

**Benefits:** Automated patching, approval controls, compliance reporting

### 3. Compliance Scanning

**Scenario:** Daily compliance checks

**Workflow:**
1. Scan for configuration drift
2. Check for unauthorized changes
3. Verify security settings
4. Generate compliance report
5. Alert on violations

**Benefits:** Continuous compliance, automated reporting, early detection

### 4. Cloud Cost Optimization

**Scenario:** Identify underutilized resources

**Workflow:**
1. Query cloud provider APIs
2. Identify idle or underutilized instances
3. Generate rightsizing recommendations
4. Request approval for changes
5. Apply optimizations
6. Track cost savings

**Benefits:** Reduced cloud spend, data-driven decisions, approval workflow

---

## Troubleshooting

### Agents Not Showing Up

**Symptoms:** Installed agent doesn't appear in inventory

**Solutions:**
- Check network connectivity (can agent reach control plane?)
- Verify firewall rules (outbound HTTPS allowed?)
- Check agent logs: `/var/log/opsz/agent.log`
- Verify agent configuration: `/etc/opsz/agent.yaml`

### Workflow Execution Fails

**Symptoms:** Workflow fails with errors

**Solutions:**
- Review job logs for error messages
- Check if target systems are reachable
- Verify command syntax and permissions
- Test command manually on target system
- Check for timeout issues

### Slow Performance

**Symptoms:** UI is slow, queries take long

**Solutions:**
- Check database performance
- Review Kubernetes resource utilization
- Check for high event volume
- Scale up replicas if needed
- Review workflow complexity

---

## Next Steps

### Learn More

- **[Architecture](architecture.md):** Understand how OpsZ works
- **[Features](features.md):** Explore all capabilities
- **[Use Cases](use-cases.md):** Real-world scenarios
- **[Integrations](integrations.md):** Connect your tools

### Get Help

- **Documentation:** [https://docs.opsz.io](https://docs.opsz.io)
- **Community:** [https://community.opsz.io](https://community.opsz.io)
- **Support:** [support@opsz.io](mailto:support@opsz.io)
- **Training:** Contact your customer success manager

### Join the Community

- **Slack:** Join our community Slack workspace
- **Office hours:** Monthly product office hours
- **User group:** Connect with other OpsZ users
- **Feedback:** Share ideas and feature requests

---

**Ready to automate?** Start with a simple workflow and expand from there. The key is to begin small, learn the platform, and gradually increase complexity.

**Questions?** We're here to help: [support@opsz.io](mailto:support@opsz.io)
