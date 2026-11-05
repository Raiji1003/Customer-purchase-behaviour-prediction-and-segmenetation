# 🛒 Customer Purchase Behavior Prediction & Segmentation

A classical machine-learning system that predicts which customers will buy again and groups customers by purchasing behavior.
This project was built to help an online retailer decide **who to target, with what, and when**, using only transaction history.


---

## ⚠️ Key Business Challenges

* 🔁 **Repeat Purchase Uncertainty** – Shops do not know which customers will return, so marketing effort is spread too thin.
* 💸 **Wasted Discounts** – Offering a discount to a customer who would have bought anyway costs money without adding sales.
* 💤 **Silent Customer Loss** – Valuable customers can go quiet without anyone noticing.
* 🧩 **One-Size-Fits-All Marketing** – Loyal buyers, occasional buyers and one-time buyers need different actions.

---

## 🧩 Project Modules

### 🎯 1) Repeat-Purchase Prediction
* **Dataset:** UCI Online Retail II (1,067,371 rows, Dec 2009 – Dec 2011)
* **Input:** 8 customer features built only from transactions *before* a cutoff date
* **Target:** Will the customer make a valid purchase in the **next 90 days**?
* **Models:** Logistic Regression, Random Forest, XGBoost (plus a Dummy baseline)
* **Output:** Repeat ✅ / No repeat ❌ with a probability

### 👥 2) Customer Segmentation
* **Input:** Recency, frequency, monetary value, average order value, recent activity, product count
* **Model:** K-Means clustering (k = 4, chosen with elbow and silhouette)
* **Output:** 🌟 Loyal high-value · 🔄 Active regulars · 💤 Lapsed mid-value · 👋 One-time / inactive

### 💡 3) Business Rules
* 🎁 **Discount eligibility:** loyalty rule of at least 7 invoices (top 25% of customers)
* ⏳ **Dormant win-back:** loyal customers silent for more than 270 days, with a 15% no-offer control group
* 🔔 **Reorder reminder:** "due now" when days since last order are 1× to 3× the usual gap
* 🛍️ **Product suggestions:** co-purchase similarity (cosine) compared with a most-popular baseline

---

## 🧮 Features Used

| Feature | Meaning |
|---|---|
| `recency` | Days since the last purchase |
| `frequency` | Number of distinct purchase invoices |
| `monetary` | Total spending |
| `purchases_per_month` | Invoices per month of customer life |
| `n_products` | Number of distinct products bought |
| `unique_product_ratio` | Distinct products ÷ purchase lines |
| `freq_last30` | Invoices in the last 30 days |
| `freq_last90` | Invoices in the last 90 days |

🔒 `CustomerID` is used only to group transactions and is **never** a model input.

---

## 🛠️ Tech Stack

**Languages & Libraries**
* Python (NumPy, Pandas, SciPy)
* Scikit-learn (preprocessing, models, clustering, metrics)
* XGBoost (gradient-boosted trees)
* Matplotlib, Seaborn (charts)

**ML Models**
* Logistic Regression, Random Forest, XGBoost (classification)
* K-Means (segmentation), PCA (visualization only)

**Tools**
* Google Colab, Jupyter Notebook
* Git/GitHub

---

## ✨ Features

* 🕒 **No data leakage** – features use only data before the cutoff, and a code check proves it
* 📅 **Chronological split** – train Feb 2010 – Jan 2011, validation May–Jun 2011, test Sep 2011, with overlapping cutoffs removed
* 🧪 **Honest evaluation** – the test set is used once, and models are compared with a Dummy baseline
* 📈 **Multiple metrics** – accuracy, precision, recall, F1, ROC-AUC, PR-AUC and confusion matrix
* 👥 **Readable segments** – named only after reading each cluster's profile
* 📋 **Campaign list** – each customer gets a probability, segment and recommended action

---

## 📊 Results & Insights

### ✅ Validation (model selection)

| Model | Features | ROC-AUC | F1 |
|---|:---:|:---:|:---:|
| Dummy baseline | 3 | 0.500 | 0.000 |
| Logistic Regression | 8 | 0.810 | 0.650 |
| Random Forest | 8 | 0.802 | 0.641 |
| **XGBoost (selected)** | **8** | **0.814** | **0.657** |
| XGBoost (RFM only) | 3 | 0.803 | 0.646 |

### 🧪 Final test (run once, Sep 2011)

| Accuracy | Precision | Recall | F1 | ROC-AUC | PR-AUC |
|:---:|:---:|:---:|:---:|:---:|:---:|
| 0.732 | 0.738 | 0.581 | 0.650 | 0.797 | 0.768 |

![Test results](outputs/figures/final_test.png)

### 👥 Customer segments (K-Means, k = 4)

| Segment | Last order (days) | Orders | Spend (£) | Customers |
|---|:---:|:---:|:---:|:---:|
| 🌟 Loyal high-value | 16 | 13 | 4,737 | 1,189 (20.3%) |
| 🔄 Active regulars | 31 | 3 | 892 | 1,511 (25.8%) |
| 💤 Lapsed mid-value | 310 | 3 | 1,018 | 1,605 (27.4%) |
| 👋 One-time / inactive | 390 | 1 | 205 | 1,547 (26.4%) |

![Customer segments](outputs/figures/cluster_scatter.png)

### 🔍 Key insights
* ✅ Always predicting "no repeat" scores only **0.572** accuracy on test; the model reaches **0.732**.
* ✅ RFM alone already reaches about **0.80** ROC-AUC, so the extra features add a small gain that was not tested for significance.
* ✅ Segments match the classifier: average repeat probability falls from **0.79** (loyal high-value) to **0.14** (one-time / inactive).
* ✅ Co-purchase product suggestions reach a **26.4%** hit rate against **16.4%** for a most-popular baseline.
* ⚠️ Recall on test (0.581) is lower than precision because the repeat rate was higher in test (42.8%) than in validation (34.2%).

---

## 📁 Repository Structure

```
├── notebooks/        📓 Main notebook
├── outputs/figures/  📈 Saved charts
├── data/             📂 Dataset instructions 
├── requirements.txt  📦 Libraries
└── README.md
```

---

## 🚀 Getting Started

### 📌 Prerequisites
Install the required dependencies:

```
pip install -r requirements.txt
```

### ▶️ Run the project
1. 📥 Download the dataset from the [UCI page](https://archive.ics.uci.edu/dataset/502/online+retail+ii) (see `data/README.md`)
2. 📓 Open `notebooks/ML_miniproject.ipynb` in Google Colab or Jupyter
3. ✏️ Set `DATA_PATH` to the location of `online_retail_II.xlsx`
4. ▶️ Run all cells from top to bottom

[🚀 Open in Colab](https://colab.research.google.com/github/<your-username>/<repo-name>/blob/main/notebooks/ML_miniproject.ipynb)

---

## ⚠️ Limitations

* Only two years of data, so seasonal behavior is covered twice
* Repeat rate differs between validation (34.2%) and test (42.8%)
* Many customers appear in more than one partition
* Features and threshold were chosen on validation data, and no significance test was run
* Segments overlap (silhouette 0.273)
* The system does **not** show that discounts cause more purchases; that needs a real experiment

## 🔮 Future Work

* Confidence intervals and significance tests
* Threshold tuning based on business costs
* A/B test for discounts using the control group
* Comparison with hierarchical clustering
* More history to cover several seasons
