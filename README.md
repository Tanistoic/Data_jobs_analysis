# 📊 Data Analyst Job Market Analysis – India

## 🧠 Introduction
This project dives deep into the Indian job market to understand the landscape for data analysts. It explores job postings to identify high-paying opportunities, essential skills, and trends that can help aspiring and current data analysts align their career paths strategically.

## 🏗️ Background
In a booming digital economy like India, data analytics is one of the most in-demand fields. Yet, many job seekers remain unsure about which skills to focus on or which companies offer the most lucrative roles. This project aims to decode such uncertainties using real job market data.

## 🛠️ Tools I Used
SQL: The backbone of my analysis, allowing me to query the database and unearth critical insights.

PostgreSQL: The chosen database management system, ideal for handling the job posting data.

Visual Studio Code: My go-to for database management and executing SQL queries.

## 📈 The Analysis
I explored the following key questions through SQL queries:

### 1. Top-paying data analyst jobs in India
→ Identified the highest average salaries and the companies offering them.
```sql
SELECT
    job_id,
    job_title,
    name as company_name,
    job_location,
    job_schedule_type,
    salary_year_avg,
    job_posted_date
FROM
    job_postings_fact
LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
WHERE
    job_title_short = 'Data Analyst' AND
    job_country = 'India' AND
    salary_year_avg IS NOT NULL
ORDER BY
    salary_year_avg DESC
LIMIT 10
```

| job_id  | job_title                                     | company_name       | job_location               | job_schedule_type | salary_year_avg | job_posted_date       |
|---------|-----------------------------------------------|--------------------|----------------------------|--------------------|------------------|------------------------|
| 226942  | Data Analyst                                   | Mantys             | Anywhere                   | Full-time          | $650000.0         | 2023-02-20 15:13:33    |
| 1642893 | Staff Applied Research Engineer                | ServiceNow         | Hyderabad, Telangana, India| Full-time          | $177283.0         | 2023-06-28 18:35:45    |
| 283661  | Technical Data Architect - Healthcare          | Srijan Technologies| Gurugram, Haryana, India   | Full-time          | $165000.0         | 2023-05-10 22:18:20    |
| 954793  | Data Architect 2023                            | Bosch Group        | Bengaluru, Karnataka, India| Full-time          | $165000.0         | 2023-01-12 13:14:51    |
| 1041666 | Data Architect - Data Migration                | Bosch Group        | Bengaluru, Karnataka, India| Full-time          | $165000.0         | 2023-05-06 20:30:35    |
| 781346  | Data Architect                                 | Eagle Genomics Ltd | Hyderabad, Telangana, India| Full-time          | $163782.0         | 2023-07-06 21:12:14    |
| 406320  | Data Architect                                 | Eagle Genomics Ltd | Hyderabad, Telangana, India| Full-time          | $163782.0         | 2023-02-07 11:12:39    |
| 325402  | Senior Business & Data Analyst                 | Deutsche Bank      | India                      | Full-time          | $119250.0         | 2023-11-21 13:11:46    |
| 908967  | Sr. Enterprise Data Analyst                    | ACA Group          | India                      | Full-time          | $118140.0         | 2023-12-21 20:10:10    |
| 309885  | Senior Analyst, Product Revenue (Data Analyst) | Zscaler            | Gurugram, Haryana, India   | Full-time          | $111175.0         | 2023-02-24 14:34:38    |


### 2. Skills required for top-paying jobs
→ Mapped job IDs to required skills to find out what top jobs demand.
```sql
with top_paying_jobs as (
    SELECT
        job_id,
        job_title,
        name as company_name,
        salary_year_avg
    FROM
        job_postings_fact
    LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
    WHERE
        job_title_short = 'Data Analyst' AND
        job_country = 'India' AND
        salary_year_avg IS NOT NULL
    ORDER BY
        salary_year_avg DESC
    LIMIT 10
)

SELECT 
    top_paying_jobs.*,
    skills
FROM top_paying_jobs
INNER JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY
    salary_year_avg DESC;
```

### From the analysis of top-paying roles, these skills stood out:

SQL and Python: Most in-demand core skills.

Power BI, Tableau, Excel: Essential for reporting and dashboards.

AWS, Azure, Snowflake, Spark: High-paying roles favour cloud and big data tools.

MongoDB, PostgreSQL, MySQL: Database versatility is key.

Tools like Airflow, Jenkins, GitLab, and Linux show the value of data engineering & automation knowledge.


### 3. Most in-demand skills
→ Counted skill frequency across all job listings to reveal the top 5.
```sql
SELECT 
    skills,
    COUNT(skills_job_dim.job_id) AS demand_count
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst' 
    AND job_country = 'India' 
GROUP BY
    skills
ORDER BY
    demand_count DESC
LIMIT 5;
```
| Skill     | Demand Count |
|-----------|--------------|
| SQL       | 3167         |
| Python    | 2207         |
| Excel     | 2118         |
| Tableau   | 1673         |
| Power BI  | 1285         |

### 4. Skills associated with higher salaries
→ Calculated average salary per skill to determine which ones offer the best ROI.
```sql
SELECT 
    skills,
    ROUND(AVG(salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst' 
    AND job_country = 'India'
    AND salary_year_avg IS NOT NULL
GROUP BY
    skills
ORDER BY
    avg_salary DESC
LIMIT 15;
```

| Skill       | Avg Salary (USD) |
|-------------|------------------|
| PostgreSQL  | $165000           |
| MySQL       | $165000           |
| Linux       | $165000           |
| PySpark     | $165000           |
| GitLab      | $165000           |
| GDPR        | $163782           |
| Neo4j       | $163782           |
| Airflow     | $138088           |
| MongoDB     | $135994           |
| Databricks  | $135994           |
| Scala       | $135994           |
| Pandas      | $122463           |
| Kafka       | $122100           |
| Confluence  | $119250           |
| Visio       | $119250           |

### 5. Most optimal skills to learn
→ Combined demand and salary data to identify skills that are both high in demand and pay well.
```sql
WITH average_salary AS (
    SELECT 
        skills_job_dim.skill_id,
        ROUND(AVG(salary_year_avg), 0) AS avg_salary
    FROM job_postings_fact
    INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
    INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
    WHERE
        job_title_short = 'Data Analyst' 
        AND job_country = 'India'
        AND salary_year_avg IS NOT NULL
    GROUP BY
        skills_job_dim.skill_id
), skills_demand AS (
    SELECT 
        skills_dim.skill_id,
        skills_dim.skills,
        COUNT(skills_job_dim.job_id) AS demand_count
    FROM job_postings_fact
    INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
    INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
    WHERE
        job_title_short = 'Data Analyst' 
        AND job_country = 'India'
        AND salary_year_avg IS NOT NULL 
    GROUP BY
        skills_dim.skill_id
)

SELECT
    skills_demand.skill_id,
    skills_demand.skills,
    demand_count,
    avg_salary
FROM
    skills_demand
INNER JOIN  average_salary ON skills_demand.skill_id = average_salary.skill_id
ORDER BY demand_count DESC,
avg_salary DESC
LIMIT 15
```
| Skill       | Demand Count | Avg Salary (USD) |
|-------------|---------------|------------------|
| SQL         | 46            | $92,984          |
| Excel       | 39            | $88,519          |
| Python      | 36            | $95,933          |
| Tableau     | 20            | $95,103          |
| R           | 18            | $86,609          |
| Power BI    | 17            | $109,832         |
| Azure       | 15            | $98,570          |
| AWS         | 12            | $95,333          |
| Spark       | 11            | $118,332         |
| Oracle      | 11            | $104,260         |
| PowerPoint  | 10            | $102,678         |
| Looker      | 10            | $98,815          |
| Word        | 10            | $83,266          |
| Flow        | 6             | $104,751         |
| Hadoop      | 5             | $113,276         |

## 📚 What I Learned
Throughout this adventure, I've turbocharged my SQL toolkit with some serious firepower:

🧩 Complex Query Crafting: Mastered the art of advanced SQL, merging tables like a pro and wielding WITH clauses for ninja-level temp table maneuvers.

📊 Data Aggregation: Got cozy with GROUP BY and turned aggregate functions like COUNT() and AVG() into my data-summarizing sidekicks.

💡 Analytical Wizardry: Leveled up my real-world puzzle-solving skills, turning questions into actionable, insightful SQL queries.

## ✅ Conclusion
### Insights

### 1. Top-Paying Data Analyst Jobs
The highest-paying roles in India labeled under Data Analyst or related titles offer salaries above $160,000 USD per year. These include:

Data Architect - Data Migration at Bosch Group – $165,000

Technical Data Architect - Healthcare at Srijan Technologies – $165,000

Data Architect 2023 at Bosch Group – $165,000

Data Architect at Eagle Genomics Ltd – $163,782

These roles typically fall under full-time, remote/hybrid setups in cities like Bangalore, Hyderabad, and Gurgaon.

### 2. Skills for Top-Paying Jobs
Top-paying positions consistently required the following high-value technical skills:

SQL, Python, Spark, Oracle, Power BI

Cloud platforms: AWS, Azure

Big data & pipeline tools: Databricks, Airflow, Kafka

Data warehousing: Snowflake, Redshift

Version control & automation: GitLab, Jenkins, Shell scripting

Supporting tools: Excel, PowerPoint, Unix/Linux

These roles prioritize strong backend data architecture, cloud integration, and big data processing capabilities.

### 3. Most In-Demand Skills
Based on the number of job postings, these are the top 5 most in-demand skills for data analyst roles in India:

| Skill     | Demand Count |
|-----------|--------------|
| SQL       | 3167         |
| Python    | 2207         |
| Excel     | 2118         |
| Tableau   | 1673         |
| Power BI  | 1285         |

These reflect widespread demand for core data analysis, reporting, and dashboarding skills.

### 4. Skills with Higher Salaries
Certain skills are strongly associated with above-average salaries in data roles (all salaries in USD):

| Skill       | Avg Salary (USD) |
|-------------|------------------|
| Spark       | $118,332         |
| Hadoop      | $113,276         |
| Power BI    | $109,832         |
| Flow        | $104,751         |
| Oracle      | $104,260         |
| PowerPoint  | $102,678         |
| Looker      | $98,815          |

These skills are often tied to data engineering, BI tools, and enterprise-level architecture, leading to premium pay.

### 5. Optimal Skills for Job Market Value
To balance high demand and strong salary potential, the most strategic skills to learn are:

SQL – Highest demand + solid salary

Python – High demand + strong versatility

Power BI – High salary + growing demand

Azure & AWS – Cloud skills = futureproofing

Spark & Hadoop – Big data tools with excellent pay

Looker & Tableau – High salary + widely adopted

These skills not only open more job opportunities but also help you target higher-paying roles with career growth potential.

### Closing Thoughts
This project enhanced my SQL skills and provided valuable insights into the data analyst job market. The findings from the analysis serve as a guide to prioritizing skill development and job search efforts. Aspiring data analysts can better position themselves in a competitive job market by focusing on high-demand, high-salary skills. This exploration highlights the importance of continuous learning and adaptation to emerging trends in the field of data analytics.
