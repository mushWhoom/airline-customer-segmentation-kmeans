# airline-customer-segmentation-kmeans
Airline customer segmentation using K-Means clustering to identify churn risk and loyalty segments. Includes EDA, feature engineering, model evaluation, and business recommendations.

# ✈️ Airline Customer Segmentation & Churn Risk Analysis with K-Means

> Customer segmentation of airline passengers using **K-Means Clustering** to identify **churn risk** and **loyalty segments**, with actionable business recommendations for marketing, retention, and service development.

---

## 📖 Table of Contents

- [Project Overview](#-project-overview)
- [Dataset](#-dataset)
- [Tech Stack](#-tech-stack)
- [Methodology](#-methodology)
- [Key Results](#-key-results)
- [Business Recommendations](#-business-recommendations)
- [Project Structure](#-project-structure)
- [How to Run](#-how-to-run)
- [Reflection](#-reflection)
- [Author](#-author)
- [License](#-license)

---

## 📌 Project Overview

This project performs **customer segmentation** on an airline passenger dataset using **K-Means Clustering** — an unsupervised machine learning algorithm. The goal is to identify distinct groups of customers based on their flight behavior, loyalty, and engagement, and to provide **data-driven business recommendations** for targeted marketing and customer retention.

The analysis follows the **LRFM framework**:

| Dimension | Meaning | Example Features |
|---|---|---|
| **L**oyalty | How long & how deep is the customer's relationship | `MEMBERSHIP_DAYS`, `FFP_TIER` |
| **R**ecency | How recently did the customer fly | `LAST_TO_END` |
| **F**requency | How often does the customer fly | `FLIGHT_COUNT`, `SEG_KM_SUM` |
| **M**onetary | How much value does the customer bring | `BP_SUM`, `Points_Sum`, `avg_discount` |

**Main objective:** Detect **churn risk** (customers likely to stop using the airline's services) and identify **high-value loyal customers**.

---

## 📂 Dataset

- **File:** `flight.csv`
- **Total Records:** 62,988 rows
- **Total Features:** 16 (after preprocessing)
- **Original Columns:** `FFP_DATE`, `FIRST_FLIGHT_DATE`, `LOAD_TIME`, `FFP_TIER`, `AGE`, `FLIGHT_COUNT`, `BP_SUM`, `SUM_YR_1`, `SUM_YR_2`, `SEG_KM_SUM`, `LAST_TO_END`, `AVG_INTERVAL`, `MAX_INTERVAL`, `EXCHANGE_COUNT`, `avg_discount`, `Points_Sum`, `Point_NotFlight`, `GENDER`, `WORK_CITY`, `WORK_PROVINCE`, `WORK_COUNTRY`

> ⚠️ **Note:** The dataset is **not included** in this repository due to size limitations. Place your own `flight.csv` in the project root before running the notebook.

---

## 🛠️ Tech Stack

| Category | Tools |
|---|---|
| **Language** | Python 3.8+ |
| **Data Manipulation** | Pandas, NumPy |
| **Visualization** | Matplotlib, Seaborn |
| **Machine Learning** | scikit-learn (`KMeans`, `StandardScaler`, `LabelEncoder`) |
| **Evaluation** | `silhouette_score`, `davies_bouldin_score`, `calinski_harabasz_score` |
| **Environment** | Google Colab / Jupyter Notebook |

---

## 🔍 Methodology

The project is structured into 8 major stages:

### 1. Data Loading & Inspection
- Load `flight.csv` using Pandas
- Inspect shape, data types, and missing values

### 2. Missing Value Imputation
| Column | Method | Reason |
|---|---|---|
| `AGE` | Median | Numerical, robust to outliers |
| `WORK_CITY`, `WORK_PROVINCE`, `WORK_COUNTRY` | `'unknown'` | Categorical |
| `GENDER` | `'unknown'` | Categorical |

### 3. Feature Engineering
- Convert date columns (`FFP_DATE`, `FIRST_FLIGHT_DATE`, `LOAD_TIME`) to `datetime`
- Create `MEMBERSHIP_DAYS` = `LOAD_TIME` − `FFP_DATE`
- Encode `GENDER` using `LabelEncoder`

### 4. Feature Scaling
- Apply `StandardScaler` to all numerical features
- **Critical** because K-Means uses Euclidean distance — features with larger scales would dominate the clustering

### 5. Exploratory Data Analysis (EDA)
- Descriptive statistics
- Distribution plots (histograms with KDE)
- Boxplots for outlier detection
- Correlation heatmap
- Scatter plots for feature relationships
- Skewness & high-correlation identification (|r| > 0.8)

### 6. Optimal `k` Selection
Evaluated `k = 2` to `k = 10` using three metrics:
- **Elbow Method** (inertia)
- **Silhouette Score** (higher is better)
- **Davies-Bouldin Index** (lower is better)
- **Calinski-Harabasz Index** (higher is better)

### 7. K-Means Clustering
- Final model: **`k = 2`** (best Silhouette Score = **0.4786**)
- Add cluster labels to the original dataframe

### 8. Cluster Profiling & Evaluation
- Scatter plots (2D & 3D)
- Pairplot
- Boxplots per cluster
- Heatmap of mean feature values per cluster

---

## 📊 Key Results

The clustering produced **two distinct customer segments**:

### 🔵 Cluster 0: "The Occasional Travelers" (Mass Customers) — ~85%

| Metric | Value |
|---|---|
| Flight Frequency | ~7 flights |
| Total Distance | ~10,000 km |
| Recency (`LAST_TO_END`) | > 200 days (inactive) |
| Loyalty (`FFP_TIER`) | 4 (base level) |
| `EXCHANGE_COUNT` | ≈ 0 |
| Joined | ~2010 |

**→ High churn risk, price-sensitive, seasonal travelers.**

### 🟢 Cluster 1: "The Frequent Flyers" (Elite/VIP Customers) — ~15%

| Metric | Value |
|---|---|
| Flight Frequency | ~40 flights |
| Total Distance | ~56,000 km |
| Recency (`LAST_TO_END`) | ~73 days (active) |
| Loyalty (`FFP_TIER`) | 4.6 – 5 |
| `Points_Sum` | 6× higher than Cluster 0 |
| Joined | ~2008 |

**→ Primary revenue contributors, loyal, active, engaged.**

### 🏆 Model Evaluation Metrics

| Metric | Value | Interpretation |
|---|---|---|
| **Silhouette Score** | 0.4786 | Moderate cluster separation ✅ |
| **Davies-Bouldin Index** | 1.2039 | Acceptable (lower is better) ✅ |
| **Calinski-Harabasz Index** | 19845.46 | Strong cluster separation ✅ |

---

## 💼 Business Recommendations

Based on the segment characteristics, the following strategies are recommended:

| Aspect | Cluster 0 (Mass) | Cluster 1 (Elite) |
|---|---|---|
| **🎯 Marketing** | Flash sales, discount campaigns, email blasts, social media ads | Personalized offers, lounge access, priority boarding, premium branding |
| **🔄 Retention** | Win-back programs, discount vouchers for inactive users | Double miles, status protection, exclusive rewards |
| **🛠️ Product** | Economy bundles (ticket + baggage + meals) | Free rescheduling, airport transfers, premium services |
| **🤝 Partnership** | E-commerce, retail banks (0% installments), travel platforms | 5-star hotels, premium credit cards, luxury car rentals |

### 🎯 Actionable Insights

1. **Re-engage Cluster 0** with targeted win-back campaigns — they represent 85% of the customer base but show strong churn signals.
2. **Retain Cluster 1** with VIP treatment — they contribute disproportionately to revenue.
3. **Personalize communication** based on segment behavior — price sensitivity vs. convenience.
4. **Monitor `LAST_TO_END`** as an early churn indicator for proactive intervention.

---

## 📁 Project Structure

```
airline-customer-segmentation-kmeans/
│
├── Airline_Customer_Segmentation.ipynb    # Main notebook
├── README.md                              # This file
├── requirements.txt                       # Dependencies
├── .gitignore                             # Git ignore rules
└── images/                                # Visualization exports
    ├── distribution_plots.png
    ├── boxplots.png
    ├── correlation_heatmap.png
    ├── scatter_plots.png
    ├── elbow_silhouette.png
    ├── cluster_scatter.png
    ├── pairplot.png
    ├── cluster_boxplots.png
    ├── cluster_heatmap.png
    └── cluster_3d.png
```

---

## 🚀 How to Run

### 1. Clone the repository
```bash
git clone https://github.com/your-username/airline-customer-segmentation-kmeans.git
cd airline-customer-segmentation-kmeans
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

Or manually:
```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
```

### 3. Prepare the dataset
Place `flight.csv` in the project root directory.

### 4. Run the notebook
```bash
jupyter notebook Airline_Customer_Segmentation.ipynb
```

Or open directly in **Google Colab**:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/your-username/airline-customer-segmentation-kmeans/blob/main/Airline_Customer_Segmentation.ipynb)

---

## 📝 Requirements

```txt
pandas>=1.3.0
numpy>=1.21.0
matplotlib>=3.4.0
seaborn>=0.11.0
scikit-learn>=1.0.0
jupyter>=1.0.0
```

---

## 🧠 Reflection

### Why is preprocessing + EDA essential before clustering?

K-Means is highly **sensitive to feature scales and outliers**. Without proper preprocessing:
- Features with large numeric ranges (e.g., `SEG_KM_SUM` in tens of thousands) would **dominate** distance calculations, drowning out smaller-scale but equally important features like `EXCHANGE_COUNT`.
- Outliers would pull cluster centroids away from true group centers.
- Missing values would break the algorithm entirely.

EDA helps us:
- Understand data distribution and detect skewness
- Identify highly correlated features (redundancy)
- Choose relevant features for clustering
- Select the optimal number of clusters through visual inspection

### How does clustering help real business decisions?

Clustering transforms raw customer data into **actionable segments**. Instead of treating all 62,988 customers the same, the airline can:
- **Personalize** marketing at scale
- **Optimize** marketing budget allocation
- **Identify churn risk** before customers leave
- **Design** products and services for specific segments

---

## 📜 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgements

- Dataset provided as part of an academic data science assignment.
- Inspired by real-world airline loyalty programs and churn analysis frameworks.
- Built with ❤️ using Google Colab.

---

## 👤 Author

**Elmira Muntaz**

- 📧 Email: [elmuntazzz@gmail.com](mailto:elmuntazzz@gmail.com)
- 🐙 GitHub: [@elmuntazzz](https://github.com/elmuntazzz)
- 💼 LinkedIn: [Elmira Muntaz](https://linkedin.com/in/your-profile)

---

## ⭐ If you found this project helpful, please give it a star!

[![Star this repo](https://img.shields.io/github/stars/your-username/airline-customer-segmentation-kmeans?style=social)](https://github.com/your-username/airline-customer-segmentation-kmeans)

---

<p align="center">
  <i>Made with 🐍 Python & ☕ Coffee</i>
</p>
