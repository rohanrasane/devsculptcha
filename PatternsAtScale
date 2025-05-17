# Architectural Patterns for Scale

A consolidated overview of key patterns—**CQRS**, **Fan‑out + Worker Pool**, and **Strangler Fig**—including descriptions and Mermaid diagrams.

---

## 1. CQRS (Command Query Responsibility Segregation)

**How It Works**

* Commands (writes) and Queries (reads) use separate models and often separate data stores.
* After a command commits, it emits an event (e.g., via a message broker) that updates the read model asynchronously.

**Why It Scales**

* **Independent Scaling**: Scale your read tier (e.g., multiple replicas, caches) without touching the write tier.
* **Optimized Data Models**: The read model can be highly denormalized or even pushed into a specialized store (search index, in‑memory cache) for ultra‑fast queries.
* **Clear Separation of Concerns**: Simplifies the code paths—command handlers focus on business logic, query handlers on retrieval.

**Caveats**

* **Eventual Consistency**: Reads may be stale immediately after writes; UX must handle this gracefully.
* **Operational Complexity**: You need to monitor two data pipelines and ensure event delivery.

**Diagram**

```mermaid
flowchart TD
  Client -->|Command| WriteService
  WriteService -->|Persist & Emit Event| EventBus
  EventBus -->|Update| ReadModel
  Client -->|Query| ReadService
  ReadService -->|Fetch from| ReadModel
```

---

## 2. Fan‑out + Worker Pool

**How It Works**

* An incoming task (e.g., a bulk job, email blast, report generation) is **fanned out** into many discrete work items.
* A **worker pool**—a group of horizontally scalable worker processes—consumes from a queue, processes items in parallel, and writes results back.

**Why It Scales**

* **Elastic Throughput**: You can increase or decrease the number of workers based on queue depth or latency SLOs.
* **Failure Isolation**: If one worker crashes on a bad item, the rest of the pipeline continues; dead‑letter queues capture failures for later replay.
* **Backpressure Control**: Queues buffer spikes, preventing downstream overload.

**Caveats**

* **Ordering Concerns**: If order matters, you need partitioned queues or sequence tracking.
* **Idempotency**: Workers must safely reprocess items without side effects.

**Diagram**

```mermaid
flowchart TD
  JobProducer -->|Submit Job| JobQueue
  subgraph Workers
    direction TB
    Worker1 -->|Process| ResultStore
    Worker2 -->|Process| ResultStore
    Worker3 -->|Process| ResultStore
  end
  JobQueue --> Workers
  Client -->|Fetch Results| ResultStore
```

---

## 3. Strangler Fig

**How It Works**

* You incrementally **replace** parts of a legacy system by **wrapping** it with a new service or module.
* Traffic for a given feature “strangles” the old code path, redirecting progressively more calls to the new implementation until the legacy code can be decommissioned.

**Why It Scales**

* **Low Risk Migration**: You avoid big‑bang rewrites; each piece can be tested and rolled back independently.
* **Continuous Delivery**: New functionality lives alongside legacy, allowing faster iteration and gradual sunsetting of old code.
* **Evolving Architecture**: Enables you to apply modern patterns (e.g., microservices, event‑driven) one component at a time.

**Caveats**

* **Routing Complexity**: Requires a façade or proxy layer to split traffic.
* **Dual-Maintenance**: For a period, you must maintain both old and new codebases.

**Diagram**

```mermaid
flowchart LR
  Client -->|Request Feature A| Proxy
  Proxy -->|Old Code Path| LegacySystem
  Proxy -->|New Code Path| NewService
  %% Over time, traffic shifts:
  click Proxy "Traffic shift: 100% ➔ 0% legacy"
```
