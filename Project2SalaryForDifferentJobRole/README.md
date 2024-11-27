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
STEP9: Checking to see if step8 worked
```{r}
head(DsSalaries)
```
STEP10: inspecting the dataset
```{r}
head(DsSalaries)
str(DsSalaries)
colnames(DsSalaries)
names(DsSalaries)
summary(DsSalaries)
```
STEP11: Checking for missing value
```{r}
missing_value<- is.na(DsSalaries)
missing_value
```
We can use this also,which will return the number of NA in each column, but there was no missing data
```{r}
colSums(is.na(DsSalaries))
```

STEP12: Removing Duplicates
```{r}
# Count duplicates before removing them
sum(duplicated(DsSalaries))
#removing the duplicate
DsSalaries <- unique(DsSalaries)
# Count duplicates after removing them
sum(duplicated(DsSalaries))
# there was no duplicate

```
STEP13: Data Manipulation, filtering the dataset, i will find the highest earners
```{r}
high_salary <- DsSalaries[DsSalaries$salary_in_usd > 200000, ]
high_salary

```
STEP14: Sorting salaries in ascending and descending other
```{r}

# Top salaries
top_salaries <- arrange(DsSalaries, desc(salary_in_usd))
top_salaries
# Least salaries
least_salaries <- arrange(DsSalaries, salary_in_usd)
least_salaries

# Top 10 salaries
top_10_salaries <- DsSalaries %>% 
  arrange(desc(salary_in_usd)) %>% 
  slice_head(n = 10)

# Least 10 salaries
least_10_salaries <- DsSalaries %>% 
  arrange(salary_in_usd) %>% 
  slice_head(n = 10)
top_10_salaries
least_10_salaries

```
STEP16: Creating a new column for experienced level
```{r}

DsSalaries <- DsSalaries %>%
  mutate(ExperienceLevelCategory = case_when(
    experience_level == "SE" ~ "Senior",
    experience_level == "MI" ~ "Mid-Level",
    experience_level == "EN" ~ "Entry-Level",
    experience_level == "EX" ~ "Executive",
    TRUE ~ "Unknown" # Catch-all for unexpected values
  ))
head(DsSalaries)
```


STEP17: Creating a new column for company_size 

```{r}
DsSalaries <- DsSalaries %>%
  mutate(companysizecategory = case_when(
    company_size == "L" ~ "Large",
    company_size == "M" ~ "Midium",
    company_size == "S" ~ "Small",
    TRUE ~ "Unknown" # Catch-all for unexpected values
  ))
head(DsSalaries)
```
```{r}
DsSalaries <- DsSalaries[, -c(ncol(DsSalaries)-1, ncol(DsSalaries))]
head(DsSalaries) #I mistakenly created extra columns for company size , i had to delete it and recode again
```
```{r}
DsSalaries <- DsSalaries %>%
  mutate(ExperienceLevelCategory = case_when(
    experience_level == "SE" ~ "Senior",
    experience_level == "MI" ~ "Mid-Level",
    experience_level == "EN" ~ "Entry-Level",
    experience_level == "EX" ~ "Executive",
    TRUE ~ "Unknown" # Catch-all for unexpected values
  ))
head(DsSalaries)
```

```{r}
DsSalaries <- DsSalaries %>%
  mutate(companysizecategory = case_when(
    company_size == "L" ~ "Large",
    company_size == "M" ~ "Midium",
    company_size == "S" ~ "Small",
    TRUE ~ "Unknown" # Catch-all for unexpected values
  ))
head(DsSalaries)
```
  STEP:18 AGREGATE
```{r}
# To find the average salary for each job title, useful for identifying trends in DSC order
avg_salary_by_job <- DsSalaries %>%
    group_by(job_title) %>%
    summarise(AverageSalary = mean(salary_in_usd, na.rm = TRUE)) %>%
    arrange(desc(AverageSalary)) 
avg_salary_by_job
 # from the result the highest average earned salary are Data Analytics Lead	405000.00			
 # and Principal Data Engineer	328333.33	
```
(II) Calculating the sum of salaries for each job title

```{r}
total_salary_by_job <- DsSalaries %>%
    group_by(job_title) %>%
    summarise(TotalSalary = sum(salary_in_usd, na.rm = TRUE)) %>%
    arrange(desc(TotalSalary))
total_salary_by_job
# From the result the top three total paid salary by job title are Data Scientist	13433726,Data Engineer	13279754			
# andData Analyst	7387347	
```

(III) TO find out how many employees are in each job title

```{r}
 
employee_count_by_job <- DsSalaries %>%
    group_by(job_title) %>%
    summarise(EmployeeCount = n()) %>%
    arrange(desc(EmployeeCount))
employee_count_by_job
# From the result the job title with the higest number of employee are ; Data Scientist	130	,Data Engineer	121	
# Data Analyst	82	

```
(IV) To calculate the median salary for each job title
```{r}
median_salary_by_job <- DsSalaries %>%
    group_by(job_title) %>%
    summarise(MedianSalary = median(salary_in_usd, na.rm = TRUE)) %>%
    arrange(desc(MedianSalary))
median_salary_by_job
# from the result the highest is still the data scientist lead
```
(V) To find Minimum and Maximum Salary by Job Title
```{r}
min_max_salary_by_job <- DsSalaries %>%
    group_by(job_title) %>%
    summarise(
        MinSalary = min(salary_in_usd, na.rm = TRUE),
        MaxSalary = max(salary_in_usd, na.rm = TRUE)
    ) %>%
    arrange(desc(MaxSalary))
min_max_salary_by_job

```
(VI)To find standard Deviation of Salary by Job Title
```{r}
salary_sd_by_job <- DsSalaries %>%
    group_by(job_title) %>%
    summarise(SalarySD = sd(salary_in_usd, na.rm = TRUE)) %>%
    arrange(desc(SalarySD))
salary_sd_by_job
```
(VII) Number  of Employees by Experience Level
```{r}
employee_count_by_experience <- DsSalaries %>%
    group_by(experience_level) %>%
    summarise(EmployeeCount = n()) %>%
    arrange(desc(EmployeeCount))
employee_count_by_experience
```
(XIII) Average Salary by Company Size
```{r}
avg_salary_by_company_size <- DsSalaries %>%
    group_by(company_size) %>%
    summarise(AverageSalary = mean(salary_in_usd, na.rm = TRUE)) %>%
    arrange(desc(AverageSalary))
avg_salary_by_company_size

```
(IX) Salary Distribution by Remote Ratio
