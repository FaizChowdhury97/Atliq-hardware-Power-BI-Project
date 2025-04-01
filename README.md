# Atliq Hardware Power BI Project



This repository contains a Power BI project based on the fictional AtliQ Hardware sales data. It involves data cleaning, modeling, DAX measures creation, and dashboard visualizations to generate actionable business insights.

---

## 📦 Dataset Overview
The project includes multiple data tables typically found in a retail hardware business:

- **sales customers**: Customer name, type, and unique customer code.
- **sales date**: Date dimension table with calendar-based attributes.
- **sales markets**: Market code, name, and zone information.
- **sales products**: Product types and product code.
- **Sales transactions**: Sales-level fact table containing transaction-level metrics.
- **Profit Target**: Helper table for target-based visuals.

---

## 🧹 Data Cleaning (Power Query)
Key cleaning operations performed:

- Removed duplicates and nulls
- Formatted date fields
- Transformed sales amount into normalized currency
- Capitalized and standardized text columns
- Added month and year metadata columns

---

## 🧠 Data Model
A star schema model was used with proper 1: many relationships:

![image](https://github.com/user-attachments/assets/89d5e1bc-7d51-44fe-bbef-54e903f5fe7c)


**Relationships:**
- `sales customers[customer_code]` ➡ `Sales transactions[customer_code]`
- `sales products[product_code]` ➡ `Sales transactions[product_code]`
- `sales markets[markets_code]` ➡ `Sales transactions[market_code]`
- `sales date[date]` ➡ `Sales transactions[order_date]`

---

## 📊 DAX Measures

### Base Measures
```dax
Revenue = SUM('Sales transactions'[norm_sales_amount])
Sales Qty = SUM('Sales transactions'[sales_qty])
Total Profit Margin = SUM('Sales transactions'[Profit_Margin])
```

### Contribution & Margin Analysis
```dax
Profit Margin % = DIVIDE([Total Profit Margin],[Revenue],0)

Profit Margin Contribution % = 
DIVIDE([Total Profit Margin],
    CALCULATE([Total Profit Margin],
        ALL('sales products'),
        ALL('sales customers'),
        ALL('sales markets')
    )
)

Revenue Contribution % = 
DIVIDE([Revenue],
    CALCULATE([Revenue],
        ALL('sales products'),
        ALL('sales customers'),
        ALL('sales markets')
    )
)

Revenue LY = CALCULATE([Revenue], SAMEPERIODLASTYEAR('sales date'[date]))
```

### Profit Target Logic
```dax
Profit Target = GENERATESERIES(-0.05, 0.15, 0.01)
Profit Target Value = SELECTEDVALUE('Profit Target'[Profit Target])
Target Diff = [Profit Margin %] - 'Profit Target'[Profit Target Value]
```

---

## 📈 Visualizations

### Key Insights
---
![image](https://github.com/user-attachments/assets/e2a18e14-4bd1-4cd9-8e7b-1aa57eff5c33)

### Profit Analysis
---
![image](https://github.com/user-attachments/assets/1488bf36-2986-4f3c-871a-96902d11ab6f)

### Profit Analysis
---
![image](https://github.com/user-attachments/assets/dfee4b16-4c8f-4e70-8301-1afa7946803a)


