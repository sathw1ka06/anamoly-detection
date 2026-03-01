# anomoly-detection
# 🚀 Network Intrusion Detection System (NSL-KDD Based)

A complete end-to-end **Machine Learning based Network Intrusion Detection System (NIDS)** built on the NSL-KDD dataset.

This project implements the full ML lifecycle — from preprocessing and feature engineering to model training, hyperparameter tuning, clustering-based compression (CluClas), and deployment-ready model export.

---

## 📌 Project Overview

Modern cybersecurity systems generate massive volumes of network traffic data. Detecting malicious activity requires intelligent modeling of connection behavior, statistical patterns, and anomaly characteristics.

This project builds a **Binary Intrusion Detection System (Normal vs Attack)** using:

- Advanced feature engineering
- Multiple ML models
- Feature selection techniques
- Hyperparameter optimization
- Hybrid clustering-classification strategy
- Model persistence & export

---

## 📂 Dataset

**Dataset Used:** NSL-KDD  
Downloaded using KaggleHub:

```python
kagglehub.dataset_download("anushonkar/network-anamoly-detection")
```

### Dataset Characteristics

- 41 network traffic features
- 1 attack label
- 1 difficulty level
- Mix of:
  - Continuous features (traffic statistics)
  - Categorical features (protocol, service, flag)

### Attack Categories

- Normal
- DoS
- Probe
- R2L
- U2R

The system converts these into:

- `attack_binary` → 0 (Normal), 1 (Attack)
- `attack_multiclass` → Original attack category

---

# ⚙️ Project Pipeline

## 1️⃣ Data Preprocessing

- Column standardization
- Missing value validation
- Target engineering
- Binary classification setup

---

## 2️⃣ Outlier Handling (IQR Method)

Outliers are clipped using:

IQR = Q3 - Q1  
Lower Bound = Q1 - 1.5 × IQR  
Upper Bound = Q3 + 1.5 × IQR  

Why?

- Network traffic contains extreme spikes
- Prevents overfitting
- Improves robustness

---

## 3️⃣ Categorical Encoding

Categorical Features:

- protocol_type
- service
- flag

Encoding Technique:

- `LabelEncoder`
- Encoders stored for deployment consistency

---

## 4️⃣ Feature Scaling

Standardization using:

```python
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
```

Applied to numerical features.

Necessary for:

- Logistic Regression
- Neural Networks
- Gradient-based models

---

# 🧠 Advanced Feature Engineering

Custom behavioral security features were engineered:

### Traffic Behavior Features

- `bytes_ratio`
- `total_bytes`
- `bytes_difference`

### Connection Density Features

- `srv_count_ratio`
- `connection_density`

### Error Interaction Features

- `total_error_rate`
- `srv_total_error_rate`
- `error_interaction`

These features enhance anomaly separability and improve detection accuracy.

---

# 🎯 Feature Selection

Two techniques were used:

### 1. Random Forest Feature Importance
- Gini-based importance ranking

### 2. SelectKBest (ANOVA F-test)

```python
from sklearn.feature_selection import SelectKBest, f_classif
```

Removes statistically weak predictors.

---

# 🤖 Models Implemented

| Model | Library |
|--------|----------|
| Logistic Regression | sklearn |
| Decision Tree | sklearn |
| Random Forest | sklearn |
| Gradient Boosting | sklearn |
| XGBoost | xgboost |
| MLP Neural Network | sklearn |

---

# 📊 Evaluation Metrics

Models evaluated using:

- Accuracy
- Precision
- Recall
- F1-Score
- ROC-AUC
- Precision-Recall Curve
- Confusion Matrix

F1-score used as primary tuning metric to balance:

- False Positives
- False Negatives

---

# ⚙️ Hyperparameter Tuning

Performed using:

```python
from sklearn.model_selection import GridSearchCV
GridSearchCV(cv=3, scoring='f1')
```

Optimized:

- max_depth
- min_samples_split
- Other tree parameters

---

# 🧠 CluClas: Hybrid Clustering + Classification

A scalable enhancement approach:

1. Cluster normal traffic using KMeans
2. Cluster attack traffic separately
3. Use cluster centroids as compressed training dataset

Benefits:

- Reduces dataset size
- Faster training
- Scalable deployment
- Maintains performance

---

# 💾 Model Persistence

Saved using:

- joblib
- pickle

Stored:

- Tuned model
- CluClas model
- Preprocessed dataset
- Evaluation results

Ensures deployment readiness.

---

# 📦 Project Structure

```
├── Train.txt
├── Test.txt
├── MyProject.ipynb
├── saved_models/
│   ├── tuned_model.pkl
│   ├── cluclas_model.pkl
├── results/
│   ├── evaluation_metrics.csv
├── README.md
```

---

# 🛠 Tech Stack

- Python 3.x
- NumPy
- Pandas
- Matplotlib
- Seaborn
- Scikit-learn
- XGBoost
- KaggleHub
- Joblib / Pickle

---

# 🚀 Key Highlights

- End-to-end ML lifecycle implementation
- Advanced domain-driven feature engineering
- Multiple model comparison framework
- Hyperparameter optimization
- Hybrid clustering-classification strategy
- Production-ready model saving
- Scalable and modular architecture

---

# 📈 Results

- Strong classification performance across multiple models
- Ensemble models (Random Forest, XGBoost) show highest robustness
- CluClas improves computational efficiency
- Balanced precision-recall performance

---

# 🎯 Future Improvements

- Real-time intrusion detection pipeline
- LSTM-based traffic modeling
- Online learning for evolving attack patterns
- Explainable AI integration (SHAP)
- Deployment via Flask / FastAPI
