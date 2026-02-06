# AWS Data Engineer Associate (DEA-C01) - Quick Reference Card

## Exam at a Glance
| | |
|---|---|
| **Duration** | 130 min | **Questions** | 65 (50 scored) |
| **Passing** | 720/1000 | **Experience** | 2-3 years data eng |

## Domain Weights
```
Domain 1: Data Ingestion & Transform  ████████████████████████   34%
Domain 2: Data Store Management       ██████████████████         26%
Domain 3: Data Operations & Support   ████████████████           22%
Domain 4: Security & Governance       █████████████              18%
```

---

## 🔥 Core Services to Master

### ETL & Ingestion
| Service | Purpose |
|---------|---------|
| **AWS Glue** ⭐ | ETL, Crawlers, Data Catalog |
| **Kinesis Streams** | Real-time streaming |
| **Kinesis Firehose** | Serverless delivery to S3 |
| **EMR** | Spark/Hadoop processing |
| **DMS** | Database migration + CDC |
| **AppFlow** | SaaS data integration |

### Data Stores
| Service | Purpose |
|---------|---------|
| **Redshift** ⭐ | Data warehouse (MPP) |
| **Athena** ⭐ | Serverless S3 queries |
| **DynamoDB** | Key-value, low latency |
| **S3** ⭐ | Data lake storage |
| **Lake Formation** ⭐ | Governance + permissions |

---

## 📊 Service Selection Guide

| Need | Use |
|------|-----|
| Real-time streaming | Kinesis Data Streams |
| Auto-load to S3/Redshift | Kinesis Firehose |
| Complex stream processing | Managed Flink |
| Ad-hoc SQL on S3 | Athena |
| Data warehouse | Redshift |
| Big data processing | EMR |
| ETL automation | Glue |

---

## 🔧 Glue Quick Reference

| Component | Purpose |
|-----------|---------|
| **Crawlers** | Auto-discover schemas |
| **Data Catalog** | Metadata store |
| **Job Bookmarks** | Incremental processing |
| **DynamicFrame** | Flexible schema handling |
| **Data Quality** | DQDL validation rules |

---

## 💾 Data Format Selection

| Format | Best For |
|--------|----------|
| **Parquet** ⭐ | Analytics (columnar) |
| **ORC** | Hive workloads |
| **Avro** | Streaming + schema evolution |
| **Iceberg** | ACID + time travel |

---

## 🔐 Security Quick Reference

| Area | Service |
|------|---------|
| **Fine-grained access** | Lake Formation |
| **Encryption** | KMS (SSE-KMS) |
| **PII detection** | Macie |
| **Data masking** | DataBrew, Redshift |
| **Audit** | CloudTrail |

### Lake Formation vs S3 Policies
- Lake Formation = column/row/cell level
- S3 = bucket/prefix level
- **Use Lake Formation for data lakes!**

---

## ⚡ Performance Tips

| Service | Optimization |
|---------|-------------|
| **Athena** | Partition + Parquet = 90% cost reduction |
| **Redshift** | Distribution KEY = join columns |
| **Glue** | G.2X workers + bookmarks |
| **Kinesis** | Shards for scaling |

---

## ✅ Exam Day Reminders

1. **Glue Crawlers** = schema discovery → Data Catalog
2. **Job Bookmarks** = incremental processing
3. **Kinesis Streams vs Firehose** = custom vs auto-delivery
4. **Athena cost** = partition + Parquet
5. **Redshift Spectrum** = query S3 from Redshift
6. **Lake Formation** = fine-grained data lake security
7. **Gateway endpoints** = S3 + DynamoDB (FREE)
8. **Macie** = PII detection in S3
9. **Iceberg/Hudi** = ACID on S3
10. **DMS + SCT** = heterogeneous migrations
