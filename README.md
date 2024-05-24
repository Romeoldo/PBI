

# Power BI Report Filtering using Current Date, Month, or Quarter

This documentation outlines the steps to create a filter for Current Date, Month, or Quarter in a Power BI report using a query from a `DIM_DATE` table containing all date-related information.

## Query Overview

The query below extracts date information and provides additional columns to identify the current month and quarter dynamically.

### Query

```sql
SELECT 
    [Date], 
    CAST(YEAR([Date]) AS VARCHAR(4)) + CAST(DATEPART(MONTH, [Date]) AS VARCHAR(2)) AS [Month Code], 
    IIF(MONTH(GETDATE()) = MONTH([Date]) AND YEAR(GETDATE()) = YEAR([Date]), 'Current Month Data', DATENAME(MONTH, [Date]) + ' ' + CAST(YEAR([Date]) AS VARCHAR(4))) AS [Month], 
    CAST(YEAR([Date]) AS VARCHAR(4)) + '' + CAST(DATENAME(QUARTER, [Date]) AS VARCHAR(2)) AS [QuarterCode], 
    IIF(YEAR(GETDATE()) = YEAR([Date]) AND DATEPART(QUARTER, GETDATE()) = DATENAME(QUARTER, [Date]), 'Current Quarter Data', CAST(YEAR([Date]) AS VARCHAR(4)) + ' Q' + DATENAME(QUARTER, [Date])) AS [Quarter]
FROM 
    DB.[Reports].[DatesIntervals]
WHERE 
    [Date] BETWEEN '2020-01-01' AND CAST(GETDATE() AS DATE);
```

## Steps to Implement in Power BI


1. **Connect to Database:**
    - Open Power BI Desktop.
    - Click on `Home` > `Get Data` > `SQL Server`.
    - Enter the server name and database name. Click `OK`.

2. **Enter the Query:**
    - Choose the option to write a query.
    - Copy and paste the provided SQL query into the query editor.
    - Click `OK`.

3. **Load Data:**
    - Once the query runs successfully, load the data into Power BI.
    - The new columns `[Month Code]`, `[Month]`, `[QuarterCode]`, and `[Quarter]` will be available for use in your report.

4. **Create Filters:**
    - To filter by current month or quarter, use the `[Month]` and `[Quarter]` columns.
    - Add these fields to your slicers or filters pane.
    - For example, to filter to the current month, select "Current Month Data" from the `[Month]` slicer.

5. **Visualization:**
    - Use the filtered data to create your visualizations.
    - The dynamic nature of the query ensures that your report always reflects the current date, month, or quarter.

## Example Usage

### Monthly Report

To create a monthly report:
- Use the `[Month]` column to filter data for the current month or any specific month.

### Quarterly Report

To create a quarterly report:
- Use the `[Quarter]` column to filter data for the current quarter or any specific quarter.

## Conclusion

This approach allows you to dynamically filter your Power BI reports based on the current date, month, or quarter using a simple SQL query. By implementing these steps, you can ensure that your reports are always up-to-date with the latest data.
