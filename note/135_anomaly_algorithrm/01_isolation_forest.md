<br>
<br>

# `#01: Isolation Foreset`

<br>
<br>

## `#Use case of Anomaly Detection:`

<br>

Basically, there are two type of use case in **anomaly detection**:

### 1. When Outliers are Bad Data (Noise)

In this case, outliers are erroneous or anomalous data points that can harm the model's performance. For example, in the NYC Taxi Trip Prediction, a 5-day trip or an unusually large distance is considered an error. Our goal here is to create a robust model for normal trips, so we **identify and remove** these outliers from the dataset. This is the common use of **Anomaly Detection**.


### 2. When Outliers are the Main Target

In some cases, the anomalous data points are our primary interest. These outliers are the core output of our model. Here, we do not remove the outliers; rather, we build a model that can correctly identify them. For example:

* **Fraud Detection in Banking:** A fraudulent transaction is an outlier among hundreds of normal transactions. Our model's job is to pinpoint these outliers.
* **Medical Diagnosis:** A rare disease or an unusual health condition is an outlier among normal data points. The objective of doctors or researchers is to find these outliers.
* **Machine Fault Detection:** An unusual vibration or temperature of a machine is a sign of a potential fault, which is an outlier in the normal operational data. The model is trained to recognize this abnormal behavior.

Therefore, whether you perform **anomaly detection** depends on your project's **goal**. If the anomalous data is an error for your model, you remove it. If the anomalous data is the very thing you want to predict, you build a model that is capable of identifying it.

<br>
<br>

# ISOLATION FOREST:

Isolation Forest is an unsupervised machine learning algorithm for anomaly detection. As the name implies, Isolation Forest is an ensemble method (similar to random forest). In other words, it use the average of the predictions by several decision trees when assigning the final anomaly score to a given data point. Unlike other anomaly detection algorithms, which first define what’s “normal” and then report anything else as anomalous, Isolation Forest attempts to isolate anomalous data points from the get go


# Algorithm:

**Suppose, we had the following data points,**


![image](img/img01.png)

**The isolation forest algorithm selects a random dimension (in this case, the dimension associated with the x axis) and randomly splits(like what to read in decision tree algorithrm) the data along that dimension.**

![image](img/img02.png)

**The two resulting subspaces define their own sub tree. In this example, the cut happens to separate a lone point from the remainder of the dataset. The first level of the resulting binary tree consists of two nodes, one which will consist of the subtree of points to the left of the initial cut and the other representing the single point on the right.**

![image](img/img03.png)

**It’s important to note, the other trees in the ensemble will select different starting splits. In the following example, the first split doesn’t isolate the outlier.**

![image](img/img04.png)

`যেহেতু, outliers গুলো অনেক দূরে থাকে, তো আমরা যে random split করি,  এতে outliers গুলো tree এর leaf node হওয়ার possibility বেড়ে যাই । ফলে,  কম depth এ যে leaf node গুলো থাকে সেই node গুলোর  outliers হওয়ার possibility বেড়ে যাই ।    Isolation Forest এ আমরা অনেক গুলো tree তৈরি করি ।   আর, কোনটা একটা node outliers কিনা তা যাচাই করার জন্য আমরা anomaly score বের করি । নিচের formula দিয়ে । আর, আমরা যেহেতু অনেক গুলো tree বানাই তো আমরা average measurement নিয়ে কাজ করি ।    `


<br>

# Formulation:
The formula used to calculate the anomaly score for a given data point `x` is:

$s(x, n) = 2^{- \frac{E(h(x))}{c(n)}}$

Where:

* $s(x, n)$ is the **anomaly score**. A score close to 1 indicates a high probability of being an anomaly. A score much smaller than 0.5 indicates a normal observation.

* $E(h(x))$ is the **average path length** of instance `x` across all the isolation trees. The path length, h(x), is the number of edges (splits) that must be traversed to reach the leaf node of the instance.

* $c(n)$ is the **average path length of an unsuccessful search** in a Binary Search Tree (BST) for a dataset of size `n`. This is a normalization constant that makes the score comparable across different datasets. It is calculated using the following formula:

$c(n) = 2H(n-1) - \frac{2(n-1)}{n}$

where $H(i)$ is the harmonic number, which can be approximated as $\ln(i) + \gamma$ (where $\gamma$ is Euler's constant).

In essence, the anomaly score is an inverse relationship with the average path length. **The shorter the path length ($E(h(x))$), the higher the anomaly score** because the data point was "isolated" more easily.

<br>
<br>

# Step-by-Step Tiny Dataset Example

Use a very tiny dataset:

```
Points:
A = 1
B = 2
C = 2.2
D = 3
E = 30 (Clear anomaly)
```

So:

Normal cluster around: **1–3**
Anomaly: **30**

## Build 1(one) isolation tree manually:
`We will build one tree for intution. But in formula E{h(x)}, we will train many tree. Then we calculate Expected value.`

Tree is built by:
1. Random split feature (only one feature here)
2. Random threshold between min & max of data
3. Split into left & right
4. Continue until 1 data point remains

Let's construct an example tree:
### Level 0

`Range:` 1 to 30 <br>
`Random threshold:` 10
`Left:` [1, 2, 2.2, 3]
`Right:` [30] <br>
→ 30 becomes leaf immediately. <br>
**Path length(30) = 1** <br>
Now process left side.

### Level 1 (Left side: 1–3 range)

`Random threshold:` **2.5**
`Left:` [1, 2, 2.2]
`Right:` [3]<br>
→ 3 isolated in one more step. <br>
**Path length(3) = 2** <br>

### Level 2 (range = 1–2.2)
`Random threshold:` **1.5**
`Left:` [1]
`Right:` [2, 2.2] <br>
→ 1 isolated <br>
**Path length(1) = 3**<br>

### Level 3
`Process right:` [2, 2.2]
Random split: threshold = **2.1**
`Left:` [2]
`Right:` [2.2] <br>
**Path length(2) = 4** <br>
**Path length(2.2) = 4** <br>


## Summary of Path Lengths (Tree 1)

| Point | Value | Path length |
| ----- | ----- | ----------- |
| A     | 1     | 3           |
| B     | 2     | 4           |
| C     | 2.2   | 4           |
| D     | 3     | 2           |
| E     | 30    | 1           |

Isolation Forest uses **many trees** (usually 100).
But even from this 1 tree, we can see:

* **30 is isolated extremely fast → anomaly**
* Normal points take 2–4 steps

Expected values with many trees:

```
h(30) ≈ 1.2
h(1–3) ≈ 3.5–5.0
```


# Now, Compute the Anomaly Score:

### 1️⃣ Isolation Forest anomaly score formula

Isolation Forest এ score হিসাব হয়:

$\boxed{
s(x, n) = 2^{-\frac{h(x)}{c(n)}}
}$

where:
* (h(x)) = average path length of point `x`
* (n) = number of samples
* (c(n)) = **expected path length** for normal data


### 2️⃣ (c(n)) how to calculate this?
$c(n) = 2H(n-1) - \frac{2(n-1)}{n}$
here,
(H(k)) = Harmonic number

$H(k) \approx \ln(k) + 0.577$


### 3️⃣ In our dataset, 

Points = **5**

```
n = 5
```
then,
$H(4) \approx \ln(4) + 0.577 = 1.386 + 0.577 = 1.963$ <br>
$c(5) = 2 × 1.963 - \frac{2×4}{5}$ <br>
$c(5) = 3.926 - 1.6 = \boxed{2.326}$ <br>
📌 **This is constant**,this value is same for all points <br>


### 4️⃣ from our pervious calculation: path length (given)

| Point | Value | h(x) |
| ----- | ----- | ---- |
| A     | 1     | 3    |
| B     | 2     | 4    |
| C     | 2.2   | 4    |
| D     | 3     | 2    |
| E     | 30    | 1    |


### 5️⃣ Let's calculate Anomaly score:

#### 🔴 Point E = 30 (anomaly)
$s(30) = 2^{- \frac{1}{2.326}}$<br>
$\frac{1}{2.326} = 0.43$ <br>
$s(30) = 2^{-0.43} ≈ \boxed{0.74}$<br>
🔥 **High score → anomaly**

#### 🟢 Point D = 3
$s(3) = 2^{- \frac{2}{2.326}}$<br>
$= 2^{-0.86} ≈ \boxed{0.55}$ <br>

#### 🟢 Point A = 1
$s(1) = 2^{- \frac{3}{2.326}}$ <br>
$= 2^{-1.29} ≈ \boxed{0.41}$ <br>

#### 🟢 Point B = 2
$s(2) = 2^{- \frac{4}{2.326}}$<br>
$= 2^{-1.72} ≈ \boxed{0.30}$<br>

#### 🟢 Point C = 2.2
$s(2.2) ≈ \boxed{0.30}$

### 6️⃣ Final anomaly score table

| Value  | Path h(x) | Score       |
| ------ | --------- | ----------- |
| **30** | **1**     | **0.74 🚨** |
| 3      | 2         | 0.55        |
| 1      | 3         | 0.41        |
| 2      | 4         | 0.30        |
| 2.2    | 4         | 0.30        |

### 7️⃣ Interpretation (this is exam / interview gold)
* **Score → 1** ⇒ strong anomaly
* **Score → 0** ⇒ very normal
* Rule of thumb:

  * `score > 0.6` → anomaly
  * `~0.5` → borderline
  * `<0.4` → normal


<br>

**class sklearn.ensemble.IsolationForest()**

<br>

## Also we can find Anomaly Detection using **DBSCAN**, **Local Factor Outlier**

<br>


