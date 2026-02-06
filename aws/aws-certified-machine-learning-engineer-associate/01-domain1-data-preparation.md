# Domain 1: Data Preparation for Machine Learning (28%)

## Task 1.1: Ingest and Store Data

### Data Formats for ML

| Format | Description | Use Case |
|--------|-------------|----------|
| **Parquet** | Columnar, compressed | Analytics, large datasets |
| **CSV** | Row-based, human-readable | Simple tabular data |
| **JSON** | Nested structure | API data, semi-structured |
| **RecordIO** | SageMaker native format | Efficient SageMaker training |
| **ORC** | Optimized row columnar | Hadoop ecosystem |
| **Avro** | Row-based, schema evolution | Streaming data |

> **Exam Tip:** Parquet is preferred for ML workloads due to columnar storage and compression!

---

### AWS Data Sources

| Service | Description | ML Use Case |
|---------|-------------|-------------|
| **Amazon S3** | Object storage | Primary ML data store |
| **Amazon EFS** | NFS file system | Shared training data |
| **Amazon FSx** | High-performance FS | Large-scale training |
| **Amazon EBS** | Block storage | Instance attached storage |
| **Amazon RDS** | Relational database | Structured data source |
| **Amazon DynamoDB** | NoSQL database | Real-time feature store |

### S3 for ML Data

| Feature | Description |
|---------|-------------|
| **S3 Transfer Acceleration** | Fast long-distance uploads |
| **S3 Versioning** | Track data versions |
| **S3 Lifecycle** | Auto-archive/delete |
| **S3 Select** | Query data in S3 |
| **S3 Glacier** | Cold storage archival |

---

### Streaming Data Ingestion

| Service | Description | Use Case |
|---------|-------------|----------|
| **Amazon Kinesis Data Streams** | Real-time streaming | Low-latency data collection |
| **Amazon Kinesis Data Firehose** | Streaming ETL | Auto-load to S3/Redshift |
| **Amazon MSK** | Managed Kafka | High-throughput streaming |
| **Amazon Managed Flink** | Stream processing | Real-time analytics |

---

### SageMaker Data Ingestion

| Component | Description |
|-----------|-------------|
| **SageMaker Data Wrangler** | Visual data preparation |
| **SageMaker Feature Store** | Feature management and sharing |
| **SageMaker Processing** | Data preprocessing jobs |
| **Pipe Mode** | Stream data from S3 to training |

#### Data Wrangler Capabilities
- Import data from S3, Redshift, Athena, EMR
- 300+ built-in transformations
- Data quality insights
- Feature engineering
- Export to Feature Store

---

### Data Merging and Integration

| Tool | Description |
|------|-------------|
| **AWS Glue** | Serverless ETL |
| **AWS Glue DataBrew** | Visual data prep |
| **Amazon EMR** | Spark/Hadoop processing |
| **AWS Glue Crawlers** | Auto-discover schema |

---

## Task 1.2: Transform Data and Feature Engineering

### Data Cleaning Techniques

| Technique | Description |
|-----------|-------------|
| **Missing Value Imputation** | Fill with mean, median, mode, or model |
| **Outlier Detection** | IQR, Z-score, isolation forest |
| **Deduplication** | Remove duplicate records |
| **Data Type Conversion** | Cast to appropriate types |
| **Normalization** | Scale to 0-1 range |
| **Standardization** | Scale to mean=0, std=1 |

---

### Feature Engineering Techniques

| Technique | Description | Example |
|-----------|-------------|---------|
| **Scaling** | Normalize numeric features | MinMax, Standard scaler |
| **Log Transform** | Handle skewed distributions | log(x+1) |
| **Binning** | Discretize continuous | Age groups |
| **Feature Splitting** | Split compound features | Date → Year/Month/Day |
| **Polynomial Features** | Create interaction terms | x1*x2, x^2 |
| **Feature Selection** | Remove irrelevant features | Correlation, importance |

---

### Encoding Techniques

| Technique | Description | Use Case |
|-----------|-------------|----------|
| **One-Hot Encoding** | Binary columns per category | Low cardinality categorical |
| **Label Encoding** | Integer mapping | Ordinal categories |
| **Binary Encoding** | Binary representation | High cardinality |
| **Target Encoding** | Mean of target per category | Tree-based models |
| **Tokenization** | Text to tokens | NLP preprocessing |
| **Embeddings** | Dense vector representation | Text, categorical |

---

### AWS Transformation Tools

| Tool | Description | Use Case |
|------|-------------|----------|
| **SageMaker Data Wrangler** | Visual transformations | Quick prototyping |
| **AWS Glue** | Spark-based ETL | Large-scale batch |
| **AWS Glue DataBrew** | No-code data prep | Business analysts |
| **Amazon EMR** | Spark clusters | Complex processing |
| **AWS Lambda** | Serverless transform | Streaming transforms |

#### AWS Glue Key Features
- Serverless Spark environment
- Data Catalog for metadata
- Job bookmarks for incremental processing
- Crawlers for schema discovery
- DataBrew for visual preparation

---

### SageMaker Feature Store

| Component | Description |
|-----------|-------------|
| **Feature Groups** | Collection of features |
| **Online Store** | Low-latency lookups |
| **Offline Store** | S3 for batch/training |
| **Data Ingestion** | Batch or streaming |
| **Feature Versioning** | Track feature changes |

> **Exam Tip:** Use Online Store for real-time inference, Offline Store for training!

---

## Task 1.3: Ensure Data Integrity and Prepare for Modeling

### Pre-training Bias Metrics

| Metric | Description |
|--------|-------------|
| **Class Imbalance (CI)** | Proportion difference in classes |
| **Difference in Proportions of Labels (DPL)** | Label distribution across groups |
| **KL Divergence** | Distribution difference measure |
| **Total Variation Distance** | Statistical distance |

---

### Addressing Class Imbalance

| Strategy | Description |
|----------|-------------|
| **Oversampling** | Duplicate minority class (SMOTE) |
| **Undersampling** | Reduce majority class |
| **Synthetic Data** | Generate new samples |
| **Class Weights** | Adjust loss function weights |
| **Stratified Sampling** | Maintain class proportions in splits |

---

### Data Labeling Services

| Service | Description |
|---------|-------------|
| **SageMaker Ground Truth** | Managed labeling workflows |
| **Amazon Mechanical Turk** | Human labeling marketplace |
| **Ground Truth Plus** | Fully managed labeling |
| **Amazon A2I** | Human review workflows |

#### Ground Truth Features
- Built-in labeling workflows (image, text, video)
- Auto-labeling with active learning
- Annotation consolidation
- Quality control mechanisms

---

### SageMaker Clarify for Bias

| Feature | Description |
|---------|-------------|
| **Pre-training Bias** | Detect bias in training data |
| **Post-training Bias** | Detect bias in predictions |
| **Explainability** | SHAP values for feature importance |
| **Model Cards** | Document model behavior |

---

### Data Quality Validation

| Tool | Description |
|------|-------------|
| **AWS Glue Data Quality** | Rule-based validation |
| **SageMaker Data Wrangler** | Data quality reports |
| **AWS Glue DataBrew** | Profile and quality checks |

#### Common Quality Checks
- Completeness (missing values)
- Uniqueness (duplicates)
- Validity (data types, ranges)
- Consistency (cross-field rules)
- Timeliness (data freshness)

---

### Data Security and Compliance

| Requirement | Solution |
|-------------|----------|
| **Encryption at Rest** | S3 SSE, KMS |
| **Encryption in Transit** | TLS/SSL |
| **PII Detection** | Amazon Macie |
| **Data Masking** | Glue DataBrew |
| **Access Control** | IAM, Lake Formation |
| **Data Residency** | Regional storage |

---

## Exam Tips for Domain 1

1. **Data format selection:**
   - Parquet = preferred for ML (columnar, compressed)
   - RecordIO = SageMaker native, most efficient
   - CSV = simple but inefficient for large data
2. **Feature Store usage:**
   - Online Store = real-time inference (low latency)
   - Offline Store = training data (S3-backed)
   - Feature Groups = logical grouping
3. **SageMaker Ground Truth:**
   - Auto-labeling = reduces cost 70%
   - Active learning = smart sample selection
   - A2I = human review workflows
4. **SageMaker Clarify:**
   - Pre-training bias = data distribution issues
   - Post-training bias = prediction fairness
   - SHAP values = feature importance
5. **AWS Glue capabilities:**
   - ETL jobs = serverless Spark
   - Crawlers = auto-discover schemas
   - Data Catalog = metadata repository
   - Job bookmarks = incremental processing
6. **Data Wrangler usage:**
   - 300+ built-in transformations
   - Visual exploration
   - Export to Feature Store or Pipelines
7. **Addressing imbalanced data:**
   - SMOTE = synthetic oversampling
   - Class weights = loss function adjustment
   - Stratified sampling = maintain proportions
8. **Streaming data:**
   - Kinesis Data Streams = real-time, custom processing
   - Kinesis Firehose = auto-load to S3/Redshift
   - MSK = high-throughput Kafka
9. **Data quality:**
   - AWS Glue Data Quality = rule-based validation
   - Completeness, uniqueness, validity checks
10. **SageMaker Processing:**
    - Data preprocessing at scale
    - Can use Spark or Scikit-learn containers
    - Pipe Mode = stream data, don't download

