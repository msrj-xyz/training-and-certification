# Domain 1: Fundamentals of AI and ML (20%)

## Task 1.1: Basic AI Concepts and Terminologies

### Core Definitions

| Term | Definition | Exam Focus |
|------|------------|------------|
| **Artificial Intelligence (AI)** | Broad field of computer science focused on creating systems that can perform tasks requiring human-like intelligence | Umbrella term encompassing ML and GenAI |
| **Machine Learning (ML)** | Subset of AI where systems learn from data without explicit programming | Pattern recognition from data |
| **Deep Learning** | Subset of ML using neural networks with multiple layers | Handles complex patterns, requires large data |
| **Neural Networks** | Computing systems inspired by biological brain structure | Foundation for deep learning |
| **Generative AI (GenAI)** | AI that creates new content (text, images, audio, code) | Newest evolution, uses foundation models |
| **Large Language Models (LLMs)** | AI models trained on massive text datasets | Powers chatbots, text generation |

### Hierarchy to Remember
```
AI (broadest) → ML (subset) → Deep Learning (subset) → GenAI (application of deep learning)
```

### Key Concept Comparisons

| Concept | Traditional ML | Deep Learning | GenAI |
|---------|---------------|---------------|-------|
| Data Requirements | Moderate | Large | Very Large |
| Feature Engineering | Manual | Automatic | Pre-trained |
| Model Complexity | Low-Medium | High | Very High |
| Use Case | Predictions | Complex patterns | Content creation |

---

### Data Types in AI Models

| Data Type | Description | Example Use Cases |
|-----------|-------------|-------------------|
| **Labeled Data** | Data with known outputs/tags | Supervised learning, classification |
| **Unlabeled Data** | Raw data without tags | Unsupervised learning, clustering |
| **Structured Data** | Organized in rows/columns | Databases, spreadsheets, tabular data |
| **Unstructured Data** | No predefined format | Images, text, audio, video |
| **Tabular Data** | Rows and columns format | Financial data, customer records |
| **Time-Series Data** | Sequential data over time | Stock prices, sensor readings |
| **Image Data** | Pixel-based visual data | Object detection, medical imaging |
| **Text Data** | Written language data | Sentiment analysis, chatbots |

---

### Learning Paradigms

| Type | How It Works | Use Cases | AWS Services |
|------|--------------|-----------|--------------|
| **Supervised Learning** | Learns from labeled data (input-output pairs) | Classification, regression, prediction | SageMaker, Comprehend |
| **Unsupervised Learning** | Finds patterns in unlabeled data | Clustering, anomaly detection, dimensionality reduction | SageMaker |
| **Reinforcement Learning** | Agent learns through trial and error with rewards | Robotics, game playing, autonomous systems | SageMaker |

> **Exam Tip:** Know which learning type applies to which business problem!

---

### Inference Types

| Type | Description | Latency | Use Case | Cost |
|------|-------------|---------|----------|------|
| **Real-time Inference** | Immediate predictions | Milliseconds | Fraud detection, chatbots | Higher |
| **Batch Inference** | Predictions on bulk data | Hours | Overnight processing, reports | Lower |

---

### Model Concepts

| Term | Definition | Exam Focus |
|------|------------|------------|
| **Training** | Process of teaching model from data | Compute-intensive, happens once/periodically |
| **Inferencing** | Using trained model to make predictions | Production use, needs low latency |
| **Model** | Mathematical representation learned from data | Output of training process |
| **Algorithm** | Method/procedure used for training | Decision trees, neural networks, etc. |
| **Bias** | Systematic error in predictions | Can affect fairness, needs monitoring |
| **Fairness** | Equal treatment across groups | Responsible AI consideration |
| **Overfitting** | Model too specific to training data | Poor generalization |
| **Underfitting** | Model too simple for data | Poor performance overall |

---

## Task 1.2: Practical Use Cases for AI

### When AI/ML Provides Value

| Scenario | Why AI/ML Helps |
|----------|-----------------|
| Large-scale data analysis | Humans cannot process volume manually |
| Pattern recognition | Complex patterns in data |
| Repetitive decision-making | Automation with consistency |
| Predictions under uncertainty | Statistical inference |
| 24/7 availability needed | Tireless automated responses |
| Personalization at scale | Individual experiences for millions |

### When AI/ML is NOT Appropriate

| Scenario | Why Not |
|----------|---------|
| Deterministic outcomes needed | AI outputs are probabilistic |
| Insufficient data available | ML requires data to learn |
| Cost exceeds benefit | ROI calculation doesn't justify |
| Explainability is mandatory | Black-box models may not work |
| Edge cases are critical | AI may fail on rare scenarios |
| Simple rule-based logic works | Overkill for simple problems |

---

### ML Technique Selection Guide

| Technique | Use When | Examples |
|-----------|----------|----------|
| **Regression** | Predicting continuous values | Price prediction, demand forecasting |
| **Classification** | Categorizing into discrete classes | Spam detection, sentiment analysis |
| **Clustering** | Grouping similar items | Customer segmentation, anomaly detection |

---

### Real-World AI Applications

| Application | Description | AWS Service |
|-------------|-------------|-------------|
| **Computer Vision** | Analyzing images/videos | Amazon Rekognition |
| **NLP (Natural Language Processing)** | Understanding text | Amazon Comprehend |
| **Speech Recognition** | Converting speech to text | Amazon Transcribe |
| **Text-to-Speech** | Converting text to audio | Amazon Polly |
| **Conversational AI** | Building chatbots | Amazon Lex |
| **Recommendation Systems** | Personalized suggestions | Amazon Personalize |
| **Fraud Detection** | Identifying fraudulent activity | Amazon Fraud Detector |
| **Forecasting** | Predicting future values | Amazon Forecast |
| **Document Processing** | Extracting data from documents | Amazon Textract |
| **Translation** | Language translation | Amazon Translate |

---

### AWS AI/ML Services Summary

| Service | Primary Capability | When to Use |
|---------|-------------------|-------------|
| **Amazon SageMaker AI** | End-to-end ML platform | Custom model development |
| **Amazon Bedrock** | Foundation model access | GenAI applications |
| **Amazon Q** | AI assistant | Enterprise productivity |
| **Amazon Rekognition** | Image/video analysis | Visual content analysis |
| **Amazon Comprehend** | NLP and text analytics | Text understanding |
| **Amazon Lex** | Conversational interfaces | Chatbots, voice assistants |
| **Amazon Polly** | Text-to-speech | Voice applications |
| **Amazon Transcribe** | Speech-to-text | Audio transcription |
| **Amazon Translate** | Language translation | Multi-language support |
| **Amazon Textract** | Document analysis | OCR, form extraction |
| **Amazon Personalize** | Recommendations | E-commerce, content |
| **Amazon Fraud Detector** | Fraud detection | Payments, account protection |
| **Amazon Kendra** | Intelligent search | Enterprise search |

---

## Task 1.3: ML Development Lifecycle

### ML Pipeline Components

```
Data Collection → EDA → Pre-processing → Feature Engineering → Model Training → 
Hyperparameter Tuning → Evaluation → Deployment → Monitoring
```

| Stage | Description | AWS Service/Feature |
|-------|-------------|---------------------|
| **Data Collection** | Gathering raw data | S3, Glue, Lake Formation |
| **Exploratory Data Analysis (EDA)** | Understanding data patterns | SageMaker Studio, QuickSight |
| **Data Pre-processing** | Cleaning, transforming data | Glue DataBrew, SageMaker Data Wrangler |
| **Feature Engineering** | Creating input features | SageMaker Feature Store |
| **Model Training** | Teaching the model | SageMaker Training Jobs |
| **Hyperparameter Tuning** | Optimizing model settings | SageMaker Automatic Model Tuning |
| **Evaluation** | Testing model performance | SageMaker Model Evaluation |
| **Deployment** | Putting model into production | SageMaker Endpoints |
| **Monitoring** | Tracking model performance | SageMaker Model Monitor |

---

### Model Sources

| Source | Description | Pros | Cons |
|--------|-------------|------|------|
| **Pre-trained Models** | Ready-to-use models | Fast deployment, no training | Limited customization |
| **Open Source Models** | Publicly available models | Free, community support | Integration effort |
| **Custom Models** | Built from scratch | Full control | Time and cost intensive |

---

### Production Deployment Methods

| Method | Description | Best For |
|--------|-------------|----------|
| **Managed API Service** | AWS-hosted endpoints | Quick deployment, scaling |
| **Self-hosted API** | Own infrastructure | Full control, specific requirements |
| **Serverless** | Event-driven inference | Irregular traffic patterns |

---

### MLOps Fundamentals

| Concept | Description |
|---------|-------------|
| **Experimentation** | Testing different approaches systematically |
| **Repeatable Processes** | Consistent, reproducible workflows |
| **Scalable Systems** | Handle growing workloads |
| **Technical Debt Management** | Maintaining code quality over time |
| **Production Readiness** | Meeting performance and reliability requirements |
| **Model Monitoring** | Tracking model performance in production |
| **Model Re-training** | Updating models with new data |

---

### Model Metrics

#### Performance Metrics (Technical)

| Metric | Use For | Description |
|--------|---------|-------------|
| **Accuracy** | Classification | Percentage of correct predictions |
| **AUC (Area Under Curve)** | Binary classification | Model's ability to distinguish classes |
| **F1 Score** | Imbalanced datasets | Harmonic mean of precision and recall |
| **Precision** | When false positives are costly | Correct positive predictions |
| **Recall** | When false negatives are costly | Captured positive cases |
| **RMSE** | Regression | Root mean squared error |

#### Business Metrics

| Metric | Description |
|--------|-------------|
| **Cost Per User** | Operational efficiency measurement |
| **Development Costs** | Total investment in building solution |
| **Customer Feedback** | Qualitative satisfaction measure |
| **ROI (Return on Investment)** | Overall business value generated |
| **Conversion Rate** | Business impact measurement |

---

## Exam Tips for Domain 1

1. **Know the hierarchy:** AI → ML → Deep Learning → GenAI
2. **Match learning types to scenarios:** Supervised = labeled data, Unsupervised = patterns, Reinforcement = rewards
3. **Understand when NOT to use AI/ML:** This is commonly tested
4. **Know AWS services by use case:** Match the right service to the problem
5. **Remember MLOps concepts:** Experimentation, monitoring, retraining cycle
6. **Differentiate metrics:** Technical (AUC, F1) vs. Business (ROI, cost)
