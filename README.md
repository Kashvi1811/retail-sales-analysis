<![CDATA[<p align="center">
  <img src="banner.jpg" alt="Retail Sales Analysis 2019 Banner" width="100%"/>
</p>

<h1 align="center">🛒 Retail Sales Analysis 2019</h1>

<p align="center">
  <b>End-to-end Exploratory Data Analysis on 12 months of US retail sales data</b><br/>
  Uncovering revenue trends, top cities, peak buying hours, and product bundling patterns.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.x-3776AB?style=for-the-badge&logo=python&logoColor=white"/>
  <img src="https://img.shields.io/badge/Pandas-Data%20Wrangling-150458?style=for-the-badge&logo=pandas&logoColor=white"/>
  <img src="https://img.shields.io/badge/Matplotlib-Visualization-11557c?style=for-the-badge&logo=matplotlib&logoColor=white"/>
  <img src="https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?style=for-the-badge&logo=powerbi&logoColor=black"/>
  <img src="https://img.shields.io/badge/Jupyter-Notebook-F37626?style=for-the-badge&logo=jupyter&logoColor=white"/>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Records-186%2C850%20rows-blueviolet?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Period-Jan–Dec%202019-success?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Cities-9%20US%20Cities-orange?style=for-the-badge"/>
</p>

---

## 📌 Table of Contents

- [📖 Project Overview](#-project-overview)
- [📁 Repository Structure](#-repository-structure)
- [📊 Dataset Description](#-dataset-description)
- [🔍 Key Business Questions Answered](#-key-business-questions-answered)
- [🧹 Data Cleaning Steps](#-data-cleaning-steps)
- [📈 Analysis & Insights](#-analysis--insights)
- [🖥️ Power BI Dashboard](#️-power-bi-dashboard)
- [⚙️ How to Run](#️-how-to-run)
- [🛠️ Tech Stack](#️-tech-stack)
- [👩‍💻 Author](#-author)

---

## 📖 Project Overview

This project performs a full **Exploratory Data Analysis (EDA)** on a US electronics retailer's 2019 sales data. Starting from 12 separate monthly CSV files, the data was merged, cleaned, enriched with new features, and analyzed to answer 4 critical business questions using **Python (Pandas + Matplotlib)** and visualized interactively in a **Power BI dashboard**.

> 💡 The goal is to turn raw transactional data into actionable business intelligence — helping stakeholders decide *when* to advertise, *where* to focus, and *which products* to bundle.

---

## 📁 Repository Structure

```
retail-sales-analysis/
│
├── 📓 sales_data.ipynb                  # Main Jupyter Notebook (EDA + visualizations)
│
├── 📊 Retail Sales Analysis Report.pbix # Power BI interactive dashboard
│
├── 📂 Monthly Raw Data (12 files)
│   ├── Sales_January_2019.csv
│   ├── Sales_February_2019.csv
│   ├── Sales_March_2019.csv
│   ├── Sales_April_2019.csv
│   ├── Sales_May_2019.csv
│   ├── Sales_June_2019.csv
│   ├── Sales_July_2019.csv
│   ├── Sales_August_2019.csv
│   ├── Sales_September_2019.csv
│   ├── Sales_October_2019.csv
│   ├── Sales_November_2019.csv
│   └── Sales_December_2019.csv
│
├── 📄 Sales.csv                         # Merged dataset (all 12 months)
├── 📄 all_data.xlsx                     # Cleaned & enriched full dataset
│
└── 📝 README.md
```

---

## 📊 Dataset Description

Each monthly CSV file contains transactional records with the following columns:

| Column | Description | Example |
|---|---|---|
| `Order ID` | Unique identifier for each order | `176558` |
| `Product` | Name of the product sold | `USB-C Charging Cable` |
| `Quantity Ordered` | Number of units ordered | `2` |
| `Price Each` | Unit price in USD | `11.95` |
| `Order Date` | Date and time of the order | `04/19/19 08:46` |
| `Purchase Address` | Full delivery address | `917 1st St, Dallas, TX 75001` |

**After merging all 12 months:**
- 📦 **186,850 total rows**
- 🗓️ **Full year: January – December 2019**
- 🏙️ **9 US cities** covered: San Francisco, Los Angeles, New York, Boston, Atlanta, Dallas, Seattle, Portland, Austin

---

## 🔍 Key Business Questions Answered

| # | Business Question | Answer |
|---|---|---|
| 🗓️ 1 | **What was the best month for sales?** | **December** (~$4.6M) |
| 🏙️ 2 | **Which city sold the most products?** | **San Francisco, CA** |
| 🕐 3 | **What time should ads be displayed?** | **11 AM – 12 PM & 6 PM – 7 PM** |
| 🛍️ 4 | **What products are most often sold together?** | **iPhone + Lightning Charging Cable** |

---

## 🧹 Data Cleaning Steps

The raw data required several cleaning steps before analysis:

```python
# 1. Merge 12 monthly CSV files into one DataFrame
all_month_data = pd.DataFrame()
for file in files:
    monthly_data = pd.read_csv(f"{base_path}/{file}")
    all_month_data = pd.concat((all_month_data, monthly_data))

# 2. Remove rows where Order Date == "Order Date" (duplicate headers)
all_data = all_data.loc[all_data["Order Date"] != "Order Date"]

# 3. Drop rows with NaN values
all_data = all_data.dropna()

# 4. Convert Order Date to datetime
all_data["Order Date"] = pd.to_datetime(all_data["Order Date"])

# 5. Cast numeric columns to correct types
all_data["Quantity Ordered"] = all_data["Quantity Ordered"].astype("float")
all_data["Price Each"]       = all_data["Price Each"].astype("float")

# 6. Feature Engineering
all_data["Month"]  = all_data["Order Date"].dt.month_name()
all_data["Hour"]   = all_data["Order Date"].dt.hour
all_data["City"]   = all_data["Purchase Address"].str.split(',', expand=True)[1]
all_data["TSV"]    = all_data["Quantity Ordered"] * all_data["Price Each"]   # Total Sales Value
```

---

## 📈 Analysis & Insights

### 1️⃣ Best Month for Sales — *December* 🎄

A grouped bar chart of Total Sales Value (TSV) by month reveals **December** as the highest-grossing month by a significant margin, driven by Christmas shopping and holiday gifting.

```python
results = all_data.groupby("Month")["TSV"].sum().sort_values(ascending=False)
results.plot(kind='bar', color='pink')
plt.ylabel("Total Sales Value ($)")
plt.show()
```

> 🔑 **Insight:** Plan your largest promotional campaigns and inventory stock-ups for November–December.

---

### 2️⃣ Top Selling City — *San Francisco* 🌉

Sales quantity was aggregated by city (extracted from the `Purchase Address` column), and plotted as a line chart.

```python
all_data["City"] = all_data["Purchase Address"].str.split(',', expand=True)[1]
highest_sales = all_data.groupby('City')['Quantity Ordered'].sum().sort_values(ascending=False)
highest_sales.plot(kind='line', color='purple', marker='d', figsize=(15,5))
```

> 🔑 **Insight:** San Francisco and Los Angeles are the two biggest markets. Focus regional ads and delivery infrastructure there first.

---

### 3️⃣ Best Time to Show Ads — *11 AM & 7 PM* 🕐

Order frequency was analyzed by hour of the day, revealing two clear peaks:

```python
all_data['Hour'] = all_data["Order Date"].dt.hour
max_selling_time = all_data.groupby('Hour')['Quantity Ordered'].sum().sort_values(ascending=False)
max_selling_time.plot(kind='bar', color='brown')
```

> 🔑 **Insight:** Schedule digital advertisements to run **just before** peak purchase hours — around **11 AM** and **6–7 PM** — to capture the highest buying intent.

---

### 4️⃣ Products Most Often Sold Together 🛍️

Duplicate `Order ID` rows indicate items bought in the same transaction. These were grouped using `transform`:

```python
df = all_data[all_data['Order ID'].duplicated(keep=False)]
df['Grouped'] = df.groupby('Order ID')['Product'].transform(lambda x: ','.join(x))
```

> 🔑 **Insight:** Identify the most frequent product pairs to design **bundle deals** and **"frequently bought together"** recommendations. Common bundles include phones + charging cables + headphones.

---

## 🖥️ Power BI Dashboard

An interactive Power BI report (`Retail Sales Analysis Report.pbix`) is included in this repo, providing dynamic slicers and drill-downs on:

- 📅 Monthly revenue trends
- 🏙️ City-wise sales performance
- 📦 Product-level analysis
- 🕐 Hourly demand patterns

> To open it, download [Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free) and open the `.pbix` file.

---

## ⚙️ How to Run

### Prerequisites

```bash
pip install pandas matplotlib openpyxl jupyter
```

### Steps

```bash
# 1. Clone the repository
git clone https://github.com/Kashvi1811/retail-sales-analysis.git
cd retail-sales-analysis

# 2. Launch Jupyter Notebook
jupyter notebook sales_data.ipynb
```

> ⚠️ **Note:** Update the `base_path` variable in the notebook to point to your local folder path before running cells.

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| ![Python](https://img.shields.io/badge/-Python-3776AB?logo=python&logoColor=white) | Core programming language |
| ![Pandas](https://img.shields.io/badge/-Pandas-150458?logo=pandas&logoColor=white) | Data loading, merging, cleaning & transformation |
| ![Matplotlib](https://img.shields.io/badge/-Matplotlib-11557c) | Data visualization (bar, line charts) |
| ![Jupyter](https://img.shields.io/badge/-Jupyter-F37626?logo=jupyter&logoColor=white) | Interactive notebook environment |
| ![Power BI](https://img.shields.io/badge/-Power%20BI-F2C811?logo=powerbi&logoColor=black) | Interactive business intelligence dashboard |
| ![Excel](https://img.shields.io/badge/-Excel-217346?logo=microsoftexcel&logoColor=white) | Final enriched dataset export |

---

## 👩‍💻 Author

<p align="center">
  <b>Kashvi Soni</b><br/>
  <a href="https://github.com/Kashvi1811">
    <img src="https://img.shields.io/badge/GitHub-Kashvi1811-181717?style=for-the-badge&logo=github&logoColor=white"/>
  </a>
</p>

---

<p align="center">
  <i>⭐ If you found this project useful, please consider giving it a star!</i>
</p>
]]>
