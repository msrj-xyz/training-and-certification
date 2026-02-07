# AWS Generative AI Developer Professional (AIP-C01) - Quick Reference Card

## Exam at a Glance
| | |
|---|---|
| **Duration** | 180 min | **Questions** | 75 (65 scored) |
| **Passing** | 750/1000 | **Level** | Professional ⚠️ |

## Domain Weights
```
Domain 1: FM Integration & Data      ██████████████████████   31%
Domain 2: Implementation             ██████████████████       26%
Domain 3: AI Safety & Security       ██████████████           20%
Domain 4: Optimization               █████████                12%
Domain 5: Testing & Troubleshooting  ████████                 11%
```

---

## 🔥 Core Bedrock Features

| Feature | Purpose |
|---------|---------|
| **Knowledge Bases** ⭐ | Managed RAG |
| **Agents** ⭐ | Autonomous workflows |
| **Guardrails** ⭐ | Safety controls |
| **Model Evaluation** | Compare FMs |
| **Prompt Management** | Version prompts |
| **Batch Inference** | Cost optimization |

---

## 📚 RAG Architecture

```
Query → Embedding → Vector Search → Context → FM → Response
```

| Component | AWS Service |
|-----------|-------------|
| **Embeddings** | Titan Embeddings |
| **Vector Store** | OpenSearch, pgvector |
| **Context** | Knowledge Bases |
| **Generation** | Claude, Titan, Llama |

---

## 🛡️ Guardrails Components

| Filter | Purpose |
|--------|---------|
| **Content Filters** | Block harm (hate, violence) |
| **Denied Topics** | Block specific subjects |
| **PII Handling** | Detect/mask/block |
| **Contextual Grounding** | Reduce hallucinations |

---

## 💰 Cost Optimization

| Strategy | Impact |
|----------|--------|
| Shorter prompts | Lower input cost |
| Smaller models (Haiku) | Lower token cost |
| Batch inference | 50% savings |
| Caching | Fewer API calls |

---

## 🔧 Prompt Techniques

| Technique | When |
|-----------|------|
| **Zero-shot** | Simple tasks |
| **Few-shot** | Pattern learning |
| **Chain-of-Thought** | Complex reasoning |
| **ReAct** | Agentic tasks |

---

## ✅ Exam Day Reminders

1. **Knowledge Bases** = managed RAG with vector stores
2. **Agents** = action groups + Lambda + instructions
3. **Strands Agents** = multi-agent coordination with MCP
4. **MCP Protocol** = standardized tool access for FMs
5. **Prompt Flows** = visual workflow builder for prompt chaining
6. **Guardrails** = input AND output filtering
7. **Claude 3 models** = Haiku (fast) → Sonnet → Opus (best)
8. **LoRA/Adapters** = parameter-efficient fine-tuning
9. **Hybrid Search** = vector + keyword for better retrieval
10. **Reranker** = improve retrieval precision
11. **Cross-Region Inference** = model availability
12. **Amazon Q** = Developer (code) + Business (knowledge)
