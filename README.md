# ETE-Prediction-LightGBM

This repository provides the source code for a LightGBM-based surrogate model developed to predict Evacuation Time Estimate (ETE) under earthquake-induced bridge damage scenarios.

The model uses the damage states of 48 bridges as input features and predicts the corresponding ETE obtained from traffic simulations.

## Overview

Large-scale traffic simulations can require substantial computational time when evaluating many earthquake-induced bridge damage scenarios. To reduce this computational burden, a LightGBM regression model is used as a surrogate model for rapid ETE prediction.

The workflow implemented in this repository consists of:

1. Data preprocessing
2. Dataset splitting
3. LightGBM model training
4. Model performance evaluation
5. Prediction time measurement
6. Model saving

## Dataset

The input dataset consists of bridge damage states and the corresponding Evacuation Time Estimate.

### Input features

The model uses 48 bridge damage-state variables as input features.

```text
Bridge 1
Bridge 2
...
Bridge 48
```

Each bridge damage state is represented as an ordinal value.

### Target variable

```text
ETE
```

ETE represents the Evacuation Time Estimate obtained from the traffic simulation.

Only valid simulation cases are retained during data preprocessing.

The dataset file path in the notebook should be modified according to the user's local execution environment.

## Data Split

The dataset is divided into three subsets:

| Dataset | Ratio | Purpose |
|---|---:|---|
| Training set | 70% | Model training |
| Validation set | 10% | Early stopping and model selection |
| Test set | 20% | Final performance evaluation |

A fixed random seed is used for reproducibility:

```python
random_state=42
```

## LightGBM Model

The LightGBM regression model is configured using the following main hyperparameters:

```python
LGBMRegressor(
    n_estimators=3000,
    learning_rate=0.01,
    max_depth=16,
    num_leaves=32,
    min_data_in_leaf=10,
    random_state=42
)
```

Early stopping is applied using the validation dataset:

```python
early_stopping(stopping_rounds=300)
```

The optimal number of boosting iterations is therefore determined based on validation performance.

## Performance Evaluation

Model performance is evaluated using the following metrics:

- Root Mean Squared Error (RMSE)
- Mean Absolute Error (MAE)
- Coefficient of Determination (R²)

Performance metrics are calculated separately for the training, validation, and test datasets.

The test dataset is reserved for final model evaluation and is not used for early stopping or parameter optimization.

## Prediction Time

The notebook also measures the prediction time required for a single bridge damage scenario.

A scenario containing 48 bridge damage-state values is provided to the trained LightGBM model, and the elapsed prediction time is recorded.

This analysis is used to evaluate the computational efficiency of the surrogate model for rapid ETE prediction.

## Output

The notebook generates the following outputs:

- Training, validation, and test performance metrics
- Best boosting iteration determined by early stopping
- Single-scenario prediction time
- Trained LightGBM model file

The trained model is saved using `joblib`.

## Requirements

The code requires Python and the following packages:

```text
pandas
numpy
lightgbm
scikit-learn
matplotlib
joblib
```

The code can be executed in Jupyter Notebook or Google Colab.


## Reproducibility

A fixed random seed of 42 is used for dataset splitting and LightGBM model training to improve reproducibility.

The source code is provided to support transparency and reproducibility of the machine-learning analysis presented in the associated research study.

## Data Availability

The dataset used for model training is not included in this repository.

Data availability follows the data availability statement provided in the associated manuscript.
