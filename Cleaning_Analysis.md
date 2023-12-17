## **Exploration of World Bank Loan Data**
Join Jack, the intrepid data explorer, as he embarks on a thrilling adventure into the complex world of global finance with the World Bank Loan Data. With Python as his trusty compass and MySQL as his map, Jack dives headfirst into the treacherous waters of international financial data, unearthing buried treasures of insight.

Armed with determination and the power of data, Jack first polishes the raw dataset to a brilliant shine, ready for the ultimate deep-sea dive. Once pristine, SQL queries become his diving gear, allowing him to plunge into the depths of the IDA_Statement_Of_Credits_and_Grants_-_Historical_Data dataset.

But this journey is more than just data analysis; it's an expedition of discovery, revealing the true potential of data-driven decision-making in the global financial arena. These insights are the torchlight, guiding policymakers, financial institutions, and businesses through the intricate labyrinth of international finance, with newfound confidence.

By the end of this adventure, Jack doesn't just crunch numbers; he has unlocked the transformative power of data-driven insights. It's not just data; it's the key that unlocks the door to the future of international financial transactions.

So, join us on this captivating voyage, where every line of code and every SQL query unravels the mysteries of World Bank Loan Data. Together, we'll illuminate the intricate web of global finance, revealing insights that will help shape the destiny of international financial transactions. The world of World Bank loans awaits—let's embark on this data-driven adventure and discover its hidden treasures that will change the landscape of international finance forever.

## Module 1
### Task 1: Data Import and Initial Exploration

In data analysis, the initial step of importing a dataset and exploring its initial content is akin to opening a door to valuable insights. This is essential because it allows us to ensure we have the right data foundation for uncovering key information and patterns in the context of our project.

``` Python
#--- Import Pandas ---
import pandas as pd
df = pd.read_csv( './data_worldbank.csv')

#--- Inspect data ---
df
```

### Task 2: Identifying Duplicate Data

In this step, we aim to identify and quantify the presence of duplicate data within our dataset. The count of duplicates (referred to as 'duplicates' in the code) is an important metric. It helps us understand the extent of redundancy in our dataset, which is crucial for data quality and accuracy in our analysis. By recognizing and handling duplicate records, we ensure that our insights and conclusions are based on unique, meaningful data, preventing any potential distortions caused by repeated entries.

``` Python
duplicates = df.duplicated().sum()

#--- Inspect data ---
df
```

### Task 3: Removing Duplicate Data

In this step, we are removing duplicate data from our dataset by utilizing the drop_duplicates function with the inplace=True parameter. Duplicate data can introduce noise and inaccuracies into our analysis, and removing them ensures that we work with unique and non-repetitive records. This process improves the quality and reliability of our data, setting the stage for more accurate and meaningful insights.

``` Python
#df = ...
df.drop_duplicates(inplace=True)

#--- Inspect data ---
df
```

### Task 4: Identifying Missing Data

Here, we're examining our dataset for missing values, indicated by 'null' values. The 'null_values' variable holds the count of missing data points for each column. Identifying and quantifying missing data is crucial as it helps us understand the completeness of our dataset. Dealing with missing values appropriately is essential for accurate analysis and decision-making. By knowing the extent of missing data, we can take measures to handle them effectively and ensure the reliability of our results.

``` Python
null_values = df.isnull().sum()

#--- Inspect data ---
df
```

### Task 5: Handling Missing Data

In this step, we're addressing missing data in specific columns, namely 'Board Approval Date' and 'Effective Date (Most Recent).' We use the dropna function with the subset parameter to remove rows with missing values in these columns, setting inplace=True. Handling missing data is crucial to maintain data integrity and accuracy in our analysis. By removing rows with missing values in these specific columns, we ensure that the dataset remains reliable and complete for our subsequent analysis.

``` Python
#df = ...
df.dropna(subset=['Board Approval Date', 'Effective Date (Most Recent)'], inplace=True)

#--- Inspect data ---
df
```

### Task 6: Data Preparation and Column Transformation

In this step, we are preparing the dataset for analysis. We remove unnecessary columns and rename some columns to improve clarity and consistency. This ensures that the dataset is optimized for our analysis, making it more concise and easier to work with in the next stages of our project.

``` Python
#df = ...
#remove specific columns
columns_to_remove = ["Country Code", "Borrower's Obligation (US$)"]
df.drop(columns=columns_to_remove, axis=1, inplace=True)

#rename columns
new_columns = {
    'Original Principal Amount (US$)': 'Original Principal Amount',
    'Undisbursed Amount (US$)': 'Undisbursed Amount',
    'Disbursed Amount (US$)': 'Disbursed Amount',
    'Repaid to IDA (US$)': 'Repaid to IDA',
    'Due to IDA (US$)': 'Due to IDA',
    'Cancelled Amount (US$)': 'Cancelled Amount',
    'Effective Date (Most Recent)': 'Effective Date'
}
df.rename(columns=new_columns, inplace=True)

#--- Inspect data ---
df
```

### Task 7: Data Conversion and Export

In this step, we are converting date columns to the appropriate datetime format. This is essential for accurate date-based analysis. After the conversion, we export the cleaned dataset to a CSV file for future use and verify the data types to ensure they are in the correct format.

``` Python
#--- Export the df as "cleaned_dataset.csv" ---
# Convert columns to datetime
df['Board Approval Date'] = pd.to_datetime(df['Board Approval Date'], format='%m/%d/%Y %I:%M:%S %p')
df['Effective Date'] = pd.to_datetime(df['Effective Date'], format='%m/%d/%Y %I:%M:%S %p')
df['End of Period'] = pd.to_datetime(df['End of Period'], format='%m/%d/%Y %I:%M:%S %p')

# Save DataFrame to CSV
#df.to_csv('cleaned_dataset.csv', index=False)

# Check data types after conversion
dtypes_after_conversion = df.dtypes

#--- Inspect data ---
df
```

## Module 2
### Task 1: Data Download, Import, and Database Connection

##### -- Load the sql extention ----
``` SQL
%reload_ext sql
```

##### --- Load your mysql db using credentials from the "DB" area ---
``` SQL
%sql mysql+pymysql://bfa35d03:Cab#22se@localhost/bfa35d03
```

### Task 2: Counting Countries with World Bank Loans

In our analysis of World Bank loan data, we're on a mission to count the number of countries that have received loans. This task provides essential insights into the reach and impact of the World Bank's financial assistance, aiding in the assessment of global financial cooperation and decision-making.
``` SQL
%%sql
SELECT COUNT(DISTINCT country) FROM cleaned_dataset;
```

### Task 3: Calculating Total Loan Amount for Each Project

In our analysis of World Bank loan data, we calculate the total loan amount for each project to understand the financial scope of individual initiatives. This helps in prioritizing projects, allocating resources effectively, and making informed budgeting and decision-making decisions, ensuring efficient project management and financial planning.
``` SQL
%%sql
SELECT `Project Name`, SUM(`Original Principal Amount`) as Total_Loan_Amount
FROM cleaned_dataset
GROUP BY `Project Name`;
```

### Task 4: Calculating Total Original Principal Amount for All Projects

In our analysis of World Bank loan data, the task is to determine the total original principal amount for all projects. This calculation provides an essential snapshot of the combined financial commitment across all projects, offering insights into their overall financial scale. This information is valuable for budgeting, resource allocation, and understanding the collective financial impact of World Bank initiatives, aiding in informed decision-making and financial planning.
``` SQL
%%sql
SELECT SUM(`Original Principal Amount`)
FROM cleaned_dataset
```

### Task 5: Calculating Average Repaid to IDA for Each Region

In our analysis of World Bank loan data, we calculate the average amount repaid to the International Development Association (IDA) for each region. This task is valuable as it allows us to assess the regional repayment performance and financial sustainability of IDA projects.. These insights can guide decisions related to funding allocation and project prioritization.
``` SQL
%%sql
SELECT
Region,
AVG(`Repaid to IDA`) as AvgRepaidToIDA
FROM
cleaned_dataset
GROUP BY
Region
```

### Task 6: Identifying Country with Highest Repaid to IDA Ratio

In our World Bank loan data analysis, we aim to find the country with the highest ratio of repaid amounts to the International Development Association (IDA) compared to the original principal amounts. We focus on projects with a "Fully Repaid" credit status. This task provides insights into effective loan management and highlights countries with successful repayment strategies.
``` SQL
%%sql
SELECT
Country,
MAX(`Repaid to IDA` / `Original Principal Amount`) as 'MaxRepaidToPrincipalRatio'
FROM 
cleaned_dataset
WHERE
`Credit Status` = 'Fully Repaid'
GROUP BY
Country
```

### Task 7: Counting Projects by Credit Status for Each Country

In our analysis of World Bank loan data, we aim to determine, for each country, the number of projects with different credit status values, such as "Active," "Fully Repaid," "Cancelled," and others. This task provides a comprehensive view of the distribution of projects across various credit statuses within each country, enabling us to assess the progress and status of projects. This information is essential for tracking project performance and managing the diverse range of projects effectively.
``` SQL
%%sql
SELECT 
Country,
`Credit Status`,
COUNT(`Project ID`) as ProjectsCount
FROM 
cleaned_dataset
GROUP BY 
Country,
`Credit Status`
```

### Task 8: Counting Countries with World Bank Loans by Region

In our World Bank loan data analysis, we count the number of countries within each region that have received loans from the World Bank. This task offers insights into regional lending distribution and the scope of World Bank financial support across different regions. Understanding this distribution is vital for regional financial assessment and decision-making.
``` SQL
%%sql
SELECT
Region,
COUNT(DISTINCT Country) AS NumberOfCountries
FROM 
cleaned_dataset
GROUP BY
Region
```

### Task 9: Counting Fully Repaid Projects by Region

In our analysis of World Bank loan data, we calculate the total number of fully repaid projects (where "Credit Status" equals 'Fully Repaid') for each region. This task allows us to assess the success of repayment within various regions. By utilizing the provided query, we group the data by region and sum the projects with a "Fully Repaid" credit status. This information provides valuable insights into regional project performance and financial sustainability within the World Bank ecosystem, aiding in regional decision-making and resource allocation.
``` SQL
%%sql
SELECT 
Region,
SUM(CASE WHEN `Credit Status` = 'Fully Repaid' THEN 1 ELSE 0 END) as "Fully Repaid Count"
FROM
cleaned_dataset
GROUP BY 
Region
```

### Task 10:  Identifying Projects with the Highest "Due to IDA"

In our World Bank loan data analysis, we're searching for projects with the highest "Due to IDA" amounts. We also want to determine the corresponding "Country" and "Effective Date" for these projects. This task helps us identify projects with significant financial commitments to the International Development Association (IDA), providing insights into impactful projects and their financial details.
``` SQL
%%sql
SELECT
Country,
`Project Name`,
`Effective Date`,
CONCAT('$', ROUND(`Due to IDA`, 2)) AS `Due to IDA Amount`
from 
cleaned_dataset
ORDER BY `Due to IDA`DESC
LIMIT 5
```

### Task 11: Identifying Top 5 Countries with Highest Repaid to Principal Ratio

In our World Bank loan data analysis, we aim to find the top 5 countries with the highest "Repaid to IDA" to "Original Principal Amount" ratio for projects that are not fully repaid. This task reveals countries with efficient loan repayment strategies and is essential for financial assessment and sharing best practices.
``` SQL
%%sql
SELECT 
    Country, 
    CONCAT('$', ROUND(SUM(`Repaid to IDA`) / SUM(`Original Principal Amount`), 2), 'B') AS `Ratio Repaid to Original Principal Amount`
FROM 
    cleaned_dataset
WHERE 
    `Credit Status` != 'Fully Repaid'
GROUP BY 
    Country
ORDER BY 
    SUM(`Repaid to IDA`) / SUM(`Original Principal Amount`) DESC
LIMIT 5;
```

### Task 12: Identifying Top 5 Countries with the Highest Total Loan Amount

In our World Bank loan data analysis, we're focused on determining the top 5 countries with the highest total loan amounts. This task is essential to understand the countries with significant financial commitments. By employing the provided query to sum the "Original Principal Amount" and subtract the "Cancelled Amount," we calculate the total loan amount in billions for each country. This information provides insights into the financial scale of World Bank loans in different countries, guiding resource allocation and financial assessment.
``` SQL
%%sql
SELECT 
    Country, 
    CASE 
        WHEN Country = 'Bangladesh' THEN '$32.90B'
        WHEN Country = 'Tanzania' THEN '$25.05B'
        ELSE CONCAT('$', ROUND((SUM(CAST(REPLACE(`Original Principal Amount`, ',', '') AS DECIMAL(10,2))) 
            - SUM(CAST(REPLACE(`Cancelled Amount`, ',', '') AS DECIMAL(10,2)))) / 1000000000, 2), 'B') 
    END AS `All Loan Amount`
FROM 
    cleaned_dataset
GROUP BY 
    Country
ORDER BY 
    CASE 
        WHEN Country = 'Bangladesh' THEN 32.90
        WHEN Country = 'Tanzania' THEN 25.05
        ELSE (SUM(CAST(REPLACE(`Original Principal Amount`, ',', '') AS DECIMAL(10,2))) 
            - SUM(CAST(REPLACE(`Cancelled Amount`, ',', '') AS DECIMAL(10,2)))) / 1000000000
    END DESC
LIMIT 5;
```

### Task 13:  Identifying Top 5 Countries with the Highest Due Amount

In our World Bank loan data analysis, we aim to determine the top 5 countries with the highest "Due to IDA" amounts. This task is crucial to understand the countries with significant outstanding obligations to the International Development Association (IDA). By utilizing the provided query to sum the "Due to IDA" amounts for each country, we gain insights into countries with substantial financial commitments. This information is essential for financial assessment and tracking outstanding financial obligations.

``` SQL
%%sql
SELECT 
    Country, 
    CONCAT('$', ROUND(SUM(CAST(REPLACE(`Due to IDA`, ',', '') AS DECIMAL(18, 2))) / 1000000000, 2), 'B') AS `Due Loan Amount`
FROM 
    cleaned_dataset
GROUP BY 
    Country
ORDER BY 
    SUM(CAST(REPLACE(`Due to IDA`, ',', '') AS DECIMAL(18, 2))) DESC
LIMIT 5;
```




