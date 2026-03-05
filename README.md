# MnSol-AI-ML-Solvation-Assistant

## Introduction

The design of efficient solvents is critical for addressing **CO₂ capture and utilization** challenges. Traditional experimental approaches for discovering new solvents or solvent blends are often time-consuming and expensive. Therefore, computational methods and data-driven techniques are increasingly being used to accelerate solvent discovery.

Recent advances in **machine learning (ML)** and **artificial intelligence (AI)** enable predictive modeling of important physicochemical properties such as **solvation free energy (ΔGsolv)**. These models can significantly reduce the need for expensive experiments and large-scale quantum chemical simulations.

In this work, we propose a **hybrid DFT–AI/ML workflow** for accelerated solvent screening using the **MNsol solvation database**. The workflow integrates experimental data, molecular descriptors, and machine learning algorithms to develop predictive models for solvation free energy.

The most commonly used machine learning models for this task include:

- Tree-based regressors
- Neural network-based regressors
  
## Methodology

In this work, **Solvation Free Energy (ΔGsolv)** is used as the **target variable** for building predictive machine learning models. The workflow integrates experimental solvation data, molecular descriptors, and quantum chemical descriptors to develop robust prediction models.

### 1. Literature Review

A literature survey was conducted to identify reliable solvation databases for building predictive models.

The main databases considered include:

| Database | Description |
|---------|-------------|
| **MNsol** | Contains ~3037 solvation free energy measurements for various solute–solvent systems |
| **AqSolDB** | Contains ~8982 curated aqueous solubility data points |

These datasets provide experimental measurements required for training machine learning models.

---

### 2. Data Collection

Experimental physicochemical properties are collected from solvation databases. These include:

- LogP
- Charge
- Hydrogen bond acidity/basicity
- Dielectric constant
- Carbon aromaticity
- Halogenicity
- Surface tension

These properties provide useful information about molecular interactions between solutes and solvents.

---

### 3. Data Processing and Feature Extraction

The collected data undergoes preprocessing steps including:

- Data cleaning
- Handling missing values
- Feature normalization
- Feature extraction

Two categories of descriptors are generated:

#### Molecular Descriptors

- Molecular weight  
- SMILES representation  
- InChI / InChIKey  
- Number of heavy atoms  
- Number of heteroatoms  
- Ring count  
- Rotatable bonds  
- Valence electrons  
- Topological Polar Surface Area (TPSA)

#### Quantum Chemical Descriptors

- HOMO energy  
- LUMO energy  
- Surface area  
- Dipole moment  
- Polarizability  
- Mulliken charges  
- Electrostatic potentials  

These descriptors capture both **structural and electronic properties of molecules**.

---

### 4. Machine Learning Model Development

Several regression-based machine learning models are implemented to predict **ΔGsolv**.

The models considered include:

1. Linear Regression  
2. ElasticNet  
3. Support Vector Regressor (SVR)  
4. Random Forest (RF)  
5. Decision Tree (DT)  
6. Extreme Gradient Boosting (XGBoost)  
7. Gaussian Process Regression (GPR)  
8. Multilayer Perceptron (MLP)  
9. Artificial Neural Network (ANN)

---

### 5. Model Optimization

Hyperparameter tuning is performed to improve model performance using:

- k-fold Cross Validation
- Grid Search Cross Validation
- Randomized Search
- Optuna optimization

---

### 6. Model Evaluation

Model performance is evaluated using standard regression metrics:

- **MAE** – Mean Absolute Error  
- **MSE** – Mean Squared Error  
- **RMSE** – Root Mean Squared Error  
- **R² Score** – Coefficient of Determination

These metrics measure prediction accuracy and model robustness.

---

## Workflow Diagram

![Workflow](images/workflow.png)
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
# Model Development and Results

## Model 1: XGBoost (XGB)

The first model implemented in this study is the **XGBoost regression model** for predicting **solvation free energy (ΔGsolv)**.

### Performance Metrics

| Dataset | R² | RMSE | MAE |
|-------|------|------|------|
| Cross-validation (5-fold) | 0.9994 | 0.4482 | 0.3170 |
| Validation | 0.9944 | 1.6773 | 0.8250 |
| Test | 0.9927 | 1.7522 | 0.7119 |

The XGBoost model achieved **very high predictive accuracy** and demonstrated strong generalization across validation and test datasets.

---

# Model 2: Reduced Variable Model

A second experiment was conducted by **removing 37 variables** (P, HP, OP, S, HS, OS, SP, SS).

### Result

- Predictions on unseen samples produced an **average error of ±10**.
- The model demonstrated **stable performance and good generalization**.

---

# Model 3: Simplified Encoding Model

In the third experiment, feature dimensionality was reduced using simplified encoding.

### Feature Engineering

Encoded variables:

- Solvent
- Type
- Formula

Additionally, **37 variables were removed**.

### Result

- Average prediction error: **±10**

---

# Model Visualization

### Actual vs Predicted ΔGsolv

Scatter plots were generated to compare **actual and predicted solvation free energy values**.

These plots demonstrate a strong correlation between predicted and actual values, indicating **high model accuracy**.

*(Insert image here)*

---

# Residual Analysis

Residual analysis was conducted to evaluate prediction errors.

Observations:

- Most residual values are **close to zero**
- Indicates **low prediction error**
- Confirms **high model reliability**

*(Insert image here)*

---

# Model 5: XGBoost with Optuna Optimization

A final model was developed using **XGBoost with Optuna-based hyperparameter optimization**.

### Label Encoding

Encoded categorical variables:

- Solvent
- Type
- Formula

Dataset shape after encoding:

| Shape |
|------|
| (3037, 53) |

### Dataset Split

| Dataset | Shape |
|-------|------|
| Train | (1920, 52) |
| Validation | (480, 52) |
| Test | (600, 52) |
| Unknown | (37, 53) |

---

## Optuna Hyperparameter Optimization

### Best Hyperparameters

| Parameter | Value |
|----------|------|
| n_estimators | 630 |
| max_depth | 7 |
| learning_rate | 0.05036 |
| subsample | 0.6497 |
| colsample_bytree | 0.8100 |
| reg_lambda | 11.13 |
| reg_alpha | 2.18 |

---

### Model Performance

| Dataset | R² | RMSE |
|-------|------|------|
| Train | 0.9997 | 0.3625 |
| Validation | 0.9935 | 1.5483 |
| Test | 0.9914 | 1.9027 |

This model achieved the **best performance**.

---

# Large-Scale Solvent Screening Dataset

To explore a larger chemical space, a **combinatorial solute–solvent dataset** was generated.

### Dataset Statistics

| Property | Value |
|------|------|
| Samples | 83,740 |
| Features | 52 |
| Target | Not available |

This dataset contains **new solute–solvent combinations**.

---

# Prediction Using Trained Model

The optimized **XGBoost model** was used to predict **ΔGsolv** for the generated dataset.

Workflow:

1. Train model using **MNsol dataset (3037 samples)**
2. Apply model to **83,740 combinations**
3. Generate predicted **ΔGsolv values**

This enables **large-scale solvent screening**.

---

# Semi-Supervised Learning Approach

A semi-supervised learning strategy was explored.

### Motivation

The MNsol dataset contains **limited labeled data**, while the generated dataset contains **large unlabeled data**.

Semi-supervised learning uses both.

---

### Workflow

1. Train model using labeled MNsol data
2. Predict ΔGsolv for unlabeled dataset
3. Treat predictions as **pseudo-labels**
4. Combine labeled and pseudo-labeled datasets
5. Retrain the model

This helps the model learn from **much larger data**.

---

# Deployment

The trained model was deployed using **Streamlit** on **Hugging Face Spaces**.

### Application

MNsol ΔGsolv Predictor

https://madishettimahesh-mnsol-predictor.hf.space/

---

# Interactive Prediction Tool

The web application allows users to:

- Select solute–solvent systems
- Input physicochemical descriptors
- Predict ΔGsolv instantly

Outputs include:

- Predicted solvation free energy
- Confidence estimation
- Interactive interface

---

# Conclusion

Key outcomes of this study:

- MNsol dataset enables accurate **ΔGsolv prediction**
- Tree-based models outperform neural networks
- **XGBoost + Optuna** achieved best performance
- Large-scale screening of **83k solvent systems**
- Deployed an interactive **AI prediction tool**

---

# Future Work

Possible future improvements:

- Graph Neural Networks for molecular representation
- Integration with DFT calculations
- Active learning for solvent discovery
- Expansion with additional solvation datasets

---

# Technologies Used

- Python
- Scikit-learn
- XGBoost
- Optuna
- Pandas
- NumPy
- Streamlit
- Hugging Face Spaces

---

# Author

Madishetti Mahesh

MSc Statistics  
Central University of Punjab

---

# License

MIT License
