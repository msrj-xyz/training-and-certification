# Domain 1: Data Ingestion and Transformation (34%)

## Task 1.1: Data Ingestion Patterns

### Batch vs Streaming Ingestion

| Pattern | Description | AWS Services |
|---------|-------------|--------------|
| **Batch** | Periodic data loads | Glue, EMR, Batch, DMS |
| **Micro-batch** | Small frequent batches | Spark Structured Streaming |
| **Streaming** | Real-time continuous | Kinesis, MSK, Flink |
| **Change Data Capture (CDC)** | Track data changes | DMS, Glue (Spark CDC) |

> **Exam Tip:** Choose batch for historical analysis, streaming for real-time requirements!

---

### AWS Glue (Core Service) ⭐

| Component | Description |
|-----------|-------------|
| **Glue Studio** | Visual ETL job authoring |
| **Glue Crawlers** | Auto-discover schemas, populate Data Catalog |
| **Data Catalog** | Centralized metadata repository |
| **Glue Jobs** | Serverless Spark ETL |
| **Glue DataBrew** | Visual data preparation (no code) |
| **Job Bookmarks** | Track processed data, incremental loads |
| **Glue Connections** | JDBC, S3, Redshift connections |
| **Glue Triggers** | Schedule or event-based job execution |

#### Glue Job Types
| Type | Engine | Use Case |
|------|--------|----------|
| **Spark** | Apache Spark | Large-scale batch ETL |
| **Spark Streaming** | Spark Structured Streaming | Micro-batch streaming |
| **Python Shell** | Python | Light transformations |
| **Ray** | Ray (Python) | Distributed Python |

#### Glue Data Quality
- Define rules for data validation
- DQDL (Data Quality Definition Language)
- Integrate with Glue ETL jobs
- Publish metrics to CloudWatch

---

### Amazon Kinesis Suite

| Service | Description | Use Case |
|---------|-------------|----------|
| **Kinesis Data Streams** | Real-time streaming | Custom processing, multiple consumers |
| **Kinesis Data Firehose** | Streaming ETL | Auto-load to S3, Redshift, OpenSearch |
| **Managed Flink** | Stream processing | Complex stream analytics |
| **Kinesis Video Streams** | Video streaming | Video ingestion |

#### Kinesis Data Streams Features
- Shards for scaling (1 MB/s in, 2 MB/s out per shard)
- 24-hour to 365-day retention
- Enhanced Fan-Out for dedicated throughput
- KCL (Kinesis Client Library) for processing

#### Kinesis Data Firehose Features
- Serverless, auto-scaling
- Built-in transformations (Lambda)
- Automatic format conversion (Parquet, ORC)
- Destinations: S3, Redshift, OpenSearch, Splunk

> **Exam Tip:** Streams = custom processing, Firehose = auto-delivery to storage!

---

### Amazon MSK (Managed Kafka)

| Feature | Description |
|---------|-------------|
| **Brokers** | Kafka broker management |
| **MSK Connect** | Managed Kafka Connect |
| **MSK Serverless** | Auto-scaling Kafka |
| **Schema Registry** | Glue Schema Registry integration |

---

### Amazon EMR

| Component | Description |
|-----------|-------------|
| **EMR on EC2** | Hadoop/Spark clusters |
| **EMR on EKS** | Spark on Kubernetes |
| **EMR Serverless** | Serverless Spark/Hive |
| **EMR Studio** | Notebooks for development |

#### EMR Frameworks
- Apache Spark (most common)
- Apache Hive (SQL-like)
- Presto/Trino (interactive SQL)
- Apache Flink (streaming)
- Apache HBase (NoSQL)

---

### Database Migration Service (DMS)

| Feature | Description |
|---------|-------------|
| **Homogeneous** | Same DB type (Oracle → Oracle) |
| **Heterogeneous** | Different DB types (Oracle → Aurora) |
| **Full Load** | Initial migration |
| **CDC** | Ongoing replication |
| **Schema Conversion Tool** | Convert schemas |

> **Exam Tip:** For heterogeneous migrations, use SCT first, then DMS!

---

### AppFlow

| Feature | Description |
|---------|-------------|
| **SaaS Integration** | Connect to Salesforce, ServiceNow, etc. |
| **Bi-directional** | Read and write |
| **Transformations** | Filter, validate, mask |
| **Scheduling** | On-demand, scheduled, event-based |

---

## Task 1.2: Data Transformation

### Transformation Techniques

| Technique | Description | Tool |
|-----------|-------------|------|
| **Filtering** | Select specific records | Glue, Athena |
| **Aggregation** | Sum, count, average | Spark, SQL |
| **Joining** | Merge datasets | Spark, Glue |
| **Deduplication** | Remove duplicates | Spark, SQL |
| **Data Type Casting** | Convert data types | Glue DynamicFrame |
| **Partitioning** | Organize by columns | Spark, Athena |
| **Bucketing** | Hash-based distribution | Spark |

---

### AWS Glue DynamicFrame

| Feature | Description |
|---------|-------------|
| **Schema Flexibility** | Handle schema variations |
| **Resolvers** | Handle ambiguous types |
| **ResolveChoice** | Handle multiple types in column |
| **ApplyMapping** | Rename, cast, restructure |
| **Relationalize** | Flatten nested structures |

```python
# DynamicFrame Example
dyf = glueContext.create_dynamic_frame.from_catalog(
    database="mydb",
    table_name="mytable"
)

# Apply mapping
mapped = dyf.apply_mapping([
    ("old_name", "string", "new_name", "string"),
    ("amount", "string", "amount", "double")
])
```

---

### Data Format Conversion

| From | To | Best Practice |
|------|----|--------------| 
| **CSV** | Parquet | Columnar, compressed |
| **JSON** | Parquet | Flatten nested structures |
| **Parquet** | ORC | Alternative columnar |
| **Any** | Iceberg | Table format with ACID |

#### Data Formats Comparison
| Format | Type | Compression | Best For |
|--------|------|-------------|----------|
| **Parquet** | Columnar | High | Analytics, Athena, Redshift |
| **ORC** | Columnar | High | Hive, EMR |
| **Avro** | Row | Medium | Streaming, schema evolution |
| **JSON** | Row | None | Flexibility, semi-structured |
| **CSV** | Row | None | Simplicity, compatibility |

---

### Partitioning Strategies

| Strategy | Description | Example |
|----------|-------------|---------|
| **Time-based** | Partition by date/time | year/month/day |
| **Key-based** | Partition by business key | region, category |
| **Composite** | Multiple partition keys | year/month/region |

> **Exam Tip:** Good partitioning = faster queries, lower costs!

---

## Task 1.3: Orchestrate Data Pipelines

### AWS Step Functions

| Component | Description |
|-----------|-------------|
| **States** | Task, Choice, Parallel, Wait, Map |
| **State Machine** | Workflow definition (ASL) |
| **Express Workflows** | Short, high-volume |
| **Standard Workflows** | Long-running, auditable |

---

### Amazon MWAA (Managed Airflow)

| Feature | Description |
|---------|-------------|
| **DAGs** | Directed Acyclic Graphs |
| **Operators** | Task executors |
| **Hooks** | AWS service connections |
| **Sensors** | Wait for conditions |
| **Connections** | Manage credentials |

---

### EventBridge for Pipelines

| Pattern | Use Case |
|---------|----------|
| **S3 Events** | Trigger on new data |
| **Scheduled** | Cron-based execution |
| **Cross-account** | Event sharing |
| **Archive** | Event replay |

---

## Exam Tips for Domain 1

1. **Glue essentials:**
   - Crawlers = schema discovery
   - Data Catalog = metadata store
   - Job Bookmarks = incremental processing
   - DynamicFrame = flexible schema handling
2. **Kinesis selection:**
   - Data Streams = custom processing, multiple consumers
   - Firehose = serverless delivery to S3/Redshift
   - Flink = complex stream processing
3. **EMR options:**
   - EMR on EC2 = full control, persistent clusters
   - EMR Serverless = auto-scaling, pay per use
   - EMR on EKS = containerized Spark
4. **Data formats:**
   - Parquet = default for analytics
   - ORC = Hive optimized
   - Avro = streaming with schema evolution
5. **DMS migration:**
   - Homogeneous = direct migration
   - Heterogeneous = SCT + DMS
   - CDC = ongoing replication
6. **Partitioning:**
   - Time-based = most common (year/month/day)
   - Avoid too many small partitions
   - Align with query patterns
7. **Orchestration:**
   - Step Functions = AWS native, visual
   - MWAA = complex dependencies, Airflow expertise
   - EventBridge = event-driven triggers
8. **Glue Data Quality:**
   - DQDL = rule definition language
   - Integrate in ETL pipelines
   - CloudWatch metrics
9. **Streaming ingestion:**
   - Enhanced Fan-Out = multiple consumers
   - Firehose transformation = Lambda
   - Format conversion = Parquet/ORC
10. **AppFlow:**
    - SaaS to S3 integration
    - Bi-directional data flows
    - Built-in transformations
