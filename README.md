<div align="center">

# 🚀 Koushik Dutta

<img src="https://readme-typing-svg.herokuapp.com?font=Fira+Code&size=24&duration=3000&pause=1000&color=00D9FF&center=true&vCenter=true&width=600&lines=Senior+Data+Engineer+%40+Deloitte;Distributed+Systems+Enthusiast;Building+Fault-Tolerant+Data+Platforms" alt="Typing SVG" />

**`Senior Data Engineer @ Deloitte Products and Engineering`**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/koushik-dutta-9797a8209/)
[![Email](https://img.shields.io/badge/Yahoo!-6001D2?style=for-the-badge&logo=Yahoo!&logoColor=white)](mailto:dot.py@yahoo.com)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/koustreak)

</div>

---

## 👨‍💻 About Me

> *Building fault-tolerant, high-performance data platforms using cutting-edge technologies and novel algorithmic approaches*

I'm a **Senior Data Engineer** specializing in **distributed systems** and **large-scale data processing**. My expertise spans real-time stream processing, consensus algorithms, and lakehouse architectures.

### 🛠️ Core Expertise

<table>
<tr>
<td width="50%">

**💾 Data Engineering**
- Apache Kafka, Spark, Flink
- Airflow, Druid, Iceberg
- Hudi, Cassandra, PostgreSQL
- DataHub, Superset

</td>
<td width="50%">

**🌐 Distributed Systems**
- Raft Consensus, CRDTs
- Fault Tolerance, MVCC
- Distributed Transactions
- Causal Consistency

</td>
</tr>
<tr>
<td width="50%">

**☁️ Cloud & Orchestration**
- AWS (S3, EMR, EKS, Kinesis)
- Kubernetes, Docker
- Terraform, Helm

</td>
<td width="50%">

**💻 Programming Languages**
- Scala, Go, Python
- SQL, Bash

</td>
</tr>
</table>

**🎯 Specializations:** Real-time Stream Processing • Data Modeling • Query Optimization • Data Lakehouse • Lineage Tracking

---

## 🔥 Technical Projects

### 🕷️ Multi-Source ETL Pipeline with Web Scraping & Real-Time Ingestion

Scalable ETL framework that ingests data from web scraping, REST APIs, and database CDC into a unified lakehouse. Handles 50+ heterogeneous data sources with automatic schema detection and normalization. Implements adaptive rate limiting and dead letter queues for fault tolerance.

**Solution:**
1. **Extract:** Web scraping using **Scrapy** with rotating proxies and **Selenium** for JavaScript-rendered pages → Store raw HTML/JSON in **MinIO** object storage
2. **CDC Ingestion:** **PostgreSQL** logical replication slots capture database changes → Stream to **Kafka** topics partitioned by table
3. **Transform:** **Spark** (Scala) jobs read from MinIO and Kafka → Apply schema normalization, deduplication (bloom filters), and data enrichment → Write to **S3** in Parquet format
4. **Load:** Batch insert Parquet files to **S3** data lake → Real-time streaming to **Druid** for sub-second analytics with bitmap indexes
5. **Orchestrate:** **Airflow** DAGs with dynamic task generation → Sensor operators for upstream dependencies → Branch operators for conditional processing
6. **Quality:** Custom validation framework checks schema compliance and data anomalies → Failed records sent to DLQ in **Kafka** for retry

**Metrics:** 10M records/day, <30min end-to-end latency, 99.5% data quality score

---

### 🔄 Batch & Streaming ETL with SQL-Based Transformations

Unified ETL platform supporting both batch and streaming workloads using SQL-first approach. Implements 500+ SQL transformations with automatic optimization and partition pruning. Handles slowly changing dimensions (SCD Type 2) and incremental loads.

**Solution:**
1. **Batch Processing:** **Airflow** triggers **Spark SQL** (Scala) jobs on schedule → Read from **S3** with partition pruning and predicate pushdown → Execute SQL transformations (joins, window functions, aggregations)
2. **Streaming Processing:** **Spark Structured Streaming** consumes from **Kafka** → Micro-batch processing with watermarks for late data → Stateful operations using RocksDB state store
3. **Data Lake:** Historical data in **S3** (Parquet, Snappy compression) → Staging and intermediate results in **MinIO** → Metadata tracked in Hive Metastore
4. **Query Engine:** **Presto** for ad-hoc SQL queries across S3 and Druid → Connector architecture enables federated queries
5. **Real-time OLAP:** **Druid** ingests streaming data → Columnar storage with bitmap indexes → Rollup and retention policies for data lifecycle
6. **SCD Type 2:** Track dimension changes with effective_start_date, effective_end_date, and is_current flag → **Spark SQL** MERGE INTO for upserts
7. **Monitoring:** Custom **Airflow** plugins track pipeline health, SLA violations, and data freshness → **Python** UDFs for complex business logic

**Metrics:** 2TB daily processing, 1000+ Airflow tasks, <5min query latency

---

### ⚡ Adaptive Pipeline Preemption Framework with Raft Consensus

Custom priority scheduler implementing preemptive multitasking for distributed stream processing. Uses Raft consensus for coordinating priority queues across executors. Achieves 85% latency reduction through intelligent task preemption and speculative checkpointing.

**Solution:**
1. **Priority Queue:** Lock-free concurrent skip list (O(log n) insert/delete) stores tasks with priorities → **Raft** consensus ensures distributed queue consistency across nodes
2. **Preemption:** High-priority tasks trigger checkpoint of running tasks → Serialize state to distributed snapshot in **HDFS** → Reschedule preempted tasks
3. **Backpressure:** Token bucket algorithm with dynamic rate adjustment → Monitors **Kafka** lag and **Spark** executor metrics → Adjusts ingestion rate
4. **Checkpointing:** Speculative checkpointing predicts task completion time → Creates checkpoints before expected preemption → Uses historical execution patterns
5. **Deployment:** **Kubernetes** DaemonSet runs Raft nodes → **Docker** containers for Spark executors → Custom scheduler plugin integrates with Spark

**Metrics:** 2-5M events/day, P99 latency 1.8s (85% reduction)

---

### 🎯 Raft Consensus Implementation with Speculative Execution

From-scratch Raft implementation in **Go** with novel optimizations for read-heavy workloads. Implements speculative execution where followers execute commands before commit. Achieves <5s failover time and 99.995% availability.

**Solution:**
1. **Leader Election:** Randomized election timeouts (150-300ms) → Candidates request votes with term number → Majority votes elect leader
2. **Log Replication:** Leader batches log entries → Pipelines AppendEntries RPCs to followers → Followers speculatively execute commands before commit
3. **Rollback:** On conflict detection, followers rollback speculative state → Re-execute from last committed index
4. **Optimization:** Memory-mapped files with fsync batching for persistent state → Adaptive heartbeat intervals using EWMA of network latency → Lease-based linearizable reads without log append
5. **Log Compaction:** Snapshot state machine at configurable intervals → Truncate log up to snapshot index → Transfer snapshots to lagging followers
6. **Deployment:** **Docker** containers for Raft nodes → **Kubernetes** StatefulSets for stable network identities → **Kafka** for external log transport

**Metrics:** <5s failover, 99.995% availability, 10K ops/sec throughput

---

### 🌍 Multi-Region CRDT-Based Analytics with Causal Consistency

Geo-distributed analytics using operation-based CRDTs with vector clocks for causal ordering. Implements anti-entropy protocol using Merkle trees for state reconciliation across 5 AWS regions. Achieves sub-200ms cross-region synchronization.

**Solution:**
1. **CRDT Implementation:** G-Counter, PN-Counter, OR-Set, LWW-Register in **Scala** → Semilattice merge functions (commutative, associative, idempotent) → Delta-state CRDTs minimize network bandwidth
2. **Causal Ordering:** Vector clocks with version vectors track causality → Lamport timestamps provide total ordering on concurrent updates
3. **State Sync:** Anti-entropy protocol uses Merkle trees to detect divergence → **Kafka** (cross-region) streams CRDT operations → **Flink** processes and merges CRDT states
4. **Storage:** **DynamoDB Global Tables** store CRDT state with multi-region replication → Eventual consistency guarantees convergence
5. **Analytics:** **Druid** ingests merged CRDT state → Bitmap indexes enable low-latency queries → Rollup aggregations for time-series data
6. **Compression:** Hybrid vector clock compression using bloom filters reduces metadata overhead

**Metrics:** 100M events/day, sub-200ms cross-region sync, 5 AWS regions

---

### 📦 Zero-Copy Lakehouse with Copy-on-Write Semantics

Pointer-based incremental materialized views using **Apache Iceberg's** snapshot isolation. Implements lazy compaction and Z-ordering for multi-dimensional clustering. Achieves 99.7% storage efficiency on 2.5PB data with sub-second queries.

**Solution:**
1. **Incremental Views:** **Iceberg** manifest files act as pointers to data files → Views reference manifests instead of copying data → Copy-on-write B+ tree indexes metadata (O(log n) lookup)
2. **Lazy Compaction:** Defer compaction until read amplification exceeds threshold → Monitor query performance metrics → Trigger compaction jobs in **Spark**
3. **Partition Pruning:** Bloom filters on data files reduce false positives by 99% → **Spark** Catalyst optimizer pushes down predicates
4. **Z-Ordering:** Space-filling curves cluster multi-dimensional data → Adaptive column selection using query log analysis and mutual information → Store in **S3** with optimal layout
5. **Snapshot Management:** Reference counting tracks snapshot usage → Mark-sweep GC expires old snapshots → **Druid** queries latest snapshots for real-time analytics
6. **Metadata:** Custom metadata layer in **Go** manages Iceberg catalog → Stores in **AWS S3** with versioning

**Metrics:** 2.5PB data, 99.7% storage efficiency, sub-second queries

---

### ⏰ Bi-Temporal Iceberg with Interval Tree Indexing

Extended **Apache Iceberg** with transaction-time and valid-time dimensions for temporal queries. Implements interval tree (augmented AVL tree) for O(log n + k) temporal range queries. Supports Allen's interval algebra for complex temporal joins.

**Solution:**
1. **Temporal Schema:** Add transaction_time (system-managed) and valid_time (user-managed) columns → **Iceberg** schema evolution adds temporal metadata
2. **Interval Tree:** Augmented AVL tree indexes temporal ranges → Each node stores max endpoint in subtree → Enables O(log n + k) range queries
3. **Temporal Joins:** Allen's interval algebra (before, meets, overlaps, etc.) → **Spark** Catalyst optimizer pushes down temporal predicates → Fractional cascading optimizes multi-way joins
4. **MVCC Extension:** Snapshot isolation extended to bi-temporal dimensions → Readers see consistent view across both time axes
5. **Compression:** Bitmap indexes for temporal validity ranges → Temporal coalescing merges adjacent intervals → Reduces storage overhead
6. **Query Engine:** Custom temporal query engine in **Scala** → Integrates with **Spark** and **Hive Metastore** → Metadata in **AWS Glue**

**Metrics:** 40TB historical data, <100ms temporal query overhead

---

### 🎲 Cost-Based Query Optimizer with Adaptive Statistics

Self-tuning **Spark SQL** optimizer using runtime statistics feedback. Implements dynamic programming for join order enumeration with O(n²) complexity. Achieves 40% speedup over Spark's default CBO on 500K+ queries.

**Solution:**
1. **Cardinality Estimation:** Histograms with equi-depth buckets estimate selectivity → **Spark** Catalyst collects column statistics → Adaptive histogram refinement based on query feedback
2. **Join Ordering:** Dynamic programming enumerates join orders → Prunes suboptimal plans using cost model → Cost = CPU cost + I/O cost + network cost
3. **Join Selection:** Choose hash join, sort-merge join, or broadcast join → Based on table sizes and data distribution → **Spark** executor metrics inform decisions
4. **Adaptive Re-optimization:** Monitor runtime statistics during execution → Detect cardinality estimation errors → Re-optimize remaining query plan
5. **Skew Handling:** Detect data skew using partition statistics → Adjust partition count dynamically → Salting for skewed joins
6. **Orchestration:** **Airflow** schedules statistics collection jobs → **Hive** stores table statistics → Deployed on **AWS EMR**

**Metrics:** 40% speedup vs Spark CBO, handles 500K+ queries

---

### ✅ Exactly-Once Semantics with Idempotent Sinks

Distributed exactly-once processing without 2PC using deterministic request IDs. Implements bloom filter-based deduplication with 99.999% accuracy. Processes 200M events/day with <2ms overhead.

**Solution:**
1. **Deterministic IDs:** Generate request ID as `hash(topic, partition, offset, record_key)` → Ensures same event always produces same ID
2. **Deduplication:** Counting bloom filter tracks seen IDs with TTL → **RocksDB** stores bloom filter state → False positive rate <0.001%
3. **Checkpointing:** **Flink** checkpoint alignment using barrier injection → Asynchronous checkpoints with incremental **RocksDB** snapshots → Checkpoint to **S3** every 5 minutes
4. **Idempotent Writes:** Sink framework uses conditional writes (compare-and-swap) → Database checks request ID before applying → Prevents duplicate writes
5. **Event Time:** Watermark propagation for event-time processing → Handle late-arriving data with allowed lateness
6. **Deployment:** **Kafka** source with exactly-once consumption → **Kubernetes** runs Flink JobManager and TaskManagers → Custom sink framework in **Scala**

**Metrics:** 200M events/day, 99.999% dedup accuracy, <2ms overhead

---

### 🔀 Adaptive Backpressure with Token Bucket Rate Limiting

Dynamic rate control for stream processing with feedback loops. Implements token bucket algorithm with AIMD (additive increase, multiplicative decrease) for stability. Achieves 60% reduction in over-provisioning while maintaining <1s P99 latency.

**Solution:**
1. **Token Bucket:** Dynamic rate adjustment using token bucket algorithm → Tokens generated at variable rate based on system health
2. **Metrics Collection:** **Prometheus** scrapes **Kafka** lag, **Spark** executor metrics, and downstream latency → EWMA smooths metrics to reduce noise
3. **Feedback Loop:** Monitor P99 latency → If latency > threshold, decrease rate (multiplicative) → If latency < threshold, increase rate (additive) → AIMD ensures stability
4. **Multi-Level Backpressure:** Source throttling at **Kafka** consumer → Operator buffering in **Spark Streaming** → Sink flow control at database
5. **Trend Detection:** Sliding window statistics detect increasing latency trends → Proactive rate adjustment before SLA violation
6. **Deployment:** **Kubernetes** HPA (Horizontal Pod Autoscaler) scales based on custom metrics → **Docker** containers on **AWS EKS** → **Prometheus** for monitoring

**Metrics:** 60% reduction in over-provisioning, <1s P99 latency maintained

---

### 🕸️ Self-Service Data Mesh with Automated Lineage Tracking

Decentralized data platform with automatic dependency graph construction using AST parsing. Implements column-level lineage tracking through SQL expression analysis. Covers 200+ data products across 15 domains with 95% lineage coverage.

**Solution:**
1. **SQL Parsing:** ANTLR4 grammar parses SQL queries → Abstract syntax tree (AST) traversal extracts table and column dependencies → **Python** lineage parser processes queries
2. **Lineage Graph:** Directed acyclic graph (DAG) represents data lineage → Nodes are tables/columns, edges are dependencies → Store in **Apache Atlas**
3. **Impact Analysis:** Transitive closure algorithm finds downstream dependencies → PageRank scores table importance → Enables change impact assessment
4. **Schema Evolution:** **Avro** schema registry tracks schema changes → Metadata versioning using Merkle DAG (content-addressable storage)
5. **Real-time Updates:** **Kafka** streams query logs and schema changes → **Airflow** DAGs process lineage updates → **Hive Metastore** and **AWS Glue** provide metadata
6. **Column-Level Lineage:** Expression analysis tracks column transformations → Handles joins, aggregations, and window functions

**Metrics:** 200+ data products, 95% lineage coverage, 15 domains

---

### 🛡️ Real-Time Data Quality Firewall with Adaptive Thresholds

Streaming validation with circuit breaker pattern and statistical process control. Implements Rete algorithm for pattern matching and adaptive thresholds using EWMA. Validates data with <5ms latency and 99.9% good data pass-through.

**Solution:**
1. **Rule Engine:** Rete algorithm in **Scala** for efficient pattern matching → Compiles validation rules into decision network → Processes **Kafka Streams** events
2. **Anomaly Detection:** Shewhart control charts (SPC) detect statistical anomalies → Upper/lower control limits based on historical data → **Flink** computes statistics
3. **Adaptive Thresholds:** EWMA adjusts thresholds based on data distribution changes → Prevents false positives during legitimate shifts
4. **Circuit Breaker:** Three states (closed, open, half-open) based on error rate → Open state bypasses validation to prevent cascading failures → Half-open state tests recovery
5. **Windowing:** Tumbling and hopping windows aggregate metrics → Count-min sketch estimates frequency with bounded memory
6. **Metrics Storage:** **Iceberg** stores data quality metrics for historical analysis → **Great Expectations** defines validation rules

**Metrics:** <5ms validation latency, 99.9% good data pass-through

---

### 🏢 Enterprise Data Warehouse with Dimensional Modeling

Scalable data warehouse implementing Kimball methodology with star schema. Handles 500TB data across 200+ dimension tables and 50+ fact tables. Implements SCD Type 2 for slowly changing dimensions with 1000+ **Airflow** DAGs orchestrating ETL.

**Solution:**
1. **Dimensional Modeling:** Star schema with conformed dimensions (customer, product, time, location) → Fact tables store transaction facts (additive), snapshot facts (semi-additive), and accumulating snapshot facts
2. **SCD Type 2:** Track dimension changes with effective_start_date, effective_end_date, is_current flag → **Spark** jobs implement SCD logic → **Hudi** merge-on-read for efficient updates
3. **Data Ingestion:** Raw data lands in **S3** → **AWS Glue** crawlers discover schema → **Spark** ETL jobs transform and load to **Hudi** staging
4. **Data Warehouse:** **Hive** stores dimension and fact tables → Dynamic partitioning by date + bucketing by customer_id → **Hive Metastore** manages metadata
5. **Query Federation:** **Presto** enables cross-source queries (Hive + S3 + Redshift) → Push down predicates for optimization
6. **Orchestration:** **Airflow** DAGs with sensor operators (wait for upstream), branch operators (conditional logic), dynamic task generation → **AWS Glue Catalog** for unified metadata
7. **Data Quality:** **Great Expectations** validates data in Airflow DAGs → Compaction: **Hudi** async compaction with configurable strategies

**Metrics:** 500TB data warehouse, 200+ dimension tables, 50+ fact tables, 1000+ Airflow DAGs, 99.9% SLA adherence

---

### 💾 Query Result Caching with Semantic Similarity

Intelligent cache using query similarity instead of exact matching. Implements MinHash for query fingerprinting and Jaccard similarity for approximate matching. Achieves 70% cache hit rate (vs 20% exact-match) with partial result reuse.

**Solution:**
1. **Query Normalization:** Canonicalize SQL queries → Constant folding, predicate reordering, whitespace removal → Produces normalized query representation
2. **Query Fingerprinting:** MinHash algorithm generates query fingerprints → Hash-based similarity enables O(1) lookup → **Presto** query logs feed fingerprinting
3. **Similarity Matching:** Jaccard similarity on query tokens → Threshold-based matching (e.g., >80% similar) → Find semantically similar cached queries
4. **Cache Storage:** **Redis** stores query results with TTL → LFU-LRU hybrid eviction (frequency-based promotion) → Hot queries stay cached longer
5. **Query Rewriting:** Substitute cached subqueries while preserving semantics → Partial result caching for common subexpressions → **Kafka** logs cache hits/misses
6. **Cache Warming:** Proactively cache popular queries based on query log analysis → Predict query patterns using historical data

**Metrics:** 70% cache hit rate (vs 20% exact-match), <10ms cache lookup

---

<div align="center">

## 🛠️ Technology Stack

</div>

### 📊 Data Processing & Streaming
<p align="center">
<img src="https://img.shields.io/badge/Apache%20Kafka-231F20?style=for-the-badge&logo=apache-kafka&logoColor=white" />
<img src="https://img.shields.io/badge/Apache%20Spark-E25A1C?style=for-the-badge&logo=apache-spark&logoColor=white" />
<img src="https://img.shields.io/badge/Apache%20Flink-E6526F?style=for-the-badge&logo=apache-flink&logoColor=white" />
<img src="https://img.shields.io/badge/Apache%20Airflow-017CEE?style=for-the-badge&logo=apache-airflow&logoColor=white" />
</p>

### 💾 Data Storage & Lakehouse
<p align="center">
<img src="https://img.shields.io/badge/Apache%20Iceberg-3C8CE7?style=for-the-badge&logo=apache&logoColor=white" />
<img src="https://img.shields.io/badge/Apache%20Druid-29F1FB?style=for-the-badge&logo=apache-druid&logoColor=black" />
<img src="https://img.shields.io/badge/Apache%20Hudi-FF6B35?style=for-the-badge&logo=apache&logoColor=white" />
<img src="https://img.shields.io/badge/Apache%20Hive-FDEE21?style=for-the-badge&logo=apache-hive&logoColor=black" />
</p>

### ☁️ Cloud & Infrastructure
<p align="center">
<img src="https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white" />
<img src="https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white" />
<img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" />
<img src="https://img.shields.io/badge/Terraform-7B42BC?style=for-the-badge&logo=terraform&logoColor=white" />
</p>

### 💻 Programming Languages
<p align="center">
<img src="https://img.shields.io/badge/Scala-DC322F?style=for-the-badge&logo=scala&logoColor=white" />
<img src="https://img.shields.io/badge/Go-00ADD8?style=for-the-badge&logo=go&logoColor=white" />
<img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" />
<img src="https://img.shields.io/badge/SQL-4479A1?style=for-the-badge&logo=postgresql&logoColor=white" />
</p>

### 🔧 Distributed Systems
<p align="center">
<img src="https://img.shields.io/badge/Raft_Consensus-FF6B6B?style=for-the-badge&logo=&logoColor=white" />
<img src="https://img.shields.io/badge/CRDTs-4ECDC4?style=for-the-badge&logo=&logoColor=white" />
<img src="https://img.shields.io/badge/MVCC-95E1D3?style=for-the-badge&logo=&logoColor=black" />
<img src="https://img.shields.io/badge/Distributed_Transactions-F38181?style=for-the-badge&logo=&logoColor=white" />
</p>

---

<div align="center">

## 📊 GitHub Stats

</div>

<p align="center">
  <img src="https://github-readme-stats.vercel.app/api?username=koustreak&show_icons=true&theme=tokyonight&hide_border=true&count_private=true" alt="GitHub Stats" />
</p>

<p align="center">
  <img src="https://github-readme-streak-stats.herokuapp.com/?user=koustreak&theme=tokyonight&hide_border=true" alt="GitHub Streak" />
</p>

<p align="center">
  <img src="https://github-readme-stats.vercel.app/api/top-langs/?username=koustreak&layout=compact&theme=tokyonight&hide_border=true&langs_count=8" alt="Top Languages" />
</p>

---

<div align="center">

## 📬 Contact

<table>
<tr>
<td align="center">
<a href="mailto:dot.py@yahoo.com">
<img src="https://img.shields.io/badge/Email-6001D2?style=for-the-badge&logo=yahoo&logoColor=white" />
</a>
</td>
<td align="center">
<a href="https://www.linkedin.com/in/koushik-dutta-9797a8209/">
<img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white" />
</a>
</td>
<td align="center">
<a href="https://github.com/koustreak">
<img src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white" />
</a>
</td>
</tr>
</table>

---

<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=6,11,20&height=100&section=footer&text=Building%20Fault-Tolerant%20Data%20Systems%20at%20Scale&fontSize=20&fontColor=fff&animation=twinkling" />

</div>

