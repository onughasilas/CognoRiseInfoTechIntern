#  PROJECT TITLE: EMPLOYEE SALARIES FOR DIFFERENT JOB ROLES 

## PROJECT OVERVIEW  
This project explores salary trends for different job roles using a dataset sourced from Kaggle and provided by CoongnoRise Infotech. The goal of the project is to analyze salaries based on job titles, experience levels, company sizes, and remote work ratios to extract meaningful insights and provide actionable recommendations.  

The analysis was performed using **R** and documented in **R Markdown**, showcasing my data manipulation, analysis, and visualization skills as part of my internship.  

---

## DATASET  
The dataset, *Employee Salaries for different job roles*, contains job-related information such as salaries (in USD), experience levels, company sizes, and remote ratios etc. It was downloaded from Kaggle with a link provided by ComgnoRise Infotech and contains the following:  
- **Rows:** 10,000  
- **Columns:** 12  
- **Features:**  
  - Job Title  
  - Experience Level  
  - Remote Ratio  
  - Company Size  
  - Salary in USD, etc.  

[Download the Dataset from Kaggle](https://www.kaggle.com/datasets/inductiveanks/employee-salaries-for-different-job-roles)  

---

## TOOLS AND TECHNIQUES USED 
- **Programming Language:** R  
- **Libraries:** `dplyr`, `ggplot2`, `tidyverse`  
- **Steps:**  
  - Data cleaning and transformation  
  - Exploratory Data Analysis (EDA)  
  - Visualizations  
  - Recommendations  

---

## PROCESS 

### 1. **DATA LOADING**  
The dataset was loaded into R using the `read.csv()` function:  
```r
DsSalaries <- read.csv("C:/Users/Administrator/Documents/ds_salaries.csv", header = TRUE, stringsAsFactors = FALSE)
head(DsSalaries)
```
## Removing the irrelevant column
```r
DsSalaries <- DsSalaries[, -1]
