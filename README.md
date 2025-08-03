# Vehicle Price Prediction with Bayesian Optimization

## NON-TECHNICAL EXPLANATION OF YOUR PROJECT

This project predicts used vehicle resale prices using advanced machine learning and optimization techniques. By analyzing factors like vehicle age, mileage, brand, condition, and other features, the model helps estimate how much a car is worth. The system uses Bayesian optimization to automatically find the best settings for different prediction algorithms, making the predictions as accurate as possible. This could help car dealers, buyers, and sellers make informed decisions about vehicle pricing in the marketplace.

## DATA

The project uses a comprehensive vehicle dataset (`vehicles.csv`) containing approximately 426,000 vehicle records with features including:
- **Price**: Target variable (vehicle selling price)
- **Vehicle Information**: Year, manufacturer, model, condition, odometer reading
- **Technical Specs**: Fuel type, transmission, drive type, cylinders
- **Market Data**: State, region, posting date

**Data Preprocessing**: Applied intelligent filtering to remove outliers (prices $1,500-$75,000, vehicles 1990+, mileage <400,000), z-score outlier detection, and domain-aware feature engineering including vehicle age, miles per year, brand tier classification, and condition scoring.

## MODEL 

**Primary Models Tested**:
1. **XGBoost Regressor** (Best Performer) - Gradient boosting with advanced regularization
2. **Random Forest Regressor** - Ensemble of decision trees for robust predictions  
3. **Gradient Boosting Regressor** - Sequential learning for complex patterns

**Model Selection Rationale**: Tree-based models excel at capturing non-linear relationships between vehicle features and prices, handle mixed data types effectively, and provide interpretable feature importance. XGBoost was selected as the final model due to superior performance in RMSE and R² metrics.

**Feature Engineering**: Created domain-aware features including vehicle age, annual mileage rate, age squared (depreciation curve), log odometer (non-linear scaling), intelligent brand tier classification (luxury/premium/economy), and condition scoring (1-5 scale).

## HYPERPARAMETER OPTIMIZATION

**Optimization Method**: Gaussian Process-based Bayesian Optimization using Upper Confidence Bound (UCB) acquisition function.

**Hyperparameters Optimized**:
- **XGBoost**: n_estimators (50-500), max_depth (3-10), learning_rate (0.01-0.3), subsample (0.6-1.0), colsample_bytree (0.6-1.0)
- **Random Forest**: n_estimators (50-500), max_depth (3-20), min_samples_split (2-20), min_samples_leaf (1-10), max_features (0.3-1.0)
- **Gradient Boosting**: n_estimators (50-300), max_depth (3-8), learning_rate (0.01-0.2), subsample (0.6-1.0), max_features (0.3-1.0)

**Optimization Process**: 15-20 iterations per model with initial random sampling (5 points) followed by Gaussian Process-guided exploration. Used RMSE as the objective function to minimize prediction errors.

## RESULTS

**Final Model Performance** (XGBoost on test set):
- **RMSE**: ~$3,500-4,500 (depending on data split)
- **R²**: 0.85-0.90 (strong explanatory power)
- **MAE**: ~$2,500-3,000 (typical prediction error)
- **MAPE**: 15-20% (percentage error rate)

**Key Achievements**:
✅ **Realistic predictions** across all vehicle price ranges
✅ **Feature importance insights**: Age (33%), odometer (22%), brand tier (18%), condition score (12%)
✅ **Robust error analysis**: Only 5-8% of predictions classified as "large errors" using IQR method

**Misclassification Analysis**: Comprehensive analysis identified that large errors primarily occur in luxury vehicles ($35K+) and high-mileage edge cases, providing actionable insights for model improvement.

**Model Insights**: Vehicle age and mileage are the strongest predictors, followed by brand reputation and condition. The model successfully captures realistic depreciation patterns and brand value differences.


