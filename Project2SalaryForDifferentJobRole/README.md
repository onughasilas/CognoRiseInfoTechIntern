# Data Analysis of Data Science Job Salaries  

## Project Overview  
This project explores salary trends for data science roles using a dataset sourced from Kaggle and provided by CoongnoRise Infotech. The goal of the project was to analyze salaries based on job titles, experience levels, company sizes, and remote work ratios to extract meaningful insights and provide actionable recommendations.  

The analysis was performed using **R** and documented in **R Markdown**, showcasing my data manipulation, analysis, and visualization skills as part of my internship.  

---

## Dataset  
The dataset, *Data Science Salaries*, contains job-related information such as salaries (in USD), experience levels, company sizes, and remote ratios. It was downloaded from Kaggle and contains the following:  
- **Rows:** 10,000  
- **Columns:** 12  
- **Features:**  
  - Job Title  
  - Experience Level  
  - Remote Ratio  
  - Company Size  
  - Salary in USD, etc.  

**Note:** The dataset is proprietary and cannot be shared publicly.  

---

## Tools and Techniques Used  
- **Programming Language:** R  
- **Libraries:** `dplyr`, `ggplot2`, `tidyverse`  
- **Steps:**  
  - Data cleaning and transformation  
  - Exploratory Data Analysis (EDA)  
  - Visualizations  
  - Recommendations  

---

## Process  

### 1. **Data Loading**  
The dataset was loaded into R using the `read.csv()` function:  
```r
DsSalaries <- read.csv("path/to/ds_salaries.csv")
head(DsSalaries)
