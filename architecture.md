# OpsZ Architecture

**Last updated:** February 2026

This document provides a high-level overview of OpsZ's technical architecture for engineering teams evaluating the platform.

## System Overview

OpsZ is built on a modern, distributed architecture with five major components:

```
┌──────────────────────────────────────────────┐
│             Web Dashboard (UI)               │
│        Secure access with RBAC               │
└───────────────┬──────────────────────────────┘
                │
    ┌───────────┴────────────┐
    │                        │
┌───▼────┐  ┌───▼────┐  ┌───▼────┐  ┌────▼────┐
│Identity│  │ Access │  │Inventory│  │Workflow │
│  Mgmt  │  │Control │  │   API   │  │  Engine │
└────┬───┘  └───┬────┘  └────┬───┘  └────┬────┘
     │          │            │            │
     └──────────┴────────────┴────────────┘
                │
         ┌──────▼───────┐
         │   Database   │
         │  + Storage   │
         └──────────────┘
```

```
┌──────────────────────────────────────────────────────────┐
│              Communication Substrate                      │
│                                                           │
│  ┌────────┐  ┌──────────┐  ┌──────────┐  ┌───────────┐  │
│  │Message │  │  Agent   │  │   Auth   │  │  Metrics  │  │
│  │  Bus   │  │Provisioner│  │ Service  │  │  Collector│  │
│  └───┬────┘  └──────────┘  └──────────┘  └───────────┘  │
│      │                                                    │
│  ┌───▼───────────┐  ┌────────────┐  ┌────────────────┐  │
│  │   Persistent  │  │   Stream   │  │    Workflow    │  │
│  │   Streaming   │──│ Replication│  │   Dispatcher   │  │
│  └───┬───────────┘  └────────────┘  └────────────────┘  │
│      │                                                    │
│  ┌───▼─────────────┐                                     │
│  │ Event Streaming │                                     │
│  │   (Kafka Bus)   │                                     │
│  └───┬─────────────┘                                     │
└──────┼──────────────────────────────────────────────────┘
       │
   ┌───▼────────────────┐
   │  Stream Processing │
   │   (Apache Flink)   │
   └───┬────────────────┘
       │
   ┌───▼────────────────┐
   │    Database        │
   │  (Inventory Data)  │
   └────────────────────┘
```

---

## Architecture Layers

### 1. Communication Substrate

The foundation of OpsZ is an agent-based publish-subscribe messaging layer.

**Message Bus**
- NATS-based high-performance messaging
- Persistent streaming with configurable retention
- Support for millions of messages per day
- Multi-cluster replication for high availability

**Agent Design**
- Single binary, lightweight footprint
- One outbound connection, no listening ports
- No inbound SSH or firewall rules required
- Runs on Linux, Windows, containers, Kubernetes, bare metal

**Security**
- Certificate-based authentication
- TLS encryption for all communication
- Short-lived tokens (no static credentials)
- Zero-trust model: agents only make outbound connections

**Performance**
- Sub-second fleet-wide queries
- Scales to tens of thousands of nodes
- High-availability replication across clusters

### 2. Real-Time Asset Inventory

Continuous infrastructure discovery maintains a live inventory without stale databases.

**Discovery Pipeline**
```
Managed Systems → Agent Events → Message Bus → Stream Processing → Storage → API
```

**Data Collection**
- On-premises: Hardware facts (CPU, memory, network, storage, system info)
- Cloud: AWS, GCP, Azure instance metadata
- Custom: Configurable metadata collection via YAML

**Data Processing**
- Real-time streaming architecture
- Exactly-once processing semantics
- Sub-second latency from event to API
- Automatic normalization and validation

**API Access**
- REST API for inventory queries
- Hierarchical grouping by metadata attributes
- Flexible querying and filtering
- Regular refresh with on-demand updates

### 3. Governance & Access Management

Identity, access control, and audit capabilities are built into every platform operation.

**Identity Management**
- OAuth 2.0 / SAML (Okta, Azure AD, other providers)
- LDAP / Active Directory integration
- Multi-factor authentication support
- Bulk user operations for enterprise scale

**Access Control**
- Fine-grained role-based access control (RBAC)
- Action-level permissions across all resources
- Role and group hierarchies with inheritance
- Temporary access grants with automatic expiration

**Audit & Compliance**
- Comprehensive logging of all user and system activity
- Every API call, approval decision, and workflow execution recorded
- Searchable audit trail for compliance reporting
- Retention policies for regulatory requirements

**Approval Workflows**
- Multi-step approval chains
- Configurable approver groups
- Escalation rules
- Full approval history

### 4. Workflow Orchestration

DAG-based workflow engine for complex automation scenarios.

**Workflow Design**
- Directed acyclic graphs (DAGs), not linear scripts
- Parallel execution paths
- Conditional branching based on step outcomes
- Re-enter failed workflows at point of failure

**Execution**
- Cross-platform targeting (single workflow, multiple environments)
- Real-time status and log collection
- Dependency tracking and validation
- Automatic retry and error handling

**Template Management**
- Searchable template library
- Version-controlled YAML definitions
- AI-powered semantic search
- Template sharing and reuse

**Scheduling**
- Cron-based scheduling for recurring jobs
- Event-driven triggers (GitHub webhooks, API calls)
- Parent-child job hierarchies
- Multi-target execution

### 5. AI & Analytics at the Edge

Intelligence that runs where the work happens.

**Semantic Search**
- AI-powered template discovery
- Vector embeddings for intelligent matching
- Fast, relevant results (sub-second response)
- No manual tagging required

**Template Intelligence**
- Automatic embedding generation
- Incremental updates as templates change
- Multi-stage search pipeline (keyword + semantic + step-level)
- Score fusion and ranking

**Edge AI (Planned)**
- Local AI model deployment on managed nodes
- Autonomous log analysis and troubleshooting
- Federated intelligence model
- Local decision-making with central oversight

---

## Data Pipeline Architecture

### Event Streaming Pipeline

```
Choria Agents → NATS → Stream Replicator → Kafka
                                              ↓
                                         Flink Jobs
                                              ↓
EC2 Metadata → Airflow → Kafka ─────────→ PostgreSQL
                                              ↓
                                      Inventory API → UI
```

**Key Characteristics**
- Exactly-once processing semantics with checkpointing
- Multi-layer payload decompression and normalization
- Connection pooling with retry logic and exponential backoff
- Asynchronous I/O for high throughput
- Automatic topic creation and partition management

### Batch Processing

- Apache Airflow for scheduled batch jobs
- Cloud metadata collection (AWS EC2, planned GCP/Azure)
- DAG-based job orchestration
- Integration with streaming pipeline via Kafka

---

## Deployment Architecture

### Container-Based Deployment

OpsZ deploys as a set of containerized services on Kubernetes:

**Namespace Isolation**
- Separated namespaces for different functional areas
- Communication substrate (messaging, agents)
- Platform services (identity, access, inventory, workflows)
- Frontend (UI and API gateway)
- Data pipeline (Kafka, Flink, Airflow)
- Persistence (database, storage)

**Scalability**
- Horizontal scaling for stateless services
- StatefulSets for stateful components (messaging, database)
- Configurable replicas based on load
- Resource requests and limits for predictable performance

**High Availability**
- Multi-replica deployments for critical services
- Stream replication across clusters
- Database replication and backups
- Health checks and automatic restarts

### Deployment Options

**Single-VM Development**
- Runs on a single virtual machine with Kubernetes
- Suitable for development and small-scale testing
- Fast deployment (approximately 30 minutes)

**Production Cluster**
- Multi-node Kubernetes cluster
- High availability and fault tolerance
- Horizontal scalability
- Load balancing and service mesh

**Customer On-Premises**
- Self-contained installation packages
- Can run in air-gapped environments
- Automated setup with minimal configuration
- Full control over data and infrastructure

**Hybrid Deployment**
- Agents in customer environment
- Control plane in cloud or on-premises
- Cross-cluster stream replication
- Flexible deployment models

---

## Security Architecture

### Zero-Trust Model

- Agents only make outbound connections
- No inbound ports or firewall rules required
- Certificate-based authentication
- Short-lived tokens with automatic rotation

### Encryption

- TLS encryption for all network communication
- Encrypted tokens for authentication
- At-rest encryption for sensitive data
- Secure key management

### Access Control

- Fine-grained RBAC at every layer
- Action-level permissions
- Multi-step approval workflows
- Comprehensive audit logging

### Compliance

- Audit trails for all operations
- Data retention policies
- Compliance-ready logging
- Customer-controlled data (no vendor lock-in)

---

## API Architecture

### REST API Design

Every platform capability is available via REST API:

- **Identity API** — User and authentication management
- **Access API** — Roles, groups, permissions
- **Inventory API** — Infrastructure metadata and topology
- **Workflow API** — Template and job management

**Key Characteristics**
- Consistent REST conventions
- JSON request/response format
- Token-based authentication
- Rate limiting and throttling
- Comprehensive API documentation

**Integration**
- Webhooks for event-driven automation
- External system integration (ServiceNow, PagerDuty, etc.)
- Headless operation (entire platform can be driven without UI)
- UI is a consumer of the same APIs available to customers

---

## Technology Stack

**Core Infrastructure**
- Kubernetes for container orchestration
- PostgreSQL with pgvector for data storage
- NATS JetStream for messaging
- Apache Kafka for event streaming
- Apache Flink for stream processing
- Apache Airflow for batch orchestration

**Application Layer**
- FastAPI (Python) for microservices
- Next.js for frontend
- Go for high-performance agents and messaging components

**Security & Observability**
- OAuth 2.0 / SAML for SSO
- Prometheus for metrics
- TLS certificate management
- Comprehensive logging

---

## Learn More

- [Feature Catalog](features.md)
- [Deployment Guide](deployment.md)
- [Security Model](security.md)
- [Integration Capabilities](integrations.md)
