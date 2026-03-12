# AI/ML based Accelerated Design of Solvents for Crystallization and Catalysts for Vapour Phase Catalytic Reactions

## 1. Introduction

The design of solvents for advanced chemical engineering applications, such as crystallization processes and phase-change CO₂ capture, faces significant challenges due to the vast chemical space (>10⁶ possible molecules) and the computational expense of traditional quantum mechanical methods (e.g., density functional theory, DFT) for evaluating key properties.

Solvation free energy is a critical thermodynamic descriptor governing solubility and phase behavior. Predicting it accurately often requires extensive experimental or simulation data, leading to prolonged trial-and-error cycles.

Similarly, identifying optimal solvents for crystallization requires balancing multiple criteria such as:

* Yield
* Purity
* Selectivity
* Hydrogen bonding environment

These bottlenecks lead to:

* Suboptimal process efficiencies
* High energy penalties (>3 GJ/ton CO₂ captured)
* Slower innovation in green chemistry

Machine learning (ML) provides an alternative approach capable of rapidly screening large chemical spaces and predicting solvation properties efficiently.

---

# Objectives

### 1. Prediction of Solvation Free Energy

Develop machine learning models to accurately predict **ΔGsolv** in diverse solvent–solute systems.

Target performance:

* Mean Absolute Error (MAE) < **5 kcal/mol**

Approach:

* Molecular descriptors
* Quantum chemical features
* Transfer learning

---

### 2. Design of Solvents for Crystallization

Construct **multi-objective ML frameworks** to screen solvent candidates based on:

* Solubility
* Supersaturation propensity
* Lattice energy
* Hydrogen bonding interactions

Goal:

Identify solvents that improve crystallization yield by **≥15%**.

---

# Deliverables

### 1. Annotated Dataset for Solvation Free Energy

Curated dataset containing:

* Experimental ΔGsolv values
* DFT-derived descriptors
* Metadata for reproducibility

Hosted on a **version-controlled repository**.

---

### 2. ML Model Prototypes

Open-source machine learning implementations including:

* Training scripts
* Hyperparameter optimization
* Validation notebooks

---

### 3. Crystallization Solvent Screening Toolkit

Integrated framework combining:

* ML surrogate models
* Genetic algorithms
* Multi-objective optimization

---

# Expected Outcomes

### Improved Predictive Accuracy

ML models capable of predicting solvation free energy with **>90% agreement with experimental data**.

Benefits:

* Reduce screening time from **weeks → hours**
* Evaluate **tens of thousands of solvent candidates**

---

### Sustainability Impact

Potential improvements for:

* Phase-change CO₂ capture solvents
* Amino-acid-based solvent systems
* Reduced energy consumption

---

### Enhanced Crystallization Efficiency

Identification of **10–15 high-performance solvents** with improved:

* Yield
* Purity
* Selectivity

---

### Knowledge Transfer

Reusable ML pipelines enabling collaboration between:

* Chemists
* Chemical engineers
* Data scientists

---

# Dataset Overview

The dataset contains **3037 samples and 58 variables**, representing **binary solute–solvent systems**.

Descriptors include:

* No.
* FileHandle
* SoluteName
* Formula
* Subset
* Charge
* Level1
* Level2
* Level3
* Solvent
* DeltaGsolv
* Type
* eps
* n
* alpha
* beta
* gamma
* phi²
* psi²
* beta²
* TotalArea

---

## Dataset Statistics

* **662 unique solutes**
* **106 solvents**
* **3037 solute–solvent systems**

Distribution:

* 615 compounds appear only as solutes
* 59 compounds appear only as solvents
* 47 compounds appear as both

Removed non-predictive variables:

* No.
* FileHandle
* SoluteName
* Subset
* beta²

No missing values remain after preprocessing.

---

# Feature Encoding

Machine learning models require numerical inputs.

### One-Hot Encoding

Applied to categorical variables:

* Solvent
* Type

Result:

* **158 features**

---

### Label Encoding

The **Formula** column was encoded using label encoding.

---

# Data Preprocessing

Target variable:

```
y = ΔGsolv
```

Input features:

```
X = all remaining features
```

Dataset split:

| Dataset    | Percentage | Shape       |
| ---------- | ---------- | ----------- |
| Training   | 64%        | (1943, 158) |
| Validation | 16%        | (486, 158)  |
| Test       | 20%        | (608, 158)  |

---

# Feature Scaling

Applied scalers:

* StandardScaler
* MinMaxScaler

Scaling was performed inside the **GridSearchCV pipeline**.

---

# Hyperparameter Optimization

Hyperparameters were optimized using:

```
GridSearchCV
```

Configuration:

* 5-fold cross-validation

---

# Machine Learning Models

A total of **11 ML models** were evaluated.

### Linear Models

* Linear Regression
* ElasticNet

### Kernel Method

* Support Vector Regressor

### Instance-Based

* k-Nearest Neighbors

### Tree-Based

* Decision Tree
* Random Forest

### Boosting

* XGBoost
* Gradient Boosting
* AdaBoost

### Probabilistic

* Gaussian Process Regression

### Neural Network

* Multi-Layer Perceptron

Best performing model:

```
XGBoost + Optuna
```

---

# Feature Encoding Strategies

Four encoding strategies were tested.

### Strategy I

Label + One-Hot Encoding

Features:

```
158
```

Prediction error:

```
±10 kcal/mol
```

---

### Strategy II

Reduced atom count features

Features:

```
121
```

Prediction error:

```
±10 kcal/mol
```

---

### Strategy III

Minimal feature encoding

Features:

```
15
```

Prediction error:

```
±10 kcal/mol
```

---

### Strategy IV (Best)

Label encoding only

Features:

```
58
```

Prediction error:

```
±3 kcal/mol
```

---

# Model Performance

| Metric | Training | Validation | Test   |
| ------ | -------- | ---------- | ------ |
| R²     | 0.9996   | 0.9936     | 0.9908 |
| MAE    | 0.2742   | 0.7684     | 0.8021 |
| MSE    | 0.1679   | 2.3782     | 3.8713 |
| RMSE   | 0.4097   | 1.5421     | 1.9676 |

---

# Large-Scale Solvent Screening

Generated dataset:

| Property | Value         |
| -------- | ------------- |
| Samples  | 83,740        |
| Features | 52            |
| Target   | Not available |

Purpose:

Explore **new solute–solvent combinations**.

---

# Semi-Supervised Learning Workflow

1. Train model using labeled MNsol dataset
2. Predict ΔGsolv for unlabeled data
3. Generate pseudo-labels
4. Combine datasets
5. Retrain model

This allows learning from **larger datasets**.

---

# Model Deployment

Deployment tools:

* Streamlit
* Hugging Face Spaces

Feature importance analysis performed using **SHAP**.

Top **10 features** were sufficient for prediction.

---

# MNsol ΔGsolv Predictor Tool

Application:

MNsol ΔGsolv Predictor

Web app:

https://madishettimahesh-mnsol-predictor.hf.space/

---

# Interactive Prediction Tool

Users can:

* Select solute–solvent systems
* Enter molecular descriptors
* Predict ΔGsolv instantly

Outputs:

* Predicted ΔGsolv
* Confidence estimate
* Interactive interface

---

# Validation using COSMO-Therm

Predictions were validated against:

* Experimental MNsol data
* COSMO-RS simulations

Empirical corrections were applied for ionic species.

---

# Future Work

Development of a **Conversational AI Assistant**.

Components:

* Prediction dataset
* Large language model
* Vector database
* Web interface

Example queries:

```
ΔG of benzene in water
Solvation energy of ethanol in methanol
Explain ΔG for acetone in water
```

---

# Conclusions

* ML models successfully predicted solvation free energy.
* XGBoost + Optuna achieved the best performance.
* The model screened **83,740 solvent combinations**.
* Semi-supervised learning enabled the use of unlabeled datasets.
* The system was deployed as an interactive prediction tool.

This study demonstrates how **AI and machine learning accelerate solvent discovery and chemical property prediction.**
