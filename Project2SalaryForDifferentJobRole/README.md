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
## 2. Removing the irrelevant column
```r
DsSalaries <- DsSalaries[, -1]
## 3. Checking to see if step8 worked
```{r}
head(DsSalaries)
```
## 4. Inspecting the dataset
```{r}
head(DsSalaries)
str(DsSalaries)
colnames(DsSalaries)
names(DsSalaries)
summary(DsSalaries)
```
## 5. Checking for missing value
```{r}
missing_value<- is.na(DsSalaries)
missing_value
```
We can use this also,which will return the number of NA in each column, but there was no missing data
```{r}
colSums(is.na(DsSalaries))
```

## 6. Removing Duplicates
```{r}
# Count duplicates before removing them
sum(duplicated(DsSalaries))
#removing the duplicate
DsSalaries <- unique(DsSalaries)
# Count duplicates after removing them
sum(duplicated(DsSalaries))
# there was no duplicate

```
## 7. Data Manipulation, filtering the dataset, i will find the highest earners
```{r}
high_salary <- DsSalaries[DsSalaries$salary_in_usd > 200000, ]
high_salary

```
## 8. Sorting salaries in ascending and descending other
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
## 9. Creating a new column for experienced level
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


## 10. Creating a new column for company_size 

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
  ## 11. AGREGATE
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
## (II) Calculating the sum of salaries for each job title

```{r}
total_salary_by_job <- DsSalaries %>%
    group_by(job_title) %>%
    summarise(TotalSalary = sum(salary_in_usd, na.rm = TRUE)) %>%
    arrange(desc(TotalSalary))
total_salary_by_job
# From the result the top three total paid salary by job title are Data Scientist	13433726,Data Engineer	13279754			
# andData Analyst	7387347	
```

## (III) TO find out how many employees are in each job title

```{r}
 
employee_count_by_job <- DsSalaries %>%
    group_by(job_title) %>%
    summarise(EmployeeCount = n()) %>%
    arrange(desc(EmployeeCount))
employee_count_by_job
# From the result the job title with the higest number of employee are ; Data Scientist	130	,Data Engineer	121	
# Data Analyst	82	

```
## (IV) To calculate the median salary for each job title
```{r}
median_salary_by_job <- DsSalaries %>%
    group_by(job_title) %>%
    summarise(MedianSalary = median(salary_in_usd, na.rm = TRUE)) %>%
    arrange(desc(MedianSalary))
median_salary_by_job
# from the result the highest is still the data scientist lead
```
## (V) To find Minimum and Maximum Salary by Job Title
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
## (VI)To find standard Deviation of Salary by Job Title
```{r}
salary_sd_by_job <- DsSalaries %>%
    group_by(job_title) %>%
    summarise(SalarySD = sd(salary_in_usd, na.rm = TRUE)) %>%
    arrange(desc(SalarySD))
salary_sd_by_job
```
## (VII) Number  of Employees by Experience Level
```{r}
employee_count_by_experience <- DsSalaries %>%
    group_by(experience_level) %>%
    summarise(EmployeeCount = n()) %>%
    arrange(desc(EmployeeCount))
employee_count_by_experience
```
## (XIII) Average Salary by Company Size
```{r}
avg_salary_by_company_size <- DsSalaries %>%
    group_by(company_size) %>%
    summarise(AverageSalary = mean(salary_in_usd, na.rm = TRUE)) %>%
    arrange(desc(AverageSalary))
avg_salary_by_company_size

```
(IX) Salary Distribution by Remote Ratio
```{r}
avg_salary_by_remote_ratio <- DsSalaries %>%
    group_by(remote_ratio) %>%
    summarise(AverageSalary = mean(salary_in_usd, na.rm = TRUE)) %>%
    arrange(desc(AverageSalary))
avg_salary_by_remote_ratio

```
## (X) The highest Paying Companies

```{r}
highest_paying_companies <- DsSalaries %>%
    group_by(company_location) %>%
    summarise(AverageSalary = mean(salary_in_usd, na.rm = TRUE)) %>%
    arrange(desc(AverageSalary))
highest_paying_companies
```

  
## 12. VISUALIZATION

```{r}
install.packages("ggplot2")
library(ggplot2)
ggplot(data = DsSalaries) +
  geom_histogram(
    mapping = aes(x = salary_in_usd), 
    fill = "steelblue", 
    binwidth = 1000,  # Specify bin width
    color = "black"
  ) +
  labs(
    title = "Distribution of Salaries",
    subtitle = "Ds_salary",
    caption = "Source: Ds_salaries",
    x = "Salary in usd",
    y = "Frequency"
  )

# Trying to facet the graph for more clarity
ggplot(data = DsSalaries) +
  geom_histogram(
    mapping = aes(x = salary_in_usd), 
    fill = "steelblue", 
    binwidth = 10000, 
    color = "black"
  ) +
  facet_wrap(~experience_level, scales = "free_y") +
  labs(
    title = "Distribution of Salaries by Experience Level",
    caption = "Source: Ds_salaries",
    x = "Salary in USD",
    y = "Frequency"
  )

```

## (I) PLOTING THE AVERAGE SALARY BY JOB TITLE
```{r}
ggplot(data = avg_salary_by_job) +
  geom_bar(
    mapping = aes(x = reorder(job_title, -AverageSalary), y = AverageSalary), 
    stat = "identity", 
    fill = "steelblue"
  ) +
  labs(
    title = "Average Salary by Job Title",
    subtitle = "Sorted in Descending Order",
    caption = "Source: DsSalaries",
    x = "Job Title",
    y = "Average Salary (USD)"
  ) +
  theme(axis.text.x = element_text(angle = 90, hjust = 1))  # Rotate x-axis labels

```
## (II) REMOT RATIO VS SALARY
```{r}
ggplot(data = DsSalaries) +
  geom_boxplot(
    mapping = aes(x = factor(remote_ratio), y = salary_in_usd), 
    fill = "steelblue"
  ) +
  labs(
    title = "Remote Ratio vs Salary",
    subtitle = "Ds_salaries",
    caption = "Source: Ds_salaries",
    x = "Remote Ratio",
    y = "Salary in USD"
  )

```

## (III) Count of Employees by Job Title
```{r}
# 1. Count of Employees by Job Title
employee_count_by_job <- DsSalaries %>%
  count(job_title, name = "EmployeeCount")

ggplot(data = employee_count_by_job) +
  geom_bar(mapping = aes(x = reorder(job_title, -EmployeeCount), y = EmployeeCount, fill = job_title), stat = "identity") +
  labs(
    title = "Count of Employees by Job Title",
    x = "Job Title",
    y = "Number of Employees"
  ) +
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) +
  theme(legend.position = "none") 
```
## (IV) MININUM AND MXIMUM SALARY BY JOB TITLE
```{r}
min_max_salary_by_job <- DsSalaries %>%
  group_by(job_title) %>%
  summarise(
    MinSalary = min(salary_in_usd, na.rm = TRUE),
    MaxSalary = max(salary_in_usd, na.rm = TRUE)
  )

ggplot(data = min_max_salary_by_job) +
  geom_segment(aes(x = job_title, xend = job_title, y = MinSalary, yend = MaxSalary), color = "blue") +
  geom_point(aes(x = job_title, y = MinSalary), color = "red", size = 3) +
  geom_point(aes(x = job_title, y = MaxSalary), color = "green", size = 3) +
  labs(
    title = "Minimum and Maximum Salary by Job Title",
    x = "Job Title",
    y = "Salary (USD)"
  ) +
  theme(axis.text.x = element_text(angle = 90, hjust = 1))

```
## (V) Count of Employees by Experience Level
```{r}
employee_count_by_experience <- DsSalaries %>%
  count(experience_level, name = "EmployeeCount")

ggplot(data = employee_count_by_experience) +
  geom_bar(mapping = aes(x = reorder(experience_level, -EmployeeCount), y = EmployeeCount, fill = experience_level), stat = "identity") +
  labs(
    title = "Count of Employees by Experience Level",
    x = "Experience Level",
    y = "Number of Employees"
  ) +
  theme_minimal() 
```
## (V) company size by salary
```{r}
ggplot(data = DsSalaries, aes(x = company_size, y = salary_in_usd, fill = company_size)) +
  geom_boxplot() +
  labs(
    title = "Company Size vs Salary",
    subtitle = "Salaries by Company Size",
    caption = "Source: DsSalaries Dataset",
    x = "Company Size",
    y = "Salary in USD"
  ) +
  scale_fill_manual(
    values = c("S" = "steelblue", "M" = "orange", "L" = "green") # Customize colors for each company size
  ) +
  theme_minimal()

```
## (VI) HIGHEST PAYING COMPANY

```{r}
highest_paying_companies <- DsSalaries %>%
  group_by(company_location) %>%
  summarise(AverageSalary = mean(salary_in_usd, na.rm = TRUE)) %>%
  arrange(desc(AverageSalary)) %>%
  slice_head(n = 10)

ggplot(data = highest_paying_companies) +
  geom_bar(mapping = aes(x = reorder(company_location, -AverageSalary), 
                         y = AverageSalary, 
                         fill = company_location), stat = "identity") +
  labs(
    title = "Top 10 Highest Paying Companies",
    x = "Company Location",
    y = "Average Salary (USD)"
  ) +
  theme(axis.text.x = element_text(angle = 90, hjust = 1))  # Optional: Rotate x-axis labels

```
