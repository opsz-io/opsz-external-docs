# OpsZ Architecture Overview

```
┌─────────────────────────────────────────────────────────────────────┐
│                         OpsZ Platform                               │
│                                                                     │
│  ┌───────────────────────────────────────────────────────────────┐ │
│  │                    Web Dashboard (UI)                         │ │
│  │              Secure Access with RBAC                          │ │
│  └───────────────────────┬───────────────────────────────────────┘ │
│                          │                                           │
│          ┌───────────────┼───────────────┐                          │
│          │               │               │                          │
│    ┌─────▼────┐    ┌────▼────┐    ┌────▼────┐    ┌────────┐       │
│    │ Identity │    │ Access  │    │Inventory│    │Workflow│       │
│    │   Mgmt   │    │ Control │    │   API   │    │ Engine │       │
│    └─────┬────┘    └────┬────┘    └────┬────┘    └────┬───┘       │
│          │              │              │              │            │
│          └──────────────┴──────────────┴──────────────┘            │
│                          │                                           │
│              ┌───────────▼────────────┐                             │
│              │   Database + Storage   │                             │
│              └────────────────────────┘                             │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│                   Communication Substrate                           │
│                                                                     │
│   ┌───────────┐  ┌────────────┐  ┌──────────┐  ┌──────────────┐   │
│   │  Message  │  │   Agent    │  │   Auth   │  │   Metrics    │   │
│   │    Bus    │  │ Provisioner│  │  Service │  │  Collector   │   │
│   └─────┬─────┘  └────────────┘  └──────────┘  └──────────────┘   │
│         │                                                           │
│   ┌─────▼──────────┐  ┌────────────┐  ┌─────────────────┐         │
│   │   Persistent   │  │   Stream   │  │    Workflow     │         │
│   │   Streaming    │──│ Replication│  │   Dispatcher    │         │
│   └─────┬──────────┘  └────────────┘  └─────────────────┘         │
│         │                                                           │
│   ┌─────▼────────────┐                                             │
│   │ Event Streaming  │                                             │
│   │   (Kafka Bus)    │                                             │
│   └─────┬────────────┘                                             │
└─────────┼───────────────────────────────────────────────────────────┘
          │
     ┌────▼─────────────┐
     │ Stream Processing│
     │  (Apache Flink)  │
     └────┬─────────────┘
          │
     ┌────▼─────────────┐
     │    Database      │
     │ (Inventory Data) │
     └──────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│                    Managed Infrastructure                           │
│                                                                     │
│   ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐             │
│   │ Agent   │  │ Agent   │  │ Agent   │  │ Agent   │  ...         │
│   │ (Linux) │  │(Windows)│  │(Container)│  │ (K8s)  │             │
│   └────┬────┘  └────┬────┘  └────┬────┘  └────┬────┘             │
│        │            │            │            │                    │
│        └────────────┴────────────┴────────────┘                    │
│                     │                                               │
│                     ▲                                               │
│         Outbound TLS Connection (no inbound ports)                  │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

## Key Components

### User Interface Layer
- **Web Dashboard:** Modern UI with role-based access control
- **APIs:** REST APIs for all platform capabilities
- **Authentication:** SSO integration with Okta, Azure AD, LDAP

### Platform Services
- **Identity Management:** User and authentication management
- **Access Control:** RBAC, roles, groups, permissions
- **Inventory API:** Infrastructure metadata and topology
- **Workflow Engine:** DAG-based orchestration with approval workflows

### Communication Substrate
- **Message Bus:** High-performance NATS-based messaging
- **Agent Provisioner:** Automatic agent enrollment
- **Auth Service:** Certificate-based authentication
- **Metrics Collector:** Platform and agent monitoring
- **Stream Replication:** Multi-cluster high availability
- **Workflow Dispatcher:** Job routing to target systems

### Data Pipeline
- **Event Streaming:** Apache Kafka for real-time events
- **Stream Processing:** Apache Flink for data transformation
- **Database:** PostgreSQL with pgvector for data storage

### Agent Layer
- **Lightweight Agents:** Single binary, minimal footprint
- **Zero-trust Security:** Outbound-only connections
- **Cross-platform:** Linux, Windows, containers, Kubernetes
- **Auto-provisioning:** Automatic enrollment and updates

## Security Model

- **Outbound-only agents:** No listening ports, no inbound connections
- **TLS encryption:** All communication encrypted
- **Certificate auth:** Strong cryptographic authentication
- **RBAC:** Fine-grained permissions at every layer
- **Audit logging:** Complete record of all actions

## Scalability

- **Horizontal scaling:** All stateless services scale horizontally
- **High availability:** Multi-replica deployments with failover
- **Multi-cluster:** Cross-cluster replication for geographic distribution
- **Performance:** Sub-second queries across tens of thousands of nodes

---

**Note:** This is a conceptual architecture diagram. For detailed technical specifications, see [Architecture Documentation](../architecture.md).
