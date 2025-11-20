<br>
<br>


# `#SK-Learn: Advance Topics:`

<br>

- 01: Estimators 
- 02: Custom Estimators 
- 03: Mixins
- 04: Transformers
- 05: Custom Transformers
- 06: Composite Transformers
- 07: Column Transformers
- 08: Feature Union
- 09: Pipeline 

<br>

<br>
<br>

# `#01: Estimators:`

<br>
<br>

**Object that can learn from data, Has a fit method is called estimators in sklearn. Like,**
- SVC(), Linear_Regression()
- Basically, all the ml algorithrm is called estimators in sklearn.

**There are two type of estimators, i) Predictor and ii) transformer**
- `Predictor:` Has a predit() method, that can predict on new data. like:
    - All the ml algorithrm.
    - There are two type of predictor: i) regression ii) classifications
        - classification predictor has two method:
            - predit() output: 0,1 
            - predit_proba output: (in probilities)
- `Transformer:` Has .transform() or .fit_transform() method apply on new data
    - ohe,oe(ordinal Encoder),MMS(Min-Max Scaler),SS(Standard Scaler),PCA


**Question:** In sklearn we have some estimators that has only .fit(), that is not predictor or transformer.
- All the clustering algorithrm, that cluster the data, but can't do predict or fit in new dataset.



<br>
<br>

# `#02: Custom Estimators:`

<br>
<br>


## #2.1:  **What is a Custom Estimator in scikit-learn?**

A **custom estimator** is a machine-learning model, transformer, or pipeline component that **you create yourself** but behaves exactly like a built-in scikit-learn estimator.

To be considered a valid scikit-learn estimator, your class must follow these rules:

### **Rules of a scikit-learn estimator**

1. Class name should follow PascalCase (e.g., `MyRegressor`).
2. `__init__()` must only set parameters as attributes.
3. Must implement:

   * `fit()` → trains/learns from data
   * optionally `transform()`, `predict()`, `predict_proba()`, etc.
4. Must inherit from scikit-learn **Mixin classes**.

These mixins enforce consistent behavior and allow your estimator to work with:

* `GridSearchCV`
* `RandomizedSearchCV`
* Pipelines
* `cross_val_score`
* Cloning & parameter tuning


## #2.2 **scikit-learn Mixin Classes (Simple Introduction):**
### **1. `BaseEstimator` (Foundation class)**

* Every custom estimator should inherit this.
* Provides:

  * `get_params()`
  * `set_params()`
These make your estimator compatible with GridSearchCV & Pipelines.


### **2. `ClassifierMixin`**

Use this when building a **classifier**.

It ensures your estimator implements:

* `score()` for classification (accuracy)

Example:

```python
class MyClassifier(BaseEstimator, ClassifierMixin):
    ***
```



### **3. `RegressorMixin`**

Use this when building a **regressor**.

It enforces:

* regression version of `score()` (R²)



### **4. `TransformerMixin`**

Use this when building a **transformer**, such as:

* scaling
* encoding
* feature engineering

It implements:

* `fit_transform()` automatically

So you only need:

* `fit()`
* `transform()`

### **5. `ClusterMixin`**

Use this for clustering algorithms.

It adds:

* `fit_predict()`

Typical for custom KMeans-like methods.



### **6. `DensityMixin`**

For density estimation models (like Kernel Density Estimation).

Adds:

* `score_samples()`



### **7. `OutlierMixin`**

For anomaly detection algorithms.

Adds:

* `fit_predict()`
* Negative class label convention (−1 for outlier)


### **8. `MetaEstimatorMixin`**

For estimators that wrap other estimators.
Examples:

* `GridSearchCV`
* `BaggingClassifier`
* `CalibratedClassifierCV`

Normally you won’t use this unless you're building an advanced model like your own ensemble method.


# 📌 Summary Table

| Mixin                  | Use Case              | Gives You              |
| ---------------------- | --------------------- | ---------------------- |
| **BaseEstimator**      | Required for all      | get/set parameters     |
| **ClassifierMixin**    | Classification models | accuracy-based score() |
| **RegressorMixin**     | Regression models     | R² scoring             |
| **TransformerMixin**   | Transformers          | fit_transform()        |
| **ClusterMixin**       | Clustering            | fit_predict()          |
| **DensityMixin**       | Density estimation    | score_samples()        |
| **OutlierMixin**       | Anomaly detection     | fit_predict() rules    |
| **MetaEstimatorMixin** | Wrapper/ensembles     | advanced capabilities  |




<br>
<br>

# `#03: Mixins:`

<br>
<br>

**While making custom estimator, we use mixins. BaseEstimator give us fit and predit. All the other like scoring accuracy score etc. give us by Mixin Class.**




<br>
<br>

# `#04: Transformers:`

<br>
<br>

In scikit-learn, a transformer is a specific type of **estimator** that is used for transforming datasets. Transformers are designed to pre-process data, which can include tasks such as scaling, encoding categorical variables, handiling missing values and feature extraction. The main goal of a transformer is to modify or create  new features from the original dataset in a way that makes the data more suitable for modeling by machine learning algorithrms. Example:
- OHE (One Hot Encoding).
- MMS (Min Max Scaler).
- OE (Ordinal Encoding).
- Imputer.
- PCA.

`Contains a fit and a transform(fit_transform) method.`

**Disadvantage of Transformer:**
- Can be applied only to the entire dataset.
- Can't cover every use case.


- Why `a fit and a transform(fit_transform) method`??
    - Sometimes: For transforming data we need to learn some value. like for appling MMS, We need to learn mean and median. For, this we need fit.Some transformer do not need to learn. In this case, there are two type of Transformer. i) Stateless ii) Statefull.
        - i) **Stateless or Function Approach:** Need to learn data.
        - ii) **Statefull or Class Approach:** No need to learn data.

<br>
<br>

# `#05: Custom Transformers:`

<br>
<br>

### **Function Approach:** 
like we need to do sqaure, recipocal, cuve in this can we can use fuction approach. 

```python
import numpy as np 
from sklearn.preprocessing import FunctionTransformer

def make_cuve(x):
    return x**3 

cuve = FunctionTransformer(make_cuve)

arr = np.array([12, 14 ,16])

cuve.transform(arr)

# output:
# array([1728, 2744, 4096])
# benefit: Now we can use the whole thing into sk-learn ecosystem.
```

### **Class Based Approach:**

```python
from sklearn.base import BaseEstimator, TransformerMixin
import numpy as np

class MedianIQRScaler(BaseEstimator, TransformerMixin):
    def __init__(self):
        self.medians_ = None
        self.iqr_ = None

    def fit(self, X, y=None):
        # Calculate medians and interquartile range for each feature
        self.medians_ = np.median(X, axis=0)
        Q1 = np.percentile(X, 25, axis=0)
        Q3 = np.percentile(X, 75, axis=0)
        self.iqr_ = Q3 - Q1

        # Handle case where IQR is 0 to avoid division by zero during transform
        self.iqr_[self.iqr_ == 0] = 1
        return self

    def transform(self, X):
        # Check if fit has been called
        if self.medians_ is None or self.iqr_ is None:
            raise RuntimeError("The transformer has not been fitted yet.")

        # Scale features using median and IQR learned during fit
        return (X - self.medians_) / self.iqr_



from sklearn.datasets import make_blobs
# Generate synthetic data
X, _ = make_blobs(n_samples=100, n_features=2, centers=3, random_state=42)

# Initialize the transformer
scaler = MedianIQRScaler()

# Fit the scaler to the data
scaler.fit(X)

# Transform the data
X_scaled = scaler.transform(X)

# Check the first few rows of the transformed data
print("Transformed data (first 5 rows):")
print(X_scaled[:5])
```


<br>
<br>

# `#06: Composite Transformer:`

<br>
<br>

A composite transformer refers to a transformer that is built from multiple other transformers or estimators, combining their functionalities to apply a series of transformations or processing steps in a specific way.

**Types:**
- Column Transformer
- Pipeline
- Feature Union



<br>
<br>

# `#07: Column Transformer:`

<br>
<br>

`In scikit-learn transformer we mainly faces two Problem. One is that all the transformer is not available in scikit learn. We solve this my made our own custom transormer. Second problem is that  transormer is apply in the whole dataset. We can solve this by using composite transformer.`

- We apply column transformer in a tuple:
[(),(),()]
- 1st name pass a name
- 2nd the transformation we want to apply 
- 3rd column names.

**see the code example from transformers.ipynb**

<br>

