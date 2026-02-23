# OpsZ Data Flow

This diagram shows how data flows through the OpsZ platform in two primary modes: **Discovery** (infrastructure awareness) and **Execution** (workflow orchestration).

## Discovery Flow (Real-time Infrastructure Awareness)

```
┌────────────────────────────────────────────────────────────────────────┐
│                          Discovery Flow                                │
│                    (Infrastructure → Platform)                         │
└────────────────────────────────────────────────────────────────────────┘

  ┌──────────────┐
  │  Managed     │
  │  Systems     │  1. Agent collects hardware facts
  │  (Agents)    │     CPU, memory, network, OS, etc.
  └──────┬───────┘
         │
         │ 2. Agent publishes events
         │    (outbound TLS connection)
         ▼
  ┌──────────────┐
  │   Message    │  3. Events stored in persistent streams
  │     Bus      │     (24-hour retention)
  │  (Streaming) │
  └──────┬───────┘
         │
         │ 4. Events replicated across clusters
         │    (high availability)
         ▼
  ┌──────────────┐
  │    Kafka     │  5. Events forwarded to Kafka
  │   Topics     │     (for processing and analytics)
  └──────┬───────┘
         │
         │ 6. Stream processing
         │    (decompress, normalize, validate)
         ▼
  ┌──────────────┐
  │    Flink     │  7. Real-time transformations
  │    Jobs      │     - Parse nested payloads
  │ (Processing) │     - Extract facts
  └──────┬───────┘     - Upsert to database
         │
         │ 8. Exactly-once write
         │    (checkpointing for reliability)
         ▼
  ┌──────────────┐
  │  PostgreSQL  │  9. Inventory stored
  │  (Inventory) │     Queryable via REST API
  └──────┬───────┘
         │
         │ 10. API queries
         ▼
  ┌──────────────┐
  │  REST API    │  11. UI and integrations query inventory
  └──────┬───────┘
         │
         ▼
  ┌──────────────┐
  │  Dashboard   │  12. Real-time infrastructure view
  │     (UI)     │
  └──────────────┘
```

**Key Characteristics:**
- **Sub-second latency:** Events processed in real-time
- **Exactly-once semantics:** No duplicate or lost data
- **High throughput:** Millions of events per day
- **Persistent storage:** Queryable via API at any time

---

## Execution Flow (Workflow Orchestration)

```
┌────────────────────────────────────────────────────────────────────────┐
│                          Execution Flow                                │
│                      (Platform → Infrastructure)                       │
└────────────────────────────────────────────────────────────────────────┘

  ┌──────────────┐
  │     User     │  1. User creates/triggers workflow
  │      or      │     (UI, API, webhook, schedule)
  │  Integration │
  └──────┬───────┘
         │
         │ 2. Authentication and authorization
         │    (check permissions, apply RBAC)
         ▼
  ┌──────────────┐
  │  Access      │  3. Approval workflow (if required)
  │  Control     │     - Route to approvers
  └──────┬───────┘     - Wait for decision
         │             - Audit decision
         │
         │ 4. Create job request
         ▼
  ┌──────────────┐
  │  Workflow    │  5. Parse DAG and dependencies
  │   Engine     │     - Identify execution order
  └──────┬───────┘     - Prepare job actions
         │
         │ 6. Dispatch to target systems
         │    (publish to message bus)
         ▼
  ┌──────────────┐
  │   Message    │  7. Route job to target agents
  │     Bus      │     (via persistent streams)
  │  (Streaming) │
  └──────┬───────┘
         │
         │ 8. Deliver job to agent
         │    (guaranteed delivery)
         ▼
  ┌──────────────┐
  │  Agents on   │  9. Agent executes workflow steps
  │   Managed    │     - Run commands
  │   Systems    │     - Collect output
  └──────┬───────┘     - Report status
         │
         │ 10. Publish status and logs
         │     (back to message bus)
         ▼
  ┌──────────────┐
  │   Message    │  11. Status events streamed
  │     Bus      │      back to control plane
  │  (Streaming) │
  └──────┬───────┘
         │
         │ 12. Workflow engine consumes status
         ▼
  ┌──────────────┐
  │  Workflow    │  13. Update job state
  │   Engine     │      - Track progress
  └──────┬───────┘      - Handle errors
         │              - Execute next steps
         │
         │ 14. Store results
         ▼
  ┌──────────────┐
  │  PostgreSQL  │  15. Job logs and results stored
  │  (Results)   │      Queryable via REST API
  └──────┬───────┘
         │
         │ 16. API queries
         ▼
  ┌──────────────┐
  │  REST API    │  17. Real-time status available
  └──────┬───────┘
         │
         ▼
  ┌──────────────┐
  │  Dashboard   │  18. User sees real-time progress
  │     (UI)     │      - Status updates
  └──────────────┘      - Log streaming
                        - Success/failure
```

**Key Characteristics:**
- **Approval gates:** Optional multi-step approvals
- **DAG execution:** Parallel and conditional steps
- **Real-time status:** Live updates as job executes
- **Audit trail:** Complete record of execution
- **Guaranteed delivery:** Reliable message routing

---

## Combined Flow (Full Lifecycle)

```
┌────────────────────────────────────────────────────────────────────────┐
│                   Detect → Decide → Act → Learn                        │
└────────────────────────────────────────────────────────────────────────┘

    ┌─────────┐
    │ DETECT  │  Continuous infrastructure awareness
    └────┬────┘  - Agent discovery
         │       - Event streaming
         │       - Real-time inventory
         ▼
    ┌─────────┐
    │ DECIDE  │  Intelligent decision-making
    └────┬────┘  - AI-powered workflow search
         │       - Policy evaluation
         │       - Approval workflows
         ▼
    ┌─────────┐
    │   ACT   │  Automated execution
    └────┬────┘  - Workflow orchestration
         │       - Multi-target execution
         │       - Error handling
         ▼
    ┌─────────┐
    │  LEARN  │  Continuous improvement
    └─────────┘  - Execution analytics
                 - Performance metrics
                 - AI model training
```

**Full Lifecycle Benefits:**
- **Continuous awareness:** Always know your infrastructure state
- **Informed decisions:** Context-driven automation
- **Governed execution:** Approval gates and audit trails
- **Continuous improvement:** Learn from each execution

---

## Data Flow Characteristics

### Security
- **Encrypted in transit:** All network communication uses TLS
- **Encrypted at rest:** Sensitive data encrypted in database
- **No static credentials:** Token-based with short expiration
- **Audit everything:** Complete data lineage

### Performance
- **Sub-second latency:** Real-time data processing
- **High throughput:** Millions of events per day
- **Horizontal scaling:** Add capacity as needed
- **Efficient storage:** Compressed, indexed, partitioned

### Reliability
- **Exactly-once semantics:** No duplicate or lost data
- **Persistent streams:** Replay capability for recovery
- **Checkpointing:** Automatic recovery from failures
- **Multi-cluster replication:** Geographic redundancy

### Observability
- **Real-time metrics:** Prometheus-compatible
- **Detailed logging:** Debug and audit
- **Tracing:** End-to-end request tracing
- **Dashboards:** Monitor data flow health

---

**Note:** This is a conceptual data flow diagram. For detailed technical specifications, see [Architecture Documentation](../architecture.md).
