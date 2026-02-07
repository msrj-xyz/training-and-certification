# Domain 5: Testing, Validation, and Troubleshooting (11%)

## Task 5.1: Model Evaluation

### Amazon Bedrock Model Evaluation

| Evaluation Type | Description |
|-----------------|-------------|
| **Automatic** | Built-in metrics |
| **Human** | Human labelers |
| **Custom** | Your own evaluation |

---

### Automatic Evaluation Metrics

| Metric | Task | Description |
|--------|------|-------------|
| **ROUGE** | Summarization | N-gram overlap |
| **BERTScore** | General | Semantic similarity |
| **BLEU** | Translation | Precision-based |
| **F1** | Q&A | Exact match |
| **Accuracy** | Classification | Correct predictions |
| **Toxicity** | Safety | Harmful content |
| **Robustness** | All | Adversarial resistance |

---

### Evaluation Job Configuration

| Parameter | Description |
|-----------|-------------|
| **Dataset** | Test data (S3) |
| **Task Type** | Summary, Q&A, etc. |
| **Models** | FMs to compare |
| **Metrics** | Metrics to compute |
| **Output** | Results location |

---

### Human Evaluation

| Aspect | Criteria |
|--------|----------|
| **Fluency** | Natural language quality |
| **Relevance** | Answer matches question |
| **Coherence** | Logical consistency |
| **Helpfulness** | Practical value |
| **Safety** | No harmful content |

---

### LLM-as-a-Judge Evaluation ⭐

| Approach | Description |
|----------|-------------|
| **Single Judge** | One FM evaluates outputs |
| **Pairwise Comparison** | Compare two responses |
| **Reference-based** | Compare to golden answer |
| **Rubric-based** | Score against criteria |

#### LLM-as-Judge Example
```python
# LLM-as-a-Judge Pattern
evaluation_prompt = """
Rate this response on a scale of 1-5:
- Accuracy: Is the information correct?
- Helpfulness: Does it address the question?
- Clarity: Is it easy to understand?

Response: {model_response}
Question: {original_question}

Provide scores and reasoning.
"""
```

> **Exam Tip:** LLM-as-a-Judge enables scalable automated evaluation beyond traditional metrics!

---

### Amazon Bedrock Model Evaluations ⭐

| Feature | Description |
|---------|-------------|
| **Automatic Evaluation** | Built-in metrics (ROUGE, BERTScore) |
| **Human Evaluation** | Managed labeling workflows |
| **A/B Testing** | Compare model performance |
| **Canary Testing** | Gradual rollout validation |

#### Bedrock Agent Evaluation ⭐
| Metric | Purpose |
|--------|---------|
| **Task Completion Rate** | Agent successfully completes tasks |
| **Tool Selection Accuracy** | Correct tool chosen |
| **Reasoning Quality** | Logical step correctness |
| **Multi-step Success** | Complex workflow completion |

---

## Task 5.2: Testing Strategies

### Testing Levels

| Level | Description | Tools |
|-------|-------------|-------|
| **Unit** | Individual functions | pytest, Jest |
| **Integration** | API interactions | Postman, pytest |
| **End-to-End** | Full workflows | Selenium, Playwright |
| **Load** | Performance testing | Locust, k6 |
| **Security** | Vulnerability testing | OWASP, Inspector |

---

### Prompt Testing

| Test Type | Purpose |
|-----------|---------|
| **Golden Dataset** | Expected outputs |
| **Edge Cases** | Boundary conditions |
| **Adversarial** | Attack resistance |
| **Regression** | No quality degradation |
| **A/B Testing** | Compare versions |

#### Golden Dataset Example
```json
{
  "test_cases": [
    {
      "input": "What is the capital of France?",
      "expected": "Paris",
      "evaluation": "contains"
    },
    {
      "input": "Summarize: [long text]",
      "expected_keywords": ["main", "points"],
      "max_length": 100
    }
  ]
}
```

---

### RAG Testing

| Test | Description |
|------|-------------|
| **Retrieval Quality** | Relevant docs returned |
| **Chunk Coverage** | All info accessible |
| **Embedding Accuracy** | Semantic matching |
| **Response Grounding** | Answer from context |
| **Citation Accuracy** | Correct sources cited |

---

### Agent Testing

| Test | Description |
|------|-------------|
| **Tool Selection** | Correct tool chosen |
| **Parameter Passing** | Correct inputs |
| **Error Handling** | Graceful failures |
| **Multi-step** | Complex workflows |
| **Guardrail Bypass** | Security testing |

---

## Task 5.3: Troubleshooting

### Common Issues and Solutions

| Issue | Cause | Solution |
|-------|-------|----------|
| **Hallucinations** | Missing context | Better RAG, guardrails |
| **Irrelevant Responses** | Poor retrieval | Tune chunking, embeddings |
| **Slow Performance** | Model size | Use smaller model |
| **High Costs** | Too many tokens | Optimize prompts, cache |
| **Rate Limiting** | API limits | Implement backoff |
| **Token Limits** | Context too long | Truncate, summarize |

---

### Debugging RAG

| Problem | Debug Approach |
|---------|----------------|
| **Wrong docs retrieved** | Check embeddings, similarity |
| **Missing info** | Verify chunking strategy |
| **Context cutoff** | Adjust chunk size |
| **No citations** | Check Knowledge Base config |

---

### Debugging Agents

| Problem | Debug Approach |
|---------|----------------|
| **Wrong tool used** | Review instructions |
| **Tool failure** | Check Lambda logs |
| **Infinite loop** | Set iteration limits |
| **Missing context** | Verify Knowledge Base |

---

### CloudWatch Log Analysis

```
# Search for errors
fields @timestamp, @message
| filter @message like /ERROR/
| sort @timestamp desc

# Token usage analysis
fields @timestamp, inputTokenCount, outputTokenCount
| stats sum(inputTokenCount) as totalInput,
        sum(outputTokenCount) as totalOutput
  by bin(1h)

# Latency analysis
fields @timestamp, latency
| stats avg(latency), pct(latency, 99) by bin(1h)
```

---

### X-Ray for GenAI

| Trace Segment | Purpose |
|---------------|---------|
| **API Gateway** | Request entry |
| **Lambda** | Processing time |
| **Bedrock** | Model inference |
| **Vector Store** | Retrieval time |
| **DynamoDB** | Session lookup |

---

## Task 5.4: Quality Assurance

### Continuous Evaluation

| Practice | Implementation |
|----------|----------------|
| **Baseline Metrics** | Establish benchmarks |
| **Regression Testing** | Compare to baseline |
| **A/B Testing** | New vs old prompts |
| **User Feedback** | Thumbs up/down |
| **Automated Monitoring** | Quality alarms |

---

### Feedback Loop

```
Production → User Feedback → Analysis → Improvement → Testing → Production
```

---

### Context Window Troubleshooting ⭐

| Issue | Diagnostic | Solution |
|-------|------------|----------|
| **Context Overflow** | Token count exceeded | Dynamic chunking |
| **Truncation Errors** | Content cut off | Summarize history |
| **Information Loss** | Key context missing | Importance scoring |

#### Dynamic Chunking Strategy
```python
def adaptive_chunking(content, max_tokens):
    if count_tokens(content) > max_tokens:
        # Prioritize recent and relevant content
        chunks = split_by_importance(content)
        return truncate_with_summary(chunks, max_tokens)
    return content
```

---

### Embedding & Retrieval Diagnostics ⭐

| Issue | Diagnostic | Resolution |
|-------|------------|------------|
| **Embedding Drift** | Compare new vs old vectors | Re-index with new model |
| **Poor Relevance** | Check similarity scores | Tune chunking, embeddings |
| **Missing Info** | Validate chunk coverage | Adjust chunk size/overlap |
| **Vector Search Slow** | Check index performance | Optimize shards, indexes |

---

### Prompt Observability Pipeline ⭐

| Component | Tool | Purpose |
|-----------|------|---------|
| **Prompt Logging** | CloudWatch Logs | Track all prompts |
| **Response Analysis** | X-Ray | Trace FM API calls |
| **Schema Validation** | JSON Schema | Detect format errors |
| **Template Testing** | Lambda | Systematic prompt QA |

> **Exam Tip:** Build observability pipelines specific to GenAI to catch FM-unique failure modes!

---

## Exam Tips for Domain 5

1. **Model evaluation:**
   - Automatic = ROUGE, BERTScore
   - Human = fluency, relevance
   - Task-specific metrics
2. **Test datasets:**
   - Golden datasets for regression
   - Edge cases for robustness
   - Adversarial for security
3. **RAG testing:**
   - Retrieval quality
   - Response grounding
   - Citation accuracy
4. **Agent testing:**
   - Tool selection verification
   - Parameter passing
   - Error handling
5. **Common issues:**
   - Hallucinations → better context
   - Slow → smaller model
   - Expensive → optimize prompts
6. **Debugging tools:**
   - CloudWatch Logs
   - X-Ray tracing
   - Bedrock invocation logs
7. **Log analysis:**
   - Token counts for cost
   - Latency for performance
   - Error patterns
8. **Continuous evaluation:**
   - Baseline metrics
   - Regression testing
   - User feedback loop
9. **Quality metrics:**
   - Accuracy, relevance
   - Latency, cost
   - Safety scores
10. **Troubleshooting order:**
    - Check logs first
    - Verify configuration
    - Test in isolation
    - Compare to baseline
