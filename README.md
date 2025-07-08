# 🔥 Algerian Forest Fire Index Prediction with Ridge Regression

## 📊 Project Summary

This project aims to **predict the Fire Weather Index (FWI)** using environmental and meteorological features from Algerian forest fire data. The FWI is a crucial indicator for assessing wildfire risk, helping authorities and responders plan prevention and response strategies.

---

## 🗂️ Dataset Description

- **Key Features Used:**  
  - `Temperature`, `Rain`, `FFMC` (Fine Fuel Moisture Code), `ISI` (Initial Spread Index), `RH` (Relative Humidity), `Ws` (Wind Speed), `DMC`, `DC`, `BUI`, and `FWI` (target).
- **Dropped Features:**  
  - Categorical columns like `Region` and `Classes` were excluded to focus on numerical modeling and avoid the need for encoding or risk of data leakage.
- **Cyclical Encoding:**  
  - The `day` and `month` columns were transformed into their **sine and cosine representations** to capture the cyclical nature of dates (e.g., December ≈ January).

---

## ⚙️ Preprocessing Steps

1. **Log Transform on Right-Skewed Features:**  
   - Applied `np.log1p` to variables like `Rain` and other right-skewed features for normalization.
2. **Inversion & Transform for Left-Skewed Features:**  
   - Features such as `FFMC` were inverted and log-transformed to address left-skewness and improve model fit.
3. **Polynomial Feature Generation:**  
   - Created polynomial features (squared, cubed) for `FFMC` to capture non-linear relationships.
4. **Scaling with StandardScaler:**  
   - All numeric features were scaled **after train-test split** using `StandardScaler` (fit on train; transform on both train and test).
5. **Feature Replacement:**  
   - After transformation, original features like `Rain` were removed and replaced by their transformed versions (e.g., `Rain_Log`).

---

## 🤖 Modeling Approach

- **Ridge Regression** was chosen over basic Linear Regression because the latter showed signs of overfitting and struggled with correlated features.
- **Alpha Tuning:**  
  - Both manual tuning (testing different alpha values) and automated cross-validation (`RidgeCV`) were used to select the best regularization strength.
  - **RidgeCV** with cross-validated alpha provided more reliable and generalizable performance compared to the best manual alpha.

---

## 📈 Performance Metrics

- **R² Score:** 0.86
- **Adjusted R² Score:** 0.85
- **RMSE:** 6.4
- **MAE:** 4.9

A scatter plot comparing Actual vs Predicted FWI values demonstrates the model's robust predictive power:

![Actual vs. Predicted FWI](plots/actual_vs_predicted.png)

> *Blue: perfect prediction line (y = x); Orange: actual regression fit.*

---

## 🚩 Beginner Mistakes & Fixes

- **Transformed features before splitting** the dataset (leakage):  
  _Fixed by splitting first, then transforming._
- **Scaled test data using fit_transform()**:  
  _Now use `fit_transform()` on train, `transform()` on test._
- **Uncertainty about scaling polynomial features**:  
  _Resolved by scaling them with the rest of the numerical data._
- **Unclear if sin/cos encodings need scaling**:  
  _Skipped scaling since their values are already in [-1, 1]._
- **Confused by high R² at low alpha**:  
  _Learned that cross-validation R² (with alpha=1.0) is more robust and generalizes better._

---

## 💡 Results & Insights

- **RidgeCV** achieved high R² and generalization.
- Careful handling of skewness and correct preprocessing order were critical.
- Polynomial features and Ridge regularization delivered a strong boost in accuracy.

---

## 🚀 Next Steps

- Explore **Lasso** and **ElasticNet** for feature selection and alternative regularization.
- Build a unified **Pipeline** for all preprocessing and modeling steps.
- Try classifying FWI into risk categories for a classification approach.

---

## 📚 Resources

- [Original Dataset (UCI ML Repo)](https://archive.ics.uci.edu/dataset/610/algerian+forest+fires+dataset)

---

> **Made with ❤️ by Dibyo Chakraborty**
