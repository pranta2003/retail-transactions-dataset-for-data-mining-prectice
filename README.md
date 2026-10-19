<div align="center">

# 🛒 Retail Transactions Data Mining & Market Basket Analysis

<img src="https://komarev.com/ghpvc/?username=pranta2003-retail-data-mining&color=blueviolet&style=flat-square" alt="Profile Views" />
<img src="https://img.shields.io/badge/Python-3.10%2B-blue?style=flat-square&logo=python" alt="Python" />
<img src="https://img.shields.io/badge/Data%20Mining-Association%20Rules-orange?style=flat-square&logo=databricks" alt="Data Mining" />
<img src="https://img.shields.io/badge/Dataset-1M%20Transactions-success?style=flat-square&logo=kaggle" alt="Dataset" />
<img src="https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square" alt="License" />

### *An advanced exploratory data mining and market basket analysis lab report built on a 1,000,000-row retail transaction dataset.*

</div>

---

## 🎯 1. Objective: What Association Rule Mining Discovers
The primary objective of this experiment is to perform **Market Basket Analysis** using **Association Rule Mining** to discover hidden buying patterns, co-occurrence correlations, and item affinities in large retail transaction databases. It answers the fundamental retail question: 
> *"If a customer purchases Item A, how likely are they to also purchase Item B during the same shopping trip?"* ($A \rightarrow B$).

---

## 📊 2. Dataset: Structure and Scale
* **Source:** [Kaggle - Retail Transactions Dataset](https://www.kaggle.com/datasets/prasad22/retail-transactions-dataset)[cite: 1]
* **Total Transactions (Rows):** `1,000,000`
* **Attributes (Columns):** `13`
* **Key Columns:**
  * `Transaction_ID`: Unique 10-digit identifier for each checkout session.
  * `Product`: String-formatted array containing the list of items purchased per basket (e.g., `['Ketchup', 'Shaving Cream', 'Light Bulbs']`).
  * `Total_Items` & `Total_Cost`: Quantitative and monetary summary of the basket.
  * *Contextual Metadata:* `Date`, `Customer_Name`, `Payment_Method`, `City`, `Store_Type`, `Discount_Applied`, `Customer_Category`, `Season`, and `Promotion`.

---

## ⚙️ 3. Preprocessing: Boolean One-Hot Transformation
To prepare text-based transaction lists for pattern mining algorithms, a robust data cleaning and vectorization pipeline was executed across all **1,000,000 rows**:
1. **String Parsing & Cleaning:** Converted string representations of lists into actual Python objects using `ast.literal_eval()`, stripping trailing whitespace.
2. **Intra-Basket Deduplication:** Converted baskets into Python `set` collections to evaluate unique item presence. *(Note: Analysis revealed `202,256` single-item baskets [20.23%] and `797,744` multi-item baskets [79.77%] with an average of `2.95` unique items per basket).*
3. **Vocabulary Extraction:** Mapped exactly **81 unique retail products**.
4. **Boolean Matrix Construction:** Generated a memory-efficient $1,000,000 \times 81$ One-Hot encoded Boolean DataFrame (`df_bool`), occupying only **77.25 MB** of RAM.

---

## 📈 4. Frequent Itemsets: Multi-Threshold Support Analysis
Using ultra-fast vectorised matrix multiplication ($X^T X$), we analyzed frequency distributions across support thresholds:
* **Threshold 1 (`min_support = 0.0370` / 3.70%):** Found **1 frequent itemset**—only `(Toothpaste)` qualified with `0.070949` support (`70,949` occurrences), appearing nearly twice as often as any other product.
* **Threshold 2 (`min_support = 0.0350` / 3.50%):** Found **81 frequent itemsets** (all single items, whose baseline support clustered around ~3.60%).
* **Threshold 3 (`min_support = 0.0011` / 0.11%):** Found **3,193 frequent itemsets** (capturing all 81 single items and **3,112 out of 3,240** possible 2-item pairs, led by `(Tomatoes, Toothpaste)` at `0.002412` support).

---

## 📐 5. Association Rules: Support, Confidence, and Lift
Evaluated **6,480 directed 2-item rules** and **3-item rules** using Support, Confidence, and Lift:
* **Top Confidence Rule:** `{Tomatoes} -> {Toothpaste}` (Support: `0.002412`, Confidence: `6.70%`, Lift: `0.9438`).
* **Top 2-Item Lift Rule:** `{Butter} -> {Banana}` (Support: `0.001295`, Confidence: `3.61%`, Lift: `0.9975`).
* **The "Slot Competition" Insight & 3-Item Positive Lift:** Because average basket size is small (`2.95` items), 2-item rules exhibit $\text{Lift} < 1.0$ due to finite slot competition. However, conditioning rules on 2-item antecedents (`{A, B} -> {C}`) successfully unlocked **15 3-item rules with $\text{Lift} > 1.0$**, peaking at **`1.124`** for `{Soap, Toothpaste} -> {Dustpan}`.

---

## 💡 6. Discussion: Business Insights & Limitations
* **Anchor Product Strategy:** Treat `Toothpaste` (7.095% frequency) as a high-visibility traffic anchor to promote adjacent personal care items (`Deodorant`, `Soap`).
* **Cross-Merchandising Bundles:** Leverage natural pairings like `{Butter} -> {Banana}` and `{Feminine Hygiene Products} -> {Deodorant}` for checkout promotions.
* **Dataset Limitations:** Synthetic random uniform sampling across 82 slots created baseline independence where Lift hovers near 1.0 until conditioned on multi-item baskets.

---

## 🏆 7. Conclusion
This project successfully implemented scalable Association Rule Mining on 1,000,000 retail transactions. By overcoming synthetic data anomalies and validating metrics with linear algebra and conditional filtering, we uncovered actionable retail insights and rigorous data mining mechanics.

---

## 📂 Repository Structure
```text
📦 retail-transactions-dataset-for-data-mining-practice
 ┣ 📜 lab-dm.ipynb         # Complete Python Jupyter Notebook containing all tasks & code
 ┗ 📜 README.md            # Project documentation and lab report summary
