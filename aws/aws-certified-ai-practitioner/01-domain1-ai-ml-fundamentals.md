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
| **Foundation Models** | Large pre-trained models adaptable to many tasks | Base for fine-tuning (GPT, Claude, Titan) |
| **Transfer Learning** | Using pre-trained models on new tasks | Reduces training time and data needs |

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
| Interpretability | High | Low | Very Low |
| Training Time | Minutes-Hours | Hours-Days | Days-Weeks |
| Use Case | Predictions | Complex patterns | Content creation |

---

### Neural Network Fundamentals

| Component | Description | Purpose |
|-----------|-------------|---------|
| **Input Layer** | Receives raw data | Data entry point |
| **Hidden Layers** | Process and transform data | Feature extraction |
| **Output Layer** | Produces final prediction | Decision/result |
| **Neurons/Nodes** | Processing units | Weighted sum + activation |
| **Weights** | Connection strengths | Learned parameters |
| **Bias** | Offset value | Adjusts activation threshold |
| **Activation Function** | Non-linear transformation | Enables complex patterns |

#### Common Activation Functions

| Function | Range | Use Case |
|----------|-------|----------|
| **ReLU** | [0, ∞) | Most hidden layers (default) |
| **Sigmoid** | [0, 1] | Binary classification output |
| **Softmax** | [0, 1] (sum=1) | Multi-class classification |
| **Tanh** | [-1, 1] | Hidden layers (older networks) |

> **Exam Tip:** ReLU is most common for hidden layers; Sigmoid for binary output; Softmax for multi-class!

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
| **Semi-Supervised Learning** | Combines small labeled + large unlabeled data | When labeling is expensive | SageMaker |
| **Reinforcement Learning** | Agent learns through trial and error with rewards | Robotics, game playing, autonomous systems | SageMaker |
| **Self-Supervised Learning** | Creates labels from data structure | Language models, embeddings | Bedrock, SageMaker |

> **Exam Tip:** Know which learning type applies to which business problem!

---

### Common ML Algorithms

#### Supervised Learning Algorithms

| Algorithm | Type | Best For | Key Characteristics |
|-----------|------|----------|--------------------|
| **Linear Regression** | Regression | Continuous predictions | Fast, interpretable |
| **Logistic Regression** | Classification | Binary outcomes | Probability output |
| **Decision Trees** | Both | Interpretable models | Easy to visualize |
| **Random Forest** | Both | General purpose | Ensemble of trees |
| **XGBoost** | Both | Structured data | High accuracy, fast |
| **Neural Networks** | Both | Complex patterns | Needs lots of data |
| **SVM (Support Vector Machine)** | Classification | High-dimensional data | Works well with small data |
| **K-Nearest Neighbors (KNN)** | Both | Simple classification | No training phase |

#### Unsupervised Learning Algorithms

| Algorithm | Type | Best For | Key Characteristics |
|-----------|------|----------|--------------------|
| **K-Means** | Clustering | Customer segmentation | Needs K specified |
| **DBSCAN** | Clustering | Arbitrary shapes | Finds outliers |
| **PCA** | Dimensionality Reduction | Feature reduction | Preserves variance |
| **t-SNE** | Dimensionality Reduction | Visualization | 2D/3D projections |
| **Isolation Forest** | Anomaly Detection | Fraud detection | Fast, scalable |

> **Exam Tip:** XGBoost is the most popular algorithm for structured/tabular data on AWS!

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
| **Variance** | Sensitivity to training data | High variance = overfitting |
| **Fairness** | Equal treatment across groups | Responsible AI consideration |
| **Overfitting** | Model too specific to training data | Poor generalization (high variance) |
| **Underfitting** | Model too simple for data | Poor performance (high bias) |

---

### Hyperparameters vs Parameters

| Aspect | Parameters | Hyperparameters |
|--------|------------|----------------|
| **Definition** | Learned during training | Set before training |
| **Examples** | Weights, biases | Learning rate, epochs, batch size |
| **Optimization** | Gradient descent | Manual tuning or AutoML |
| **When Set** | Automatically | By practitioner |

#### Common Hyperparameters

| Hyperparameter | Description | Impact |
|----------------|-------------|--------|
| **Learning Rate** | Step size for updates | Too high = unstable; too low = slow |
| **Epochs** | Training iterations | More = better fit (risk of overfitting) |
| **Batch Size** | Samples per update | Smaller = more updates, slower |
| **Number of Layers** | Neural network depth | More = complex patterns |
| **Regularization** | Penalty for complexity | Prevents overfitting (L1, L2, Dropout) |

> **Exam Tip:** SageMaker Automatic Model Tuning (AMT) optimizes hyperparameters automatically!

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

#### Confusion Matrix (Classification)

```
                    Predicted
                 Positive  Negative
Actual Positive    TP        FN
       Negative    FP        TN
```

| Term | Definition | Example |
|------|------------|---------|
| **True Positive (TP)** | Correctly predicted positive | Spam correctly flagged |
| **True Negative (TN)** | Correctly predicted negative | Ham correctly passed |
| **False Positive (FP)** | Type I Error - incorrectly positive | Ham flagged as spam |
| **False Negative (FN)** | Type II Error - incorrectly negative | Spam missed to inbox |

#### Classification Metrics

| Metric | Formula | Use When | Range |
|--------|---------|----------|-------|
| **Accuracy** | (TP+TN) / Total | Balanced classes | 0-1 |
| **Precision** | TP / (TP+FP) | False positives costly (spam filter) | 0-1 |
| **Recall (Sensitivity)** | TP / (TP+FN) | False negatives costly (disease detection) | 0-1 |
| **Specificity** | TN / (TN+FP) | True negative rate important | 0-1 |
| **F1 Score** | 2 × (Precision × Recall) / (Precision + Recall) | Imbalanced datasets | 0-1 |
| **AUC-ROC** | Area under ROC curve | Ranking/threshold selection | 0-1 |

> **Exam Tip:** Precision vs Recall trade-off - improving one typically decreases the other!

#### Regression Metrics

| Metric | Formula | Interpretation |
|--------|---------|----------------|
| **MSE** | Mean[(y - ŷ)²] | Average squared error (penalizes large errors) |
| **RMSE** | √MSE | Same units as target |
| **MAE** | Mean[|y - ŷ|] | Average absolute error |
| **R²** | 1 - (SS_res/SS_tot) | Proportion of variance explained (0-1) |
| **MAPE** | Mean[|y - ŷ|/y × 100] | Percentage error |

#### Business Metrics

| Metric | Description |
|--------|-------------|
| **Cost Per User** | Operational efficiency measurement |
| **Development Costs** | Total investment in building solution |
| **Customer Feedback** | Qualitative satisfaction measure |
| **ROI (Return on Investment)** | Overall business value generated |
| **Conversion Rate** | Business impact measurement |
| **Time to Value** | Speed of deployment and adoption |
| **Customer Lifetime Value** | Long-term revenue impact |

---

## Exam Tips for Domain 1

1. **Know the hierarchy:** AI → ML → Deep Learning → GenAI (Foundation Models → LLMs)
2. **Match learning types to scenarios:**
   - Supervised = labeled data (classification, regression)
   - Unsupervised = patterns without labels (clustering, anomaly)
   - Reinforcement = rewards and actions (robotics, games)
   - Semi-Supervised = small labeled + large unlabeled
3. **Understand when NOT to use AI/ML:** This is commonly tested
   - Deterministic outcomes needed
   - Insufficient data
   - Simple rule-based logic works
4. **Know AWS services by use case:**
   - SageMaker = custom ML
   - Bedrock = GenAI/Foundation Models
   - Rekognition = images/video
   - Comprehend = text/NLP
   - Transcribe = speech-to-text
   - Lex = conversational bots
5. **Metrics awareness:**
   - Precision = minimize false positives (spam filter)
   - Recall = minimize false negatives (disease detection)
   - F1 = imbalanced datasets
   - Accuracy = balanced datasets only!
6. **MLOps lifecycle:** Data → Train → Evaluate → Deploy → Monitor → Retrain
7. **Overfitting vs Underfitting:**
   - Overfitting = high variance, too complex, memorizes training data
   - Underfitting = high bias, too simple, misses patterns
8. **XGBoost** is the go-to algorithm for structured/tabular data on AWS
9. **Real-time vs Batch inference:** Know cost vs latency trade-offs
10. **Transfer Learning** saves time and data by using pre-trained models

