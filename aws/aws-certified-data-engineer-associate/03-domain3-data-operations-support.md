# Domain 3: Data Operations and Support (22%)

## Task 3.1: Automate Data Processing

### Data Pipeline Automation

| Service | Use Case |
|---------|----------|
| **AWS Glue Triggers** | Event or schedule-based jobs |
| **EventBridge** | S3 events, cross-service |
| **Step Functions** | Complex workflows |
| **MWAA (Airflow)** | DAG-based orchestration |
| **Lambda** | Event-driven processing |

---

### AWS Glue Job Automation

| Feature | Description |
|---------|-------------|
| **Scheduled Triggers** | Cron-based execution |
| **Event Triggers** | On job completion |
| **Conditional Triggers** | Success/failure conditions |
| **Job Bookmarks** | Track processed data |
| **Crawler Triggers** | Auto-catalog new data |

#### Glue Job Bookmarks
- Track processed files/partitions
- Enable incremental processing
- Reset or pause as needed
- Stored in Glue service

---

### CI/CD for Data Pipelines

| Stage | Tool |
|-------|------|
| **Source** | CodeCommit, GitHub |
| **Build** | CodeBuild |
| **Test** | Glue Data Quality |
| **Deploy** | CodePipeline, CloudFormation |
| **Monitor** | CloudWatch |

---

### Infrastructure as Code

| Tool | Description |
|------|-------------|
| **CloudFormation** | Declarative templates |
| **AWS CDK** | Programmatic (Python, TypeScript) |
| **Terraform** | Multi-cloud IaC |
| **AWS SAM** | Serverless framework |

---

## Task 3.2: Analyze Data Using SQL

### Amazon Athena SQL

| Feature | Description |
|---------|-------------|
| **DDL** | CREATE, ALTER, DROP |
| **DML** | SELECT, INSERT (CTAS) |
| **Views** | Virtual tables |
| **CTAS** | Create Table As Select |
| **UNLOAD** | Export query results |
| **Prepared Statements** | Parameterized queries |

#### Athena Query Examples
```sql
-- Create external table
CREATE EXTERNAL TABLE logs (
    timestamp string,
    request_id string,
    status int
)
PARTITIONED BY (year string, month string)
STORED AS PARQUET
LOCATION 's3://bucket/logs/';

-- Add partitions
MSCK REPAIR TABLE logs;

-- Query with partition pruning
SELECT * FROM logs
WHERE year = '2024' AND month = '01';
```

---

### Amazon Redshift SQL

| Feature | Description |
|---------|-------------|
| **Spectrum** | Query S3 data |
| **Materialized Views** | Pre-computed results |
| **Stored Procedures** | PL/pgSQL |
| **UDFs** | User-defined functions |
| **Federated Query** | Query RDS, Aurora |

---

### OpenSearch Query

| Query Type | Use Case |
|------------|----------|
| **Match** | Full-text search |
| **Term** | Exact value |
| **Range** | Numeric/date ranges |
| **Bool** | Combine conditions |
| **Aggregations** | Analytics |

---

## Task 3.3: Maintain and Monitor Data Pipelines

### Amazon CloudWatch for Data Pipelines

| Component | Use Case |
|-----------|----------|
| **Metrics** | Glue, EMR, Kinesis metrics |
| **Logs** | Job logs, error tracking |
| **Alarms** | Threshold alerts |
| **Dashboards** | Unified monitoring |
| **Logs Insights** | Query and analyze logs |

#### Key Glue Metrics
- `glue.driver.aggregate.bytesRead`
- `glue.driver.aggregate.recordsRead`
- `glue.driver.aggregate.elapsedTime`
- `glue.driver.jvm.heap.usage`

#### Key Kinesis Metrics
- `IncomingRecords`
- `IncomingBytes`
- `GetRecords.IteratorAgeMilliseconds`
- `WriteProvisionedThroughputExceeded`

---

### AWS CloudTrail

| Feature | Description |
|---------|-------------|
| **API Logging** | All AWS API calls |
| **Event History** | 90-day lookup |
| **Trail** | S3 destination for long-term |
| **Insights** | Unusual activity detection |
| **Organization Trail** | Multi-account |

---

### Performance Troubleshooting

| Issue | Check |
|-------|-------|
| **Slow Glue Jobs** | Worker type, parallelism, data skew |
| **High Athena Cost** | Partitioning, file format |
| **Kinesis Lag** | Shard count, consumer throughput |
| **Redshift Slow** | Distribution, sort keys, vacuum |

#### Glue Performance Optimization
| Technique | Benefit |
|-----------|---------|
| **G.2X Workers** | More memory |
| **Parallelism** | Concurrent tasks |
| **Pushdown Predicates** | Filter at source |
| **Job Bookmarks** | Incremental processing |
| **Partitioning** | Reduce data read |

---

## Task 3.4: Ensure Data Quality

### AWS Glue Data Quality

| Feature | Description |
|---------|-------------|
| **DQDL** | Data Quality Definition Language |
| **Rules** | Completeness, uniqueness, range |
| **Analysis** | Profile data automatically |
| **Integration** | Embed in Glue jobs |
| **Metrics** | Publish to CloudWatch |

#### DQDL Rule Examples
```
Rules = [
    ColumnExists "customer_id",
    Completeness "email" >= 0.95,
    Uniqueness "order_id" = 1.0,
    ColumnValues "status" in ["active", "inactive"],
    ColumnLength "phone" = 10
]
```

---

### Data Validation Strategies

| Strategy | Description |
|----------|-------------|
| **Schema Validation** | Type, nullability |
| **Business Rules** | Domain-specific checks |
| **Referential Integrity** | Foreign key validation |
| **Statistical Checks** | Outliers, distributions |
| **Cross-source Reconciliation** | Compare source and target |

---

## Exam Tips for Domain 3

1. **Pipeline automation:**
   - Glue Triggers = scheduled, event-based
   - Job Bookmarks = incremental loads
   - Step Functions = complex workflows
2. **Athena optimization:**
   - Partition pruning = WHERE on partition columns
   - CTAS = create optimized tables
   - MSCK REPAIR = add partitions
3. **CloudWatch monitoring:**
   - Glue metrics = heap usage, records
   - Kinesis = iterator age for lag
   - Alarms = threshold notifications
4. **Glue Data Quality:**
   - DQDL = rule definition language
   - Completeness, uniqueness, validity
   - Integration in ETL jobs
5. **Troubleshooting:**
   - Slow Glue = check worker type, parallelism
   - High Athena cost = check partitioning
   - Kinesis lag = add shards
6. **CI/CD for data:**
   - CodePipeline = orchestration
   - CloudFormation = IaC for Glue/Redshift
   - CDK = programmatic infrastructure
7. **CloudTrail:**
   - API audit logging
   - 90-day event history
   - Insights = unusual activity
8. **Redshift performance:**
   - VACUUM = reclaim space
   - ANALYZE = update statistics
   - Workload management (WLM)
9. **Log analysis:**
   - CloudWatch Logs Insights
   - Query Glue job logs
   - Error pattern detection
10. **Data reconciliation:**
    - Compare source vs target counts
    - Hash comparison for data integrity
    - Cross-region validation
