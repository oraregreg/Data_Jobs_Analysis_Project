# Overview

An analysis of the data job market, focusing on data analyst roles. The project looks at the different top-paying and in-demand skills to help find optimal job opportunities for data analysts.

The data contains detailed information on job titles, salaries, locations, and essential skills. Through a series of Python scripts, I explore key questions such as the most demanded skills, salary trends, and the intersection of demand and salary in data analytics.

The Questions

1. What are the skills most in demand for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the optimal skills for data analysts to learn? (High Demand AND High Paying)

Tools Used

Python: The backbone of my analysis, allowing me to analyze the data and find critical insights.I also used the following Python libraries:

Pandas Library: This was used to analyze the data.

Matplotlib Library: I visualized the data.

Seaborn Library: Helped me create more advanced visuals.

Jupyter Notebooks: The tool I used to run my Python scripts which let me easily include my notes and analysis.

Visual Studio Code IDE

Git & GitHub: Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.


# The Analysis
## 1. What are the top 3 skills for the top 3 most popular data roles?

Filter out the positions which were the most popular, and get the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills to pay attention to depending on the role being targeted.

View my notebook with detailed steps here:
[2_Skills_Demand.ipynb](2_PROJECT/2_Skills_Demand.ipynb)

### Visualize the data
``` Python
# Merge and calculate percentages
df_skills_perc = pd.merge(df_skills_count, df_job_title_count, how='left', on='job_title_short')
df_skills_perc['skill_perc_numeric'] = (df_skills_perc['skill_count'] / df_skills_perc['jobs_total']) * 100
df_skills_perc['skill_perc'] = df_skills_perc['skill_perc_numeric'].round(1).astype(str) + '%'

# Create the plots
fig, ax = plt.subplots(len(job_titles), 1, figsize=(6, 6))

sns.set_theme(style='ticks')

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)[::-1]
    
    # Create barplot
    sns.barplot(data=df_plot, x='skill_perc_numeric', y='job_skills', ax=ax[i], 
                hue='skill_perc_numeric', palette='dark:b_r', legend=False)
    
    # Add percentage labels on each bar
    for j, (idx, row) in enumerate(df_plot.iterrows()):
        ax[i].text(row['skill_perc_numeric'] + 1, j, 
                   f"{row['skill_perc_numeric']:.1f}%", 
                   va='center', fontsize=9)
    
    ax[i].set_title(job_title)
    ax[i].invert_yaxis()
    ax[i].set_ylabel('')
    ax[i].set_xlabel('')
    ax[i].set_xlim(0, 80)

fig.suptitle('Percentage of Skills Requested in UK Job Postings', fontsize=15)
fig.tight_layout(h_pad=0.5)
plt.show()
```
### Results
![Visualization of top skills](2_PROJECT\Images\skills_demand_all_roles.png)

### Insights
Based on the visualization showing skill percentages for UK data roles, here are the Key insights:

•	Most requested skill is SQL across all three positions: 69% for Data Scientists, 60% for Data Engineers, 43% for Data Analysts
•	Excel Remains Critical as 41-55% of all roles require Excel - highest for Data Engineers (55%)
•	Power BI outranks Python for Data Analysts (27% vs 20%) and Data Engineers (41% vs 33%)
•	Python surprisingly low for Data Scientists at only 17%

Role-Specific Highlights
•	Data Analysts: SQL (43%) and Excel (41%) are nearly tied as top skills
•	Data Engineers: SQL (60%) leads, followed by Excel (55%)
•	Data Scientists: SQL (69%) dominates; Power BI (29%) second

Key Takeaway
Traditional tools (SQL, Excel, Power BI) are more widely requested than Python across all UK data roles in this dataset.

## 2. How are in-demand skills trending for data analysts in the UK?

### Visualize data

``` Python
from matplotlib.ticker import PercentFormatter

df_plot = df_DA_UK_percent.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, legend='full', palette='tab10')
sns.set_theme(style='ticks')
sns.despine()

plt.title('Trending Top Skills for Data Analysts in the UK')
plt.ylabel('Likelihood in Job Posting')
plt.xlabel('2023')
plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))
plt.legend(title='Skills', bbox_to_anchor=(1.05, 1), loc='upper left')  # Put legend outside

plt.tight_layout()
plt.show()
```
![Trending top skills for Data Analysts in the UK](2_PROJECT\Images\skills_trend_data_analysts_UK.png)

### Insights
•	SQL and Excel are consistently the most in-demand skills (40-55% range)

•	Python shows the strongest upward trend, rising from 18% in Jan to 39% in Dec

•	Tableau remains relatively flat and lowest overall (12-23%)

Notably:

•	Excel spikes dramatically in summer months (reaching 85% in June/July/Sept)

•	SQL remains stable throughout the year (45-54% range)

•	Power BI fluctuates between 23-38%, with no clear seasonal pattern

•	Python doubles from Jan (18%) to Dec (39%) - fastest growing skill

Key Takeaway

While SQL/Excel dominates traditional analyst work, Python is rapidly gaining ground - suggesting UK Data Analysts should prioritize learning Python for future career growth.

## 3. How well do jobs and skills pay for different data jobs in the UK?

### Visualize the data

``` Python
# Set theme BEFORE plotting
sns.set_theme(style='ticks')
sns.despine()

# Create boxplot
sns.boxplot(data=df_UK_top6, x='salary_year_avg', y='job_title_short', order=job_order)

# Formatting
plt.title('Salary Distributions of Data Jobs in the UK')
plt.xlabel('Yearly Salary (USD)')
plt.ylabel('')
plt.xlim(0, 400000) 
ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```

![Salary distribution of jobs in the UK](2_PROJECT\Images\skills_trend_data_analysts_UK.png)


### Insights
Based on the UK salary boxplot:

Highest paid: Data Scientist & Senior Data Engineer (~$100K+ median)

Mid-tier: Data Engineer (~$90-100K), Senior Data Analyst

Lowest paid: Data Analyst & Business Analyst (~$60-70K)

Key points:

•	Senior roles earn significantly more

•	Data Engineer has tightest salary range

•	Outliers reach $300K+ for senior technical roles

•	Business Analyst has most compressed range

## 4. How well do jobs and skills pay for data

### Highest Paid and most demanded skills for data

### Visualize the data

``` Python
fig, ax = plt.subplots(2, 1, figsize=(10, 8))

# Top 10 Highest Paid Skills for Data Analysts in the UK
sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')
ax[0].legend().remove()
ax[0].set_title('Highest Paid Skills for Data Analysts in the UK', fontsize=12)
ax[0].set_ylabel('')
ax[0].set_xlabel('')
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K'))

# Top 10 Most In-Demand Skills for Data Analysts in the UK
sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, hue='median', ax=ax[1], palette='light:b')
ax[1].legend().remove()
ax[1].set_title('Most In-Demand Skills for Data Analysts in the UK', fontsize=12)
ax[1].set_ylabel('')
ax[1].set_xlabel('Median Salary (USD)')
ax[1].set_xlim(ax[0].get_xlim())  # Set the same x-axis limits as the first plot
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K'))

sns.set_theme(style='ticks')
plt.tight_layout()
plt.show()
```
![Highest Paid skills for data analysts](2_PROJECT\Images\skills_trend_data_analysts_UK.png)

### Insights

Top-paying skills ($170-175K):
•	c++, numpy, tensorflow, pytorch, pandas, aurora, kafka, aws, mysql, postgresql

Mid-range skills ($150-170K):
•	tableau, sql, looker, power bi, python, sas, r

Lower range ($130-150K):
•	excel, go, outlook

Notable Observations

•	Programming & cloud skills (c++, Python, AWS, Kafka) dominate top pay tiers

•	Traditional analytics tools (Excel, SQL, Tableau) pay less than programming languages

•	Only 3 skills (c++, numpy, tensorflow) reached the $175K maximum

Takeaway

For UK Data Analysts, programming and cloud skills command significantly higher salaries than traditional BI and spreadsheet tools.

## 5. What is the most optimal skill for data analysts in the UK?

``` Python
sns.scatterplot(
    data=df_DA_skills_tech_high_demand,
    x='skill_percent',
    y='median_salary',
    hue='technology'
)

sns.despine()
sns.set_theme(style='ticks')

# Prepare texts for adjustText
texts = []
for i, txt in enumerate(df_DA_skills_high_demand.index):
    texts.append(plt.text(df_DA_skills_high_demand['skill_percent'].iloc[i], df_DA_skills_high_demand['median_salary'].iloc[i], txt))

# Adjust text to avoid overlap
adjust_text(texts, arrowprops=dict(arrowstyle='->', color='gray'))

# Set axis labels, title, and legend
plt.xlabel('Percent of Data Analyst Jobs')
plt.ylabel('Median Yearly Salary')
plt.title('Most Optimal Skills for Data Analysts in the UK')
plt.legend(title='Technology')

from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))
ax.xaxis.set_major_formatter(PercentFormatter(decimals=0))

# Adjust layout and display plot 
plt.tight_layout()
plt.show()
```

![Most optimal skills for data analysts in the UK](2_PROJECT/Images/Most%20optimal%20skills%20for%20data%20analysts%20UK.png)

### Key Insights

High Demand & High Pay:

•	Python (30% demand, $85K) - Best balance of demand and salary

•	Tableau (18%, $105K) - Highest pay among common skills

•	Azure (13%, $100K) & **Looker** (12%, $100K) - Strong pay with moderate demand

High Demand, Lower Pay:

•	Excel (40% demand, $75K) - Most demanded but lower paying

•	Power BI (14%, $90K) - Solid demand, decent pay

Niche but Well-Paid:

•	SQL Server (11%, $130K) - Highest paying skill

•	Flow (10%, $118K) - Second highest pay

Takeaway

UK Data Analysts maximize career potential by balancing Python (high demand + good pay) or specializing in SQL Server/Flow (lower demand but premium salaries). Excel dominates job postings but pays below average.




