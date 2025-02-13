# The Analysis

## What are the most demanded skills for the top 3 most popular data roles in the USA and in Germany?

To identify the most demanded skills, I analyzed the top 3 most popular data roles in each market separately. I filtered out the job titles that were most frequently posted in the U.S. and Germany and then determined the top 5 skills required for these roles in each country. This analysis highlights key similarities and differences between the U.S. and German markets, offering valuable insights into the specific skills that are highly sought after depending on the region.

This comparison is particularly useful for understanding how the demand for data-related skills varies by market, allowing job seekers to tailor their skill set to better match the needs of employers in their target country.

View my notebook with detailed steps here: [2_Skill_Demand.ipynb](3_Project\2_Skills_Count.ipynb)

### Visualize Data

```python
# Side-by-Side Comparison for USA and Germany with Percentage Labels
max_rows = max(len(job_titles_US), len(job_titles_DE))
fig, axes = plt.subplots(max_rows, 2, figsize=(14, 4 * max_rows))  # Adjust figure size dynamically

for i in range(max_rows):
    # Handle USA Job Titles
    if i < len(job_titles_US):
        job_title = job_titles_US[i]
        df_US_plot = df_skills_perc_US[df_skills_perc_US['job_title_short'] == job_title].head(5)
        sns.barplot(
            data=df_US_plot,
            x='skill_percent',
            y='job_skills',
            ax=axes[i, 0],
            palette='Oranges_r'  # Set palette to orange for USA
        )
        axes[i, 0].set_title(f"{job_title} (USA)")
        axes[i, 0].set_ylabel('')
        axes[i, 0].set_xlabel('Likelihood (%)')
        axes[i, 0].set_xlim(0, 78)

        # Add percentage labels to USA bars
        for n, v in enumerate(df_US_plot['skill_percent']):
            axes[i, 0].text(v + 1, n, f'{v:.0f}%', va='center')
    else:
        axes[i, 0].axis('off')  # Turn off unused subplot

    # Handle Germany Job Titles
    if i < len(job_titles_DE):
        job_title = job_titles_DE[i]
        df_DE_plot = df_skills_perc_DE[df_skills_perc_DE['job_title_short'] == job_title].head(5)
        sns.barplot(
            data=df_DE_plot,
            x='skill_percent',
            y='job_skills',
            ax=axes[i, 1],
            palette='Blues_r'  # Set palette to blue for Germany
        )
        axes[i, 1].set_title(f"{job_title} (Germany)")
        axes[i, 1].set_ylabel('')
        axes[i, 1].set_xlabel('Likelihood (%)')
        axes[i, 1].set_xlim(0, 78)

        # Add percentage labels to Germany bars
        for n, v in enumerate(df_DE_plot['skill_percent']):
            axes[i, 1].text(v + 1, n, f'{v:.0f}%', va='center')
    else:
        axes[i, 1].axis('off')  # Turn off unused subplot

# Adjust layout with tight layout
fig.suptitle('Likelihood of Skills Requested in Job Postings (USA vs Germany)', fontsize=15)
fig.tight_layout(h_pad=0.8, w_pad=2.5)  # Adjust spacing between plots
plt.show()
```

![Visualization of Top Skills for Data Nerds in the USA and Germany](https://github.com/chriskorol/Job_Market_Analysis/blob/main/3_Project/images/Salary_distribution_of_top_6.png)

### Insights

#### _Python Dominates Across Roles:_

Python is the most requested skill in both the U.S. and Germany across all roles, especially for Data Scientists, where it appears in 72% of U.S. job postings and 62% of German job postings.
For Data Engineers, Python also plays a crucial role, being required in 65% of U.S. postings and 53% in Germany.

#### _SQL as a Fundamental Skill:_

SQL is a highly sought-after skill for Data Analysts in both markets, appearing in 51% of U.S. job postings and 41% of German postings.
It also ranks high for Data Scientists and Data Engineers in both countries, reflecting its importance as a core data manipulation tool.

#### _Regional Differences in Tool Preferences:_

In the U.S., technical tools such as AWS, Azure, and Spark are more prominent for Data Engineers, while in Germany, these skills are requested less frequently, with Azure and AWS at 29% and 25%, respectively.
German job postings for Data Analysts emphasize Power BI (18%), which is absent from the top tools in U.S. postings.

#### _Versatility of Python and SQL:_

Python and SQL consistently appear across all three roles in both markets, highlighting their versatility and critical importance for data professionals.
However, the prominence of Python for Data Scientists is greater in the U.S. (72%) compared to Germany (62%).

#### _General vs. Specialized Skills:_

Data Analysts in both countries are expected to have broader, general-purpose skills such as Excel (41% in the U.S.), Tableau (28% in the U.S., 19% in Germany), and Power BI (Germany-specific).
Conversely, Data Engineers require more specialized technical skills in both markets, with the U.S. showing a stronger emphasis on cloud-based tools.

## How are in-demand skills trending for Data Analysts in the USA and in Germany?

### Visualize Data

```python
from matplotlib.ticker import PercentFormatter

# Create a figure with two subplots
fig, axes = plt.subplots(1, 2, figsize=(14, 5))

# Plot for the USA
sns.lineplot(data=df_DA_US_percent.iloc[:, :5], ax=axes[0], dashes=False)
axes[0].set_title('Top Skills for Data Analysts in the USA (2023)')
axes[0].set_ylabel('Likelihood in Job Postings (%)')
axes[0].set_xlabel('Month')
axes[0].yaxis.set_major_formatter(PercentFormatter())

# Plot for Germany
sns.lineplot(data=df_DA_DE_percent.iloc[:, :5], ax=axes[1], dashes=False)
axes[1].set_title('Top Skills for Data Analysts in Germany (2023)')
axes[1].set_ylabel('Likelihood in Job Postings (%)')
axes[1].set_xlabel('Month')
axes[1].yaxis.set_major_formatter(PercentFormatter())

# Adjust layout and display
plt.tight_layout()
plt.show()

```

### Results

![Trending Top Skills for Data Analysts in the USA and Germany](Job_Market_Analysis/tree/main/3_Project/images/Trending_Top_skills.png)
_Bar graph visualizing the trending top skills for data analysts in the USA and Germany in 2023._

### Insights

SQL and Python dominate in both markets, but the U.S. market shows more stability, while the German market exhibits greater volatility in skill demand.
Visualization tools like Tableau and Power BI are in moderate demand in both markets but appear slightly more important in Germany towards the end of the year.

## 3. How well do jobs and skills pay for Data Analysts in the USA and Germany?

### Salaty Analysis for Data Nerds

#### Visualize Data

```python

fig, axes = plt.subplots(1, 2, figsize=(14, 6))

# USA Plot
sns.boxplot(
    data=df_US_top6,
    x='salary_year_avg',
    y='job_title_short',
    ax=axes[0],
    palette='Oranges_r'  # Reverse orange colors
)
axes[0].set_title('Top 6 Data Job Salaries in the USA')
axes[0].set_xlabel('Yearly Salary (USD)')
axes[0].xaxis.set_major_formatter(FuncFormatter(lambda x, _: f'${int(x/1000)}K'))

# Germany Plot
sns.boxplot(
    data=df_DE_top6,
    x='salary_year_avg',
    y='job_title_short',
    ax=axes[1],
    palette='Blues_r'  # Reverse blue colors
)
axes[1].set_title('Top 6 Data Job Salaries in Germany')
axes[1].set_xlabel('Yearly Salary (EUR)')
axes[1].xaxis.set_major_formatter(FuncFormatter(lambda x, _: f'€{int(x/1000)}K'))

plt.tight_layout()
plt.show()

```

#### Results

![Salary Distributions of Data Jobs in the US and Germany](3_Project\images\Salary_distribution_of_top_6.png)
_Box plot visualizing the salary distributiona for the top 6 data titles._

#### Insights

##### Higher Salaries in the USA:

-Median salaries for all roles in the USA are higher compared to Germany.
Senior positions, especially Senior Data Scientist, offer significantly higher top-end salaries in the USA, with outliers exceeding $400K.
Smaller Salary Ranges in Germany:

-Salary distributions in Germany are tighter, suggesting more consistent compensation across roles.
Fewer extreme outliers compared to the USA.
Machine Learning Engineer:

-A notable difference between markets. This role has significant variability in Germany but is not present among the top 6 in the USA.

### Highest Paid & Most Demanded Skills for Data Analysts in the US and Germany

#### Visualize Data

```python

fig, axes = plt.subplots(2, 2, figsize=(16, 10))

# USA and Germany datasets
datasets = [(df_DA_top_pay_US, 'Highest Paid Skills (USA)', 'Median Salary (USD)', 'Oranges_r', axes[0, 0]),
            (df_DA_top_pay_DE, 'Highest Paid Skills (Germany)', 'Median Salary (EUR)', 'Blues_r', axes[0, 1]),
            (df_DA_skills_US, 'Most In-Demand Skills (USA)', 'Median Salary (USD)', 'Oranges_r', axes[1, 0]),
            (df_DA_skills_DE, 'Most In-Demand Skills (Germany)', 'Median Salary (EUR)', 'Blues_r', axes[1, 1])]

# Loop through datasets to create each plot
for data, title, xlabel, palette, ax in datasets:
    sns.barplot(data=data, x='median', y=data.index, palette=palette, ax=ax)
    ax.set_title(title)
    ax.set_xlabel(xlabel)
    ax.set_xlim(0, 200000)
    ax.xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K' if 'USD' in xlabel else f'€{int(x/1000)}K'))
    ax.set_ylabel('')

plt.tight_layout()
plt.show()

```

#### Results

![The Highest Paid & Most In-Demand Skills for Data Analysts in the US and Germany](3_Project\images\highest_paid_skills.png)
_For separate bar graphs visualizing the highest paid skills and most in-demand skills for data analysts in the US._

#### Insights

##### Highest Paid Skills:

In the USA, skills like dplyr, solidity, hugging face, and gitlab dominate with median salaries up to $200K, reflecting high demand for niche technical expertise. In Germany, top-paying skills include github, bigquery, and nosql, emphasizing cloud technologies and infrastructure tools, with salaries reaching up to €150K–€200K. Overall, the USA shows higher earning potential across most top-paid skills.

##### Most In-Demand Skills:

Both markets heavily demand Python, SQL, and Tableau, but the USA offers higher median salaries for these foundational skills. Germany places additional focus on large-scale data processing tools like spark and emerging technologies such as looker and go, reflecting its emphasis on modern infrastructure and data engineering.

##### Key Comparison:

The USA provides higher salaries for both niche and general skills, making it lucrative for specialists. Germany offers steadier compensation, with a focus on advanced cloud and data engineering tools. Professionals should prioritize skills like Python, SQL, and Tableau for both markets while considering niche technologies for their targeted region.
