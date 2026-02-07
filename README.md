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
- **Specializations:** Real-time stream processing, consensus algorithms, vector databases, query optimization

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
- Speculative checkpointing using LSTM to predict preemption 500ms ahead

**Metrics:** 50M events/day, P99 latency 1.8s, 85% reduction from baseline

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

**Novel:** Adaptive Z-order column selection using mutual information

---

#### 5. AI-Native Multimodal Lakehouse with Vector Embeddings
Unified storage for structured data + high-dimensional vectors.

**Stack:** Iceberg + Milvus (HNSW) + Spark + ONNX Runtime

**Technical Implementation:**
- HNSW graph for approximate nearest neighbor search
- Product quantization (32x compression, <5% recall loss)
- SIMD-accelerated distance calculations (AVX-512 instructions)
- Cross-modal embedding alignment using contrastive learning (CLIP-style)
- Hybrid index: HNSW for vectors + inverted index for metadata
- Incremental index updates using LSM-tree structure

**Metrics:** 500M vectors, 10ms P95 search latency, 95% recall@10

**Novel:** Learned index structures using neural networks

---

#### 6. Bi-Temporal Iceberg with Interval Tree Indexing
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

#### 7. Reinforcement Learning Query Optimizer with Deep Q-Networks
Self-tuning Spark SQL optimizer using DQN with experience replay.

**Stack:** Spark Catalyst + PyTorch + Custom cost model + Airflow

**Technical Implementation:**
- State space: query plan DAG, table statistics, cluster metrics (500+ features)
- Action space: join order, join algorithm, partition count, shuffle strategy
- Reward function: -1 × (execution_time + cost_penalty)
- Deep Q-Network with dueling architecture (separate value/advantage streams)
- Prioritized experience replay using TD-error for sample importance
- Cardinality estimation using neural density estimators

**Metrics:** 45% speedup vs Spark CBO, trained on 500K queries

**Novel:** Multi-task learning - jointly optimize latency, cost, memory

---

#### 8. Exactly-Once Semantics with Idempotent Sinks
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

#### 9. Predictive Auto-Scaling with LSTM Time-Series Forecasting
ML-driven autoscaling predicting load 15 minutes ahead.

**Stack:** Kafka metrics + LSTM (PyTorch) + Kubernetes HPA + Prometheus

**Technical Implementation:**
- Multi-variate LSTM with attention (input: lag, CPU, memory, event rate)
- Sliding window forecasting with 5-minute granularity
- Multi-objective optimization: minimize cost + latency violations + scaling oscillations
- Pareto frontier approximation using NSGA-II genetic algorithm
- Kalman filter for smoothing noisy metrics
- Exponential smoothing for trend detection

**Metrics:** 60% reduction in over-provisioning, <1s P99 latency maintained

**Novel:** Transfer learning - pre-train on similar workloads

---

### Tier 4: Vector Databases & Approximate Nearest Neighbor Search

#### 10. Hybrid OLAP-Vector Database with Integrated HNSW
Unified columnar storage with vector similarity search.

**Stack:** Modified ClickHouse (C++) + Custom HNSW index + Kafka

**Technical Implementation:**
- HNSW graph embedded in ClickHouse MergeTree engine
- Hierarchical graph: multiple layers with exponentially decreasing density
- Greedy search with beam width tuning (recall vs latency trade-off)
- Scalar quantization (SQ8) for memory efficiency
- Hybrid query execution: predicate filtering → vector search on subset
- Incremental graph construction during merge operations

**Metrics:** 1B vectors, <50ms hybrid queries, 10M inserts/day

**Novel:** Learned proximity graphs using graph neural networks

---

#### 11. Real-Time Vector Embedding with Model Drift Detection
Streaming embedding generation with statistical drift monitoring.

**Stack:** Kafka + Flink + ONNX Runtime + Custom drift detector (Python)

**Technical Implementation:**
- ONNX model inference in Flink operators (avoid serialization overhead)
- Drift detection using Kolmogorov-Smirnov test on embedding distributions
- Population Stability Index (PSI) for feature drift monitoring
- ADWIN algorithm for concept drift detection
- Model versioning with A/B testing (Thompson sampling)
- Embedding cache with LRU eviction policy

**Metrics:** 50M docs/day, <100ms embedding latency, detected 3 drift events

**Novel:** Adversarial drift detection using discriminator network

---

#### 12. Distributed Vector Index with Learned Partitioning
Scalable ANN search using ML-learned data partitioning.

**Stack:** Custom HNSW (Go) + K-means++ clustering + etcd + gRPC

**Technical Implementation:**
- Learned partitioning: k-means++ on vector space (minimize intra-cluster distance)
- Voronoi diagram partitioning for query routing
- Graph sharding: minimize edge cuts using METIS algorithm
- Distributed coordination using etcd for cluster membership
- Query routing: compute distance to centroids, route to top-k partitions
- Replication factor 3 with consistent hashing

**Metrics:** 5B vectors, 100 nodes, 20ms P95 latency, 80% single-partition queries

**Novel:** Neural network-based partition routing

---

### Tier 5: Data Mesh & Observability

#### 13. Self-Service Data Mesh with Automated Lineage Tracking
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

**Novel:** ML-based lineage inference for black-box transformations

---

#### 14. Real-Time Data Quality Firewall with Adaptive Thresholds
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

**Novel:** Bayesian inference for threshold adaptation

---

### Tier 6: Cost Optimization & Caching

#### 15. Intelligent Data Tiering with LSTM Access Prediction
Automated S3 tiering using ML-predicted access patterns.

**Stack:** S3 (Standard/IA/Glacier) + Athena + LSTM (PyTorch)

**Technical Implementation:**
- LSTM with attention for access probability prediction
- Feature engineering: recency, frequency, temporal patterns, file metadata
- Markov chain model for state transitions (hot → warm → cold)
- Cost-benefit analysis: storage cost vs retrieval cost vs access latency
- Proactive tiering: move data 48 hours before predicted access
- Reinforcement learning for policy optimization (Q-learning)

**Metrics:** 5PB data, 65% cost reduction, 99.5% prediction accuracy

**Novel:** Hierarchical temporal memory (HTM) for pattern recognition

---

#### 16. Semantic Query Caching with Sentence Embeddings
Intelligent cache using query similarity instead of exact matching.

**Stack:** Presto + Redis + Sentence-BERT + Custom cache layer

**Technical Implementation:**
- SQL query normalization: canonicalization, constant folding, predicate reordering
- Sentence-BERT embeddings for semantic similarity (cosine distance)
- Approximate nearest neighbor search using LSH (locality-sensitive hashing)
- Cache eviction: LRU with frequency-based promotion (LFU-LRU hybrid)
- Query rewriting: substitute cached subqueries while preserving semantics
- Partial result caching for common subexpressions

**Metrics:** 70% cache hit rate (vs 20% exact-match), 5x cost reduction

**Novel:** Learned cache replacement policy using neural networks

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
- **Storage Engines:** LSM-trees, B+ trees, HNSW graphs
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
- **Spatial:** R-trees, Quadtrees, HNSW graphs for vector search

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
