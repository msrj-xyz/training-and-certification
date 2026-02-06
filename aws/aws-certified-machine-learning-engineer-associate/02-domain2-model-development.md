# Domain 2: ML Model Development (26%)

## Task 2.1: Choose a Modeling Approach

### ML Problem Types

| Type | Description | Algorithms |
|------|-------------|------------|
| **Classification** | Predict categories | XGBoost, Linear Learner, Image Classification |
| **Regression** | Predict continuous values | Linear Learner, XGBoost |
| **Clustering** | Group similar items | K-Means |
| **Anomaly Detection** | Find outliers | Random Cut Forest |
| **Recommendation** | Suggest items | Factorization Machines |
| **Time Series** | Forecast future values | DeepAR |
| **NLP** | Text processing | BlazingText, Seq2Seq |
| **Computer Vision** | Image/video analysis | Image Classification, Object Detection |

---

### SageMaker Built-in Algorithms

| Algorithm | Type | Use Case |
|-----------|------|----------|
| **XGBoost** | Classification/Regression | Structured data, most common |
| **Linear Learner** | Classification/Regression | Linear relationships |
| **K-Means** | Clustering | Customer segmentation |
| **Random Cut Forest** | Anomaly Detection | Fraud detection |
| **DeepAR** | Time Series | Demand forecasting |
| **BlazingText** | NLP | Text classification, word2vec |
| **Seq2Seq** | NLP | Translation, summarization |
| **Image Classification** | CV | Classify images |
| **Object Detection** | CV | Detect objects in images |
| **Semantic Segmentation** | CV | Pixel-level classification |
| **Factorization Machines** | Recommendation | Personalization |
| **Neural Topic Model** | NLP | Topic modeling |
| **LDA** | NLP | Topic modeling |
| **IP Insights** | Security | Anomalous IP detection |

> **Exam Tip:** XGBoost is the most commonly used algorithm for structured data!

---

### AWS AI Services

| Service | Capability |
|---------|------------|
| **Amazon Rekognition** | Image/video analysis, face detection |
| **Amazon Comprehend** | NLP, sentiment, entities, PII |
| **Amazon Transcribe** | Speech-to-text |
| **Amazon Translate** | Language translation |
| **Amazon Polly** | Text-to-speech |
| **Amazon Textract** | Document OCR |
| **Amazon Lex** | Conversational bots |
| **Amazon Personalize** | Recommendations |
| **Amazon Forecast** | Time series forecasting |
| **Amazon Fraud Detector** | Fraud detection |
| **Amazon Kendra** | Enterprise search |

---

### Amazon Bedrock

| Feature | Description |
|---------|-------------|
| **Foundation Models** | Pre-trained LLMs (Claude, Titan, etc.) |
| **Fine-tuning** | Customize models with your data |
| **RAG** | Retrieval Augmented Generation |
| **Agents** | Autonomous AI workflows |
| **Guardrails** | Control model outputs |

---

### SageMaker JumpStart

| Feature | Description |
|---------|-------------|
| **Pre-trained Models** | Ready-to-use models |
| **Solution Templates** | End-to-end ML solutions |
| **Foundation Models** | LLMs for various tasks |
| **One-click Deploy** | Easy model deployment |
| **Fine-tuning** | Customize with your data |

---

### Model Selection Criteria

| Factor | Consideration |
|--------|---------------|
| **Data Size** | Small → simpler models; Large → deep learning |
| **Interpretability** | Linear models more explainable |
| **Latency** | Simpler models faster inference |
| **Cost** | Training and inference costs |
| **Accuracy** | Model performance metrics |

---

## Task 2.2: Train and Refine Models

### Training Process Elements

| Element | Description |
|---------|-------------|
| **Epoch** | One pass through entire dataset |
| **Batch Size** | Samples per gradient update |
| **Steps** | Iterations per epoch |
| **Learning Rate** | Gradient descent step size |
| **Loss Function** | Optimization objective |

---

### Reducing Training Time

| Method | Description |
|--------|-------------|
| **Early Stopping** | Stop when validation loss plateaus |
| **Distributed Training** | Multi-GPU, multi-instance |
| **Spot Instances** | Cost reduction with checkpoints |
| **Mixed Precision** | FP16 training |
| **Gradient Accumulation** | Virtual larger batches |

#### SageMaker Distributed Training
| Mode | Description |
|------|-------------|
| **Data Parallel** | Split data across GPUs |
| **Model Parallel** | Split model across GPUs |
| **Hybrid** | Combine both approaches |

---

### Regularization Techniques

| Technique | Description | Prevents |
|-----------|-------------|----------|
| **L1 (Lasso)** | Sparse weights, feature selection | Overfitting |
| **L2 (Ridge)** | Small weights | Overfitting |
| **Dropout** | Random neuron deactivation | Overfitting |
| **Weight Decay** | Penalize large weights | Overfitting |
| **Batch Normalization** | Normalize layer inputs | Training instability |
| **Early Stopping** | Stop before overfitting | Overfitting |

---

### Hyperparameter Tuning

| Method | Description |
|--------|-------------|
| **Grid Search** | Exhaustive search over grid |
| **Random Search** | Random sampling |
| **Bayesian Optimization** | Model-based optimization |
| **Hyperband** | Early stopping + random search |

#### SageMaker Automatic Model Tuning (AMT)
- Define hyperparameter ranges
- Specify objective metric
- Choose tuning strategy (Bayesian, Random, Hyperband)
- Set max training jobs
- Warm start from previous tunings

---

### Common Hyperparameters

| Model Type | Key Hyperparameters |
|------------|-------------------|
| **XGBoost** | max_depth, eta, num_round, subsample |
| **Neural Networks** | learning_rate, batch_size, layers, neurons |
| **Random Forest** | n_estimators, max_depth, min_samples_split |
| **Linear Models** | regularization, learning_rate |

---

### Model Improvement Techniques

| Technique | Description |
|-----------|-------------|
| **Ensembling** | Combine multiple models |
| **Stacking** | Meta-learner on base models |
| **Boosting** | Sequential error correction |
| **Bagging** | Parallel averaging |
| **Cross-validation** | Robust evaluation |

---

### Reducing Model Size

| Method | Description |
|--------|-------------|
| **Pruning** | Remove unnecessary weights |
| **Quantization** | Reduce precision (FP16, INT8) |
| **Knowledge Distillation** | Train smaller student model |
| **Compression** | Model compression techniques |
| **Feature Selection** | Remove unimportant features |

---

### SageMaker Model Registry

| Feature | Description |
|---------|-------------|
| **Model Groups** | Organize related models |
| **Model Versioning** | Track model iterations |
| **Approval Workflow** | Manage model promotion |
| **Lineage Tracking** | Track model provenance |
| **Deployment** | Deploy from registry |

---

### Training Frameworks

| Framework | SageMaker Support |
|-----------|-------------------|
| **TensorFlow** | Pre-built containers, script mode |
| **PyTorch** | Pre-built containers, script mode |
| **Scikit-learn** | Pre-built containers |
| **MXNet** | Pre-built containers |
| **Hugging Face** | Pre-built containers |
| **XGBoost** | Built-in algorithm |

---

## Task 2.3: Analyze Model Performance

### Classification Metrics

| Metric | Description | When to Use |
|--------|-------------|-------------|
| **Accuracy** | Correct predictions / Total | Balanced classes |
| **Precision** | TP / (TP + FP) | Minimize false positives |
| **Recall** | TP / (TP + FN) | Minimize false negatives |
| **F1 Score** | Harmonic mean of Precision & Recall | Imbalanced classes |
| **AUC-ROC** | Area under ROC curve | Ranking quality |
| **Confusion Matrix** | TP, TN, FP, FN | Overall view |

---

### Regression Metrics

| Metric | Description |
|--------|-------------|
| **MSE** | Mean Squared Error |
| **RMSE** | Root Mean Squared Error |
| **MAE** | Mean Absolute Error |
| **R²** | Coefficient of determination |
| **MAPE** | Mean Absolute Percentage Error |

---

### Overfitting vs Underfitting

| Issue | Training | Validation | Solution |
|-------|----------|------------|----------|
| **Overfitting** | Low error | High error | Regularization, dropout, more data |
| **Underfitting** | High error | High error | More features, complex model |
| **Good Fit** | Low error | Low error | Maintain |

---

### SageMaker Clarify

| Capability | Description |
|------------|-------------|
| **Bias Detection** | Pre/post-training bias |
| **Explainability** | SHAP-based feature importance |
| **Model Cards** | Document model behavior |
| **Fairness Reports** | Bias analysis reports |

#### Clarify Bias Metrics
- Demographic Parity
- Disparate Impact
- Equalized Odds
- Predictive Parity

---

### SageMaker Model Debugger

| Feature | Description |
|---------|-------------|
| **Training Issues** | Vanishing gradients, exploding gradients |
| **Profiling** | CPU/GPU utilization |
| **Built-in Rules** | Automatic issue detection |
| **Convergence Monitoring** | Track training progress |

---

### A/B Testing and Shadow Variants

| Method | Description |
|--------|-------------|
| **A/B Testing** | Split traffic between models |
| **Shadow Variant** | No traffic, parallel inference |
| **Canary Deployment** | Gradual traffic shift |

---

## Exam Tips for Domain 2

1. **XGBoost** - Default choice for structured data
2. **Built-in Algorithms** - Know when to use each
3. **Hyperparameter Tuning** - Bayesian optimization most efficient
4. **Regularization** - L1 for sparsity, L2 for small weights
5. **Overfitting** - High training, low validation performance
6. **Clarify** - Bias detection AND explainability
7. **Model Registry** - Version control for models
8. **F1 Score** - Use for imbalanced classification
