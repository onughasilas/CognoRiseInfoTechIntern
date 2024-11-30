# PROJECT TITLE: ANALYSIS OF UNEMPLOYMENT RATE IN INDIA 
## PROJECT REVIEW
As part of my internship at CongnoRise Infotech, I undertook a comprehensive analysis of unemployment trends in India using a dataset from Kaggle whose link was provided by CongnoRise InfoTech. The analysis involved data cleaning, transformation, exploration, and visualization. 
## TOOLS USED 
- MySQL for data manipulation
- Tableau for visualization.

## DATASET DESCRIPTION
The dataset contains information on unemployment rates in different regions of India over time. It includes the following key columns:

- Region: The geographical area.
- Date: The period (formatted as DD-MM-YYYY).
- Frequency: The frequency of data collection.
- Estimated Unemployment Rate (%): Percentage of the unemployed population.
- Estimated Employed: The estimated number of employed individuals.
- Estimated Labour Participation Rate (%): Percentage of the labour force participating in the workforce.
- Region Category, Latitude, Longitude: Additional geographical details.

## THE PROCESS
**Step 1**: Data Importation
I created a new schema in MySQL named unemployment, then
The dataset was imported using MySQL's Table Data Import Wizard.
 After importing the data, I ensured all column headers were reformatted for consistency:
I replaced spaces with underscores and made all headers lowercase (e.g., Estimated Unemployment Rate (%) → estimated_unemployment_rate_percent). Below is the syntax with an explanation;


```sql
-- To see the headers
SELECT * FROM unemployment.unemployment_rate;
DESCRIBE unemployment.unemployment_rate; ```

- Changing the column names to unique type 
```sql ALTER TABLE unemployment.unemployment_rate
CHANGE COLUMN `Region` region VARCHAR(255); 

ALTER TABLE unemployment.unemployment_rate
CHANGE COLUMN `Date` date DATE;

ALTER TABLE unemployment.unemployment_rate
CHANGE COLUMN `Frequency` frequency VARCHAR(255); ```

ALTER TABLE unemployment.unemployment_rate
CHANGE COLUMN `Estimated Unemployment Rate (%)` estimated_unemployment_rate_percent DECIMAL(5,2);

ALTER TABLE unemployment.unemployment_rate
CHANGE COLUMN `Estimated Employed` estimated_employed VARCHAR(255);

ALTER TABLE unemployment.unemployment_rate
CHANGE COLUMN `Estimated Labour Participation Rate (%)` estimated_labour_participation_rate_percent DECIMAL(5,2);

-- No need to change these, as they are already in snake_case:
-- `region_category`, `longitude`, and `latitude`
SELECT region, date, frequency, estimated_unemployment_rate_percent, estimated_employed,
       estimated_labour_participation_rate_percent, region_category, longitude, latitude
FROM unemployment.unemployment_rate; -- checking if the above code ran well


-- Data cleaning , removing spaces
-- first i will turn off the safe mode
SET SQL_SAFE_UPDATES = 0; -- setting the mysql to safe mode zero to be able to alter and update the table

-- TRIMMING ALL THE COLUMNS FOR UNIQUENESS
UPDATE unemployment.unemployment_rate
SET date = TRIM(date);
UPDATE unemployment.unemployment_rate
SET Region = TRIM(Region);
UPDATE unemployment.unemployment_rate
SET Frequency = TRIM(Frequency);
UPDATE unemployment.unemployment_rate
SET estimated_employed = TRIM(estimated_employed);
UPDATE unemployment.unemployment_rate
SET latitude = TRIM(latitude);
UPDATE unemployment.unemployment_rate
SET longitude = TRIM(longitude);
UPDATE unemployment.unemployment_rate
SET estimated_unemployment_rate_percent = TRIM( estimated_unemployment_rate_percent);
UPDATE unemployment.unemployment_rate
SET  estimated_labour_participation_rate_percent = TRIM( estimated_labour_participation_rate_percent);
UPDATE unemployment.unemployment_rate
SET region_category = TRIM(region_category);
 -- trimming the data  to be able to set the column to standard "YYYY-MM-DD"
 
 SELECT * FROM unemployment.unemployment_rate;

UPDATE unemployment.unemployment_rate
SET date = STR_TO_DATE(date, '%d-%m-%Y'); -- converting the date to mysql recognized data formate

SELECT DISTINCT date
FROM unemployment.unemployment_rate; -- checking if the date got converted

ALTER TABLE unemployment.unemployment_rate
MODIFY COLUMN Date DATE; -- Converts the date column from VARCHAR (or similar) to MySQL's DATE data type

-- Now, i want to remove duplicate , but first i need to now if there is any rows repeated
SELECT *,
ROW_NUMBER() OVER(
PARTITION BY region, date, frequency, estimated_unemployment_rate_percent, estimated_employed,
       estimated_labour_participation_rate_percent, region_category, longitude, latitude) AS row_num
FROM unemployment.unemployment_rate;

-- NOW, I WANT TO CHECK THE ROWS GREATER THAN 1
WITH duplicate_cte AS (
SELECT *,
ROW_NUMBER() OVER(
PARTITION BY region, date, frequency, estimated_unemployment_rate_percent, estimated_employed,
       estimated_labour_participation_rate_percent, region_category, longitude, latitude) AS row_num
FROM unemployment.unemployment_rate
)
SELECT *
FROM duplicate_cte
WHERE row_num >1;
-- ZERO WAS RETURNED WHICH SHOWS THAT THERE WAS NO ROWS GREATER THAN 1, AND IMPLIES NO DUPLICATE


-- CHECKING FOR NULL VALUES
SELECT *
FROM unemployment.unemployment_rate
WHERE frequency IS NULL; -- checking NULLs FREQUENCY COLUMN

SELECT *
FROM unemployment.unemployment_rate
WHERE estimated_employed IS NULL;

SELECT*  
FROM unemployment.unemployment_rate
WHERE estimated_labour_participation_rate_percent IS NULL;

SELECT *
FROM unemployment.unemployment_rate
WHERE estimated_unemployment_rate_percent IS NULL; 

SELECT *
FROM unemployment.unemployment_rate
WHERE unemployment_rate IS NULL
   OR employed IS NULL
   OR labour_participation_rate IS NULL;
   
   SELECT *
FROM unemployment.unemployment_rate
WHERE region IS NULL
   OR date IS NULL
   OR frequency IS NULL
   OR estimated_unemployment_rate_percent IS NULL
   OR estimated_employed IS NULL
   OR estimated_labour_participation_rate_percent IS NULL
   OR region_category IS NULL
   OR longitude IS NULL
   OR latitude IS NULL; -- I USED THIS CODE TO CHECK FOR ALL ROWS AT SAME TIME INSTEAD OF TYPING ONE AFTER THE OTHER
-- CHECKED ALL, THERE WAS NO MISSING VALUE

-- EXPLORING THE DATA
-- 1. Average employment by region
SELECT region, AVG(estimated_employed) AS avg_employed_rate
FROM unemployment.unemployment_rate
GROUP BY region
ORDER BY avg_employed_rate DESC; -- rgeion Uttar Pradesh has the highest employed rate

-- 2. Average Unemployment Rate by Region
SELECT region, AVG(estimated_unemployment_rate_percent) AS avg_unemployed_rate
FROM unemployment.unemployment_rate
GROUP BY region
ORDER BY avg_unemployed_rate DESC -- region Haryana has the highest unemployed rate

-- 3. Unemployment Trends Over Time
SELECT DATE_FORMAT(date, '%Y-%m') AS month, AVG(estimated_unemployment_rate_percent) AS avg_rate
FROM unemployment.unemployment_rate
GROUP BY month
ORDER BY avg_rate DESC; -- 2020-05 has the highest uneployment rate

-- 4. Regions with the Highest Labour Participation
 SELECT region, AVG(estimated_labour_participation_rate_percent) AS avg_labour_participation_rate
FROM unemployment.unemployment_rate
GROUP BY region
ORDER BY avg_labour_participation_rate DESC; -- Meghalaya has the highest avg labour participation rate

SELECT region, MAX(estimated_labour_participation_rate_percent) AS max_labour_participation_rate
FROM unemployment.unemployment_rate
GROUP BY region
ORDER BY max_labour_participation_rate DESC; -- Tripura has the highest max rate

-- 5.  Unemployment Rate by Region Category
SELECT region_category, AVG(estimated_unemployment_rate_percent) AS avg_uemployed_rate
FROM unemployment.unemployment_rate
GROUP BY region_category
ORDER BY avg_uemployed_rate DESC; -- North ewst has the highest unemployed rate


## STEP 3
tep 4: Visualization in Tableau
Created multiple visualizations to highlight key insights:
Geographic Map: Showcased regional unemployment rates using longitude and latitude.
Line Chart: Tracked unemployment trends over time.
Bar Chart: Compared unemployment rates across regions.
Heatmap: Visualized intensity of unemployment over different time periods.
Histogram: Displayed the distribution of unemployment rates.

