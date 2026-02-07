# Domain 4: Operational Efficiency and Optimization (12%)

## Task 4.1: Cost Optimization

### Token Cost Management

| Strategy | Description | Impact |
|----------|-------------|--------|
| **Prompt Optimization** | Shorter prompts | Lower input cost |
| **Response Limits** | Set max_tokens | Cap output cost |
| **Model Selection** | Right-size model | Balance cost/quality |
| **Caching** | Cache responses | Reduce API calls |
| **Batch Inference** | Group requests | 50% cost savings |

---

### Bedrock Pricing Models

| Model | Description | Best For |
|-------|-------------|----------|
| **On-Demand** | Pay per token | Variable workloads |
| **Provisioned Throughput** | Reserved capacity | Consistent traffic |
| **Batch Inference** | Async processing | Non-urgent tasks |

#### Cost Calculation
```
Total Cost = (Input Tokens × Input Price) + (Output Tokens × Output Price)

Example (Claude 3 Sonnet):
- Input: 1000 tokens × $0.003/1K = $0.003
- Output: 500 tokens × $0.015/1K = $0.0075
- Total: $0.0105 per request
```

> **Exam Tip:** Output tokens typically cost 3-5x more than input tokens!

---

### Model Efficiency Comparison

| Model | Speed | Cost | Quality |
|-------|-------|------|---------|
| **Claude 3 Haiku** | Fast | Low | Good |
| **Claude 3 Sonnet** | Medium | Medium | Better |
| **Claude 3 Opus** | Slow | High | Best |
| **Titan Text Lite** | Fast | Low | Basic |
| **Llama 3 8B** | Fast | Low | Good |

---

### Caching Strategies

| Cache Type | Description | Benefit |
|------------|-------------|---------|
| **Response Cache** | Store full responses | Cost reduction |
| **Semantic Cache** | Similar query matching | Fewer API calls |
| **Embedding Cache** | Store vectors | Compute savings |
| **Context Cache** | Reuse conversation context | Token savings |

#### Prompt Caching ⭐
| Feature | Description |
|---------|-------------|
| **Static Prefix** | Cache system prompts |
| **Repetitive Context** | Cache common prefixes |
| **Deterministic Requests** | Hash-based caching |
| **Result Fingerprinting** | Match similar outputs |

> **Exam Tip:** Prompt caching significantly reduces costs for repeated system prompts!

---

### Token Efficiency Optimization ⭐

| Technique | Description | Impact |
|-----------|-------------|--------|
| **Prompt Compression** | Shorten prompts without losing meaning | Lower input cost |
| **Context Pruning** | Remove irrelevant context | Token savings |
| **Response Limiting** | Set max_tokens appropriately | Cap output cost |
| **Context Window Optimization** | Fit within limits efficiently | Better performance |

---

## Task 4.2: Performance Optimization

### Latency Optimization

| Technique | Description | Impact |
|-----------|-------------|--------|
| **Streaming** | Token-by-token response | Better UX |
| **Model Selection** | Smaller, faster models | Lower latency |
| **Regional Deployment** | Closer to users | Network latency |
| **Provisioned Throughput** | Reserved capacity | Consistent latency |
| **Parallel Processing** | Concurrent requests | Throughput |

---

### RAG Performance

| Optimization | Description |
|--------------|-------------|
| **Chunk Size** | Balance context vs precision |
| **Top-K Retrieval** | Limit retrieved documents |
| **Hybrid Search** | Combine vector + keyword |
| **Re-ranking** | Improve retrieval quality |
| **Embedding Selection** | Match use case |

---

### Throughput Scaling

| Approach | Description |
|----------|-------------|
| **Provisioned Throughput** | Guaranteed capacity |
| **Request Queuing** | SQS for batch processing |
| **Load Balancing** | Distribute across regions |
| **Auto-scaling** | Lambda concurrency |

---

## Task 4.3: Monitoring and Observability

### CloudWatch Metrics

| Metric | Description |
|--------|-------------|
| **Invocation Count** | Number of API calls |
| **Input Token Count** | Tokens sent to model |
| **Output Token Count** | Tokens generated |
| **Latency** | Response time |
| **Error Rate** | Failed requests |
| **Throttling** | Rate limit hits |

---

### Custom Metrics to Track

| Metric | Purpose |
|--------|---------|
| **Cost per Request** | Financial tracking |
| **Tokens per Session** | Usage patterns |
| **Response Quality** | User feedback |
| **Cache Hit Rate** | Caching effectiveness |
| **Retrieval Relevance** | RAG quality |

---

### Dashboard Components

| Component | Metrics |
|-----------|---------|
| **Usage** | Requests, tokens, cost |
| **Performance** | Latency, throughput |
| **Quality** | Errors, user feedback |
| **Cost** | Spend per model, per user |

---

### Alerting

| Alert | Condition |
|-------|-----------|
| **Cost Spike** | Daily spend > threshold |
| **Latency** | P99 > SLA |
| **Error Rate** | Errors > 5% |
| **Throttling** | Any throttled requests |

---

### GenAI-Specific Monitoring ⭐

| Metric | Purpose |
|--------|---------|
| **Hallucination Rate** | Track factual accuracy |
| **Semantic Drift** | Monitor response consistency |
| **Prompt Effectiveness** | Measure output quality |
| **Response Drift** | Detect behavior changes |

#### Advanced Observability
| Component | Tool | Tracking |
|-----------|------|----------|
| **Token Burst Patterns** | CloudWatch Anomaly Detection | Usage spikes |
| **Model Invocation Logs** | Bedrock Logs | Request/response analysis |
| **Tool Calling** | Agent Tracing | Multi-agent coordination |
| **Vector Store Health** | OpenSearch Metrics | Index performance |

```python
# Semantic Drift Detection
def detect_drift(baseline_responses, current_responses):
    from scipy.spatial.distance import cosine
    # Compare embedding similarity over time
    drift_score = cosine(baseline_embeddings, current_embeddings)
    if drift_score > THRESHOLD:
        cloudwatch.put_metric_data(MetricName='SemanticDrift', Value=drift_score)
        trigger_alert("Response drift detected")
```

> **Exam Tip:** Monitor hallucination rate and semantic drift - these are GenAI-specific failure modes!

---

## Task 4.4: Infrastructure Architecture

### Serverless GenAI Architecture

```
API Gateway → Lambda → Bedrock
                ↓
         DynamoDB (sessions)
                ↓
         S3 (documents) → Knowledge Bases
```

---

### Scaling Considerations

| Component | Scaling Strategy |
|-----------|-----------------|
| **Lambda** | Concurrent executions |
| **API Gateway** | Throttling, caching |
| **Bedrock** | Provisioned throughput |
| **Vector Store** | OpenSearch sizing |
| **Cache** | ElastiCache nodes |

---

## Exam Tips for Domain 4

1. **Cost optimization:**
   - Shorter prompts = lower cost
   - Output costs more than input
   - Batch inference = 50% savings
2. **Model selection:**
   - Haiku = fast, cheap
   - Sonnet = balanced
   - Opus = best quality
3. **Caching importance:**
   - Semantic caching for similar queries
   - Response caching for common requests
   - Embedding caching for RAG
4. **Latency reduction:**
   - Streaming for better UX
   - Smaller models, faster response
   - Regional deployment
5. **RAG performance:**
   - Optimize chunk size
   - Use top-K limiting
   - Re-ranking improves quality
6. **CloudWatch monitoring:**
   - Token counts for cost
   - Latency for performance
   - Error rates for reliability
7. **Provisioned throughput:**
   - Consistent workloads
   - Predictable latency
   - Reserved pricing
8. **Dashboard essentials:**
   - Usage, performance, cost
   - Real-time visibility
   - Trend analysis
9. **Alerting strategy:**
   - Cost thresholds
   - Latency SLAs
   - Error rate limits
10. **Serverless architecture:**
    - Lambda + API Gateway
    - Auto-scaling
    - Pay-per-use
