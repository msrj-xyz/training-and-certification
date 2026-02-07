# Domain 2: Implementation and Integration (26%)

## Task 2.1: Prompt Engineering ⭐

### Prompt Components

| Component | Description | Example |
|-----------|-------------|---------|
| **System Prompt** | Role, behavior, constraints | "You are a helpful assistant..." |
| **User Prompt** | User's request | "Explain quantum computing" |
| **Context** | Retrieved or provided info | RAG documents, examples |
| **Instructions** | Task specification | "Summarize in 3 bullets" |
| **Output Format** | Expected structure | JSON, markdown, list |

---

### Prompt Engineering Techniques

| Technique | Description | Use Case |
|-----------|-------------|----------|
| **Zero-shot** | No examples | Simple tasks |
| **Few-shot** | Include examples | Complex patterns |
| **Chain-of-Thought** | Step-by-step reasoning | Complex reasoning |
| **ReAct** | Reasoning + Actions | Agentic tasks |
| **Self-Consistency** | Multiple reasoning paths | Accuracy |
| **Constitutional AI** | Self-critique | Safety |

#### Few-Shot Example
```
Classify the sentiment:
Text: "I love this product!" → Positive
Text: "Terrible experience" → Negative
Text: "It's okay, nothing special" → Neutral
Text: "Best purchase ever!" → ?
```

#### Chain-of-Thought Example
```
Q: If a store has 15 apples and sells 7, how many remain?
A: Let me think step by step:
1. Starting apples: 15
2. Apples sold: 7
3. Remaining: 15 - 7 = 8
Answer: 8 apples
```

---

### Amazon Bedrock Prompt Management

| Feature | Description |
|---------|-------------|
| **Prompt Templates** | Reusable prompt structures |
| **Variables** | Dynamic content injection |
| **Versioning** | Track prompt changes |
| **Sharing** | Cross-team access |
| **Testing** | A/B testing prompts |

> **Exam Tip:** Version your prompts - small changes can significantly impact output quality!

---

## Task 2.2: Agents ⭐

### Amazon Bedrock Agents

| Component | Description |
|-----------|-------------|
| **Instructions** | Agent behavior definition |
| **Action Groups** | Tool/API definitions |
| **Knowledge Bases** | RAG integration |
| **Lambda Functions** | Custom actions |
| **Session Attributes** | Conversation state |
| **Guardrails** | Safety controls |

---

### Agent Architecture

```
User Input → Agent → Planning → Tool Selection → Execution → Response
                ↓            ↓
         Knowledge Base   Action Groups
```

---

### Action Groups

| Component | Description |
|-----------|-------------|
| **API Schema** | OpenAPI specification |
| **Lambda Executor** | Function to call API |
| **Parameters** | Input mapping |
| **Return Schema** | Output format |

#### Action Group Example
```python
# Lambda for action group
def handler(event, context):
    action = event['actionGroup']
    api_path = event['apiPath']
    parameters = event['parameters']
    
    if api_path == '/getWeather':
        city = parameters[0]['value']
        weather = get_weather(city)
        return {
            'response': {'weather': weather}
        }
```

---

### ReAct Pattern (Reasoning + Acting)

| Step | Description |
|------|-------------|
| **Thought** | Reason about next step |
| **Action** | Select and execute tool |
| **Observation** | Process tool output |
| **Repeat** | Until task complete |

---

### Strands Agents & AWS Agent Squad ⭐

| Component | Description |
|-----------|-------------|
| **Strands Agents** | AWS native multi-agent framework |
| **AWS Agent Squad** | Multi-agent coordination |
| **Memory Management** | State across interactions |
| **Tool Orchestration** | Coordinate multiple tools |

#### Multi-Agent Patterns
| Pattern | Description |
|---------|-------------|
| **Supervisor** | Central agent coordinates others |
| **Peer-to-Peer** | Agents collaborate directly |
| **Hierarchical** | Agents with sub-agents |
| **Specialized** | Each agent handles specific domain |

---

### Model Context Protocol (MCP) ⭐

| Component | Description |
|-----------|-------------|
| **MCP Servers** | Expose tools via standard protocol |
| **MCP Clients** | Consume tools from servers |
| **Stateless** | Lambda-backed lightweight servers |
| **Stateful** | ECS-backed complex servers |

```python
# MCP Server Example (Lambda)
def mcp_handler(event, context):
    tool_name = event['tool']
    parameters = event['parameters']
    
    if tool_name == 'search_documents':
        return search_knowledge_base(parameters['query'])
```

> **Exam Tip:** MCP standardizes how FMs access external tools and resources!

---

### Amazon Bedrock Prompt Flows ⭐

| Component | Description |
|-----------|-------------|
| **Flows** | Visual workflow builder |
| **Nodes** | Processing steps |
| **Connections** | Data flow between nodes |
| **Conditions** | Branching logic |
| **Prompt Chaining** | Sequential prompts |

#### Flow Node Types
| Node | Purpose |
|------|---------|
| **Input** | Receive user input |
| **Prompt** | Call FM with template |
| **Condition** | Branch based on output |
| **Lambda** | Custom processing |
| **Knowledge Base** | RAG retrieval |
| **Output** | Return final response |

---

## Task 2.3: Application Integration

### API Integration Patterns

| Pattern | Description | Use Case |
|---------|-------------|----------|
| **Synchronous** | Request-response | Interactive chat |
| **Streaming** | Token-by-token | Real-time display |
| **Async** | Background processing | Long tasks |
| **Batch** | Multiple requests | Cost optimization |

---

### AWS SDK for Bedrock

```python
import boto3

bedrock = boto3.client('bedrock-runtime')

response = bedrock.invoke_model(
    modelId='anthropic.claude-3-sonnet',
    contentType='application/json',
    body=json.dumps({
        'anthropic_version': 'bedrock-2023-05-31',
        'messages': [{'role': 'user', 'content': 'Hello'}],
        'max_tokens': 1024
    })
)
```

---

### Streaming Responses

```python
response = bedrock.invoke_model_with_response_stream(
    modelId='anthropic.claude-3-sonnet',
    body=json.dumps({...})
)

for event in response['body']:
    chunk = json.loads(event['chunk']['bytes'])
    print(chunk['delta']['text'], end='')
```

---

### Caching Strategies

| Strategy | Description | Benefit |
|----------|-------------|---------|
| **Prompt Caching** | Cache common prompts | Latency, cost |
| **Response Caching** | Cache frequent responses | Cost |
| **Embedding Caching** | Cache vector embeddings | Compute |
| **Semantic Caching** | Similar query matching | Cost |

---

## Task 2.4: Conversation Management

### Session Management

| Pattern | Description |
|---------|-------------|
| **Stateless** | No history | Simple queries |
| **Short-term** | In-memory history | Chat sessions |
| **Long-term** | Persistent history | User personalization |
| **Summarized** | Compressed history | Long conversations |

---

### Context Window Management

| Technique | Description |
|-----------|-------------|
| **Truncation** | Remove old messages |
| **Summarization** | Summarize history |
| **Sliding Window** | Keep recent N messages |
| **Importance Scoring** | Keep relevant messages |

---

## Task 2.5: Enterprise Integration & Development Tools ⭐

### Amazon Q Services

| Service | Purpose |
|---------|---------|
| **Amazon Q Developer** | Code generation, refactoring |
| **Amazon Q Business** | Internal knowledge assistant |
| **Data Sources** | Connect enterprise knowledge |
| **Custom Apps** | Build Q-powered applications |

---

### CI/CD for GenAI ⭐

| Stage | Implementation |
|-------|----------------|
| **Prompt Testing** | Automated prompt regression |
| **Model Evaluation** | Performance benchmarks |
| **Security Scan** | Detect sensitive data in prompts |
| **Deployment** | Blue/Green with rollback |

```yaml
# GenAI CI/CD Pipeline
stages:
  - prompt_validation
  - model_evaluation
  - security_check
  - deploy_staging
  - integration_test
  - deploy_production
```

---

### Bedrock Data Automation

| Feature | Description |
|---------|-------------|
| **Document Processing** | Extract structure from documents |
| **Workflow Automation** | Automated data pipelines |
| **Integration** | Connect to business systems |
| **CRM Enhancement** | AI-powered customer insights |

---

### Enterprise Deployment Patterns ⭐

| Pattern | Service | Use Case |
|---------|---------|----------|
| **On-premises** | AWS Outposts | Data residency requirements |
| **Edge** | AWS Wavelength | Ultra-low latency |
| **Hybrid** | Direct Connect + VPC | On-prem to cloud FM access |
| **Multi-region** | Cross-Region Inference | High availability |

#### GenAI Gateway Architecture ⭐
| Component | Description |
|-----------|-------------|
| **Abstraction Layer** | Unified API across models |
| **Observability** | Centralized logging, metrics |
| **Security Controls** | Authentication, rate limiting |
| **Cost Management** | Token tracking, quotas |

```
Enterprise Apps → GenAI Gateway → Bedrock APIs
                       ↓
              Logging/Monitoring → CloudWatch
```

> **Exam Tip:** GenAI Gateway provides centralized control for enterprise FM consumption patterns!

---

### Model Deployment Strategies ⭐

| Strategy | Description | Use Case |
|----------|-------------|----------|
| **On-Demand** | Pay per token | Variable workloads |
| **Provisioned** | Reserved capacity | Consistent latency |
| **SageMaker Endpoints** | Custom models | Fine-tuned FMs |
| **Model Cascading** | Tier by complexity | Cost optimization |

#### Model Cascading Example
```
Query → Simple FM (Haiku) → If complex → Advanced FM (Sonnet)
             ↓
        Fast response for routine queries
```

---

## Exam Tips for Domain 2

1. **Prompt techniques:**
   - Zero-shot = simple tasks
   - Few-shot = pattern learning
   - Chain-of-Thought = reasoning
   - ReAct = agentic tasks
2. **Prompt management:**
   - Version all prompts
   - Use variables for dynamic content
   - A/B test for optimization
3. **Agent components:**
   - Instructions = behavior
   - Action Groups = tools (Lambda)
   - Knowledge Bases = context
4. **Action Groups:**
   - OpenAPI schema required
   - Lambda for execution
   - Map parameters correctly
5. **API patterns:**
   - Streaming for UX
   - Batch for cost
   - Async for long tasks
6. **Caching benefits:**
   - Semantic caching for similar queries
   - Embedding caching for RAG
   - Response caching for cost
7. **Context management:**
   - Summarize long conversations
   - Truncate old messages
   - Keep context relevant
8. **Streaming implementation:**
   - `invoke_model_with_response_stream`
   - Process delta chunks
   - Better user experience
9. **Multi-turn conversations:**
   - Maintain message history
   - System prompt consistency
   - Session management
10. **Error handling:**
    - Retry with backoff
    - Graceful degradation
    - Fallback responses
