# System Design Case Study Curriculum (Skill-Progressive)

## Phase 1 — Foundations: Statelessness, Throughput, Failure Awareness

**Goal:**  
Build intuition for scale, hot paths, caching, and traffic shaping *before* touching deep consistency.

### 1. URL Shortener
**Teaches**
- ID generation strategies
- Write vs read amplification
- Cache-first architectures
- Hot-key mitigation
- Stateless service design

### 2. Distributed Rate Limiter
**Teaches**
- Time-window algorithms
- Approximate vs exact enforcement
- Centralized vs local state tradeoffs
- Latency vs correctness
- Failure-tolerant control planes

### 3. Notification System
**Teaches**
- Async system design
- Queues and buffering
- Fan-out mechanics
- Backpressure fundamentals
- Idempotency as a primitive

### 4. Metrics & Monitoring
**Teaches**
- Observability primitives
- Metrics vs logs vs traces
- Cardinality economics
- Sampling theory
- Instrumentation-first thinking

---

## Phase 2 — Stateful Systems: Consistency, Storage, Evolution

**Goal:**  
Learn how state breaks systems and how to control the blast radius.

### 5. Distributed Key-Value Store (Mini Dynamo / Cassandra)
**Teaches**
- Partitioning strategies
- Replication models
- Quorum-based consistency
- Failure detection
- CAP tradeoffs in practice

### 6. Chat System (WhatsApp / Discord)
**Teaches**
- Real-time communication models
- Message ordering
- Delivery guarantees
- Presence systems
- Stateful connections at scale

### 7. Search Autocomplete (Typeahead)
**Teaches**
- Read-optimized data structures
- Index lifecycle management
- Cache invalidation realities
- Latency-sensitive query paths

### 8. Schema Migrations & Backfills
**Teaches**
- Data evolution patterns
- Backward/forward compatibility
- Zero-downtime changes
- Operational risk management

---

## Phase 3 — Complex Logic & Spatial Problems

**Goal:**  
Handle high-cardinality relationships and continuous data.

### 9. Social Feed (Twitter / Instagram)
**Teaches**
- Fan-out strategies
- Graph scaling challenges
- Ranking latency budgets
- Celebrity vs normal-user asymmetry

### 10. Proximity / Geo-Spatial Service (Uber / Yelp)
**Teaches**
- Spatial indexing
- Geo-sharding
- Write-heavy index tradeoffs
- Moving data problems

### 11. File Storage Service (Drive / Dropbox)
**Teaches**
- Large object management
- Metadata vs blob separation
- Consistency boundaries
- Conflict resolution strategies

---

## Phase 4 — Financial Correctness (High Integrity Systems)

**Goal:**  
Absolute correctness under partial failure.

### 12. Payment System (Capstone)
**Teaches**
- Distributed transactions
- Idempotency at scale
- Ledger-based accounting
- Auditability and reconciliation
- Failure as a first-class scenario

---

## Phase 5 — Extreme Throughput & Latency

**Goal:**  
Design under strict performance budgets.

### 13. Video Streaming Platform (Netflix / YouTube)
**Teaches**
- CDN economics
- Bandwidth optimization
- Adaptive delivery
- Offline vs online pipelines

### 14. Real-Time Ad Bidding (RTB)
**Teaches**
- Sub-100ms system design
- In-memory architectures
- Approximation techniques
- Graceful degradation

### 15. Stock Exchange / Matching Engine
**Teaches**
- Deterministic execution
- Latency predictability
- Lock-free design
- Memory and GC control

---

## Phase 6 — Big Data & Discovery

**Goal:**  
Operate on massive datasets with cost discipline.

### 16. Distributed Web Crawler
**Teaches**
- Work distribution
- Politeness and throttling
- Duplicate detection
- Priority scheduling

### 17. Log Aggregation & Search (ELK / Splunk)
**Teaches**
- High-volume ingestion
- Indexing strategies
- Retention economics
- Query-time vs ingest-time tradeoffs

---

## Phase 7 — High-Contention & Collaboration

**Goal:**  
Manage simultaneous writes safely and efficiently.

### 18. E-commerce Flash Sale
**Teaches**
- Load smoothing
- Inventory correctness
- Queue-based admission control
- Failure containment

### 19. Collaborative Document Editing
**Teaches**
- Real-time collaboration models
- Conflict-free convergence
- State synchronization
- Eventual consistency UX

### 20. Distributed Task Scheduler (Airflow / Temporal)
**Teaches**
- Stateful workflows
- Deterministic retries
- Checkpointing
- Leader election

---

## Phase 8 — Edge Cases & Operational Mastery (Optional)

**Goal:**  
Think like a staff+ engineer.

### 21. SLA & Real-Time Guarantees
**Teaches**
- Tail latency
- Error budgets
- Priority shedding

### 22. Edge & CDN Operations
**Teaches**
- Geo-failover
- Cache control semantics
- ISP-level constraints
- Origin protection strategies
