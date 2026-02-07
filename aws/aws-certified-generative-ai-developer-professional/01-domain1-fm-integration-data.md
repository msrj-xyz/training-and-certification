# Domain 1: FM Integration, Data Management, and Compliance (31%)

## Task 1.1: Foundation Models

### Amazon Bedrock Foundation Models

| Provider | Models | Strengths |
|----------|--------|-----------|
| **Anthropic** | Claude 3 (Haiku, Sonnet, Opus) | Reasoning, safety, long context |
| **Amazon** | Titan (Text, Embeddings, Image) | Cost-effective, AWS native |
| **Meta** | Llama 2, Llama 3 | Open-source, customizable |
| **Mistral** | Mistral, Mixtral | Fast, efficient |
| **Cohere** | Command, Embed | Enterprise, multilingual |
| **Stability AI** | Stable Diffusion | Image generation |
| **AI21 Labs** | Jurassic | Specialized tasks |

> **Exam Tip:** Know model strengths - Claude for reasoning, Titan for cost, Llama for customization!

---

### Model Selection Criteria

| Factor | Consideration |
|--------|---------------|
| **Context Window** | Input/output token limits |
| **Latency** | Response time requirements |
| **Cost** | Token pricing differences |
| **Capabilities** | Vision, code, multilingual |
| **Accuracy** | Task-specific performance |
| **Safety** | Built-in guardrails |
| **Customization** | Fine-tuning availability |

---

### Model Inference Patterns

| Pattern | Description | Use Case |
|---------|-------------|----------|
| **On-Demand** | Pay per token | Variable workloads |
| **Provisioned Throughput** | Reserved capacity | Consistent traffic |
| **Batch Inference** | Async processing | Cost optimization |
| **Cross-Region** | Multi-region access | Availability |

---

### Cross-Region Inference & Resilience ⭐

| Pattern | Description |
|---------|-------------|
| **Cross-Region Inference** | Route to available regions automatically |
| **Circuit Breaker** | Step Functions pattern for failure handling |
| **Graceful Degradation** | Fallback to simpler models |
| **Model Failover** | Switch providers on failure |
| **Retry with Backoff** | Exponential backoff strategy |

```python
# Circuit Breaker Pattern with Step Functions
{
  "Type": "Task",
  "Resource": "arn:aws:bedrock:...",
  "Retry": [{
    "ErrorEquals": ["ThrottlingException"],
    "IntervalSeconds": 2,
    "MaxAttempts": 3,
    "BackoffRate": 2
  }],
  "Catch": [{
    "ErrorEquals": ["States.ALL"],
    "Next": "FallbackModel"
  }]
}
```

> **Exam Tip:** Cross-Region Inference is key for models with limited regional availability!

---

## Task 1.2: RAG (Retrieval Augmented Generation) ⭐

### RAG Architecture

```
Query → Embedding → Vector Search → Context Retrieval → Prompt + Context → FM → Response
```

| Component | Description |
|-----------|-------------|
| **Query** | User input |
| **Embedding Model** | Convert text to vectors |
| **Vector Store** | Store/search embeddings |
| **Retriever** | Find relevant documents |
| **Augmented Prompt** | Query + retrieved context |
| **Generator** | FM generates response |

---

### Amazon Bedrock Knowledge Bases ⭐

| Feature | Description |
|---------|-------------|
| **Data Sources** | S3, web crawlers, Confluence |
| **Chunking** | Fixed, semantic, hierarchical |
| **Embeddings** | Titan, Cohere embeddings |
| **Vector Stores** | OpenSearch, Aurora, Pinecone |
| **Managed** | Auto-sync, updates |
| **Citations** | Source attribution |

#### Chunking Strategies
| Strategy | Description | Best For |
|----------|-------------|----------|
| **Fixed-size** | Split by character/token count | Simple documents |
| **Semantic** | Split by meaning/topics | Complex documents |
| **Hierarchical** | Parent-child chunks | Multi-level retrieval |
| **Overlap** | Overlapping chunks | Preserve context |

> **Exam Tip:** Chunk size affects retrieval quality - too small loses context, too large adds noise!

---

### Vector Stores

| Store | Features | Use Case |
|-------|----------|----------|
| **OpenSearch** ⭐ | k-NN, hybrid search | Full-text + vector |
| **Aurora pgvector** | SQL + vector | Existing PostgreSQL |
| **Pinecone** | Managed, fast | High-scale vector |
| **Redis (MemoryDB)** | In-memory, fast | Low-latency search |
| **Neptune** | Graph + vector | Relationship queries |

#### Vector Search Algorithms
| Algorithm | Description |
|-----------|-------------|
| **k-NN** | Exact nearest neighbors |
| **HNSW** | Approximate, fast |
| **IVF** | Inverted file index |
| **Cosine Similarity** | Direction-based distance |
| **Euclidean** | Straight-line distance |

---

### Embedding Models

| Model | Dimensions | Use Case |
|-------|------------|----------|
| **Titan Embeddings V2** | 1024 | General purpose |
| **Titan Multimodal** | 1024 | Text + image |
| **Cohere Embed** | 1024 | Multilingual |
| **Custom** | Variable | Domain-specific |

---

### Advanced Retrieval Techniques ⭐

#### Hybrid Search (Vector + Keyword)
| Component | Description |
|-----------|-------------|
| **Vector Search** | Semantic similarity |
| **Keyword Search** | Exact term matching |
| **Score Fusion** | Combine both scores |
| **Weighted Blend** | Adjust semantic vs keyword weight |

> **Exam Tip:** Hybrid search combines best of both - semantic understanding + exact matching!

#### Reranker Models
| Feature | Description |
|---------|-------------|
| **Purpose** | Re-score retrieved documents for relevance |
| **When** | After initial retrieval, before context injection |
| **Bedrock Reranker** | Built-in reranking capability |
| **Benefit** | Improved precision, better context |

#### Query Enhancement
| Technique | Description |
|-----------|-------------|
| **Query Expansion** | Add related terms |
| **Query Decomposition** | Break into sub-queries |
| **Query Transformation** | Rephrase for better retrieval |
| **HyDE** | Hypothetical Document Embeddings |

---

## Task 1.3: Data Management

### Data Ingestion for RAG

| Source | Method |
|--------|--------|
| **S3** | Direct integration |
| **Databases** | Connectors, ETL |
| **APIs** | Lambda, AppFlow |
| **Web** | Web crawlers |
| **SaaS** | Confluence, SharePoint |

---

### Data Preparation

| Task | Description |
|------|-------------|
| **Cleaning** | Remove noise, duplicates |
| **Parsing** | Extract text from PDFs, docs |
| **Chunking** | Split into manageable pieces |
| **Metadata** | Add labels, tags |
| **Deduplication** | Remove redundant content |
| **Validation** | Quality checks |

---

### Data Validation Pipelines for FM ⭐

| Component | Description |
|-----------|-------------|
| **AWS Glue Data Quality** | Define rules with DQDL |
| **SageMaker Data Wrangler** | Visual data preparation |
| **Custom Lambda** | Custom validation logic |
| **CloudWatch Metrics** | Track data quality metrics |

#### Data Validation Workflow
```
Input Data → Validation Rules → Quality Score → Pass/Fail → FM Consumption
                                      ↓
                               CloudWatch Alerts
```

> **Exam Tip:** Glue Data Quality DQDL rules can validate completeness, uniqueness, and format before FM inference!

---

### Multimodal Data Processing ⭐

| Type | Service | Use Case |
|------|---------|----------|
| **Audio** | Amazon Transcribe | Speech-to-text for FM |
| **Image** | Bedrock Multimodal | Vision understanding |
| **Video** | Transcribe + Rekognition | Extract text, objects |
| **Tabular** | SageMaker Processing | Structured data prep |

#### Multimodal Pipeline Architecture
```
Audio/Video → Transcribe → Text → Embedding → Vector Store
     ↓
Images → Titan Multimodal → Combined Embeddings → RAG
```

---

### OpenSearch Advanced Features ⭐

| Feature | Description |
|---------|-------------|
| **Neural Plugin** | Native embedding generation |
| **k-NN Index** | HNSW algorithm for fast search |
| **Sharding Strategies** | Distribute for scale |
| **Multi-index** | Domain-specific indexes |

#### OpenSearch Sharding for Vector Search
| Strategy | Use Case |
|----------|----------|
| **Shard per domain** | Isolated search spaces |
| **Hot/Warm tiers** | Cost optimization |
| **Cross-cluster** | Multi-region search |

> **Exam Tip:** OpenSearch Neural plugin integrates directly with Bedrock for seamless RAG!

---

### Data Privacy and Compliance

| Requirement | Implementation |
|-------------|----------------|
| **PII Detection** | Macie, Comprehend |
| **Data Masking** | Guardrails, Lambda |
| **Encryption** | KMS at rest, TLS in transit |
| **Access Control** | IAM, resource policies |
| **Data Residency** | Regional storage |
| **Audit Logging** | CloudTrail |

---

## Task 1.4: Model Customization

### Fine-Tuning in Bedrock

| Aspect | Description |
|--------|-------------|
| **Continued Pre-training** | Domain adaptation |
| **Instruction Tuning** | Task-specific training |
| **Data Format** | JSONL with prompts/completions |
| **Hyperparameters** | Epochs, learning rate, batch size |
| **Evaluation** | Validation dataset |

---

### When to Fine-Tune

| Use Fine-Tuning | Use Prompt Engineering |
|-----------------|----------------------|
| Domain-specific language | General tasks |
| Consistent style/format | Flexible outputs |
| Performance critical | Quick iteration |
| Proprietary knowledge | Public knowledge |

---

### Parameter-Efficient Fine-Tuning (PEFT) ⭐

| Technique | Description |
|-----------|-------------|
| **LoRA (Low-Rank Adaptation)** | Train small adapter layers |
| **Adapters** | Insert trainable modules |
| **Prefix Tuning** | Prepend learnable tokens |
| **Full Fine-Tuning** | Train all parameters |

#### LoRA Benefits
| Benefit | Description |
|---------|-------------|
| **Lower Cost** | Train fewer parameters |
| **Faster Training** | Reduced compute |
| **Portability** | Small adapter files |
| **Composable** | Combine multiple adapters |

---

### Model Lifecycle Management

| Stage | Tool |
|-------|------|
| **Registry** | SageMaker Model Registry |
| **Versioning** | Track model versions |
| **Deployment** | Automated pipelines |
| **Rollback** | Failed deployment recovery |
| **Retire** | Deprecate old models |

---

## Exam Tips for Domain 1

1. **Model selection:**
   - Claude = best reasoning, safety
   - Titan = cost-effective, AWS native
   - Llama = customization, fine-tuning
   - Mistral = speed, efficiency
2. **RAG architecture:**
   - Embedding → Vector Store → Retrieval → Generation
   - Chunk size affects quality
   - Citations for transparency
3. **Knowledge Bases:**
   - Managed RAG solution
   - Auto-sync from S3
   - Multiple vector store options
4. **Chunking strategies:**
   - Fixed = simple, fast
   - Semantic = better quality
   - Hierarchical = complex docs
5. **Vector stores:**
   - OpenSearch = hybrid search
   - pgvector = SQL integration
   - Pinecone = managed scale
6. **Embedding selection:**
   - Match dimensions to store
   - Multimodal for images
   - Consistency across pipeline
7. **Data preparation:**
   - Clean before chunking
   - Add metadata for filtering
   - Validate quality
8. **Fine-tuning vs prompting:**
   - Fine-tune for domain-specific
   - Prompt for flexibility
9. **Compliance:**
   - PII detection essential
   - Encryption always on
   - Audit with CloudTrail
10. **Provisioned throughput:**
    - Consistent workloads
    - Cost predictability
    - Reserved capacity
