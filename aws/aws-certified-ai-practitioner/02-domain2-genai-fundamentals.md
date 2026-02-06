# Domain 2: Fundamentals of GenAI (24%)

## Task 2.1: Basic Concepts of GenAI

### Core Terminology

| Term | Definition | Exam Focus |
|------|------------|------------|
| **Tokens** | Basic units of text that models process | Word pieces, affects pricing and limits |
| **Chunking** | Breaking large text into smaller pieces | Used for document processing in RAG |
| **Embeddings** | Numerical vector representations of data | Enables semantic understanding |
| **Vectors** | Arrays of numbers representing meaning | Stored in vector databases |
| **Prompt Engineering** | Crafting inputs to get desired outputs | Critical for GenAI applications |
| **Foundation Models (FMs)** | Large pre-trained models applicable to many tasks | Basis for GenAI applications |
| **Large Language Models (LLMs)** | FMs specifically trained on text | GPT, Claude, Amazon Nova |
| **Transformer Architecture** | Neural network design enabling parallel processing | Powers modern LLMs |
| **Multimodal Models** | Models that process multiple data types | Text, images, audio, video |
| **Diffusion Models** | Models that generate images from noise | Image generation (Stable Diffusion) |

---

### How Tokens Work

| Aspect | Description |
|--------|-------------|
| **What are tokens** | Subword units (not always full words) |
| **Average ratio** | ~1 token ≈ 4 characters or ¾ of a word in English |
| **Why they matter** | Affect cost, context limits, response length |
| **Token limits** | Maximum input + output tokens per request |

> **Exam Tip:** Understand that token-based pricing affects cost management decisions.

---

### Foundation Model Types

| Type | Description | Examples |
|------|-------------|----------|
| **Text-to-Text** | Generate text from text prompts | Claude, Amazon Nova, Llama |
| **Text-to-Image** | Generate images from text descriptions | Stable Diffusion, DALL-E |
| **Text-to-Code** | Generate code from natural language | Code Llama, StarCoder |
| **Multimodal** | Process and generate multiple types | Claude 3, GPT-4V |
| **Embedding Models** | Convert content to vector representations | Amazon Titan Embeddings |

---

### GenAI Use Cases

| Use Case | Description | Business Value |
|----------|-------------|----------------|
| **Image Generation** | Create images from text | Marketing, design |
| **Video Generation** | Create video content | Entertainment, training |
| **Audio Generation** | Create music, voice | Accessibility, media |
| **Text Summarization** | Condense long content | Productivity |
| **AI Assistants** | Interactive help systems | Customer service |
| **Translation** | Cross-language conversion | Global reach |
| **Code Generation** | Write/explain code | Developer productivity |
| **Customer Service Agents** | Automated support | Cost reduction |
| **Search Enhancement** | Semantic search | Better discovery |
| **Recommendation Engines** | Personalized suggestions | Engagement |

---

### Foundation Model Lifecycle

```
Data Selection → Model Selection → Pre-training → Fine-tuning → Evaluation → Deployment → Feedback
```

| Stage | Description |
|-------|-------------|
| **Data Selection** | Choosing training data quality and diversity |
| **Model Selection** | Picking base model architecture |
| **Pre-training** | Initial training on large datasets |
| **Fine-tuning** | Customizing for specific tasks/domains |
| **Evaluation** | Testing performance and safety |
| **Deployment** | Making model available for use |
| **Feedback** | Collecting user feedback for improvement |

---

## Task 2.2: Capabilities and Limitations of GenAI

### Advantages of GenAI

| Advantage | Description | Business Impact |
|-----------|-------------|-----------------|
| **Adaptability** | Can handle diverse tasks without retraining | Multi-purpose solutions |
| **Responsiveness** | Real-time interaction capability | Better user experience |
| **Simplicity** | Natural language interface | Lower barrier to entry |
| **Creativity** | Generates novel content | Innovation enablement |
| **Scalability** | Handle many requests simultaneously | Cost efficiency at scale |

---

### Disadvantages and Limitations

| Limitation | Description | Mitigation |
|------------|-------------|------------|
| **Hallucinations** | Generating false but convincing information | Use RAG, fact-checking |
| **Interpretability** | Difficulty explaining decisions | Use explainable AI tools |
| **Inaccuracy** | May produce incorrect outputs | Human review, validation |
| **Nondeterminism** | Same input may give different outputs | Temperature, seed settings |
| **Bias** | May reflect biases in training data | Bias testing, diverse data |
| **Context Limits** | Maximum input/output token limits | Chunking, summarization |
| **Knowledge Cutoff** | Training data has end date | RAG for current info |

> **Exam Tip:** Hallucinations are one of the most frequently tested limitations!

---

### Model Selection Factors

| Factor | Consideration |
|--------|---------------|
| **Model Type** | Text, image, multimodal - match to use case |
| **Performance Requirements** | Latency, throughput needs |
| **Capabilities** | What the model can do |
| **Constraints** | Token limits, language support |
| **Compliance** | Data residency, regulations |
| **Cost** | Per-token pricing, provisioned throughput |
| **Accuracy** | Quality of outputs for your use case |

---

### Business Metrics for GenAI

| Metric | Description | When to Use |
|--------|-------------|-------------|
| **Cross-domain Performance** | How well model works across different tasks | Multi-use applications |
| **Efficiency** | Speed and resource usage | Cost optimization |
| **Conversion Rate** | User actions from AI recommendations | E-commerce, marketing |
| **ARPU (Average Revenue Per User)** | Revenue impact | Monetization strategies |
| **Accuracy** | Correctness of outputs | Quality-focused applications |
| **Customer Lifetime Value** | Long-term customer impact | Retention strategies |
| **User Engagement** | How much users interact | UX improvement |

---

## Task 2.3: AWS Infrastructure and Technologies for GenAI

### Primary AWS GenAI Services

| Service | Description | When to Use |
|---------|-------------|-------------|
| **Amazon Bedrock** | Fully managed FM access | Production GenAI applications |
| **Amazon Bedrock PartyRock** | No-code GenAI playground | Experimentation, prototyping |
| **Amazon Q Developer** | AI coding assistant | Developer productivity |
| **Amazon Q Business** | AI assistant for enterprise | Knowledge management |
| **Amazon SageMaker JumpStart** | Pre-built models and solutions | Quick ML deployment |
| **Amazon Bedrock Data Automation** | Automated data processing with AI | Document processing |

---

### Amazon Bedrock Deep Dive

| Feature | Description |
|---------|-------------|
| **Foundation Models** | Access to multiple FM providers (Anthropic, AI21, Cohere, Meta, Stability, Amazon) |
| **Knowledge Bases** | RAG implementation for grounding responses |
| **Agents** | Multi-step task automation |
| **Guardrails** | Content filtering and safety controls |
| **Custom Models** | Fine-tuning and continued pre-training |
| **Model Evaluation** | Compare and evaluate model performance |

### Amazon Bedrock FM Providers

| Provider | Models | Primary Use |
|----------|--------|-------------|
| **Amazon** | Nova, Titan | General purpose, embeddings |
| **Anthropic** | Claude | Complex reasoning, safety |
| **AI21 Labs** | Jurassic | Text generation |
| **Cohere** | Command, Embed | Text, embeddings |
| **Meta** | Llama | Open-weight models |
| **Stability AI** | Stable Diffusion | Image generation |

---

### Advantages of AWS GenAI Services

| Advantage | Description |
|-----------|-------------|
| **Accessibility** | Easy access to multiple FMs in one place |
| **Lower Barrier to Entry** | Managed infrastructure, no ML expertise needed |
| **Efficiency** | Serverless, pay-per-use model |
| **Cost-effectiveness** | No infrastructure management overhead |
| **Speed to Market** | Quick deployment capabilities |
| **Business Alignment** | Services designed for enterprise use |

---

### AWS Infrastructure Benefits

| Benefit | Description |
|---------|-------------|
| **Security** | Data encryption, VPC integration, access controls |
| **Compliance** | HIPAA, SOC, PCI-DSS certifications |
| **Responsibility** | Clear shared responsibility model |
| **Safety** | Built-in guardrails and content filtering |
| **Global Reach** | Multiple regions worldwide |
| **Scalability** | Automatic scaling for demand |

---

### Cost Tradeoffs for AWS GenAI

| Factor | Consideration |
|--------|---------------|
| **Token-based Pricing** | Pay per input/output tokens; optimize prompts |
| **Provisioned Throughput** | Reserved capacity for consistent workloads; lower per-token cost |
| **Custom Models** | Higher upfront cost for training; better fit for specific needs |
| **Responsiveness** | Faster response = higher cost |
| **Availability** | Multi-AZ deployments increase cost |
| **Redundancy** | Cross-region backup adds cost |
| **Regional Coverage** | Some models only in certain regions |

### When to Use Each Pricing Model

| Model | Best For |
|-------|----------|
| **On-demand** | Variable/unpredictable workloads, experimentation |
| **Provisioned Throughput** | Consistent, high-volume production workloads |
| **Custom Models** | Specialized domain requirements |

---

## Exam Tips for Domain 2

1. **Know token concepts:** Understanding tokens is fundamental to cost and limit calculations
2. **Memorize hallucination mitigation:** RAG is the primary answer for grounding responses
3. **Understand trade-offs:** Cost vs. latency, accuracy vs. speed
4. **Know Amazon Bedrock features:** Knowledge Bases, Agents, Guardrails are key
5. **Match services to use cases:** Bedrock for FMs, Q for assistance, SageMaker for custom ML
6. **Remember business metrics:** GenAI is about business value, not just technical metrics
