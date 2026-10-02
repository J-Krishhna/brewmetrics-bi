# DAX Development Notes

## Measure 1: Total Sales

### AI Prompt
> My Power BI model has these tables: Fact_Sales (Date, CityID, StoreFormatID, ProductID, quantity, unit_price, sales_amount), Dim_Date (Date, Year, MonthNumber, MonthName, Day), Dim_City (CityID, City), Dim_Product (ProductID, Category, Item). Write a DAX measure called Total Sales that sums sales_amount. Give plain DAX only, not TMDL.

### AI Suggestion
```DAX
Total Sales = SUM(Fact_Sales[sales_amount])
```

