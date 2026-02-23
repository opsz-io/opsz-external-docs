# OpsZ Deployment Models

OpsZ offers flexible deployment options to match your infrastructure, compliance requirements, and operational preferences.

---

## Model 1: Cloud Deployment

**Best for:** Quick setup, managed infrastructure, cloud-native workloads

```
┌──────────────────────────────────────────────────────────────────────┐
│                     Cloud Provider (AWS/GCP/Azure)                   │
│                                                                      │
│  ┌────────────────────────────────────────────────────────────────┐ │
│  │                    OpsZ Control Plane                          │ │
│  │                   (Kubernetes Cluster)                         │ │
│  │                                                                │ │
│  │  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌──────────────┐    │ │
│  │  │   UI    │  │   APIs  │  │Database │  │Data Pipeline │    │ │
│  │  └─────────┘  └─────────┘  └─────────┘  └──────────────┘    │ │
│  └────────────────────────────────────────────────────────────────┘ │
│           ▲                                                          │
│           │ TLS Connection (HTTPS)                                  │
│           │                                                          │
│  ┌────────┴─────────────────┐    ┌────────────────────────┐        │
│  │  Managed Systems         │    │  Managed Systems       │        │
│  │  (Cloud VMs)             │    │  (Kubernetes Pods)     │        │
│  │                          │    │                        │        │
│  │  [Agent] [Agent] [Agent] │    │  [Agent] [Agent] [...]│        │
│  └──────────────────────────┘    └────────────────────────┘        │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**Characteristics:**
- **Installation:** Automated deployment via Helm/Terraform
- **Scaling:** Elastic scaling with cloud auto-scaling groups
- **Backup:** Cloud-managed backups and snapshots
- **Cost:** Cloud infrastructure costs + OpsZ licensing
- **Data residency:** Data stays within cloud provider region

**Use Cases:**
- Cloud-native organizations
- Startups and fast-growing companies
- Development and testing environments
- Multi-region deployments with replication

---

## Model 2: On-Premises Deployment

**Best for:** Data sovereignty, air-gapped environments, compliance requirements

```
┌──────────────────────────────────────────────────────────────────────┐
│                     Customer Data Center                             │
│                                                                      │
│  ┌────────────────────────────────────────────────────────────────┐ │
│  │                    OpsZ Control Plane                          │ │
│  │                   (Kubernetes Cluster)                         │ │
│  │                                                                │ │
│  │  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌──────────────┐    │ │
│  │  │   UI    │  │   APIs  │  │Database │  │Data Pipeline │    │ │
│  │  └─────────┘  └─────────┘  └─────────┘  └──────────────┘    │ │
│  └────────────────────────────────────────────────────────────────┘ │
│           ▲                                                          │
│           │ TLS Connection (Intranet)                               │
│           │                                                          │
│  ┌────────┴─────────────────┐    ┌────────────────────────┐        │
│  │  Managed Systems         │    │  Managed Systems       │        │
│  │  (Physical Servers)      │    │  (VMs)                 │        │
│  │                          │    │                        │        │
│  │  [Agent] [Agent] [Agent] │    │  [Agent] [Agent] [...]│        │
│  └──────────────────────────┘    └────────────────────────┘        │
│                                                                      │
│                    NO OUTBOUND INTERNET ACCESS REQUIRED              │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**Characteristics:**
- **Installation:** Self-contained package, air-gap capable
- **Control:** Full customer control over infrastructure
- **Data:** All data stays on-premises
- **Compliance:** Easier compliance for regulated industries
- **Cost:** Hardware costs + OpsZ licensing

**Use Cases:**
- Regulated industries (finance, healthcare, government)
- Air-gapped environments
- Organizations with strict data residency requirements
- Existing on-premises infrastructure investment

---

## Model 3: Hybrid Deployment

**Best for:** Mixed infrastructure, gradual cloud migration, disaster recovery

```
┌──────────────────────────────────────────────────────────────────────┐
│                         Cloud (AWS/GCP/Azure)                        │
│                                                                      │
│  ┌────────────────────────────────────────────────────────────────┐ │
│  │              OpsZ Control Plane (Primary)                      │ │
│  │                                                                │ │
│  │  ┌─────────┐  ┌──���──────┐  ┌─────────┐  ┌──────────────┐    │ │
│  │  │   UI    │  │   APIs  │  │Database │  │Data Pipeline │    │ │
│  │  └─────────┘  └─────────┘  └─────────┘  └──────────────┘    │ │
│  └────┬───────────────────────────────────────────────────────────┘ │
│       │                                                              │
│  ┌────▼────────────────┐                                            │
│  │  Cloud Workloads    │                                            │
│  │  [Agent] [Agent]... │                                            │
│  └─────────────────────┘                                            │
│                                                                      │
└────────────────┬─────────────────────────────────────────────────────┘
                 │
                 │ TLS Connection (VPN/Direct Connect)
                 │
┌────────────────▼─────────────────────────────────────────────────────┐
│                     Customer Data Center                             │
│                                                                      │
│  ┌────────────────────────────────────────────────────────────────┐ │
│  │          OpsZ Control Plane (Replica - Optional)               │ │
│  │                (Stream Replication)                            │ │
│  └────────────────────────────────────────────────────────────────┘ │
│           │                                                          │
│  ┌────────▼─────────────────┐    ┌────────────────────────┐        │
│  │  On-Prem Workloads       │    │  Database Systems      │        │
│  │  [Agent] [Agent] [Agent] │    │  [Agent] [Agent] [...]│        │
│  └──────────────────────────┘    └────────────────────────┘        │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

**Characteristics:**
- **Flexibility:** Manage both cloud and on-prem from one platform
- **Migration:** Support gradual cloud migration
- **DR:** Geographic redundancy with cross-site replication
- **Compliance:** Keep sensitive data on-prem, other workloads in cloud
- **Cost:** Mixed infrastructure + OpsZ licensing

**Use Cases:**
- Organizations in cloud migration
- Disaster recovery requirements
- Sensitive workloads on-prem, others in cloud
- Multi-site geographic distribution

---

## Model 4: Multi-Cloud

**Best for:** Avoid vendor lock-in, leverage best-of-breed services, resilience

```
┌──────────────────────────────┐      ┌──────────────────────────────┐
│         AWS Cloud            │      │         GCP Cloud            │
│                              │      │                              │
│  ┌────────────────────────┐  │      │  ┌────────────────────────┐  │
│  │ OpsZ Control Plane     │  │      │  │ OpsZ Control Plane     │  │
│  │     (Primary)          │◄─┼──────┼─►│     (Replica)          │  │
│  └───┬────────────────────┘  │      │  └───┬────────────────────┘  │
│      │                       │      │      │                       │
│  ┌───▼─────────────────┐     │      │  ┌───▼─────────────────┐     │
│  │  AWS Workloads      │     │      │  │  GCP Workloads      │     │
│  │  [Agent] [Agent]... │     │      │  │  [Agent] [Agent]... │     │
│  └─────────────────────┘     │      │  └─────────────────────┘     │
│                              │      │                              │
└──────────────┬───────────────┘      └───────────────┬──────────────┘
               │                                      │
               │      Cross-Cloud Replication         │
               │                                      │
               └──────────────┬───────────────────────┘
                              │
               ┌──────────────▼──────────────┐
               │     Azure Cloud             │
               │                             │
               │  ┌───────────────────────┐  │
               │  │  Azure Workloads      │  │
               │  │  [Agent] [Agent]...   │  │
               │  └───────────────────────┘  │
               │                             │
               └─────────────────────────────┘
```

**Characteristics:**
- **Vendor independence:** No lock-in to single cloud provider
- **Resilience:** Failover between cloud providers
- **Optimization:** Use best services from each cloud
- **Cost arbitrage:** Shift workloads to cheapest provider
- **Compliance:** Geographic data residency across clouds

**Use Cases:**
- Avoid vendor lock-in
- Disaster recovery across cloud providers
- Leverage unique features from multiple clouds
- Cost optimization through cloud arbitrage

---

## Model 5: Edge Deployment

**Best for:** IoT, retail, manufacturing, distributed locations

```
┌──────────────────────────────────────────────────────────────────────┐
│                         Central Cloud                                │
│                                                                      │
│  ┌────────────────────────────────────────────────────────────────┐ │
│  │              OpsZ Control Plane (Central)                      │ │
│  │                                                                │ │
│  │  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌──────────────┐    │ │
│  │  │   UI    │  │   APIs  │  │Database │  │Data Pipeline │    │ │
│  │  └─────────┘  └─────────┘  └─────────┘  └──────────────┘    │ │
│  └────────────────────────────────────────────────────────────────┘ │
│                                                                      │
└────────────┬─────────────────────────────────────┬───────────────────┘
             │                                     │
             │ Stream Replication                  │
             │                                     │
    ┌────────▼────────┐                   ┌───────▼────────┐
    │  Edge Site 1    │                   │  Edge Site 2   │
    │  (Retail Store) │                   │  (Factory)     │
    │                 │                   │                │
    │  ┌───────────┐  │                   │  ┌──────────┐  │
    │  │OpsZ Agent │  │                   │  │OpsZ Agent│  │
    │  │  (Local)  │  │                   │  │ (Local)  │  │
    │  └─────┬─────┘  │                   │  └────┬─────┘  │
    │        │        │                   │       │        │
    │  ┌─────▼──────┐ │                   │  ┌────▼──────┐ │
    │  │   Edge     │ │                   │  │   Edge    │ │
    │  │  Devices   │ │                   │  │ Devices   │ │
    │  │ [IoT] [POS]│ │                   │  │[Sensors]  │ │
    │  └────────────┘ │                   │  └───────────┘ │
    │                 │                   │                │
    └─────────────────┘                   └────────────────┘
```

**Characteristics:**
- **Local processing:** Agents at edge for low-latency operations
- **Offline capability:** Continue working during connectivity loss
- **Bandwidth efficient:** Only sync necessary data to central
- **Scalability:** Hundreds or thousands of edge locations
- **Central visibility:** Unified view of all edge locations

**Use Cases:**
- Retail stores (POS systems, inventory)
- Manufacturing plants (equipment monitoring)
- IoT deployments (sensors, devices)
- Branch offices with unreliable connectivity

---

## Deployment Comparison

| Feature | Cloud | On-Premises | Hybrid | Multi-Cloud | Edge |
|---------|-------|-------------|--------|-------------|------|
| **Setup Time** | Fast (~30 min) | Moderate | Moderate | Moderate | Complex |
| **Scaling** | Elastic | Manual | Mixed | Elastic | Distributed |
| **Data Control** | Cloud provider | Customer | Mixed | Mixed | Mixed |
| **Cost Model** | Consumption | CapEx | Mixed | Consumption | Mixed |
| **Internet Dependency** | Required | Optional | Required | Required | Optional |
| **Compliance** | Provider-dependent | Customer control | Mixed | Mixed | Mixed |
| **High Availability** | Cloud-native | Customer-managed | Cross-site | Cross-cloud | Distributed |
| **Best For** | Cloud-native | Regulated | Migration | Vendor independence | Distributed |

---

## Choosing Your Deployment Model

### Decision Factors

**1. Regulatory and Compliance Requirements**
- **High compliance needs:** On-premises or hybrid
- **Moderate compliance:** Hybrid or cloud with compliance certifications
- **Low compliance concerns:** Cloud or multi-cloud

**2. Data Sensitivity**
- **Highly sensitive data:** On-premises
- **Mixed sensitivity:** Hybrid (sensitive on-prem, other in cloud)
- **Low sensitivity:** Cloud or multi-cloud

**3. Existing Infrastructure**
- **Significant on-prem investment:** On-premises or hybrid
- **Cloud-native:** Cloud or multi-cloud
- **Mixed environment:** Hybrid
- **Distributed locations:** Edge

**4. Internet Connectivity**
- **Reliable high-speed:** Cloud or multi-cloud
- **Unreliable or restricted:** On-premises or edge
- **Air-gapped:** On-premises only

**5. Operational Expertise**
- **Cloud expertise:** Cloud or multi-cloud
- **Infrastructure expertise:** On-premises
- **Mixed:** Hybrid
- **Limited:** Managed cloud

**6. Scale and Growth**
- **Rapid growth:** Cloud (elastic scaling)
- **Stable scale:** On-premises or hybrid
- **Global expansion:** Multi-cloud or edge

**7. Budget and Cost Model**
- **OpEx preferred:** Cloud or multi-cloud
- **CapEx preferred:** On-premises
- **Mixed:** Hybrid

---

## Migration Paths

### Cloud-First Organization

Start with cloud deployment, add on-premises as needed:
```
Cloud Only → Hybrid (cloud + on-prem) → Multi-Cloud
```

### Traditional Organization

Start on-premises, migrate to cloud gradually:
```
On-Premises → Hybrid (on-prem + cloud) → Cloud or Multi-Cloud
```

### Distributed Organization

Start with central deployment, add edge as needed:
```
Cloud/On-Prem → Hybrid → Edge
```

---

## Deployment Support

Need help choosing the right deployment model?

- **Evaluation:** [Schedule an architecture review](mailto:solutions@opsz.io)
- **Proof of concept:** Test multiple models before committing
- **Migration assistance:** Professional services for deployment and migration
- **Training:** Learn to manage your chosen deployment model

---

**Next Steps:**
- [Deployment Guide](../deployment.md) — Detailed installation instructions
- [Architecture](../architecture.md) — Technical architecture details
- [Security](../security.md) — Security considerations for each model
