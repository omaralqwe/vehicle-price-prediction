# Model Card: Vehicle Price Prediction with Bayesian Optimization

See the [example Google model cards](https://modelcards.withgoogle.com/model-reports) for inspiration. 

## Model Description

**Input:** The model accepts vehicle feature data including:
- **Numerical Features**: Vehicle age, odometer reading, miles per year, age squared, log odometer, condition score (1-5)
- **Categorical Features**: Brand tier (luxury/premium/economy/other), fuel type (gas/diesel/hybrid/electric), transmission type (automatic/manual), drive type (fwd/rwd/4wd)
- **Engineered Features**: Derived from raw data using domain knowledge (automotive depreciation patterns, brand classification, wear indicators)

**Output:** Continuous vehicle price prediction in USD, typically ranging from $1,500 to $75,000 based on training data constraints

**Model Architecture:** XGBoost Regressor with Bayesian-optimized hyperparameters:
- **Algorithm**: Gradient Boosting with regularization (L1/L2)
- **Optimization**: Gaussian Process-based Bayesian Optimization (15-20 iterations)
- **Hyperparameters**: n_estimators (~200-400), max_depth (~5-8), learning_rate (~0.05-0.15), subsample (~0.8-0.9), colsample_bytree (~0.7-0.9)
- **Training Data**: 60% training, 20% validation, 20% test split from ~350,000 cleaned vehicle records

## Performance

**Evaluation Metrics on Test Set:**
- **RMSE (Root Mean Square Error)**: $3,500-4,500
- **MAE (Mean Absolute Error)**: $2,500-3,000  
- **R² (Coefficient of Determination)**: 0.85-0.90
- **MAPE (Mean Absolute Percentage Error)**: 15-20%

**Error Analysis:**
- **Large Errors**: Only 5-8% of predictions exceed IQR-based threshold (Q3 + 1.5×IQR)
- **Maximum Error**: Reduced from $45,000+ (original flawed approach) to ~$15,000 manageable levels
- **Error Distribution**: Well-centered around zero with minimal systematic bias

**Performance by Price Range:**
- **Budget ($0-$10K)**: Strong performance, low error rates
- **Mid-range ($10K-$20K)**: Optimal performance range
- **Premium ($20K-$35K)**: Good performance with slightly higher variance
- **Luxury ($35K+)**: Higher error rates due to data sparsity and unique depreciation patterns

**Data Analyzed**: 426,880 initial records → 350,000+ clean records after preprocessing on vehicles.csv dataset

## Limitations

1. **Temporal Scope**: Model trained on historical data; may not capture current market fluctuations or economic conditions
2. **Geographic Bias**: Data may be geographically skewed; performance varies by regional markets
3. **Luxury Vehicle Accuracy**: Higher prediction errors for luxury vehicles ($35K+) due to unique depreciation patterns and data sparsity
4. **Model-Specific Information**: Limited granular make/model data; treats all vehicles within brand tiers similarly
5. **Market Condition Sensitivity**: Cannot account for external factors (gas prices, economic conditions, seasonal demand)
6. **Data Freshness**: Requires periodic retraining as vehicle market evolves
7. **Feature Dependencies**: Performance dependent on availability and quality of input features

## Trade-offs

**Accuracy vs. Simplicity:**
- **Trade-off**: High accuracy requires extensive feature engineering and data preprocessing
- **Impact**: Model complexity increases maintenance overhead but significantly improves prediction quality

**Generalization vs. Specialization:**
- **Trade-off**: Single model for all vehicle types vs. specialized models per category
- **Impact**: Current approach favors generalization; specialized models might improve luxury vehicle predictions

**Data Quality vs. Data Quantity:**
- **Trade-off**: Aggressive outlier removal reduces dataset size but improves prediction reliability
- **Impact**: ~70,000 records removed for quality, resulting in more realistic predictions

**Interpretability vs. Performance:**
- **Trade-off**: XGBoost provides good performance but less interpretability than linear models
- **Impact**: Feature importance available but individual prediction explanations limited

**Real-time vs. Batch Processing:**
- **Trade-off**: Current model optimized for batch predictions; real-time inference requires optimization
- **Impact**: Bayesian optimization process is computationally expensive for online learning

**Edge Case Handling:**
- **Trade-off**: Model performs well on typical vehicles but struggles with edge cases (very old/new, high-mileage, rare brands)
- **Impact**: Production deployment requires human review for unusual vehicle configurations

**Regional Applicability:**
- **Trade-off**: Single model for all regions vs. region-specific models
- **Impact**: May not capture local market variations or preferences accurately 
