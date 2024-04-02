# Introduction
Dive into the data job market! Focusing on data scientist roles, this project explores top-paying job, in-demanded skills, and where high demand meets high salary in data sciences.

SQL queries? Check them out here:
[project_sql folder](/project1/)


# Background
Driven by a quest to navigate the data scientist job market more effectively, this project was born from a desire to pinpoint top-paid and in-demand skills, streamlining others work to find optimal jobs.

### The questions I wanted to answer through my SQL queries were:
1. What are the top-paying data scientist jobs?
2. What skills are required for these top-paying jobs?
3. What skills are most in demand for data scientist?
4. Which skills are associated with higher salaries?
5. What are the most optimal skills to learn

# Tools I Used
For my deep dive into the data scientist job market, I harnessed the power of several key tools:

- **SQL:** The backbone of my analysis, allowing me to query the database and unearth critical insights.
- **PostgreSQL:** The chosen database management system, ideal for handling the job posting data.
- **Visual Studio Code:** My go-to for database management and executing SQL queries.
- **Git & GitHub:** Essential for version control and sharing my SQL scripts and analysis, ensuring collaboration and project tracking.

# The Analysis
Each query for this project aimed at investigating specific aspects of the data scientist job market. Here’s how I approached each question:
### 1. Top Paying Data Scientist Jobs
To identify the highest-paying roles, I filtered data scientist positions by average yearly salary and location, focusing on remote jobs. This query highlights the high paying opportunities in the field.
```sql
SELECT
    job_id,
    job_title,
    job_location,
    job_schedule_type,
    salary_year_avg,
    job_posted_date,
    name as company_name
FROM
    job_postings_fact
LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
WHERE
    job_title_short = 'Data Scientist' AND
    job_location = 'Anywhere' AND
    salary_year_avg is NOT NULL
ORDER BY
    salary_year_avg desc
LIMIT 10
```
Here's the breakdown of the top data scientist jobs in 2023:

- **Wide Salary Range:** Top 10 paying data scientist roles above $300k, indicating significant salary potential in the field.
- **Diverse Employers:** Companies like Walmart, Demandbaseare, and Lawrence Harvey are among those offering high salaries, showing a broad interest across different industries.
- **Job Title Variety:** There's a high diversity in job title, reflecting varied roles and specializations within data science.
![Top Paying Roles](/asset/top_paying_job.png)
Bar graph visualizing the salary for the top 10 salaries for data scientist; By using Python and its libraries(Pandas, Matplotlib) to generate the graph.

## 3. In_Demand Skills for Data Scientist
This query helped identify the skills most frequently requested in job postings, directing focus to areas with high demand.
```sql
SELECT 
    skills,
    count(skills_job_dim.job_id) AS demand_count
FROM job_postings_fact
INNER JOIN skills_job_dim on job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim on skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Scientist' AND
    job_work_from_home = TRUE
GROUP by
    skills
ORDER BY
    demand_count DESC
limit 5
```
Here's the breakdown of the most demanded skills for data scientist in 2023

**SQL and Python** emphasizing the need for strong foundational skills in data processing, analysis and visualising.
Programming and Visualization Tools and Platform like r, aws, and tableau are essential, pointing towards the increasing importance of technical skills in data storytelling and decision support.
![Skills In_Demand](/asset/table_for_In_demand_job.png)
Table of the demand for the top 5 skills in data scientist job postings

## 4.Skills Based on Salary
Exploring the average salaries associated with different skills revealed which skills are the highest paying.
```sql
SELECT 
    skills,
    ROUND(AVG(salary_year_avg), 2) as avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim on job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim on skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Scientist' 
    AND salary_year_avg is not NULL
    AND job_work_from_home = TRUE
GROUP by
    skills
ORDER BY
    avg_salary DESC
limit 10
```
Here's the results for top paying skills for Data science:
![Top skills base on salary](/asset/table_skills_based_on_salary.png)
Table of the average salary for the top 10 paying skills for data scientist.

## 5.Most Optimal Skills to Learn
Combining insights from demand and salary data, this query aimed to pinpoint skills that are both in high demand and have high salaries, offering a strategic focus for skill development.
```sql
SELECT
    skills_dim.skill_id,
    skills_dim.skills,
    COUNT(skills_job_dim.job_id) AS demand_count,
    ROUND(AVG(job_postings_fact.salary_year_avg), 2) as avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim on job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim on skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Scientist' 
    AND salary_year_avg is not NULL
    AND job_work_from_home = TRUE
GROUP BY
    skills_dim.skill_id
HAVING
    Count(skills_job_dim.job_id) > 10
ORDER BY
    avg_salary DESC,
    demand_count DESC
LIMIT 10
```
![Optimal skills for data scientist](/asset/table_optimal_skill.png)
Table of the most optimal skills for data analyst sorted by salary

# What I Learned
Over this journey, I've learned a great deal of SQL toolkit:

Crafting Complex Query: You've mastered the art of sophisticated SQL, combining tables with ease and utilising WITH clauses for temp table tricks fit for a ninja.
Data Aggregation: I became familiar with GROUP BY and used aggregate functions such as AVG() and COUNT() to make my sidekicks for data summarization.
Analytical Wizardry: I improved my ability to solve practical puzzles by converting inquiries into intelligent, useful SQL queries.
# Conclusion
**Insights**

From the analysis, several general insights emerged:

**Top-Paying Data Analyst Jobs:** The highest-paying jobs for data scientist that allow remote work offer a wide range of salaries!
Skills for Top-Paying Jobs: High-paying data scientist jobs require advanced proficiency in SQL, suggesting it’s a critical skill for earning a top salary.
Most In-Demand Skills: SQL is also the most demanded skill in the data analyst job market, thus making it essential for job seekers.
**Skills with Higher Salaries:** Specialized skills are associated with the highest average salaries, indicating a premium on niche expertise.
**Optimal Skills for Job Market Value:** SQL leads in demand and offers for a high average salary, positioning it as one of the most optimal skills for data scientist to learn to maximize their market value.
**Closing Thoughts**
This project enhanced my SQL skills and provided valuable insights into the data scientist job market. The findings from the analysis serve as a guide to prioritizing skill development and job search efforts. Aspiring data scientist can better position themselves in a competitive job market by focusing on high-demand, high-salary skills. This exploration highlights the importance of continuous learning and adaptation to emerging trends in the field of data science.
