# MnSol-AI-ML-Solvation-Assistant

## Introduction

The design of efficient solvents is critical for addressing **CO₂ capture and utilization** challenges. Traditional experimental approaches for discovering new solvents or solvent blends are often time-consuming and expensive. Therefore, computational methods and data-driven techniques are increasingly being used to accelerate solvent discovery.

Recent advances in **machine learning (ML)** and **artificial intelligence (AI)** enable predictive modeling of important physicochemical properties such as **solvation free energy (ΔGsolv)**. These models can significantly reduce the need for expensive experiments and large-scale quantum chemical simulations.

In this work, we propose a **hybrid DFT–AI/ML workflow** for accelerated solvent screening using the **MNsol solvation database**. The workflow integrates experimental data, molecular descriptors, and machine learning algorithms to develop predictive models for solvation free energy.

The most commonly used machine learning models for this task include:

- Tree-based regressors
- Neural network-based regressors
  
## Methodology

In this study, **Solvation Free Energy (ΔGsolv)** is used as the **target variable** for developing predictive machine learning models. The workflow integrates experimental solvation data, molecular descriptors, and quantum chemical descriptors to build accurate prediction models for solvent screening.

### 1. Dataset

The **primary dataset used in this work is the MNsol Database**.

The **MNsol dataset** contains experimentally measured **solvation free energies (ΔGsolv)** for a wide range of solute–solvent systems. It includes approximately **3037 solvation data points**, covering various organic solvents and molecular compounds.

The MNsol database provides reliable experimental data that can be used for training machine learning models to predict solvation free energy.

| Dataset | Description |
|--------|-------------|
| **MNsol** | Experimental solvation free energy dataset (~3037 solute–solvent systems) |

This dataset serves as the **primary data source for model development and evaluation** in this study.

---

### 2. Data Processing

The MNsol dataset is preprocessed before model development. The preprocessing steps include:

- Data cleaning
- Handling missing values
- Feature extraction
- Feature normalization

---

### 3. Feature Engineering

Two types of descriptors are extracted from molecular data.

#### Molecular Descriptors

- Molecular weight  
- SMILES representation  
- InChI / InChIKey  
- Heavy atoms  
- Heteroatoms  
- Ring count  
- Rotatable bonds  
- Valence electrons  
- Topological Polar Surface Area (TPSA)

#### Quantum Chemical Descriptors

- HOMO energy  
- LUMO energy  
- Dipole moment  
- Polarizability  
- Mulliken charges  
- Electrostatic potentials  

---

### 4. Machine Learning Models

Regression-based machine learning models are developed to predict **ΔGsolv**, including:

- Linear Regression
- ElasticNet
- Support Vector Regression (SVR)
- Random Forest
- Decision Tree
- XGBoost
- Gaussian Process Regression
- Multilayer Perceptron (MLP)
- Artificial Neural Networks (ANN)

---

### 5. Model Optimization

Hyperparameter tuning is performed using:

- k-fold Cross Validation
- Grid Search
- Randomized Search
- Optuna optimization

---

### 6. Model Evaluation

Model performance is evaluated using:

- **MAE (Mean Absolute Error)**
- **MSE (Mean Squared Error)**
- **RMSE (Root Mean Squared Error)**
- **R² Score**

## Dataset Overview

The **MNsol dataset** is used as the primary dataset in this study for predicting **solvation free energy (ΔGsolv)**.

### Dataset Statistics

- Total samples: **3037**
- Total variables: **58**
- Number of solutes: **662**
- Number of solvents: **106**
- Binary solute–solvent systems: **3037**

Among these:

- **615 samples** are uniquely solutes  
- **59 samples** are uniquely solvents  
- **47 samples** can act as both solutes and solvents

### Column Information

The dataset includes several molecular and physicochemical descriptors. Key columns include:

- `No.`
- `FileHandle`
- `SoluteName`
- `Formula`
- `Subset`
- `Charge`
- `Level1`
- `Level2`
- `Level3`
- `Solvent`
- `DeltaGsolv`
- `Type`
- `eps`
- `n`
- `alpha`
- `beta`
- `gamma`
- `phi²`
- `psi²`
- `beta²`
- `TotalArea`
- and other molecular descriptors.

---

## Data Cleaning

Non-predictive variables were removed from the dataset before model development.

Removed columns:

- `No.`
- `FileHandle`
- `SoluteName`
- `Subset`
- `beta²`

After preprocessing:

- **No missing values remain in the dataset.**

---

## Solvent Distribution

The dataset contains an imbalanced distribution of solvents.

Examples:

| Solvent | Samples |
|-------|---------|
| Water | 533 |
| Octanol | 247 |
| Hexadecane | 198 |

Many other solvents have relatively **few samples**.

---

## Feature Encoding

Several categorical features were encoded for machine learning models.

### One-Hot Encoding

The following categorical variables were **one-hot encoded**:

- `Solvent`
- `Type`

After encoding, the dataset contained:

**158 total features**

### Label Encoding

The categorical feature **Formula** was **label encoded**, converting each unique molecular formula into an integer representation.

---

## Data Preprocessing

The dataset was prepared for machine learning as follows:

- **X** → Input features (all variables except the target)
- **y** → Target variable (`DeltaGsolv`)

---

## Train–Validation–Test Split

The dataset was divided into three sets:

| Dataset | Percentage | Shape |
|-------|-----------|-------|
| Training | 64% | (1943, 158) |
| Validation | 16% | (486, 158) |
| Test | 20% | (608, 158) |

---

## Feature Scaling

Numerical features were scaled during model training using:

- **StandardScaler**
- **MinMaxScaler**

Scaling was performed inside the **GridSearchCV pipeline** to avoid data leakage.

---

## Hyperparameter Optimization

Hyperparameter tuning was performed using:

- **GridSearchCV**
- **5-fold Cross Validation**

This approach ensures robust model selection and improved predictive performance.
