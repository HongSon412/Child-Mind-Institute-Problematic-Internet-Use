# Predicting Problematic Internet Use in Children Using Physical and Fitness Data

This project aims to develop a machine learning model to predict problematic internet use in children and adolescents by leveraging physical and fitness-related data. By using easily accessible physical measurements, the model seeks to identify early signs of problematic internet behavior, enabling timely interventions to foster healthier digital habits.

## Background

Problematic internet use (PIU) among children and adolescents is increasingly prevalent in the digital age and is often linked to mental health challenges such as depression and anxiety. Traditional assessment methods are complex, requiring professional expertise and posing accessibility, cultural, and language barriers for many families. In contrast, physical and fitness indicators—such as posture, diet, and activity levels—are easy to obtain and can act as proxies to detect excessive technology use. This project explores those indicators to provide a scalable solution for identifying problematic internet usage, especially in resource-limited settings.

## Dataset

The dataset originates from a competition organized by the Child Mind Institute and includes:

- **Physical Attributes**: BMI, height, weight, etc.
- **Behavioral Aspects**: Patterns of internet use.
- **Fitness Data**: Endurance time, heart rate, etc.
- **Time-Series Data**: Actigraphy data processed into statistical features.

The data is split into training and testing sets, with time-series data merged in to enhance predictive performance.

## Methodology

The machine learning pipeline includes the following steps:

### Feature Engineering

- Selected relevant physical, behavioral, and fitness-related features.
- Encoded categorical variables (e.g., season) numerically.
- Extracted statistical features (mean, std, etc.) from time-series actigraphy data.
- Created derived features like `BMI_Age`, `Internet_Hours_Age`.

### Data Preprocessing

- Handled missing values using KNN imputation.
- Normalized and scaled features for model compatibility.

### Model Training

- Applied various regression models: LightGBM, XGBoost, CatBoost, RandomForest, and GradientBoosting.
- Used ensemble techniques like `VotingRegressor` to combine model predictions.
- Fine-tuned hyperparameters for optimal performance.

### Evaluation

- Performed 5-fold stratified cross-validation.
- Used Quadratic Weighted Kappa (QWK) as the evaluation metric.
- Optimized thresholds to maximize QWK.

## Results

The models were evaluated using 5-fold cross-validation. The average and optimized QWK scores are as follows:

| Model                  | Avg QWK (CV) | Optimized QWK |
|------------------------|--------------|----------------|
| LightGBM              | 0.4078       | 0.445          |
| CatBoost              | 0.3602       | 0.457          |
| XGBoost               | 0.3913       | 0.434          |
| VotingRegressor       | 0.3893       | 0.449          |
| Full Ensemble         | 0.3785       | 0.448          |

The final submission used a majority vote ensemble of model predictions to determine the `sii` (Severity of Internet Issue) score.

## How to Use

To run the code and reproduce results:

1. **Install Dependencies**:
   - Required Python libraries: `numpy`, `pandas`, `scikit-learn`, `lightgbm`, `xgboost`, `catboost`, `torch`, `polars`, `matplotlib`, `seaborn`.
   - GPU usage is recommended and configured in model parameters.

2. **Prepare Data**:
   - Download the dataset from the Child Mind Institute Kaggle competition.
   - Place the files in appropriate directories:
     - `train.csv`, `test.csv`, `sample_submission.csv` in the root folder.
     - Time-series files: `series_train.parquet`, `series_test.parquet`.

3. **Run Notebook**:
   - Open `lb0-494-with-tabnet-ds102.ipynb` in Jupyter Notebook or a compatible environment.
   - Execute all cells in order:
     - Load and preprocess data.
     - Train models.
     - Generate predictions.

4. **Output**:
   - A `submission.csv` file will be created containing the predicted `sii` values for the test set.

## Conclusion

This project demonstrates the potential of using physical and fitness data as indicators for predicting problematic internet usage in children. The ensemble of machine learning models achieved a reasonable prediction performance, with optimized QWK scores around 0.45. Future improvements may include:

- Advanced feature engineering.
- Integration of additional data sources.
- Exploration of deep learning methods.

We welcome contributions to this repository to further improve the model and promote healthier digital behaviors in children!
