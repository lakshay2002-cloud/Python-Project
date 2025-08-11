# Overview 
Welcome to my analysis of the data job market, focusing on data analyst roles. This project was created out of a desire to navigate and understand the job market more effectively. It delves into the top-paying and in-demand skills to help find optimal job opportunities for data analysts.

The data sourced from [Luke Barousse's Python Course](https://www.youtube.com/watch?v=wUSDVGivd-8&t=6186s) which provides a foundation for my analysis, containing detailed information on job titles, salaries, locations, and essential skills. Through a series of Python scripts, I explore key questions such as the most demanded skills, salary trends, and the intersection of demand and salary in data analytics.

# The Questions 
Below are the questions I want to answer in my project:
##### 1. What are the skills most in demand for the top 3 most popular data roles?
##### 2.How are in-demand skills trending for Data Analysts?
##### 3. How well do jobs and skills pay for Data Analysts?
##### 4. What are the optimal skills for data analysts to learn? (High Demand AND High Paying)

# Tools I Used

For my deep dive into the data analyst job market, I harnessed the power of several key tools:

- **Python**: The backbone of my analysis, allowing me to analyze the data and find critical insights.
- **Pandas Library**: Used to analyze the data.
- **Matplotlib Library**: Used to visualize the data.
- **Seaborn Library**: Helped create more advanced visuals.
- **Jupyter Notebooks**: The tool I used to run my Python scripts, which let me easily include my notes and analysis.
- **Visual Studio Code**: My go-to for executing my Python scripts.
- **Git & GitHub**: Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.

# Data Preparation and Cleanup
This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.

## Import & Clean Up Data 
I start by importing necessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure data quality.

``` import ast
import pandas as pd
import seaborn as sns
from datasets import load_dataset
import matplotlib.pyplot as plt  

# Loading Data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
 ``` 

##  Filter US Jobs 
To focus my analysis on the U.S. job market, I apply filters to the dataset, narrowing down to roles based in the United States.

``` df_US = df[df['job_country'] == 'United States'] ```

# The Analysis 
Each Jupyter notebook for this project aimed at investigating specific aspects of the data job market. Here’s how I approached each question:

## 1. What are the most demanded skills for the top 3 most popular data roles?

To find the most demanded skills for the top 3 most popular data roles. I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting.
View my notebook with detailed steps here: [2_Skill_Demand](Project\2_Skills_Count.ipynb)

### Visualize Data 
```  
fig, ax = plt.subplots(len(job_titles), 1)

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)[::-1]
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')

plt.show()
```
### Results
![Likelihood of skills Requested in US job Postings](Images\output.png)

Bar graph visualizing the salary for the top 3 data roles and their top 5 skills associated with each.
#### Insights: 

- **SQL** is the most requested skill for **Data Analysts** and **Data Scientists**, appearing in over half of the job postings for both roles.  
- For **Data Engineers**, **Python** is the most sought-after skill, appearing in **68%** of job postings.  
- **Data Engineers** require more specialized technical skills (**AWS, Azure, Spark**) compared to Data Analysts and Data Scientists, who are expected to be proficient in more general data management and analysis tools (**Excel, Tableau**).  
- **Python** is a versatile skill, highly demanded across all three roles, but most prominently for **Data Scientists (72%)** and **Data Engineers (65%)**.  

## 2. How are in-demand skills trending for Data Analysts?
To find how skills are trending in 2023 for Data Analysts, I filtered data analyst positions and grouped the skills by the month of the job postings. This got me the top 5 skills of data analysts by month, showing how popular skills were throughout 2023.

View my notebook with detailed steps here:[3_Skills_Trend.](Project\3_Skills_Trend.ipynb)

### Visualize data
``` 
from matplotlib.ticker import PercentFormatter

df_plot = df_DA_US_percent.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, legend='full', palette='tab10')

plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.show()
 ```
### Results 
![Trending Top Skills](C:\Users\Lenovo\OneDrive\Documents\Projects\Luke Barousse Projects\Python Course  Data Analysis\Python Project\Images\3 output.png)
