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

#### Tokenization Methods

| Method | Description | Example |
|--------|-------------|---------|
| **Byte Pair Encoding (BPE)** | Merge frequent character pairs | Most LLMs (GPT, Claude) |
| **WordPiece** | Similar to BPE, used by BERT | BERT, DistilBERT |
| **SentencePiece** | Language-agnostic tokenization | Multilingual models |

#### Token Calculation Examples

| Text | Approximate Tokens |
|------|--------------------|
| "Hello world" | 2 tokens |
| "Artificial intelligence" | 3-4 tokens |
| 1 page of text (~500 words) | ~375 tokens |
| 1000 words | ~750 tokens |

> **Exam Tip:** Understand that token-based pricing affects cost management decisions.

---

### Transformer Architecture

| Component | Description | Purpose |
|-----------|-------------|---------|
| **Self-Attention** | Weighs importance of each token | Understanding relationships |
| **Multi-Head Attention** | Multiple attention in parallel | Capture different patterns |
| **Positional Encoding** | Adds position information | Sequence understanding |
| **Feed-Forward Networks** | Process attention outputs | Feature transformation |
| **Layer Normalization** | Stabilizes training | Better convergence |
| **Encoder** | Processes input sequence | Understanding context |
| **Decoder** | Generates output sequence | Text generation |

#### Attention Mechanism

```
Query × Key → Attention Weights → Weights × Value → Output
```

> **Exam Tip:** Know that "Attention is All You Need" paper introduced Transformers - foundation of modern LLMs!

---

### Embeddings Deep Dive

| Concept | Description | Use Case |
|---------|-------------|----------|
| **Word Embeddings** | Words as vectors | NLP tasks |
| **Sentence Embeddings** | Entire sentences as vectors | Semantic search |
| **Document Embeddings** | Full documents as vectors | Document retrieval |
| **Image Embeddings** | Images as vectors | Visual search |
| **Multimodal Embeddings** | Combined text+image vectors | Cross-modal search |

#### Why Embeddings Matter

| Property | Benefit |
|----------|--------|
| **Semantic Similarity** | Similar meanings = close vectors |
| **Dimensionality** | Fixed-size representation |
| **Computability** | Math operations on meaning |
| **Transfer Learning** | Reuse across tasks |

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
        ↑                                                                              |
        └──────────────────────────────────────────────────────────────────────────────┘
```

| Stage | Description | AWS Service |
|-------|-------------|-------------|
| **Data Selection** | Choosing training data quality and diversity | S3, Glue |
| **Model Selection** | Picking base model architecture | Bedrock, SageMaker |
| **Pre-training** | Initial training on large datasets | SageMaker Training |
| **Fine-tuning** | Customizing for specific tasks/domains | Bedrock Custom Models |
| **Evaluation** | Testing performance and safety | Bedrock Model Evaluation |
| **Deployment** | Making model available for use | Bedrock, SageMaker Endpoints |
| **Feedback** | Collecting user feedback for improvement | CloudWatch, A2I |

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

1. **Token fundamentals:**
   - 1 token ≈ 4 characters or ¾ word
   - Token-based pricing affects cost calculations
   - Longer prompts = more cost
2. **Know hallucination mitigation:** RAG is the primary answer for grounding responses
3. **Transformer architecture:** Know attention mechanism is the key innovation
4. **Temperature settings:**
   - Low (0-0.3) = factual, deterministic
   - High (0.8-1.0) = creative, varied
5. **Amazon Bedrock features:**
   - Knowledge Bases = RAG implementation
   - Agents = multi-step automation
   - Guardrails = safety/content filtering
6. **FM Provider matching:**
   - Anthropic Claude = reasoning, safety
   - Amazon Titan = embeddings, general purpose
   - Stability AI = image generation
7. **Pricing models:**
   - On-demand = experimentation, variable load
   - Provisioned = production, consistent load
8. **Embeddings purpose:**
   - Convert text to vectors
   - Enable semantic search
   - Power RAG retrieval
9. **Business metrics matter:** GenAI is about business value, not just technical metrics
10. **Context window limits:** Understand token limits affect what you can process

