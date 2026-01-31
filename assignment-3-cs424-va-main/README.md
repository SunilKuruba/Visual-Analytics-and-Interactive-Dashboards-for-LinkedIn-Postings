# Visual Analytics of LinkedIn Job Postings

<img width="795" height="400" alt="image" src="https://github.com/user-attachments/assets/31f64ee5-b04e-484d-9ccc-e292f2d1dc37" />

## (1) Group Information  

| Name         | UIN       | Email             | Contribution |
|--------------|-----------|-------------------|--------------|
| Sunil Kuruba | 659375633 | skuru@uic.edu     | Q1, Q2, Q3, Q4 |
| Richa Rameshkrishna   | 651622805    | rrame11@uic.edu   | Q5, Q6, Q7 |

---

## 1: Data description

**Dataset**: [LinkedIn Job Postings](https://www.kaggle.com/datasets/arshkon/linkedin-job-postings/data)  

The LinkedIn Job Postings dataset provides a comprehensive view of more than 124,000 job postings spanning 2023 and 2024, enriched with detailed company, skill, industry, salary, and benefits information. Each posting includes attributes such as job title, description, posting time, work type (remote, hybrid, full-time, contract), and application links. Complementary tables add depth by linking postings to company data (including company size, employee and follower counts, and headquarters location), skill requirements, and industry categories. This relational structure makes the dataset multi-dimensional, enabling analyses not only of job postings themselves but also of the broader ecosystem of employers, industries, and demanded skills. Its temporal coverage across two consecutive years further allows the study of hiring trends and seasonality.

We found this dataset particularly relevant because it bridges multiple areas of interest—labor market analysis, skill demand, geographic hiring trends, and compensation insights—all of which are highly topical in the current job market. Unlike a simple postings feed, the dataset allows exploration across layers: comparing startup vs. enterprise hiring activity, mapping geographic concentrations of roles, analyzing remote work adoption, or examining which skills command higher salaries. The richness of text fields such as job descriptions and skill tags also enables more advanced processing, from keyword extractions to clustering and demand pattern visualization. This combination of scale, depth, and timeliness makes it a strong foundation for building meaningful visualizations that can help students, professionals, and policymakers understand how opportunities and requirements in the job market are shifting.

### Schema

Each record corresponds to one job listing, including attributes such as:
- **Job details:** `title`, `description`, `work_type`, `company_name`
- **Compensation:** `min_salary`, `med_salary`, `max_salary`, `pay_period`
- **Experience level:** `formatted_experience_level`
- **Location:** `state`, `zip_code`
- **Engagement metrics:** `views`, `applies`
- **Posting details:** `listed_time`, `remote_allowed`, `application_type`

## Data cleaning and preprocessing

This script loads, profiles, and cleans the dataset in a structured and reproducible way. It first reads multiple CSV files (companies, jobs, mappings, etc.) from the project’s data/ folder and prints quick summaries of each table. A profiling step then lists column types, missing values, and unique counts for an overview of the data quality. The cleaning module performs key preprocessing tasks such as extracting U.S. state abbreviations from company headquarters, mapping them to FIPS codes, converting salary ranges into numeric midpoints, normalizing seniority levels, parsing skill lists, and removing duplicates. After cleaning and normalizing, salary values were converted into annual equivalents to ensure consistency across different pay periods (hourly, weekly, monthly, yearly). The cleaned dataset is saved as data_science_job_posts_2025_clean.csv and is ready for use in analysis and visualizations for the assignment. 

---

## 2. Exploratory Questions and Visualizations

### **Question 1: How do job postings vary month-to-month, and what seasonal hiring patterns can be observed?**

**Description:**

This line chart tracks the **total number of job postings per month** to reveal hiring fluctuations throughout the year.

* **X-axis:** Month of the year
* **Y-axis:** Number of job postings

**Key Findings:**

* Hiring activity spikes noticeably in **March** and again in **September**, likely aligning with new budget cycles or the start of academic terms when companies plan new hires.
* Lows in **February**, **August**, and **November** suggest slower recruitment periods — possibly due to holidays, project transitions, or year-end closures.
* Overall, the pattern shows how recruitment ebbs and flows rather than staying steady all year.

**Sketch to Visualization:**

I initially imagined this as a **simple time-series line chart** that could show peaks and troughs clearly. The final version was built in **Altair** using `.mark_line(point=True)` so that each data point could be individually visible. I also added clean gridlines and adjusted axis labels for better readability. The goal was to make the chart feel like a quick glance summary of hiring rhythms — something that immediately tells you when most job opportunities appear.

![skill-demand](visualizations/1-monthly-trend.svg)

---

### **Question 2: How does skill demand differ across industries and role seniority levels?**

**Description:**

A multi-line chart comparing how **skill requirements scale with seniority** across five major industries.

* **X-axis:** Role level (Junior → Senior)
* **Y-axis:** Count of skills required in job postings
* **Color:** Industry type

**Key Findings:**

* The **Technology** and **Retail** industries show sharp jumps at senior levels, meaning higher positions demand a broader or deeper skill set.
* **Finance** and **Healthcare** show more gradual growth, implying consistent expectations across roles.
* **Education** remains relatively flat, likely reflecting more standardized skill requirements.

**Sketch to Visualization:** 

Originally, I visualized this as **layered colored lines** connecting different industries across the same role axis. In Altair, I used `.mark_line(point=True)` to show the trend and individual data points. I experimented with multiple color palettes before choosing distinct hues for each industry to avoid confusion. The line chart format was ideal for showing “growth” or “change” in skill expectations as one moves from junior to senior roles. This visual directly communicates how technical intensity scales with experience in different sectors.


![skill-demand-industry-role](visualizations/2-skill-demand-industry-role.svg)
---

### **Question 3: What are the top 10 most in-demand technical skills across job postings?**

**Description:**

A vertical bar chart ranking the **most frequently mentioned skills** in job descriptions.

* **X-axis:** Technical skill name
* **Y-axis:** Number of job postings mentioning that skill

**Key Findings:**

* **Python** dominates the market, followed closely by **Machine Learning** and **SQL**, confirming their role as core data tools.
* **R**, **AWS**, and **Deep Learning** represent the next tier, often appearing in specialized or advanced positions.
* While **Spark** and **Azure** are less frequent, their presence highlights the growing importance of big-data and cloud ecosystems.

**Sketch to Visualization:** 

I sketched this as a **ranking bar chart** to make it easy to see which skills stand out. The visualization was implemented in Altair using `.mark_bar()`, sorting the bars by descending frequency. I also rotated the x-axis labels slightly for better readability. The design choice was to make the top skills instantly obvious and to provide a visual hierarchy — Python towers above, showing its unmatched popularity in data science roles.

![skill-demand](visualizations/3-1-skill-demand.svg)

**Enchanced Visualization:**

Building on the static version, I developed an interactive ranking chart to improve user engagement and exploration. The updated chart allows viewers to dynamically inspect specific skills and their counts via tooltips, filter top N skills, and explore relationships more intuitively. By switching to a horizontal layout, the enhanced version optimized readability for long skill names and created a cleaner, more professional dashboard-like presentation. This evolution demonstrates the shift from a simple, static insight to an interactive, analytical experience.

![skill-demand](visualizations/3-2-skill-demand.gif)

---

### **Question 4: How do median salaries for data-related roles vary geographically across U.S. states?**

**Description:**

A choropleth (color-coded map) displaying **median salary levels** for data jobs across U.S. states.

* **Color gradient:** Salary range (light blue = lower pay, dark blue = higher pay)
* **Geometry:** U.S. state boundaries

**Key Findings:**

* **Western** (CA, WA) and **Northeastern** (MA, NY) states have the highest salaries, reflecting their tech-focused economies and higher living costs.
* **Central** and **Southern** states tend to offer lower median pay, though cost of living adjustments may balance real income.
* The pattern highlights strong regional inequality — proximity to tech hubs still correlates with better compensation.

**Sketch to Visualization:**

Initially drawn as a **thematic U.S. map** where states would be shaded based on salary. In Altair, I used `.mark_geoshape()` combined with a TopoJSON map of U.S. states and joined it with the processed median salary data. I adjusted the color scale (`scale=alt.Scale(scheme='blues')`) to create a smooth gradient and added a legend for interpretability. The visualization feels intuitive — darker states immediately communicate higher pay zones. It connects spatial reasoning with compensation patterns, making the data story geographic and visual.

![salary-median](visualizations/4-1-salary-median.svg)

**Enchanced Visualization:**

In the enhanced version, I transformed the static map into an interactive dashboard-style visualization. Users can now toggle between color encodings (median salary or job postings) and filter states by a dynamic slider for minimum postings. This interactivity allows deeper exploration—revealing not just where salaries are highest, but also where job demand is concentrated. The enhancement turns a static insight into an exploratory tool, blending clarity with engagement for a more complete geographic analysis.

![salary-median](visualizations/4-2-salary-median.gif)

---

### **Question 5: How does the availability of high-paying, remote jobs spread geographically over time? Does it start in tech hubs  and then "wave out" to other metropolitan areas? This investigates the diffusion of modern work culture.**
**Visualization:**  
![bubble_remote_distribution](visualizations/Bubble.gif)

**Description:**  
Each bubble represents a state.  
- **Bubble size:** total job postings  
- **Color (reds):** percentage of remote jobs  

**Key Findings:**  
- States with the highest total postings (CA, TX, NY) also offer the largest number of remote opportunities.  
- Some smaller states (e.g., remote-heavy tech hubs) show high remote percentages despite fewer total postings.  

**Sketch to Visualization:**  
Originally sketched as a “proxy map” bubble chart in Matplotlib; implemented in Vega-Altair using `.mark_circle()`.  
Bubbles are centered horizontally and scaled proportionally to posting volume, with color encoding remote work ratio.

---


### **Question 6: Is there a relationship between a company's popularity and its willingness to disclose salary information in postings? Do well-known companies rely on their brand to attract applicants without being transparent?**
**Visualization:**  
![heatmap_state_experience](visualizations/Heat.gif)

**Description:**  
This heatmap visualizes the number of job postings across U.S. states (x-axis) and experience levels (y-axis).  
The color intensity encodes posting volume.

**Key Findings:**  
- California, Texas, and New York dominate job postings across all experience levels.  
- Entry- and Mid-Senior level positions are most common overall.  
- Director- and Executive-level postings are concentrated in large states with strong corporate presence.

**Sketch to Visualization:**  
Initially sketched as a grid showing density of postings per region and level.  
Implemented in Vega-Altair using `.mark_rect()` with counts and labeled axes for readability.

---

### **Question 7: For the same job title, what is the salary premium for requiring a higher experience level? And does this premium vary significantly across different industries?**
**Visualization:**  
![box_salary_experience](visualizations/Box.gif)

**Description:**  
A box plot showing the distribution of annualized median salaries for each experience level.

**Key Findings:**  
- Clear upward trend in salary with experience level.  
- Entry-level roles cluster around \$60–80 k, while senior roles exceed \$130 k.  
- A few high outliers suggest executive or specialized technical positions.

**Sketch to Visualization:**  
The sketch used a Seaborn boxplot; converted to Vega-Altair with `.mark_boxplot()`, trimmed to the 95th percentile for interpretability and scaled consistently using annualized salaries.

---

## Learning Summary

Through this project, I learned how to transform raw job posting data into meaningful visual narratives using Altair and Vega-Lite. I practiced key data visualization principles—choosing the right encoding, balancing aesthetics with readability, and building interactive dashboards that allow deeper exploration of job market trends. Beyond coding, I gained hands-on experience in data storytelling, learning how to use visuals to reveal insights about skill demand, remote work diffusion, and salary disparities across the U.S.

---

## Insights Summary
- **Skill Demand:** Python and Machine Learning continue to dominate job postings, followed by SQL and AWS—highlighting the cross-disciplinary nature of data science roles.
- **Geographic trends:** The majority of postings are concentrated in a few high-population states. States like California, New York, and Washington consistently offer higher median salaries, reinforcing the concentration of high-paying tech roles in major hubs.
- **Experience and pay:** Salary scales increase steadily with experience level, confirming standard market trends.
- **Remote work:** Remote opportunities are widespread but correlate with total posting volume.
- **Temporal dynamics:** Posting activity suggests cyclical trends possibly tied to hiring seasons or economic cycles.

