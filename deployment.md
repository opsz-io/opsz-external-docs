# OpsZ Deployment Guide

**Last updated:** February 2026

This guide provides an overview of deployment options and requirements for OpsZ. For detailed installation instructions, contact your account team or visit our customer portal.

---

## Deployment Options

OpsZ offers flexible deployment models to match your infrastructure and requirements.

### Option 1: Kubernetes / Helm (Recommended for Production)

**Best for:** Production environments, high availability, scalability

**Overview:**
- Container-based deployment on Kubernetes
- Helm charts for automated installation
- Single-command deployment
- Namespace isolation for security

**Requirements:**
- Kubernetes cluster (version 1.24+)
- Helm 3.x
- Persistent storage (for database and streaming components)
- Load balancer (for external access)

**Characteristics:**
- **Installation time:** Approximately 30 minutes
- **Scalability:** Horizontal scaling for all components
- **High availability:** Multi-replica deployments with failover
- **Updates:** Rolling updates with zero downtime

**Architecture:**
```
┌─────────────────────────────────────────┐
│         Kubernetes Cluster              │
│                                         │
│  ┌─────────────────────────────────┐   │
│  │   Control Plane Namespace       │   │
│  │  (UI, APIs, Database)           │   │
│  └─────────────────────────────────┘   │
│                                         │
│  ┌─────────────────────────────────┐   │
│  │   Data Pipeline Namespace       │   │
│  │  (Kafka, Flink, Airflow)        │   │
│  └─────────────────────────────────┘   │
│                                         │
│  ┌─────────────────────────────────┐   │
│  │   Messaging Namespace           │   │
│  │  (NATS, Agents, Routing)        │   │
│  └─────────────────────────────────┘   │
└─────────────────────────────────────────┘
```

### Option 2: Docker Compose

**Best for:** Development, testing, proof-of-concept

**Overview:**
- All services in Docker containers
- Single docker-compose.yml file
- Run on a single host or VM
- Simplified configuration

**Requirements:**
- Docker 20.x or later
- Docker Compose 2.x
- Modern Linux host or macOS
- Recommended: 16GB RAM, 4+ CPU cores, 50GB storage

**Characteristics:**
- **Installation time:** 10-15 minutes
- **Scalability:** Limited to single host
- **Ease of use:** Simplest deployment option
- **Use case:** Development and testing, not production

**Quick Start:**
```bash
# Download compose file
curl -O https://install.opsz.io/docker-compose.yml

# Start all services
docker-compose up -d

# Access UI
open http://localhost:8082
```

### Option 3: Customer On-Premises

**Best for:** Air-gapped environments, strict data residency requirements

**Overview:**
- Self-contained installation package
- Can run without internet access
- Full platform in a portable format
- Customer-managed infrastructure

**Requirements:**
- Kubernetes cluster or VM with Kubernetes (Minikube, K3s, etc.)
- Installation package (provided by OpsZ)
- Configuration file (provided by OpsZ)

**Characteristics:**
- **Installation time:** 30-45 minutes
- **Internet access:** Not required after package download
- **Data residency:** All data stays in customer environment
- **Control:** Full customer control over infrastructure

**Architecture:**
```
┌──────────────────────────────────────┐
│    Customer Data Center              │
│                                      │
│  ┌────────────────────────────────┐ │
│  │     OpsZ Control Plane         │ │
│  │  (Kubernetes/VM)               │ │
│  └────────────────────────────────┘ │
│           ▲                          │
│           │                          │
│  ┌────────┴──────────────────────┐  │
│  │   Managed Systems (Agents)    │  │
│  │                               │  │
│  │  [Server1] [Server2] [...]    │  │
│  └───────────────────────────────┘  │
└──────────────────────────────────────┘
```

### Option 4: Hybrid Cloud

**Best for:** Mixed on-premises and cloud infrastructure

**Overview:**
- Control plane in cloud or on-premises
- Agents in both environments
- Cross-cluster stream replication
- Unified management

**Requirements:**
- Kubernetes cluster for control plane
- Network connectivity between sites
- Cross-site stream replication configured

**Characteristics:**
- **Flexibility:** Run control plane anywhere
- **Scalability:** Scale independently by site
- **Resilience:** Multi-site high availability
- **Compliance:** Keep sensitive data on-premises

**Architecture:**
```
┌──────────────────────┐      ┌──────────────────────┐
│   Cloud (AWS/GCP)    │      │   On-Premises        │
│                      │      │                      │
│  ┌────────────────┐  │      │  ┌────────────────┐  │
│  │ Control Plane  │  │      │  │  Agents        │  │
│  │ (Kubernetes)   │◄─┼──────┼─►│  (Production)  │  │
│  └────────────────┘  │      │  └────────────────┘  │
│                      │      │                      │
│  ┌────────────────┐  │      │  ┌────────────────┐  │
│  │  Agents        │  │      │  │  Database      │  │
│  │  (Dev/Test)    │  │      │  │  (Sensitive)   │  │
│  └────────────────┘  │      │  └────────────────┘  │
└──────────────────────┘      └──────────────────────┘
```

---

## Sizing Guidelines

### Small Deployment (< 500 managed systems)

**Control Plane:**
- 3 Kubernetes nodes
- 4 vCPU, 16GB RAM per node
- 100GB storage (for database, logs, metrics)

**Expected Performance:**
- Sub-second fleet queries
- Hundreds of concurrent workflows
- Millions of events per day

### Medium Deployment (500-5,000 managed systems)

**Control Plane:**
- 5-7 Kubernetes nodes
- 8 vCPU, 32GB RAM per node
- 500GB storage

**Expected Performance:**
- Sub-second fleet queries at scale
- Thousands of concurrent workflows
- Tens of millions of events per day

### Large Deployment (5,000+ managed systems)

**Control Plane:**
- 10+ Kubernetes nodes
- 16 vCPU, 64GB RAM per node
- 1-2TB storage
- Multi-cluster with replication

**Expected Performance:**
- Sub-second fleet queries across entire fleet
- Tens of thousands of concurrent workflows
- Hundreds of millions of events per day

**Note:** These are guidelines. Actual requirements depend on usage patterns, workflow complexity, and data retention policies.

---

## Installation Process

### Pre-Installation

1. **Review requirements** — Ensure your environment meets prerequisites
2. **Plan architecture** — Decide on single-cluster or multi-cluster
3. **Prepare infrastructure** — Provision Kubernetes cluster or VMs
4. **Obtain installation package** — Contact OpsZ team
5. **Review security** — Plan network policies, RBAC, encryption

### Installation Steps

**For Kubernetes/Helm:**
```bash
# 1. Add OpsZ Helm repository
helm repo add opsz https://charts.opsz.io
helm repo update

# 2. Create namespace
kubectl create namespace opsz

# 3. Install with default configuration
helm install opsz opsz/opsz-stack --namespace opsz

# 4. Wait for all pods to be ready (~30 minutes)
kubectl wait --for=condition=ready pod --all -n opsz --timeout=40m

# 5. Access the UI
kubectl port-forward -n opsz svc/cosmos-ui 8082:80
```

**For Docker Compose:**
```bash
# 1. Download compose file
curl -O https://install.opsz.io/docker-compose.yml

# 2. Configure environment
cp .env.example .env
vi .env  # Edit configuration

# 3. Start all services
docker-compose up -d

# 4. Check status
docker-compose ps

# 5. Access UI
open http://localhost:8082
```

**For On-Premises:**
```bash
# 1. Extract installation package
tar -xzf opsz-install-package.tar.gz
cd opsz-install

# 2. Review and edit configuration
vi config.yaml

# 3. Run installation script
./install.sh

# 4. Verify installation
./verify-install.sh
```

### Post-Installation

1. **Access UI** — Navigate to the web interface
2. **Log in** — Use default credentials (change immediately)
3. **Configure identity provider** — Set up SSO (Okta, LDAP, etc.)
4. **Deploy agents** — Install agents on managed systems
5. **Create users and roles** — Set up your team
6. **Import workflows** — Add templates from the library
7. **Test** — Run a simple workflow to verify everything works

---

## Agent Deployment

### Agent Installation

Agents are deployed on systems you want to manage:

**Linux (via package manager):**
```bash
# Debian/Ubuntu
curl -O https://packages.opsz.io/install-agent.sh
sudo bash install-agent.sh

# RHEL/CentOS
sudo yum install opsz-agent

# From binary
curl -O https://packages.opsz.io/opsz-agent
chmod +x opsz-agent
sudo ./opsz-agent install
```

**Windows:**
```powershell
# Download installer
Invoke-WebRequest -Uri https://packages.opsz.io/opsz-agent-windows.exe -OutFile opsz-agent.exe

# Run installer
.\opsz-agent.exe install
```

**Kubernetes (as DaemonSet):**
```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: opsz-agent
spec:
  selector:
    matchLabels:
      app: opsz-agent
  template:
    metadata:
      labels:
        app: opsz-agent
    spec:
      containers:
      - name: agent
        image: opsz/agent:latest
        env:
        - name: OPSZ_SERVER
          value: "https://opsz.example.com"
```

### Agent Configuration

Minimal configuration required:

```yaml
# /etc/opsz/agent.yaml
server: https://opsz.example.com
identity:
  certificate: /etc/opsz/agent-cert.pem
  key: /etc/opsz/agent-key.pem
```

Agents self-provision on first connection — no manual certificate distribution needed.

---

## Network Configuration

### Firewall Rules

**Agents (managed systems):**
- Allow outbound HTTPS (TCP 443) to control plane
- No inbound ports required

**Control plane (Kubernetes cluster):**
- Allow inbound HTTPS (TCP 443) for UI/API access
- Allow inbound connections from agents (TCP 443 or configured port)
- Inter-pod communication (handled by Kubernetes networking)

### Load Balancer

For production deployments, configure a load balancer:

- Distribute UI/API traffic across replicas
- SSL/TLS termination (optional)
- Health check endpoints available
- Session affinity not required (stateless APIs)

### DNS

Configure DNS for easier access:

- `opsz.example.com` → Load balancer IP (UI/API)
- `agents.opsz.example.com` → Agent connection endpoint
- Wildcard SSL certificate recommended

---

## High Availability

### Multi-Replica Deployment

All stateless components support multiple replicas:

- **UI/APIs:** 3+ replicas for high availability
- **Job processors:** 2+ replicas for redundancy
- **Stream processors:** 2+ replicas with failover

### Stateful Components

Stateful components require special configuration:

- **Database:** PostgreSQL with replication (primary + standby)
- **Message bus:** NATS with clustering (3+ nodes)
- **Streaming:** Kafka with replication factor 3
- **Storage:** Persistent volumes with backups

### Multi-Site Deployment

For geographic redundancy:

- Deploy control plane in each site
- Configure cross-cluster stream replication
- DNS-based failover for UI/API access
- Agent routing to nearest control plane

---

## Upgrades

### Upgrade Process

**Kubernetes/Helm:**
```bash
# 1. Backup current state
kubectl get all -n opsz -o yaml > backup.yaml

# 2. Update Helm repository
helm repo update

# 3. Review changes
helm diff upgrade opsz opsz/opsz-stack --namespace opsz

# 4. Perform upgrade
helm upgrade opsz opsz/opsz-stack --namespace opsz

# 5. Verify
kubectl rollout status deployment -n opsz
```

**Docker Compose:**
```bash
# 1. Backup database
docker-compose exec db pg_dump > backup.sql

# 2. Pull new images
docker-compose pull

# 3. Restart with new images
docker-compose up -d

# 4. Verify
docker-compose ps
```

### Agent Updates

Agents update automatically:

- Package server distributes new agent versions
- Agents check for updates periodically
- Updates applied during maintenance window
- Rollback available if issues occur

---

## Backup & Recovery

### What to Backup

**Critical data:**
- PostgreSQL database (all schemas)
- Configuration files and secrets
- TLS certificates and keys
- Workflow templates and job history

**Optional:**
- Kafka topics (if long-term event history needed)
- Logs and metrics (if not exported to external system)

### Backup Procedures

**Database:**
```bash
# Automated daily backup
kubectl exec -n opsz postgres-0 -- \
  pg_dumpall -U postgres | gzip > backup-$(date +%Y%m%d).sql.gz

# Restore
gunzip < backup-20260215.sql.gz | \
  kubectl exec -i -n opsz postgres-0 -- psql -U postgres
```

**Configuration:**
```bash
# Backup Kubernetes resources
kubectl get all,configmap,secret -n opsz -o yaml > opsz-backup.yaml

# Restore
kubectl apply -f opsz-backup.yaml
```

---

## Troubleshooting

### Common Issues

**Issue: Pods not starting**
- Check resource availability (CPU, memory)
- Review pod logs: `kubectl logs -n opsz <pod-name>`
- Check persistent volume claims

**Issue: Agents not connecting**
- Verify network connectivity from agent to control plane
- Check firewall rules
- Review agent logs
- Verify certificate validity

**Issue: Slow performance**
- Check resource utilization
- Review database query performance
- Check Kafka lag
- Scale up replicas if needed

### Support

For deployment assistance:
- **Documentation:** [https://docs.opsz.io/deployment](https://docs.opsz.io/deployment)
- **Support:** [support@opsz.io](mailto:support@opsz.io)
- **Emergency:** Contact your customer success manager

---

## Next Steps

After deployment:

1. [**Getting Started Guide**](getting-started.md) — First steps with OpsZ
2. [**Security Configuration**](security.md) — Harden your deployment
3. [**Integration Setup**](integrations.md) — Connect existing tools
4. [**User Training**](https://docs.opsz.io/training) — Train your team
