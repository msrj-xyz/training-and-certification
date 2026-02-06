# Domain 3: Applications of Foundation Models (28%)

> **This is the highest-weighted domain!** Focus extra study time here.

## Task 3.1: Design Considerations for FM Applications

### Pre-trained Model Selection Criteria

| Criterion | Description | Trade-offs |
|-----------|-------------|------------|
| **Cost** | Token pricing, infrastructure costs | Lower cost may mean lower capability |
| **Modality** | Text, image, audio, multimodal | Match to your use case |
| **Latency** | Response time | Lower latency = higher cost |
| **Multi-lingual** | Language support | More languages = larger model |
| **Model Size** | Parameter count | Larger = more capable but costlier |
| **Model Complexity** | Architecture sophistication | Complex = better reasoning |
| **Customization** | Fine-tuning options | More options = flexibility |
| **Input/Output Length** | Token limits | Longer context = higher cost |
| **Prompt Caching** | Reuse common prompts | Reduces latency and cost |

---

### Inference Parameters

| Parameter | Effect | Typical Range |
|-----------|--------|---------------|
| **Temperature** | Controls randomness/creativity | 0.0 (deterministic) to 1.0 (creative) |
| **Top-p (Nucleus Sampling)** | Limits token selection probability | 0.0 to 1.0 |
| **Top-k** | Limits to top k tokens | 1 to 100+ |
| **Max Tokens** | Maximum output length | Varies by model |
| **Stop Sequences** | Defines when to stop generating | Custom strings |

### Temperature Guidelines

| Temperature | Use Case | Example |
|-------------|----------|---------|
| **0.0 - 0.3** | Factual, consistent responses | FAQ answers, code generation |
| **0.4 - 0.7** | Balanced creativity and accuracy | General conversation |
| **0.8 - 1.0** | Creative, varied responses | Creative writing, brainstorming |

> **Exam Tip:** Lower temperature = more deterministic, higher = more creative/random

---

### Retrieval Augmented Generation (RAG)

#### What is RAG?
RAG combines foundation models with external knowledge retrieval to provide accurate, contextual responses grounded in specific data.

#### RAG Architecture Flow
```
User Query → Embedding Generation → Vector Search → Context Retrieval → 
FM + Context → Grounded Response
```

#### RAG Benefits

| Benefit | Description |
|---------|-------------|
| **Reduces Hallucinations** | Responses grounded in real data |
| **Current Information** | Access to up-to-date content |
| **Domain Specific** | Answers from your data |
| **No Fine-tuning Required** | Uses existing FM capabilities |
| **Source Attribution** | Can cite sources |

#### AWS RAG Implementation

| Component | AWS Service |
|-----------|-------------|
| **Data Storage** | Amazon S3 |
| **RAG Platform** | Amazon Bedrock Knowledge Bases |
| **Embedding Model** | Amazon Titan Embeddings, Cohere Embed |
| **Vector Database** | OpenSearch Service, Aurora, RDS PostgreSQL, Neptune |
| **Foundation Model** | Amazon Bedrock FMs |

---

### Vector Databases on AWS

| Service | Type | Best For |
|---------|------|----------|
| **Amazon OpenSearch Service** | Search engine with vector search | Full-text + semantic search |
| **Amazon Aurora** | PostgreSQL with pgvector | Existing PostgreSQL users |
| **Amazon RDS for PostgreSQL** | PostgreSQL with pgvector | Standard vector storage |
| **Amazon Neptune** | Graph database | Knowledge graphs + vectors |
| **Amazon MemoryDB** | In-memory with vector search | Low-latency applications |

---

### FM Customization Approaches

| Approach | Description | Cost | Effort | Use When |
|----------|-------------|------|--------|----------|
| **In-context Learning** | Provide examples in prompt | Lowest | Minimal | Quick start, few examples |
| **RAG** | Add external knowledge | Low | Medium | Need current/specific data |
| **Fine-tuning** | Train on your data | Medium-High | High | Custom behavior needed |
| **Continued Pre-training** | Extend base training | Highest | Highest | Domain-specific language |
| **Pre-training from Scratch** | Build new model | Very High | Very High | Unique requirements |

### When to Use Each Approach

| Scenario | Recommended Approach |
|----------|---------------------|
| Need up-to-date information | RAG |
| Specific output format/style | Fine-tuning |
| Domain-specific terminology | Continued pre-training |
| Quick experimentation | In-context learning (few-shot) |
| Proprietary data queries | RAG with Knowledge Bases |

---

### Agents and Multi-Step Tasks

#### What are Agents?
Agents are AI systems that can autonomously perform multi-step tasks by breaking down complex goals, using tools, and taking actions.

| Concept | Description |
|---------|-------------|
| **Amazon Bedrock Agents** | Fully managed agent capabilities |
| **Agentic AI** | AI that can plan and execute autonomously |
| **Model Context Protocol** | Standardizing how agents access data and tools |
| **Tool Use** | Agents calling external APIs/functions |
| **Orchestration** | Managing multi-step workflows |

#### Agent Capabilities

| Capability | Description |
|------------|-------------|
| **Task Decomposition** | Breaking complex tasks into steps |
| **Tool Integration** | Calling APIs, databases, functions |
| **Memory** | Maintaining context across interactions |
| **Reasoning** | Making decisions based on information |
| **Action Taking** | Executing steps to complete tasks |

---

## Task 3.2: Prompt Engineering Techniques

### Prompt Components

| Component | Description | Example |
|-----------|-------------|---------|
| **Context** | Background information | "You are a helpful customer service agent..." |
| **Instruction** | What you want the model to do | "Summarize the following text..." |
| **Input Data** | The content to process | [Article text] |
| **Output Indicator** | Expected format | "Provide your answer as bullet points" |
| **Negative Prompts** | What to avoid | "Do not include personal opinions" |

---

### Prompt Engineering Techniques

| Technique | Description | When to Use |
|-----------|-------------|-------------|
| **Zero-shot** | No examples provided | Simple tasks, capable models |
| **Single-shot (One-shot)** | One example provided | Showing format/style |
| **Few-shot** | Multiple examples provided | Complex patterns, specific format |
| **Chain-of-Thought (CoT)** | Step-by-step reasoning | Math, logic, complex reasoning |
| **Prompt Templates** | Reusable prompt structures | Consistent applications |
| **Prompt Routing** | Directing to appropriate models/prompts | Multi-model systems |

### Technique Examples (Conceptual)

| Technique | Structure |
|-----------|-----------|
| **Zero-shot** | [Instruction] + [Input] |
| **Few-shot** | [Examples] + [Instruction] + [Input] |
| **Chain-of-Thought** | [Instruction: Think step by step] + [Input] |

---

### Best Practices for Prompt Engineering

| Practice | Description |
|----------|-------------|
| **Be Specific** | Clear, detailed instructions |
| **Be Concise** | Remove unnecessary words |
| **Provide Context** | Relevant background information |
| **Use Delimiters** | Separate sections clearly |
| **Specify Format** | Describe desired output structure |
| **Iterate and Experiment** | Test and refine prompts |
| **Use Guardrails** | Add safety constraints |
| **Multiple Approaches** | Try different phrasings |

---

### Prompt Engineering Risks

| Risk | Description | Mitigation |
|------|-------------|------------|
| **Prompt Injection** | Malicious instructions in user input | Input validation, guardrails |
| **Prompt Exposure** | System prompts being revealed | Separate sensitive instructions |
| **Prompt Poisoning** | Corrupted training data via prompts | Data validation |
| **Jailbreaking** | Bypassing safety controls | Robust guardrails, monitoring |
| **Prompt Hijacking** | Redirecting model to unintended tasks | Clear context, guardrails |

---

## Task 3.3: Training and Fine-tuning FMs

### Key Training Elements

| Element | Description |
|---------|-------------|
| **Pre-training** | Initial training on large general datasets |
| **Fine-tuning** | Customizing pre-trained model for specific tasks |
| **Continued Pre-training** | Extending knowledge with domain data |
| **Distillation** | Creating smaller model from larger one |

---

### Fine-tuning Methods

| Method | Description | Use Case |
|--------|-------------|----------|
| **Instruction Tuning** | Training on instruction-response pairs | Following user commands |
| **Domain Adaptation** | Training on domain-specific data | Specialized vocabulary/knowledge |
| **Transfer Learning** | Applying learned knowledge to new tasks | Similar task applications |
| **RLHF** | Learning from human feedback | Preference alignment |

---

### Data Preparation for Fine-tuning

| Consideration | Description |
|---------------|-------------|
| **Data Curation** | Selecting high-quality, relevant examples |
| **Governance** | Ensuring compliance and data rights |
| **Size** | Sufficient examples (typically hundreds to thousands) |
| **Labeling** | Proper annotation of training examples |
| **Representativeness** | Data reflects real-world distribution |
| **Diversity** | Varied examples to avoid bias |

---

## Task 3.4: Evaluating FM Performance

### Evaluation Approaches

| Approach | Description | Best For |
|----------|-------------|----------|
| **Human Evaluation** | Human judges assess outputs | Quality, nuance, safety |
| **Benchmark Datasets** | Standard test sets | Comparing models |
| **Amazon Bedrock Model Evaluation** | AWS automated evaluation | Quick model comparison |
| **A/B Testing** | Compare models in production | Real-world performance |

---

### Evaluation Metrics

| Metric | Full Name | Use For |
|--------|-----------|---------|
| **ROUGE** | Recall-Oriented Understudy for Gisting Evaluation | Summarization |
| **BLEU** | Bilingual Evaluation Understudy | Translation |
| **BERTScore** | BERT-based semantic similarity | Text generation quality |
| **Perplexity** | Model confidence measure | Language model quality |
| **F1 Score** | Precision-recall balance | Classification tasks |

### Metric Quick Reference

| Task | Primary Metric |
|------|----------------|
| Text Summarization | ROUGE |
| Machine Translation | BLEU |
| Text Generation | BERTScore, Perplexity |
| Q&A | F1, Exact Match |

---

### Business Objective Alignment

| Question to Ask | What It Measures |
|-----------------|------------------|
| Does it improve productivity? | Efficiency gains |
| Does it increase user engagement? | User satisfaction |
| Does it meet task requirements? | Task engineering success |
| Does it reduce costs? | ROI measurement |
| Do users prefer it? | User testing results |

---

### Evaluating RAG, Agents, and Workflows

| Component | Evaluation Focus |
|-----------|------------------|
| **RAG Applications** | Retrieval accuracy, answer groundedness, citation quality |
| **Agents** | Task completion rate, step accuracy, tool use appropriateness |
| **Workflows** | End-to-end success, latency, error handling |

---

## Exam Tips for Domain 3

1. **RAG is critical:** Understand the flow: embed → search → retrieve → generate
2. **Know your temperatures:** Low = deterministic, High = creative
3. **Memorize prompt techniques:** Zero-shot, few-shot, chain-of-thought
4. **Understand customization trade-offs:** Cost vs. control vs. effort
5. **Know evaluation metrics:** ROUGE for summarization, BLEU for translation
6. **Agents are emerging:** Understand multi-step task orchestration
7. **Prompt security matters:** Know the risks (injection, jailbreaking)
8. **Vector databases:** Know which AWS services support vector storage
