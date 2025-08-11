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
<img width="622" height="472" alt="output" src="https://github.com/user-attachments/assets/3bcb5515-1bd1-4bf2-b4e7-c435535834b3" />


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
![Trending](Images\output3.png)

##### Bar graph visualizing the trending top skills for data analysts in the US in 2023.

### Insights:
- **SQL** remains the most consistently demanded skill throughout the year, although it shows a gradual decrease in demand.
- **Excel** experienced a significant increase in demand starting around September, surpassing both Python and Tableau by the end of the year.
- Both **Python** and **Tableau** show relatively stable demand throughout the year with some fluctuations but remain essential skills for data analysts.
- **Power BI**, while less demanded compared to the others, shows a slight upward trend towards the year's end.

## 3. How well do jobs and skills pay for Data Analysts?

##### To identify the highest-paying roles and skills, I only got jobs in the United States and looked at their median salary. But first I looked at the salary distributions of common data jobs like Data Scientist, Data Engineer, and Data Analyst, to get an idea of which jobs are paid the most.

View my notebook with detailed steps here: [4_Salary_Analysis.](Project\4_Salary_Analysis.ipynb)

#### Visualize Data
``` 
sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short', order=job_order)

ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
``` 
### Results 
![](Images\output4.png)

#### Box plot visualizing the salary distributions for the top 6 data job titles.

### Insights

- There’s a **significant variation** in salary ranges across different job titles.
- **Senior Data Scientist** positions tend to have the **highest salary potential**, with up to **$600K**, indicating the high value placed on advanced data skills and experience in the industry.
- **Senior Data Engineer** and **Senior Data Scientist** roles show a considerable number of **outliers** on the higher end of the salary spectrum, suggesting that exceptional skills or circumstances can lead to high pay in these roles.
- In contrast, **Data Analyst** roles demonstrate more **consistency** in salary, with fewer outliers.
- The **median salaries** increase with the **seniority** and **specialization** of the roles.
- Senior roles not only have **higher median salaries** but also **larger differences** in typical salaries, reflecting greater variance in compensation as responsibilities increase.

## Highest Paid & Most Demanded Skills for Data Analysts
##### Next, I narrowed my analysis and focused only on data analyst roles. I looked at the highest-paid skills and the most in-demand skills. I used two bar charts to showcase these.

## Visualize Data
``` 

fig, ax = plt.subplots(2, 1)  

# Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')

# Top 10 Most In-Demand Skills for Data Analystsr')
sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, hue='median', ax=ax[1], palette='light:b')

plt.show()
```
### Results 
#### Here's the breakdown of the highest-paid & most in-demand skills for data analysts in the US:
![](Images\output5.png)

##### Two Seperate Bar Graphs visualizing the highest paid skills and most in demand skills for data analyst in the US.

### Insights 

- The **top graph** shows specialized technical skills like **dplyr**, **Bitbucket**, and **Gitlab** are associated with **higher salaries** (up to **$200K**), suggesting that advanced technical proficiency can significantly increase earning potential.
- The **bottom graph** highlights that foundational skills like **Excel**, **PowerPoint**, and **SQL** are the **most in-demand**, even though they may not offer the highest salaries. This demonstrates the importance of these core skills for employability in data analysis roles.
- There’s a clear distinction between the skills that are **highest paid** and those that are **most in-demand**.
- **Career tip:** Data analysts aiming to **maximize career potential** should develop a **diverse skill set** that includes both **high-paying specialized skills** and **widely demanded foundational skills**.

## 4. What are the most optimal skills to learn for Data Analysts?

###### To identify the most optimal skills to learn ( the ones that are the highest paid and highest in demand) I calculated the percent of skill demand and the median salary of these skills. To easily identify which are the most optimal skills to learn.

###### View my notebook with detailed steps here: [5_Optimal_Skills.](Project\5_optimal_Skills.ipynb)

 ### Visualize Data
 ```
 from adjustText import adjust_text
import matplotlib.pyplot as plt

plt.scatter(df_DA_skills_high_demand['skill_percent'], df_DA_skills_high_demand['median_salary'])
plt.show()
```
## Results 
![](Images\output6.png)

#### A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in the US.
## Insights

- **Oracle** has the **highest median salary** (~$97K) despite being **less common** in job postings, indicating the high value placed on specialized database skills in the data analyst profession.
- **Excel** and **SQL** are **widely required** skills with a large presence in job listings, but they have **lower median salaries** compared to specialized skills like **Python** and **Tableau**.
- **Python**, **Tableau**, and **SQL Server** sit towards the **higher end of the salary spectrum** while also being **fairly common** in job listings, suggesting that proficiency in these tools can provide strong career opportunities in data analytics.

### Visualizing Different Techonologies

Let's visualize the different technologies as well in the graph. We'll add color labels based on the technology (e.g., {Programming: Python})

Visualize Data
```
from matplotlib.ticker import PercentFormatter

# Create a scatter plot
scatter = sns.scatterplot(
    data=df_DA_skills_tech_high_demand,
    x='skill_percent',
    y='median_salary',
    hue='technology',  # Color by technology
    palette='bright',  # Use a bright palette for distinct colors
    legend='full'  # Ensure the legend is shown
)
plt.show()
```
## Results 
![](Images\output7.png)
### A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in the US with color labels for technology.

## Insights:

- **Programming Skills** (blue) tend to cluster at **higher salary levels** compared to other categories, suggesting that programming expertise can provide greater salary benefits within the data analytics field.
- **Database Skills** (orange), such as **Oracle** and **SQL Server**, are linked with some of the **highest salaries** among data analyst tools, indicating strong demand and value for data management and manipulation expertise.
- **Analyst Tools** (green), including **Tableau** and **Power BI**, are **widely used** in job postings and offer **competitive salaries**. This category is not only well-paid but also versatile across different types of data tasks, making it essential for modern data roles.

## What I Learned
1. **Advanced Python Usage**  
   Utilized libraries such as **Pandas** for data manipulation, **Seaborn** and **Matplotlib** for visualization, enabling complex data analysis tasks to be performed more efficiently.
2. **Data Cleaning Importance**  
   Realized the critical role of thorough data cleaning and preparation to ensure accurate insights.
3. **Strategic Skill Analysis**  
   Understood the importance of aligning skills with market demand, analyzing the relationship between **skill demand**, **salary**, and **job availability** for strategic career planning.

---

## General Insights
- **Skill Demand & Salary Correlation**  
  Specialized skills like **Python** and **Oracle** often lead to **higher salaries**.
- **Market Trends**  
  Skill demand is dynamic, requiring professionals to adapt to changing trends for career growth.
- **Economic Value of Skills**  
  Knowing which skills are both **in-demand** and **well-compensated** can guide learning priorities.

---

## Challenges Faced
- **Data Inconsistencies**: Managing missing or inconsistent data entries required advanced cleaning techniques.  
- **Complex Data Visualization**: Designing effective visuals for complex datasets was challenging but crucial.  
- **Balancing Breadth & Depth**: Needed to balance detailed analysis with a broad market overview.

---

## Conclusion
This exploration into the data analyst job market has been highly informative, highlighting critical skills and trends shaping the field. The insights gained will guide future skill development and career planning. As the market evolves, **continuous learning and adaptation** will be essential for staying ahead in data analytics.
