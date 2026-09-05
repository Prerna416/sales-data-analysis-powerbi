# 📄 Documentation

## 🔧 Requirements
- Power BI Desktop (free — download from Microsoft's official website)
- Windows OS (Power BI Desktop is not available for Mac)

## ▶️ How to Use
1. Clone or download this repository.
2. Open the `Dashboard/Sales_Data_Analysis.pbix` file using Power BI Desktop.
3. Use the slicers and filters on each page to explore the data interactively.
4. Navigate between report pages (Overview, Top & Bottom 5 Analysis, Comparison, Interactive Analysis, Detailed Table) using the page tabs at the bottom of Power BI Desktop.

## 📂 Data Source
The raw dataset used in this project is included in this repository: [Store_Data.xlsx](./Store_Data.xlsx)

It is a custom-structured Excel dataset organized in a star-schema format, containing:
- **Dim Customers** — Customer ID, Name, City, State, Pincode, Email, Phone Number
- **Dim Product** — Product ID, Product Name, Product Line, Price
- **Dim Promotion** — Promotion ID, Promotion Name, Ad Type, Coupon Code, Price Reduction Type
- **Fact Table** — Date, Customer ID, Promotion ID, Product ID, Units Sold, Price Per Unit, Total Sales, Discount %, Discount Value, Net Sales

The fact table was linked to each dimension table in Power BI's Data Model view to enable filtering and analysis by customer, product, and promotion. Some calculated fields (Total Sales, Discount Value, Net Sales) were derived using Power Query/DAX where source data was incomplete.

## 🔄 How the Dashboard Was Built
1. Imported raw sales data using Power Query.
2. Cleaned and transformed the data (removed duplicates, handled nulls, formatted columns).
3. Created relationships between tables in the Data Model view, including two Date tables (`Date Table1` and `Date Table2`) to support period-over-period comparison.
4. Built DAX measures for KPIs such as Total Sales, Total Profit, Quantity Sold, and Average Discount.
5. Designed interactive report pages using slicers, cards, charts, and tables.

---

## 🧮 Key DAX Measures

### Sum of Net Sales
```dax
CALCULATE(
    SUM('FACT TABLE'[Net sales]),
    ALL('Date Table1'),
    USERELATIONSHIP('Date Table 2'[Date], 'FACT TABLE'[Date (dd/mm/yyyy)])
)
```

### Total Profit
```dax
CALCULATE(
    SUM('FACT TABLE'[Profit column]),
    ALL('Date Table1'),
    USERELATIONSHIP('Date Table 2'[Date], 'FACT TABLE'[Date (dd/mm/yyyy)])
)
```

### Quantity Sold
```dax
CALCULATE(
    SUM('FACT TABLE'[Units Sold]),
    ALL('Date Table1'),
    USERELATIONSHIP('Date Table 2'[Date], 'FACT TABLE'[Date (dd/mm/yyyy)])
)
```

### Why `USERELATIONSHIP`?
The data model contains two Date tables (`Date Table1` and `Date Table2`) connected to the Fact Table, enabling side-by-side comparison of two custom time periods using independent slicers. Since Power BI allows only one *active* relationship between two tables at a time, `USERELATIONSHIP` is used to activate the second date table for these specific measures, while `ALL('Date Table1')` removes any conflicting filter from the first date table. This technique powers the **Sales, Profit & Quantity Comparison** dashboard page.
