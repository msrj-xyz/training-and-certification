# Domain 2: Data Store Management (26%)

## Task 2.1: Choose Data Stores

### Data Store Selection Matrix

| Use Case | Best Service | Why |
|----------|--------------|-----|
| **Analytics/Warehouse** | Redshift | Columnar, MPP, SQL |
| **Ad-hoc SQL on S3** | Athena | Serverless, pay per query |
| **Big Data Processing** | EMR | Spark, Hive, flexibility |
| **Real-time Key-Value** | DynamoDB | Low latency, scalable |
| **Document Store** | DocumentDB | MongoDB-compatible |
| **Graph Database** | Neptune | Relationship queries |
| **Time Series** | Timestream | IoT, metrics |
| **Search** | OpenSearch | Full-text search, logs |
| **Data Lake** | S3 + Lake Formation | Centralized, governed |

> **Exam Tip:** Know when to use Redshift vs Athena vs EMR!

---

### Amazon Redshift ⭐

| Feature | Description |
|---------|-------------|
| **Columnar Storage** | Optimized for analytics |
| **MPP Architecture** | Massively parallel processing |
| **Redshift Spectrum** | Query S3 data directly |
| **Concurrency Scaling** | Auto-add clusters for burst |
| **RA3 Nodes** | Separate compute and storage |
| **Serverless** | Auto-scaling, pay per use |
| **Data Sharing** | Cross-cluster data sharing |

#### Redshift Distribution Styles
| Style | Description | Use Case |
|-------|-------------|----------|
| **AUTO** | Redshift decides | Default |
| **EVEN** | Round-robin | No join key |
| **KEY** | Hash by column | Join columns |
| **ALL** | Copy to all nodes | Small dimension tables |

#### Redshift Sort Keys
| Type | Description |
|------|-------------|
| **Compound** | Multiple columns in order |
| **Interleaved** | Equal weight to columns |

---

### Amazon Athena ⭐

| Feature | Description |
|---------|-------------|
| **Serverless** | No infrastructure to manage |
| **Pay per Query** | Charged by data scanned |
| **SQL Standard** | Presto/Trino engine |
| **Federated Query** | Query other data sources |
| **CTAS** | Create Table As Select |
| **Views** | Create virtual tables |
| **Partitions** | Optimize query performance |

#### Athena Performance Optimization
| Technique | Benefit |
|-----------|---------|
| **Partitioning** | Reduce data scanned |
| **Columnar formats** | Parquet, ORC |
| **Compression** | Snappy, GZIP |
| **Bucketing** | Reduce shuffle |
| **CTAS** | Create optimized tables |

> **Exam Tip:** Use partitioning + Parquet to reduce Athena costs by 90%+!

---

### Amazon DynamoDB ⭐

| Feature | Description |
|---------|-------------|
| **Key-Value** | Single-digit millisecond latency |
| **Partition Key** | Required, for distribution |
| **Sort Key** | Optional, for range queries |
| **Capacity Modes** | On-demand or Provisioned |
| **Global Tables** | Multi-region replication |
| **DAX** | In-memory caching |
| **Streams** | Change data capture |
| **TTL** | Auto-expire items |

#### DynamoDB Access Patterns
| Pattern | Key Design |
|---------|-----------|
| **Single item** | Partition key only |
| **Range query** | Partition + Sort key |
| **Multiple access** | GSI (Global Secondary Index) |
| **LSI** | Local Secondary Index (same partition) |

---

### Amazon S3 for Data Lake ⭐

| Feature | Description |
|---------|-------------|
| **Storage Classes** | Standard, IA, Glacier |
| **Lifecycle Policies** | Auto-transition, expiration |
| **Versioning** | Object version control |
| **Replication** | Cross-region, same-region |
| **Object Lock** | WORM compliance |
| **S3 Select** | Query within objects |
| **S3 Tables** | Apache Iceberg support |

#### S3 Storage Classes
| Class | Use Case | Retrieval |
|-------|----------|-----------|
| **Standard** | Frequent access | Immediate |
| **Intelligent-Tiering** | Variable patterns | Automatic |
| **Standard-IA** | Infrequent access | Immediate |
| **One Zone-IA** | Reproducible data | Immediate |
| **Glacier Instant** | Archive, fast access | Milliseconds |
| **Glacier Flexible** | Archive | Minutes-hours |
| **Glacier Deep Archive** | Long-term archive | Hours |

---

### Table Formats

| Format | Features |
|--------|----------|
| **Apache Iceberg** | ACID, schema evolution, time travel |
| **Apache Hudi** | Upserts, incremental processing |
| **Delta Lake** | ACID, versioning (Databricks) |
| **Apache Parquet** | Columnar storage format |

#### Iceberg Benefits
- ACID transactions on S3
- Schema evolution without rewrite
- Time travel queries
- Partition evolution
- Hidden partitioning

---

## Task 2.2: Design Data Models

### Data Modeling Approaches

| Approach | Description | Use Case |
|----------|-------------|----------|
| **Star Schema** | Fact + Dimension tables | Traditional BI |
| **Snowflake Schema** | Normalized dimensions | Complex relationships |
| **Data Vault** | Hub, Link, Satellite | Enterprise, audit |
| **Denormalized** | Flat tables | Performance, simplicity |
| **Wide Tables** | All columns together | Analytics |

---

### Schema Design Considerations

| Factor | Approach |
|--------|----------|
| **Query patterns** | Design for access patterns |
| **Write vs Read** | Normalize vs Denormalize |
| **Data volume** | Partitioning strategy |
| **Update frequency** | Append vs Upsert |
| **Join complexity** | Pre-join vs Runtime join |

---

## Task 2.3: Manage Data Lifecycle

### AWS Glue Data Catalog ⭐

| Component | Description |
|-----------|-------------|
| **Databases** | Namespace for tables |
| **Tables** | Schema definitions |
| **Partitions** | Data organization |
| **Crawlers** | Auto-discover schemas |
| **Connections** | Data source access |
| **Schema Registry** | Schema versioning |

---

### AWS Lake Formation ⭐

| Feature | Description |
|---------|-------------|
| **Data Lake Setup** | Simplified S3 data lake |
| **Fine-grained Access** | Column/row-level security |
| **Data Filters** | Row/cell-level filtering |
| **Cross-account** | Share data across accounts |
| **Tag-based Access** | LF-Tags for permissions |
| **Blueprints** | Pre-built ingestion workflows |

#### Lake Formation Permissions
| Level | Scope |
|-------|-------|
| **Database** | Access to database |
| **Table** | Access to table |
| **Column** | Column-level filtering |
| **Row** | Row-level filtering |
| **Cell** | Combination of row + column |

> **Exam Tip:** Lake Formation replaces S3 bucket policies for fine-grained data lake security!

---

### Data Lifecycle Management

| Stage | Actions |
|-------|---------|
| **Ingestion** | Raw data landing |
| **Curation** | Cleansed, validated |
| **Consumption** | Aggregated, ready for use |
| **Archive** | Move to cold storage |
| **Deletion** | Expire, remove |

---

## Exam Tips for Domain 2

1. **Service selection:**
   - Redshift = data warehouse, complex queries
   - Athena = ad-hoc S3 queries, serverless
   - EMR = big data processing, Spark
   - DynamoDB = key-value, low latency
2. **Redshift optimization:**
   - Distribution KEY = join columns
   - Sort keys = filter columns
   - Spectrum = extend to S3
   - Concurrency scaling = burst capacity
3. **Athena cost optimization:**
   - Partition data = reduce scans
   - Columnar formats = Parquet, ORC
   - Compression = Snappy, GZIP
   - CTAS = create optimized tables
4. **DynamoDB design:**
   - Partition key = even distribution
   - GSI = alternative access patterns
   - DAX = microsecond latency
   - Streams = CDC, event-driven
5. **Lake Formation:**
   - Centralized permissions
   - LF-Tags = tag-based access
   - Fine-grained (column/row/cell)
   - Cross-account sharing
6. **Data Catalog:**
   - Crawlers = auto-discovery
   - Schema Registry = versioning
   - Central metadata store
7. **Table formats:**
   - Iceberg = ACID, time travel
   - Hudi = upserts, streaming
   - S3 Tables = native Iceberg
8. **S3 storage classes:**
   - Standard = hot data
   - IA = infrequent access
   - Glacier = archive
   - Lifecycle policies = auto-transition
9. **Data modeling:**
   - Star schema = BI workloads
   - Denormalized = performance
   - Design for query patterns
10. **Schema evolution:**
    - Iceberg = automatic
    - Glue = Schema Registry
    - Backward/forward compatibility
