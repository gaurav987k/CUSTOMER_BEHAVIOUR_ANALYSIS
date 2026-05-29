# 🛍️ Customer Shopping Behavior Analysis

> *"How can a retail company leverage consumer shopping data to identify trends, improve customer engagement, and optimize marketing and product strategies?"*

![GitHub repo size](https://img.shields.io/github/repo-size/gaurav987k/CUSTOMER_BEHAVIOUR_ANALYSIS)
![GitHub last commit](https://img.shields.io/github/last-commit/gaurav987k/CUSTOMER_BEHAVIOUR_ANALYSIS)
![License](https://img.shields.io/badge/license-MIT-blue.svg)

---

## 📌 Project Overview

A leading retail company wants to better understand its customers' shopping behavior in order to improve sales, customer satisfaction, and long-term loyalty. The management team has noticed changes in purchasing patterns across demographics, product categories, and sales channels. They are particularly interested in uncovering which factors — such as discounts, reviews, seasons, or payment preferences — drive consumer decisions and repeat purchases.

This end-to-end data analytics project analyzes **3,900 retail transactions** across 18 features using **Python, PostgreSQL, and Power BI** to answer 10 key business questions and deliver actionable recommendations.

---

## 🔄 Project Workflow

```
Business Problem
      ↓
Data Modelling & EDA in Python
      ↓
Load to PostgreSQL (SQL Analysis)
      ↓
Connect with Power BI → Interactive Dashboard
      ↓
Summarize Findings → Project Report
      ↓
Presentation (Gamma AI)
      ↓
Upload to GitHub ✅
```

---

## 🗂️ Repository Files

| File | Description |
|---|---|
| `customer_shopping_behavior.csv` | Raw dataset – 3,900 rows, 18 columns |
| `Untitled1.ipynb` | Jupyter Notebook – EDA, cleaning & feature engineering |
| `PSSQL.sql` | PostgreSQL queries answering 10 business questions |
| `CUSTOMER_BOARD.pbix` | Power BI interactive dashboard |
| `Customer Shopping Behavior Analysis.pdf` | Full written project report |
| `Customer-Shopping-Behavior-Analysis.pptx` | Final presentation slides |
| `LICENSE` | MIT License |

---

## 📊 Dataset Summary

| Property | Details |
|---|---|
| **Rows** | 3,900 |
| **Columns** | 18 |
| **Missing Data** | 37 values in `Review Rating` → imputed with category median |

### Features

| Column | Description |
|---|---|
| `Customer ID` | Unique customer identifier |
| `Age` | Customer age (18–70) |
| `Gender` | Male / Female |
| `Item Purchased` | Product name (25 unique items) |
| `Category` | Clothing, Footwear, Accessories, Outerwear |
| `Purchase Amount (USD)` | Transaction value ($20–$100) |
| `Location` | US State |
| `Size` | S / M / L / XL |
| `Color` | Product color |
| `Season` | Spring, Summer, Fall, Winter |
| `Review Rating` | 1–5 scale |
| `Subscription Status` | Yes / No |
| `Shipping Type` | Standard, Express, Free Shipping, 2-Day, Next Day Air, Store Pickup |
| `Discount Applied` | Yes / No |
| `Previous Purchases` | Count of prior purchases |
| `Payment Method` | Cash, Venmo, PayPal, Credit Card, Debit Card, Bank Transfer |
| `Frequency of Purchases` | Weekly, Fortnightly, Monthly, Every 3 Months, Annually, Quarterly |

---

## 🐍 Python – EDA & Data Preparation (`Untitled1.ipynb`)

- **Data Loading** – Imported dataset using `pandas`
- **Initial Exploration** – `df.info()` and `.describe()` for structure and statistics
- **Missing Data Handling** – Imputed 37 missing `Review Rating` values using **median per product category**
- **Column Standardization** – Renamed columns to `snake_case`
- **Feature Engineering**
  - Created `age_group` by binning ages → Young Adult / Adult / Middle-aged / Senior
  - Created `purchase_frequency_days` from frequency text data
- **Data Consistency Check** – Confirmed `discount_applied` and `promo_code_used` were redundant; dropped `promo_code_used`
- **Database Integration** – Loaded cleaned DataFrame into **PostgreSQL** via `SQLAlchemy`

---

## 🗄️ SQL – Business Analysis (`PSSQL.sql`)

10 business questions answered in **PostgreSQL**:

| # | Business Question | Key Result |
|---|---|---|
| Q1 | Total revenue by gender? | Male: **$157,890** · Female: **$75,191** |
| Q2 | Discount users who spent above average? | **839 customers** |
| Q3 | Top 5 products by avg review rating? | Gloves (3.86), Sandals (3.84), Boots (3.82), Hat (3.80), Skirt (3.78) |
| Q4 | Avg purchase: Standard vs Express shipping? | Standard: **$58.46** · Express: **$60.48** |
| Q5 | Do subscribers spend more? | Subscribers avg: **$59.49** · Non-subscribers: **$59.87** |
| Q6 | Top 5 discount-dependent products? | Hat (50%), Sneakers (49.66%), Coat (49.07%), Sweater (48.17%), Pants (47.37%) |
| Q7 | Customer segments by purchase history? | Loyal: **3,116** · Returning: **701** · New: **83** |
| Q8 | Top 3 products per category? | Jewelry, Blouse, Sandals, Jacket lead each category |
| Q9 | Repeat buyers likely to subscribe? | Non-subscribers: **2,518** repeat buyers vs Subscribers: **958** |
| Q10 | Revenue by age group? | Young Adult: **$62,143** (highest) → Senior: $55,763 |

### Sample Queries

```sql
-- Q1: Revenue by Gender
SELECT gender, SUM(purchase_amount) AS total_revenue
FROM customer
GROUP BY gender
ORDER BY total_revenue DESC;

-- Q7: Customer Segmentation
WITH customer_type AS (
  SELECT *,
    CASE
      WHEN previous_purchases BETWEEN 1 AND 5 THEN 'New'
      WHEN previous_purchases BETWEEN 6 AND 20 THEN 'Returning'
      ELSE 'Loyal'
    END AS customer_segment
  FROM customer
)
SELECT customer_segment, COUNT(*)
FROM customer_type
GROUP BY customer_segment;

-- Q8: Top 3 Products per Category (Window Function)
WITH items_counts AS (
  SELECT category, item_purchased,
    COUNT(customer_id) AS total_order,
    ROW_NUMBER() OVER(PARTITION BY category ORDER BY COUNT(customer_id) DESC) AS item_rnk
  FROM customer
  GROUP BY category, item_purchased
)
SELECT * FROM items_counts WHERE item_rnk <= 3;
```

---

## 📈 Power BI Dashboard (`CUSTOMER_BOARD.pbix`)

An interactive dashboard built in **Power BI** featuring:

| Visual | Description |
|---|---|
| 🔢 KPI Cards | Total Customers (3.9K) · Avg Purchase ($59.76) · Avg Rating (3.75) |
| 🍩 Donut Chart | Subscription Status – Yes 27% / No 73% |
| 📊 Bar Charts | Revenue by Category · Sales by Category |
| 📉 Horizontal Bars | Revenue by Age Group · Sales by Age Group |
| 🎛️ Slicers | Filter by Subscription, Gender, Category, Shipping Type |

---

## 💡 Business Recommendations

1. **📣 Boost Subscriptions** – Only 27% subscribe. Offer exclusive perks, early access, or loyalty points to grow this segment.
2. **🏆 Customer Loyalty Programs** – Reward 701 "Returning" customers to convert them to "Loyal" (already 3,116 loyal customers).
3. **💰 Review Discount Policy** – Products like Hat and Sneakers show ~50% discount rates. Audit margin impact before continuing.
4. **⭐ Product Positioning** – Highlight top-rated items (Gloves, Sandals, Boots) in all marketing campaigns.
5. **🎯 Targeted Marketing** – Focus on Young Adults (highest revenue: $62K) and Express Shipping users (highest avg spend).
6. **♀️ Female Segment Strategy** – Male customers generate 2× female revenue ($157K vs $75K). Improve female-targeted offerings and promotions.

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| **Python** (pandas, matplotlib, seaborn) | EDA, cleaning, feature engineering |
| **PostgreSQL** | SQL-based business analysis |
| **SQLAlchemy** | Python ↔ PostgreSQL integration |
| **Power BI** | Interactive dashboard & visualization |
| **Jupyter Notebook** | Development environment |
| **Gamma AI** | Presentation design |
| **GitHub** | Version control & project hosting |

---

## 🚀 How to Run This Project

### 1. Clone the Repository
```bash
git clone https://github.com/gaurav987k/CUSTOMER_BEHAVIOUR_ANALYSIS.git
cd CUSTOMER_BEHAVIOUR_ANALYSIS
```

### 2. Install Python Dependencies
```bash
pip install pandas matplotlib seaborn sqlalchemy psycopg2 jupyter
```

### 3. Run the Jupyter Notebook
```bash
jupyter notebook Untitled1.ipynb
```

### 4. Set Up PostgreSQL
- Create a database: `shopping_db`
- Update the connection string in the notebook
- Run `PSSQL.sql` in **pgAdmin** or `psql`:
```bash
psql -U postgres -d shopping_db -f PSSQL.sql
```

### 5. Open Power BI Dashboard
- Open `CUSTOMER_BOARD.pbix` in **Power BI Desktop**
- Refresh data source connection if prompted

---

## 📄 Reports & Presentation

- 📝 [Full Project Report (PDF)](Customer%20Shopping%20Behavior%20Analysis.pdf)
- 📊 [Presentation Slides (PPTX)](Customer-Shopping-Behavior-Analysis.pptx)

---

## 👤 Author

**Gaurav** · [@gaurav987k](https://github.com/gaurav987k)

---

## 📃 License

This project is open source and available under the [MIT License](LICENSE).
