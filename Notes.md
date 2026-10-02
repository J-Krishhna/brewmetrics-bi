# DAX Development Notes

## Measure 1: Total Sales

### AI Prompt
> My Power BI model has these tables: Fact_Sales (Date, CityID, StoreFormatID, ProductID, quantity, unit_price, sales_amount), Dim_Date (Date, Year, MonthNumber, MonthName, Day), Dim_City (CityID, City), Dim_Product (ProductID, Category, Item). Write a DAX measure called Total Sales that sums sales_amount. Give plain DAX only, not TMDL.

### AI Suggestion
```DAX
Total Sales = SUM(Fact_Sales[sales_amount])
```
**What was wrong / changed:** Nothing

## Measure 2: MoM Growth %

### AI Prompt
> Using the same model, write a DAX measure called MoM Growth %. It should calculate month-over-month percentage growth based on Total Sales. Use standard DAX; no TMDL.

### AI Suggestion
```DAX
MoM Growth % = 
VAR CurrentMonthSales = [Total Sales]
VAR PreviousMonthSales = 
    CALCULATE(
        [Total Sales],
        DATEADD('Dim_Date'[Date], -1, MONTH)
    )
RETURN
    DIVIDE(CurrentMonthSales - PreviousMonthSales, PreviousMonthSales)
```
**What was wrong / changed:** nothing

### AI Prompt
Using the Power BI model in this workspace, write a DAX measure called Running Total that accumulates [Total Sales] across the date table. Don't edit any files, just reply with the DAX.

### AI Suggestion
Running Total = 
CALCULATE(
    [Total Sales],
    FILTER(
        ALLSELECTED('Dim_Date'[Date]),
        'Dim_Date'[Date] <= MAX('Dim_Date'[Date])
    )
)
**What was wrong / changed:** nothing

### AI Prompt
Using the Power BI model in this workspace, write a DAX measure called City Rank that ranks cities by [Total Sales] using RANKX, highest sales as rank 1. Don't edit any files, just reply with the DAX.

### AI Suggestion
City Rank = 
RANKX(
    ALL(Dim_City[City]),
    [Total Sales],
    ,
    DESC
)

### AI Prompt
Using the Power BI model in this workspace, write a DAX measure called Item Share of Category % that shows each item's sales as a percentage of its own category's total sales. Don't edit any files, just reply with the DAX.

### AI suggestion
Item Share of Category % = 
VAR ItemSales = [Total Sales]
VAR CategorySales = 
    CALCULATE(
        [Total Sales],
        ALL(Dim_Product)
    )
RETURN
    DIVIDE(ItemSales, CategorySales)

### Why It Is Incorrect
Using ALL(Dim_Product) clears filters from all columns in the Dim_Product table, including both Item and Category.

Because the filter on Category is removed along with Item, CategorySales evaluates the grand total sales across the entire product catalog rather than scoping the denominator to the item's specific category. This produces the item's share of total company sales instead of its share of its own category's sales.
