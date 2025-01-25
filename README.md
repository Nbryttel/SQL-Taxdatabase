# SQL-Taxdatabase (Tax Expense Schema)

A database schema for managing tax-related expenses.

![github prezent](https://github.com/user-attachments/assets/c3372657-ea3a-442d-ba43-8561987e4b7a)


## Overview
This project defines a relational database schema to manage tax-related expenses. It categorizes expenses into different dimensions like courses, conferences, and travel, and provides calculated columns for automatic data handling. The aim was to extract actionable insights that help optimize budgets, monitor trends, and support strategic decisions.

## Tools used:
- SQL Server on Azure: Database hosting and management.
- Azure Data Studio: Query development and data exploration.
- Tableau: Visualization of insights and reporting.
- Lucidchart: Designing the database schema and entity relationships.

## Skills Demonstrated
As a data analyst, I applied the following skills:
- Data Modeling: Designing a relational database for structured storage of expense data.
- Data Wrangling: Writing SQL queries to clean, transform, and prepare data for analysis-
- Data Analysis: Extracting meaningful patterns and trends from large datasets.
- Reporting and Visualization: Presenting results with dynamic dashboards.

## Workflow
1. Designed Entity-Relationship Diagram (ERD), defining tables, establishing relationships, and normalizing data to ensure consistency, accuracy, and scalability.
3. Database Setup:
- Designed a database schema to manage expenses with tables for travels, conferences, meetings, and more.
- Deployed the database on SQL Server (Azure).
2. Data Exploration:
- Populated the database with sample data for business travel and related expenses.
- Wrote SQL queries to calculate total expenses, tips, and other key metrics.
3. Analysis:
-Performed cost analysis based on event type, location, and year.
- Identified high-cost trends and potential areas for savings.
4. Visualization:
- Migrated the data to Tableau to create dashboards with insights such as:
- Year-over-year expense comparisons
- Breakdown of costs by location and event type.
- Trends in business vs. personal travel costs.


![code i yoytube (1)](https://github.com/user-attachments/assets/d2f7c661-5379-4546-b50c-7b19b6b1c74c)


## Check Live Data Analysis Recording 
https://www.youtube.com/watch?v=gpad3xV-eWc


## Example SQL Code


### Table Example
```sql
CREATE TABLE dbo.DimTravel (
    TravelID INT IDENTITY(1,1) PRIMARY KEY,
    TaxYear INT NOT NULL,
    TravelCity NVARCHAR(100) NOT NULL,
    TravelCountry NVARCHAR(5) NOT NULL,
    TravelYear INT NOT NULL,
    TravelMonth NVARCHAR(50) NOT NULL CHECK (TravelMonth IN (
        'January', 'February', 'March', 'April', 'May', 'June', 
        'July', 'August','September', 'Oktober' 'November', 'December'
    )),
    TotalExpenseEUR DECIMAL(10,2) AS (HotelExpense + ISNULL(HotelTip, 0.00)) PERSISTED
);
```

### Sample Query 1
```sql
SELECT 
    TravelMonth,
    SUM(TotalExpenseEUR) AS TotalMonthlyExpenses
FROM dbo.DimTravel
GROUP BY TravelMonth
ORDER BY TotalMonthlyExpenses DESC;
```

### Sample Query 2

```sql
SELECT
    te.EventCity AS Destination, -- City of travel (basic travel information)
    te.TaxYear AS TaxYear, -- Tax year associated with the travel expenses
    te.ExpenseAmountEUR AS TravelExpenseAmountEUR, -- Main travel cost in EUR (e.g., hotel)
    SUM(COALESCE(ate.ExpenseAmountEUR, 0)) AS TotalAdditionalExpenseAmountEUR, -- Total additional costs associated with the trip
    te.ExpenseAmountEUR + SUM(COALESCE(ate.ExpenseAmountEUR, 0)) AS TotalTravelExpenses, -- Total travel costs (main + additional)
    COALESCE(ce.ConferenceName, 'N/A') AS ConferenceName -- Name of the associated conference or "N/A" (if no association exists)
FROM 
    dbo.DimTravelExpenses AS te -- Main table containing travel information
LEFT JOIN 
    dbo.DimAdditionalTravelExpenses AS ate -- Join with the table of additional travel costs
ON 
    te.TravelID = ate.TravelID -- Linking additional expenses to the main trip using TravelID
LEFT JOIN 
    dbo.DimTravelPurposeMapping AS tp -- Join with the table of travel purposes
ON 
    te.TravelID = tp.TravelID AND tp.PurposeType = 'Conference' -- Filtering travel purpose as "Conference"
LEFT JOIN 
    dbo.DimConferencesExpenses AS ce -- Join with the conferences table
ON 
    tp.ConferenceID = ce.ConferenceID -- Linking the conference by its ID
GROUP BY 
    te.EventCity, -- Grouping results by the city of travel
    te.TaxYear, -- Grouping results by the tax year
    te.HotelName, -- Grouping by hotel name (if available)
    te.ExpenseAmountEUR, -- Grouping by main travel expenses
    ce.ConferenceName; -- Grouping by conference name (if available)
```

## Features
- Multiple dimensional tables (e.g., `DimCourses`, `DimConferences`, `DimTravel`).
- Calculated fields for total expenses, tips, and travel distance.
- Foreign key constraints to ensure referential integrity.
- Cascading deletes for dependent data cleanup.

## Key findings
- Top Travel Destinations: Identified cities with the highest expenses for business-related events.
- Cost Overruns: Highlighted events and categories where spending exceeded expected limits.
- Trends Over Time: Analyzed how travel and conference expenses evolved over the years.
- Optimization Opportunities: Suggested potential savings by prioritizing virtual meetings in certain locations.

## Visual Dashboards
Setting up dashboards:

![Copy of Copy of cover UX FTI (35)](https://github.com/user-attachments/assets/4c845a93-7973-4d6a-942a-0df4e45cf9a5)

![join](https://github.com/user-attachments/assets/23ca7a02-785a-44ff-91db-e07871bf4237)



Below is an example of the Tableau dashboard showcasing:

![wykres](https://github.com/user-attachments/assets/c9fc5643-09d8-4dac-9377-ee8871c0c5bc)


Total expenses by year and event type.
Interactive filters for country, event type, and date.

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss your ideas.

## LICENSE
This project is licensed under the MIT License. See the LICENSE file for more details.

## How to Use
1. Clone the repository.
2. Open the SQL script [setup_taxdatabase.sql](./setup_taxdatabase.sql) in your preferred SQL environment, such as:
- Azure Data Studio
- SQL Server Management Studio (SSMS)
3. Execute the scripts to:
- Create the database schema: Set up all tables, constraints, and relationships.
- Populate the tables with sample data (if provided).
4. Analyze the data:
- Use the provided SQL queries (above: "Sample Query 2") or write your own to:
- Retrieve insights (e.g., travel expenses by month).
- Perform advanced analysis for reporting purposes.

Clone this repository:
   ```bash
   git clone https://github.com/Nbryttel/SQL-Tax-Expense-Schema.git
 ```





