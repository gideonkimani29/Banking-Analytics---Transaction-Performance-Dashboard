This detailed README is designed to showcase the technical depth of your project, including the data modeling, DAX logic, and the business insights derived from the banking dataset.

---

# Banking Analytics & Transaction Performance Dashboard

## Project Overview
This Power BI project provides a comprehensive analysis of banking operations, focusing on customer demographics, account performance, and transaction efficiency. The dashboard transforms raw transactional data into actionable insights to help bank managers understand channel popularity, regional growth, and customer behavior.

## Data Architecture
The project utilizes a **Snowflake Schema** to manage complex relationships between products and categories while ensuring data integrity across customer and account dimensions.



### Data Sources
* **FactTransaction:** 10,000+ records of customer transactions including amounts, channels, and status.
* **DimCustomer / DimAccount:** Demographic data and account-level details (Balance, Status, Type).
* **Product Hierarchy:** Three-tiered structure (Category $\rightarrow$ Sub-Category $\rightarrow$ Product) for granular sales analysis.

## Key Features & Visualizations

### Page 1: Executive Performance
* **Dynamic KPIs:** Real-time tracking of Total Transaction Volume, Success Rate, and Net Liquidity.
* **Transaction Trends:** A time-series area chart highlighting seasonal peaks in transaction activity.
* **Composition Analysis:** Donut charts illustrating the split between Credit and Debit transactions to understand cash flow direction.

### Page 2: Customer & Regional Insights
* **Geospatial Distribution:** A regional breakdown of the customer base, identifying high-growth areas like Florida and California.
* **Demographic Slicing:** Interactive filters for Gender, Age Groups (calculated from DOB), and Account Status.
* **Account Health:** Stacked bar charts comparing "Open" vs. "Closed" accounts across different products (Savings, Checking, Credit).

## Technical Implementation

### Power Query (ETL)
* **Data Cleaning:** Handled null values in `ClosedDate` to distinguish between active and historical accounts.
* **Merging:** Appended regional customer data (DimCustomerUSA) to create a unified master customer table.
* **Data Typing:** Standardized date formats and currency types for accurate time-intelligence calculations.

### DAX Calculations
Several custom measures were developed to drive the analytics:
```dax
// Calculate the percentage of successful transactions
Success Rate =
DIVIDE(
    CALCULATE(COUNT(FactTransaction[TransactionID]), FactTransaction[Status] = "Success"),
    COUNT(FactTransaction[TransactionID]),
    0
)

// Dynamic Customer Growth (Year-over-Year)
Customer Growth YoY =
VAR PreviousYear = CALCULATE(DISTINCTCOUNT(DimCustomer[CustomerID]), DATEADD('Calendar'[Date], -1, YEAR))
RETURN
DIVIDE(DISTINCTCOUNT(DimCustomer[CustomerID]) - PreviousYear, PreviousYear)
```

## Insights Derived
1. **Channel Preference:** Mobile transactions consistently outperform Web and ATM channels in volume, suggesting a "Mobile First" customer trend.
2. **Regional Concentration:** A significant portion of the high-balance accounts are concentrated in specific coastal regions, providing a target for premium service marketing.
3. **Operational Efficiency:** The transaction success rate remains high (>95%), but specific product categories show intermittent failures that warrant technical investigation.

## How to Use
1. Clone this repository.
2. Open the `.pbix` file in **Power BI Desktop**.
3. Ensure the `.csv` and `.txt` files in the `/Data` folder are linked correctly in the Power Query Data Source settings.
4. Interact with the filters on the left-hand pane to slice the data by Region or Date.

---

### Future Enhancements (Page 3)
* **Predictive Analytics:** Implementation of a churn prediction model based on account inactivity.
* **Product Affinity:** Market basket analysis to see which banking products are frequently used together.

