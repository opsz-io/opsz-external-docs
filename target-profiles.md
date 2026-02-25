# Target Profiles

**Last updated:** February 2026

## Overview

A Target Profile maps nicknames to real servers. Instead of a workflow saying "run this on `10.0.1.50`", it says "run this on `primary_db`" — and the profile knows that `primary_db` is actually `10.0.1.50`.

This means you write a workflow once and reuse it everywhere. Customer A's profile says `primary_db` is their AWS database. Customer B's profile says `primary_db` is their on-prem database. Same workflow, different infrastructure.

**Quick example:**

```
Profile: "Acme Production"
  web_server  → acme-web-01.example.com   (agent-based)
  primary_db  → acme-db-prod.example.com  (SSH)
  alerts      → https://acme.pagerduty.com/events  (HTTP)

Profile: "BigCorp Production"
  web_server  → bc-web-01.internal        (agent-based)
  primary_db  → bc-oracle-prod.internal   (SSH)
  alerts      → https://bigcorp.slack.com/webhook  (HTTP)
```

One "Deploy Web App" workflow targets `web_server`, `primary_db`, and `alerts`. Run it against Acme's profile — it hits Acme's servers. Run it against BigCorp's profile — it hits BigCorp's servers. No changes to the workflow.

## Why Target Profiles?

Without profiles, every workflow has server names baked in. That means:

- **Copy-paste workflows** — rewriting the same workflow for each customer or environment
- **Brittle maintenance** — a server gets replaced and you're hunting through every workflow that references it
- **No dev-to-prod promotion** — can't test a workflow in dev and run the same thing in production

Profiles fix this: workflows reference nicknames, profiles translate them to real infrastructure.

## How It Works

### Target Mapping Types

Each mapping in a profile associates a logical name with one of four target types:

| Type | What It Resolves To | Example |
|------|-------------------|---------|
| **Host** | A single server or instance | `"primary_db"` maps to `db-prod-01.example.com` |
| **Host Group** | A dynamic set of servers matching a query | `"web_servers"` maps to all hosts tagged `web` in `us-east-1` |
| **API Endpoint** | An external service URL | `"notification"` maps to a PagerDuty or Slack webhook |
| **Kubernetes Cluster** | A K8s cluster | `"k8s_runner"` maps to a production EKS/GKE/AKS cluster |

Each mapping also specifies a **connection method** (agent-based, SSH, HTTP, or Kubernetes API), an optional **execution user**, and a **location** hint for routing.

### Dynamic Host Groups

Host group mappings don't point to a static list of servers — they define a query that resolves at execution time. Queries can filter by:

- **Tags** — all specified tags must match (e.g., `["web", "production"]`)
- **Region** — geographic or cloud region
- **Provider** — AWS, GCP, Azure, on-premises
- **Status** — active, maintenance, etc.

This means your profile automatically picks up new servers as they're added to your fleet, without manual updates.

### Profile Metadata

Every profile includes:

- **Environment classification** — dev, staging, production, or testing
- **Status** — active, inactive, or draft
- **Default flag** — mark one profile as the default per environment
- **Tags** — arbitrary metadata for organization and filtering

## Key Use Cases

### One Template, Many Customers

A managed service provider creates a single `deploy-web-app` workflow template. Each customer gets their own target profile:

| Profile | `web_servers` resolves to | `primary_db` resolves to |
|---------|--------------------------|--------------------------|
| `acme-prod` | `acme-web-01`, `acme-web-02` (AWS) | `acme-db-01` (AWS) |
| `bigcorp-prod` | `bc-web-01` through `bc-web-10` (on-prem) | `bc-db-cluster` (on-prem) |
| `startup-prod` | `startup-web-01` (GCP) | `startup-db-01` (GCP) |

The same template runs identically across all three — the profile handles the infrastructure differences.

### Environment Promotion

Clone a profile to promote workflows across environments:

| Profile | `app_server` | `database` |
|---------|-------------|------------|
| `acme-dev` | 1 dev server | dev database |
| `acme-staging` | 2 staging servers | staging database |
| `acme-prod` | 10 production servers (via tag query) | production cluster |

The workflow template stays the same — only the profile changes.

### Multi-Cloud Within a Single Profile

A profile can map different targets to different cloud providers and connection types:

| Logical Name | Type | Provider | Target |
|-------------|------|----------|--------|
| `web_servers` | Host Group | AWS | All hosts tagged `web` in AWS |
| `api_gateway` | K8s Cluster | GCP | GKE cluster |
| `monitoring` | API Endpoint | — | Grafana API |
| `batch_workers` | Host Group | Azure | All hosts tagged `batch` in Azure |

## Workflow Integration

### Execution Flow

1. **Create a job** — select a workflow template and a target profile
2. **Validation** — OpsZ checks the profile has mappings for all targets the template requires
3. **Resolution** — each logical name is resolved to actual infrastructure (hostnames, IPs, endpoints, cluster details)
4. **Execution** — each workflow step runs against its resolved target using the appropriate connection method
5. **Monitoring** — real-time status tracking per step, per target

### Pre-Execution Validation

Before a workflow runs, OpsZ validates the profile against the template's requirements:

- **Missing targets** — the template needs a target the profile doesn't have (blocks execution)
- **Extra targets** — the profile has mappings the template doesn't use (warning only)
- **Offline targets** — a mapped host or endpoint is unreachable (warning)

The UI shows these results in real time as you select a profile, so issues are caught before execution.

## Profile Management

### Clone

Deep-copy a profile to create a variant for a different environment or customer. Cloning copies all mappings — you only need to update the targets that differ.

### Import / Export

Export profiles as JSON for backup, sharing, or migrating between OpsZ instances. Import recreates the profile with all its mappings.

### UI

Target Profiles are managed through the OpsZ dashboard:

- **List view** — browse profiles by environment (dev, staging, production, testing), search by name, paginate through large sets
- **Edit view** — modify profile metadata and manage individual target mappings
- **Profile selector** — when creating a job, an autocomplete dropdown shows available profiles with real-time validation against the selected template

---

**Questions about Target Profiles?** [Contact us](mailto:info@opsz.io)
