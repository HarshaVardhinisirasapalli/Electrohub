# ⚡ ElectroHub Sales Analysis — Power BI Dashboard

A comprehensive Power BI sales analytics dashboard for **ElectroHub**, a multi-category electronics and lifestyle retail store. This project transforms raw transactional data into interactive, insight-driven reports covering sales performance, customer behavior, discount analysis, and geographic distribution.

---

## 📁 Project Structure

```
ElectroHub-PowerBI/
│
├── ElectroHub.pbix               ← Main Power BI report file
├── README.md                     ← Project documentation
└── Data/
    ├── Fact Table.csv            ← Transactional sales data
    ├── Dim_Customers.csv         ← Customer dimension table
    ├── Dim_Product.csv           ← Product dimension table
    └── Dim_Promotion.csv         ← Promotion dimension table
```

---

## 🗂️ Data Model

The project follows a **Star Schema** with one Fact Table and three Dimension Tables:

| Table | Type | Description |
|-------|------|-------------|
| `Fact Table` | Fact | Core transactional data (orders, sales, discounts) |
| `Dim_Customers` | Dimension | Customer details (City, State, Contact Info) |
| `Dim_Product` | Dimension | Product details (Name, Line, Price) |
| `Dim_Promotion` | Dimension | Promotion/discount category details |

### Key Columns in Fact Table

| Column | Description |
|--------|-------------|
| Date | Order date |
| CustomerID | Links to Dim_Customers |
| Product ID | Links to Dim_Product |
| PromotionID | Links to Dim_Promotion |
| Units Sold | Quantity of items sold |
| Price Per Unit | Unit selling price (INR) |
| Total Sales | Units Sold × Price Per Unit |
| Discount Percentage | Discount % applied |
| Discount Value | Total Sales × Discount% / 100 |
| Net Sales | Total Sales − Discount Value |

---

## 🛒 Product Categories

ElectroHub sells across **8 product categories**:

- Electronics
- Footwear
- Clothing
- Home Appliances
- Accessories
- Kitchenware
- Bags
- Personal Care

---

## 📊 Dashboard Requirements & Implementation

### Requirement 1 — Top/Bottom 5 Products by Sales, Profit & Quantity
- **Visual:** Horizontal Bar Chart
- **Filter:** Top N filter applied (Top 5 / Bottom 5)
- **Fields:** Product Name (Y-axis), Sum of Total Sales / Net Sales / Units Sold (X-axis)
- **How:** Filters pane → Top N → By value: Sum of Total Sales

---

### Requirement 2 — Sales Trends Over Time
- **Visual:** Line Chart
- **Fields:** Date hierarchy (Daily → Monthly → Quarterly → Annually) on X-axis, Sum of Total Sales on Y-axis
- **Feature:** Drill-down enabled for time hierarchy navigation

---

### Requirement 3 — Relationship Between Sales & Profit
- **Visual:** Scatter Chart
- **Fields:** Total Sales (X-axis), Net Sales/Profit (Y-axis), Product Name (Details)
- **Insight:** Shows correlation between revenue and profitability per product

---

### Requirement 4 — Compare Two User-Selected Periods
- **Visual:** Clustered Column Chart + 2 Date Slicers
- **DAX Measures Used:**

```dax
Period 1 Sales =
CALCULATE(
    SUM('Fact Table'[Total Sales]),
    DATESBETWEEN('Fact Table'[Date (dd/mm/yyyy)],
        MIN('Period1 Dates'[Date]),
        MAX('Period1 Dates'[Date]))
)

Period 2 Sales =
CALCULATE(
    SUM('Fact Table'[Total Sales]),
    DATESBETWEEN('Fact Table'[Date (dd/mm/yyyy)],
        MIN('Period2 Dates'[Date]),
        MAX('Period2 Dates'[Date]))
)
```

- Same pattern repeated for **Profit** (Net Sales) and **Quantity** (Units Sold)
- Two disconnected date tables (`Period1 Dates`, `Period2 Dates`) created using `CALENDARAUTO()`

---

### Requirement 5 — Average Discount by Discount Category
- **Visual:** Bar Chart or Matrix
- **Fields:** Promotion Category (Axis), Average of Discount Percentage (Values)
- **DAX:**

```dax
Avg Discount = AVERAGE('Fact Table'[Discount Percentage])
```

---

### Requirement 6 — Total Number of Orders
- **Visual:** Card Visual
- **DAX:**

```dax
Total Orders = COUNTROWS('Fact Table')
```

---

### Requirement 7 — Order Detail Table with Visual Filters
- **Visual:** Table Visual
- **Fields included:** Date, Customer ID, Product Name, Units Sold, Price Per Unit, Total Sales, Discount Percentage, Discount Value, Net Sales, Promotion ID
- **Slicers (Filters):**

| Slicer | Field | Source Table |
|--------|-------|--------------|
| Filter by Product | Product Name | Dim_Product |
| Filter by Date | Date | Fact Table |
| Filter by Customer | Customer ID | Dim_Customers |
| Filter by Promotion | Promotion Name | Dim_Promotion |

---

### Requirement 8 — Sales by City (Map Visual)
- **Visual:** Map / Filled Map Visual
- **Fields:** City or State (Location), Sum of Total Sales (Bubble Size)
- **Setup:** Enable map visuals via File → Options → Security → ✅ Use Map and Filled Map visuals

---

## 🧮 Key DAX Measures

```dax
-- Discount Value (Calculated Column)
Discount Value = 'Fact Table'[Total Sales] * 'Fact Table'[Discount Percentage] / 100

-- Net Sales (Calculated Column)
Net Sales = 'Fact Table'[Total Sales] - 'Fact Table'[Discount Value]

-- Total Orders
Total Orders = COUNTROWS('Fact Table')

-- Average Discount
Avg Discount = AVERAGE('Fact Table'[Discount Percentage])

-- Period Comparison (see Requirement 4 above)
```

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|------|---------|
| Power BI Desktop | Report building & visualization |
| Power Query Editor | Data transformation & cleaning |
| DAX | Calculated columns & measures |
| Star Schema | Data modeling approach |

---

## 🚀 How to Run the Project

1. Download and install **Power BI Desktop** from [microsoft.com/power-bi](https://powerbi.microsoft.com)
2. Clone or download this repository
3. Open `ElectroHub.pbix` in Power BI Desktop
4. If prompted, update the data source path to your local `Data/` folder
5. Click **Refresh** to load the latest data
6. Explore the report pages using the slicers and filters

---

## 📌 Notes

- All monetary values are in **INR (₹)**
- Dataset contains **3,510 rows** of transactional data
- Date range spans **2020 to 2023**
- Map visuals require enabling in Power BI Options → Security settings

---


