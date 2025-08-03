# Overview
Welcome to my exploratory data analysis of the data job market, focusing on data engineer roles. This project was created out of a desire to navigate and understand the job market more effectively. It delves into the top-paying and in-demand skills to help find optimal job opportunities for data engineers.

# The Questions
What are the most demanded skills for the top 3 most popular data roles?

How are in-demand skills trending for Data Engineers?

How well do jobs and skills pay for Data Engineers?

What is the most optimal skill to learn for Data Engineers?

# Tools I Used
For my deep dive into the data engineer job market, I harnessed the power of several key tools:

Python: The backbone of my analysis, allowing me to analyze the data and find critical insights. I also used the following Python libraries:
1. Pandas Library: This was used to analyze the data.
2. Matplotlib Library: This was used to visualized the data
3. Seaborn Library: This was used to create more advanced visuals

Jupyter Notebooks: The tool I used to run my Python scripts, which let me easily include my notes and analysis.

Visual Studio Code: My go-to for executing my Python scripts.

Git & GitHub: Essential for version control and sharing my Python code and analysis, ensuring collaboration and seamless tracking of changes across projects.

# Analysis 1

## 1. What are the most demanded skills for the top 3 most popular data roles?

To identify the most demanded skills for the top 3 data roles, I filtered out those positions and got the top 5 skills for each roles in the US market. This query underscores the most popular job titles as well as the most requested skills.

View my notebook with detailed steps here:
[2_Skill_Demand.ipynb](project\2_Project_Skill_Demand.ipynb)

### Visualize Data

```python
fig, ax = plt.subplots(len(job_titles),1)

for i, job_title in enumerate(job_titles):
    df_plot = (df_skills_perc[df_skills_perc['job_title_short'] == job_title].head())
    sns.set_theme(style='ticks')
    sns.barplot(data=df_plot,x='skill_percent',y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')
    for n, v in enumerate(df_plot['skill_percent']):
        ax[i].text(v + 1, n, f'{v:.1f}%', va='center')
    if i!=len(job_titles)-1:
        ax[i].set_xticks([])
plt.show()
```
### Results
![Visualization of Top Skills](project\skill_demand.png)

### Insights
1. SQL are highly demanded and versitile skills across all three roles.
2. For Data Scientists, Python is prominently important for Data Scientist (72%).
3. The skills demanded for Data Engineers seem peculiar compared to skills requested for the other top 3 roles, but those idiosyncratic skills are more specialized technical skills (AWS, Azure, and Spark) to handle unique daily task.

# Analysis 2
## 2. How are in-demand skills trending for Data Engineer?
To find the subtle skill trend hidden, I first filtered Data Engineer jobs as well as its top 5 skills in the US market. This query highlights trends for each highly requested skills throughout the year.
### Visualize Data

```python

from matplotlib.ticker import PercentFormatter

sns.lineplot(data=df_de_us_percent, dashes=False, palette='tab10')
sns.despine()

ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter())
for i in range(5):
    plt.text(11.3, df_de_us_percent.iloc[-1, i], df_de_us_percent.columns[i])

plt.show()
```
### Result
![Trending Skills for Data Engineer in the US](project\skill_trend.png)

### Insight
1. SQL is the regularly demanded skill throughout the year.
2. Python has a similar trend compared to SQL, where both encountered a significant decline between July and August.
3. The specialized skills - aws, azure, and spark - are also consistent, which fluctuate less than 10% throughout the year.

# Analysis 3

## 3. How well do jobs and skills pay for Data Engineer?
To discover how lucrative Data Engineer jobs are, I first filtered Top 6 data jobs in the US and use boxplot to visualize them. Then, I filtered skills that is frequently requested (>15%) and calculated the median annual salary for each skills. This query provides a comprehensive view of how well do jobs pay for Data Engineer as well as how lucrative each skill is for Data Engineer.
### Salary Analysis for Top 6 Data Role
#### Visualize Data
```python
job_list = [df_us[df_us['job_title_short']==job_title]['salary_year_avg'] for job_title in job_titles]

sns.boxplot(data=df_us_top6, x='salary_year_avg',y='job_title_short', order=job_order)
ax = plt.gca()
ax.xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}K'))
plt.show()
```
#### Result
![Top 6 data role salary](project\salary_analysis.png)
#### Insight
From the boxplot, it is obvious that the median starting salary of Data Scientist and Data Engineer as a rookie is similar. However, as moving forward to senior roles, the median salary of Data Scientist is higher than other data roles. For Data Analyst, even if stepping into a senior role, it is still the worse paying job among the data roles in this analysis.
### Top Skills for Data Engineer 
#### Visualize Data
```python
fig, ax = plt.subplots(2,1)
sns.barplot(data=df_de_top_pay, x='median', y=df_de_top_pay.index,ax=ax[0], hue='median',palette='dark:b_r')
sns.barplot(data=df_de_skills, x='median', y=df_de_skills.index,ax=ax[1], hue='median',palette='light:b')

ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}k'))
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}k'))
ax[1].set_xlim(ax[0].get_xlim())

plt.show()
```
#### Result
![Top skill for Data Engineer](project\data_engineer_top_skills.png)

#### Insight
From the bar chart, it is worth noting that the highest-paying skills are not necessarily the most in-demand skills in the current job market. Nevertheless, it is clear from both charts that cloud technology skills are valuable to Data Engineers due to the depth of cloud technology involved in their daily tasks.
# Analysis 4

## 4. What is the most optimal skill to learn for Data Engineer?
As the previous section took a closer look at skills, the results betrayed a lack of alignment between lucrativeness and demand. This query will reveal the most optimal skill to learn for Data Engineer by taking both demand and payments into account.
### Visualize Data
```python
sns.scatterplot(
    data=df_de_skills,
    x='skill_percent',
    y='median_salary',
    hue='technology'
)
texts = []
for i, txt in enumerate(df_de_skills.index):
    texts.append(plt.text(df_de_skills['skill_percent'].iloc[i], df_de_skills['median_salary'].iloc[i],txt))
adjust_text(texts, arrowprops=dict(arrowstyle="->",color='gray',lw=2))
ax=plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}k'))
ax.xaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.show()
```
### Results
![Optimal Skills for DE](project\optimal_skills_DE.png)
### Insight
The scatter plot suggests that Python and SQL are the top 2 in-demand skills for Data Analysts in terms of programming languages; however, Java and other programming languages are associated with higher salary, which may be explained by the fact that as one moves up the hierarchy, these skills become more common in day-to-day tasks, but deeper research is necessitated to validate this point of view. Similarly, skills in programming libraries could also be influenced by this fact; however, there is also a possibility that proficiency in certain libraries directly contributes to higher compensation due to their highly specialized application in some complex project. In addition, AWS, a cloud computing skill, is also in demand (45%+), ranking just behind SQL and Python but more lucrative.

# Conclusion

This project provided several general insights into the data job market for engineers:

1. Skill Demand and Salary Correlation: There is no clear correlation between the demand for specific skills and the salaries these skills command. The top in-demand skills tend to be associated with lower salaries while skills linked to higher salaries often show less demand. This gap suggests an opportunity for further research: As advances in their career hierarchy, how does the skills set evolves? 

2. Economic Value of Skills: Understanding which skills are both in-demand and well-compensated can guide data engineer in prioritizing learning to maximize their economic returns.

3. Market Trends: The trends in skill demand is static, indicating the economic value of skills will not drastically change in a short term.

