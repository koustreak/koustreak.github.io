# Koushik Dutta

**Senior Data Engineer @ Deloitte Products and Engineering**  
**Distributed Systems Enthusiast**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/koushik-dutta-9797a8209/)
[![Email](https://img.shields.io/badge/Yahoo!-6001D2?style=for-the-badge&logo=Yahoo!&logoColor=white)](mailto:dot.py@yahoo.com)

---

## About Me

I'm a Senior Data Engineer specializing in **distributed systems** and **large-scale data processing**. My work focuses on building fault-tolerant, high-performance data platforms using cutting-edge technologies and novel algorithmic approaches.

**Core Expertise:**
- **Data Engineering:** Kafka, Spark, Airflow, Druid, Apache Iceberg, Apache Superset
- **Distributed Systems:** Raft consensus, CRDTs, distributed transactions, fault tolerance
- **Cloud & Orchestration:** AWS (S3, EMR, EKS, Kinesis), Kubernetes, Docker
- **Languages:** Scala, Go, Python, SQL
- **Specializations:** Real-time stream processing, consensus algorithms, query optimization, data lakehouse

---

## Advanced Technical Projects

### Tier 1: Distributed Systems & Consensus Algorithms

#### 1. Adaptive Pipeline Preemption Framework with Raft Consensus
Custom priority scheduler implementing preemptive multitasking for distributed stream processing.

**Stack:** Modified Kafka + Spark Structured Streaming + Kubernetes DRA

**Technical Implementation:**
- Raft consensus for distributed priority queue coordination across executors
- Lock-free concurrent priority queue using skip lists (O(log n) insert/delete)
- Preemption mechanism: checkpoint → serialize to distributed snapshot → reschedule
- Backpressure handling via token bucket algorithm with dynamic rate adjustment
- Speculative checkpointing based on historical task execution patterns

**Metrics:** 2-5M events/day, P99 latency 1.8s, 85% reduction from baseline

---

#### 2. Raft Consensus Implementation with Speculative Execution
From-scratch Raft implementation in Go with novel optimizations.

**Stack:** Pure Go, etcd (benchmarking), Kafka (log transport)

**Technical Implementation:**
- All Raft phases: leader election, log replication, membership changes, log compaction
- Speculative execution: followers execute commands before commit (rollback on conflict)
- Batched log replication with pipelining (60% RTT reduction)
- Adaptive heartbeat intervals using EWMA of network latency
- Memory-mapped files with fsync batching for persistent state
- Lease-based linearizable reads without log append

**Metrics:** <5s failover, 99.995% availability, 10K ops/sec throughput

**Novel:** Hybrid Raft+Multi-Paxos for read-heavy workloads

---

#### 3. Multi-Region CRDT-Based Analytics with Causal Consistency
Geo-distributed analytics using operation-based CRDTs with vector clocks.

**Stack:** Kafka (cross-region) + Custom CRDT library (Scala) + DynamoDB Global Tables

**Technical Implementation:**
- Implemented G-Counter, PN-Counter, OR-Set, LWW-Register CRDTs
- Vector clock with version vectors for causal ordering
- Lamport timestamps for total ordering on concurrent updates
- Anti-entropy protocol using Merkle trees for state reconciliation
- Semilattice merge functions (commutative, associative, idempotent)
- Delta-state CRDTs to minimize network bandwidth

**Metrics:** 100M events/day, sub-200ms cross-region sync, 5 AWS regions

**Novel:** Hybrid vector clock compression using bloom filters

---

### Tier 2: Lakehouse Architecture & Storage Engines

#### 4. Zero-Copy Lakehouse with Copy-on-Write Semantics
Pointer-based views using Apache Iceberg's snapshot isolation.

**Stack:** Modified Iceberg core + Spark + Custom metadata layer (Go) + Druid

**Technical Implementation:**
- Incremental materialized views using Iceberg manifest files as pointers
- Copy-on-write B+ tree for metadata indexing (O(log n) lookup)
- Lazy compaction: defer until read amplification > threshold
- Bloom filters on data files for partition pruning (99% FP reduction)
- Z-ordering (space-filling curves) for multi-dimensional clustering
- Snapshot expiration using reference counting and mark-sweep GC

**Metrics:** 2.5PB data, 99.7% storage efficiency, sub-second queries

**Novel:** Adaptive Z-order column selection using query log analysis and mutual information

---

#### 5. Bi-Temporal Iceberg with Interval Tree Indexing
Extended Iceberg with transaction-time and valid-time dimensions.

**Stack:** Modified Iceberg core (Scala) + Spark + Custom temporal query engine

**Technical Implementation:**
- Interval tree (augmented AVL tree) for temporal range queries
- Allen's interval algebra for temporal join predicates
- Snapshot isolation extended to bi-temporal dimensions (MVCC with two time axes)
- Temporal predicate pushdown in Spark Catalyst optimizer
- Compressed bitmap indexes for temporal validity ranges
- Temporal coalescing to merge adjacent valid-time intervals

**Metrics:** 40TB historical data, <100ms temporal query overhead

**Novel:** Fractional cascading for O(log n + k) temporal joins

---

### Tier 3: Stream Processing & Query Optimization

#### 6. Cost-Based Query Optimizer with Adaptive Statistics
Self-tuning Spark SQL optimizer using runtime statistics feedback.

**Stack:** Spark Catalyst + Custom cost model + Airflow

**Technical Implementation:**
- Dynamic programming for join order enumeration (O(3^n) → O(n^2) with pruning)
- Cardinality estimation using histograms with equi-depth buckets
- Adaptive query re-optimization based on runtime statistics
- Join algorithm selection: hash join, sort-merge join, broadcast join
- Partition count optimization using data skew detection
- Cost model: CPU cost + I/O cost + network cost

**Metrics:** 40% speedup vs Spark CBO, handles 500K+ queries

**Novel:** Adaptive histogram refinement based on query feedback

---

#### 7. Exactly-Once Semantics with Idempotent Sinks
Distributed exactly-once without 2PC using deterministic request IDs.

**Stack:** Kafka + Flink + Custom sink framework (Scala)

**Technical Implementation:**
- Deterministic request ID: `hash(topic, partition, offset, record_key)`
- Bloom filter-based deduplication (counting bloom filter for TTL)
- Flink checkpoint alignment using barrier injection
- Asynchronous checkpoint with incremental RocksDB snapshots
- Sink idempotency via conditional writes (compare-and-swap)
- Watermark propagation for event-time processing

**Metrics:** 200M events/day, 99.999% dedup accuracy, <2ms overhead

**Novel:** Probabilistic deduplication using HyperLogLog

---

#### 8. Adaptive Backpressure with Token Bucket Rate Limiting
Dynamic rate control for stream processing with feedback loops.

**Stack:** Kafka + Spark Streaming + Kubernetes HPA + Prometheus

**Technical Implementation:**
- Token bucket algorithm with dynamic rate adjustment
- Exponential weighted moving average (EWMA) for smoothing metrics
- Feedback control loop: monitor latency → adjust rate → observe effect
- Multi-level backpressure: source throttling + operator buffering + sink flow control
- Sliding window statistics for trend detection
- Additive increase, multiplicative decrease (AIMD) for stability

**Metrics:** 60% reduction in over-provisioning, <1s P99 latency maintained

**Novel:** Hierarchical backpressure propagation across operator chains

---

### Tier 4: Data Mesh & Observability

#### 9. Self-Service Data Mesh with Automated Lineage Tracking
Decentralized data platform with automatic dependency graph construction.

**Stack:** Apache Atlas + Kafka + Airflow + Custom lineage parser (Python)

**Technical Implementation:**
- SQL query parsing using ANTLR4 grammar for lineage extraction
- Abstract syntax tree (AST) traversal for table/column dependencies
- Directed acyclic graph (DAG) construction for data lineage
- Graph algorithms: transitive closure for impact analysis, PageRank for importance
- Schema evolution tracking using Avro schema registry
- Metadata versioning with Merkle DAG (content-addressable storage)

**Metrics:** 200+ data products, 95% lineage coverage, 15 domains

**Novel:** Column-level lineage tracking through expression analysis

---

#### 10. Real-Time Data Quality Firewall with Adaptive Thresholds
Streaming validation with circuit breaker pattern.

**Stack:** Kafka Streams + Custom rule engine (Scala) + Great Expectations

**Technical Implementation:**
- Rule engine using Rete algorithm for pattern matching
- Statistical process control (SPC): Shewhart charts for anomaly detection
- Adaptive thresholds using exponential weighted moving average (EWMA)
- Circuit breaker states: closed, open, half-open (based on error rate)
- Sliding window aggregations using tumbling/hopping windows
- Count-min sketch for frequency estimation

**Metrics:** <5ms validation latency, 99.9% good data pass-through

**Novel:** Statistical threshold adaptation using control charts

---

### Tier 5: Cost Optimization & Caching

#### 11. Intelligent Data Tiering with Access Pattern Analysis
Automated S3 tiering using statistical access pattern analysis.

**Stack:** S3 (Standard/IA/Glacier) + Athena + Custom tiering engine (Python)

**Technical Implementation:**
- Time-series analysis: recency, frequency, temporal patterns
- Markov chain model for state transitions (hot → warm → cold)
- Cost-benefit analysis: storage cost vs retrieval cost vs access latency
- Proactive tiering: move data based on predicted access windows
- Exponential smoothing for trend forecasting
- Sliding window statistics for pattern detection

**Metrics:** 5PB data, 65% cost reduction, 99.5% prediction accuracy

**Novel:** Multi-objective optimization balancing cost, latency, and retrieval frequency

---

#### 12. Query Result Caching with Semantic Similarity
Intelligent cache using query similarity instead of exact matching.

**Stack:** Presto + Redis + Custom cache layer

**Technical Implementation:**
- SQL query normalization: canonicalization, constant folding, predicate reordering
- Query fingerprinting using hash-based similarity (MinHash)
- Approximate matching using Jaccard similarity on query tokens
- Cache eviction: LRU with frequency-based promotion (LFU-LRU hybrid)
- Query rewriting: substitute cached subqueries while preserving semantics
- Partial result caching for common subexpressions

**Metrics:** 70% cache hit rate (vs 20% exact-match)

**Novel:** Query plan-based caching with partial result reuse

---

## Technology Stack

### Data Processing & Streaming
![Kafka](https://img.shields.io/badge/Apache%20Kafka-231F20?style=flat&logo=apache-kafka&logoColor=white)
![Spark](https://img.shields.io/badge/Apache%20Spark-E25A1C?style=flat&logo=apache-spark&logoColor=white)
![Flink](https://img.shields.io/badge/Apache%20Flink-E6526F?style=flat&logo=apache-flink&logoColor=white)
![Airflow](https://img.shields.io/badge/Apache%20Airflow-017CEE?style=flat&logo=apache-airflow&logoColor=white)

### Data Storage & Lakehouse
![Iceberg](https://img.shields.io/badge/Apache%20Iceberg-3C8CE7?style=flat&logo=apache&logoColor=white)
![Druid](https://img.shields.io/badge/Apache%20Druid-29F1FB?style=flat&logo=apache-druid&logoColor=black)
![Superset](https://img.shields.io/badge/Apache%20Superset-20A6C9?style=flat&logo=apache&logoColor=white)

### Cloud & Infrastructure
![AWS](https://img.shields.io/badge/AWS-232F3E?style=flat&logo=amazon-aws&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=flat&logo=kubernetes&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)

### Programming Languages
![Scala](https://img.shields.io/badge/Scala-DC322F?style=flat&logo=scala&logoColor=white)
![Go](https://img.shields.io/badge/Go-00ADD8?style=flat&logo=go&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=flat&logo=postgresql&logoColor=white)

### Distributed Systems
- **Consensus Algorithms:** Raft, Multi-Paxos
- **Consistency Models:** CRDTs, Vector Clocks, Lamport Timestamps
- **Storage Engines:** LSM-trees, B+ trees, Skip Lists
- **Fault Tolerance:** Replication, Checkpointing, Circuit Breakers

---

## Distributed Systems Expertise

### Consensus & Coordination
- **Raft Consensus:** Leader election, log replication, membership changes, log compaction
- **Paxos Variants:** Multi-Paxos, Fast Paxos for high-throughput scenarios
- **Coordination Services:** etcd, ZooKeeper for distributed configuration and leader election

### Consistency & Replication
- **CRDTs:** G-Counter, PN-Counter, OR-Set, LWW-Register for eventual consistency
- **Vector Clocks:** Causal ordering in distributed systems
- **Quorum-based Replication:** Read/write quorums, anti-entropy protocols

### Data Structures for Distributed Systems
- **Probabilistic:** Bloom filters, HyperLogLog, Count-Min Sketch
- **Concurrent:** Lock-free skip lists, concurrent hash maps
- **Spatial:** R-trees, Quadtrees, K-d trees

### Distributed Transactions
- **MVCC:** Multi-version concurrency control for snapshot isolation
- **2PC/3PC:** Two-phase and three-phase commit protocols
- **Saga Pattern:** Long-running transactions with compensating actions

---

## GitHub Stats

![GitHub Stats](https://github-readme-stats.vercel.app/api?username=koustreak&show_icons=true&theme=dark&hide_border=true&count_private=true&include_all_commits=true)

![Top Languages](https://github-readme-stats.vercel.app/api/top-langs/?username=koustreak&layout=compact&theme=dark&hide_border=true&langs_count=8)

---

## Contact

📧 **Email:** dot.py@yahoo.com  
💼 **LinkedIn:** [linkedin.com/in/koushik-dutta-9797a8209](https://www.linkedin.com/in/koushik-dutta-9797a8209/)  
🐙 **GitHub:** [@koustreak](https://github.com/koustreak)

---

*Building fault-tolerant, high-performance data systems at scale.*
