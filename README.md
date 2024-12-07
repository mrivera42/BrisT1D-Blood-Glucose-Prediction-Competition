# BrisT1D-Blood-Glucose-Prediction-Competition


### Placed 211/634


### Competition 
Using historical blood glucose readings, insulin dosage, carbohydrate intake, and smartwatch activity, predict blood glucose value 1 hour into the future. 

### Steps Taken: 

### Data 
- Dataset appeared more like cross-sectional data, with lag features premade.
- numerical features: timestamp, blood glucose, insulin dosage, carb intake, steps
- categorical features: activity type
- not all user ids in the training set are in the test set (we shouldn't rely on id number as a feature)

### Preprocessing 
- Handle categorical missing values: converted categories to numerical values. missing/na category gets mapped to -1. This supposedly makes it easier for models like xgboost to handle. 
- Handle missing numerical values: used forward fill and backward fill to fill in the values row-wise.
- Removed features with more than 80% null values

### Feature Engineering 
- time/date features: extracted hour & minute from timestamp. Leave out seconds because it was always zero.
- participant features: grouped by the participant number (p_num) and took the mean, std, and maximum of each numerical feature at time 0:00. I added features one by one to see if they improved the cross-validated score, and only added them to the notebook submission if they resulted in increase performance. The features that resulted in improvement were bg, insulin, steps, cals (hr did not show improvement).

### Model Training and Evaluation 
- Model: XGBoost Regressor
- Used 5-fold CV with default model parameters, a higher n_estimators (1000) with early stopping.
- Calculated the average RMSE across folds
- Used the average boosting rounds from early stopping as n_estimators number for final training.

# Retrain on full dataset 
- Once feature engineering was completed, and the average n_estimators was through early stopping & CV, the model was retrained from scratch on the entire training dataset.

# Feature Importance 
- Feature importance was calculated using implicit XGBoost score and SHAP values.
- According to implicit scoring, bg-0:00 was the most important feature by far, followed by some of the engineered features, surprisingly.
- The same result was observed from SHAP values.


### Discussion / Learnings 
- handling missing values improved the CV score greatly. I did not expect this since XGBoost supposedly can automatically handle categorical values and missing data. I'll have to do more research.
- engineered features provided a smaller boost. Given more time, I believe there was a lot more signal to be extracted that could have significantly improved the score.
