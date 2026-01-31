# Visual Analytics of LinkedIn Job Postings

<img width="795" height="400" alt="image" src="https://github.com/user-attachments/assets/077ad007-75a7-4be1-8563-da23ba6e1783" />

---

## (1) Group Information  

| Name         | UIN       | Email             | Contribution |
|--------------|-----------|-------------------|--------------|
| Sunil Kuruba | 659375633 | skuru@uic.edu     | Q1, Q2, Q3 |
| Richa Rameshkrishna   | 651622805    | rrame11@uic.edu   | Q4, Q5, Q6 |

---

## Task 1: Data description

**Dataset**: [LinkedIn Job Postings](https://www.kaggle.com/datasets/arshkon/linkedin-job-postings/data)  

The LinkedIn Job Postings dataset provides a comprehensive view of more than 124,000 job postings spanning 2023 and 2024, enriched with detailed company, skill, industry, salary, and benefits information. Each posting includes attributes such as job title, description, posting time, work type (remote, hybrid, full-time, contract), and application links. Complementary tables add depth by linking postings to company data (including company size, employee and follower counts, and headquarters location), skill requirements, and industry categories. This relational structure makes the dataset multi-dimensional, enabling analyses not only of job postings themselves but also of the broader ecosystem of employers, industries, and demanded skills. Its temporal coverage across two consecutive years further allows the study of hiring trends and seasonality.

We found this dataset particularly relevant because it bridges multiple areas of interest—labor market analysis, skill demand, geographic hiring trends, and compensation insights—all of which are highly topical in the current job market. Unlike a simple postings feed, the dataset allows exploration across layers: comparing startup vs. enterprise hiring activity, mapping geographic concentrations of roles, analyzing remote work adoption, or examining which skills command higher salaries. The richness of text fields such as job descriptions and skill tags also enables more advanced processing, from keyword extractions to clustering and demand pattern visualization. This combination of scale, depth, and timeliness makes it a strong foundation for building meaningful visualizations that can help students, professionals, and policymakers understand how opportunities and requirements in the job market are shifting.

## Task 2: Domain questions

---

### Question 1:
**How have job postings trended over time (2023–2024)? Which industries are generating the most job postings?**

A key question we want to investigate is how job postings have trended over time in 2023 and 2024, since the dataset includes original_listed_time for each posting. By aggregating postings across months, we can explore whether hiring activity exhibits seasonal peaks, steady growth, or unexpected dips, revealing important temporal hiring patterns. Another dimension of this analysis is identifying which industries are generating the most postings, using the job_industries.csv and mappings/industries.csv tables. By comparing industry-level volumes, we can determine which sectors are expanding, stable, or contracting, and highlight how hiring demand shifts across domains such as technology, finance, or healthcare. Together, these questions emphasize time-based and categorical trends, helping us build a foundation for understanding the broader structure of the job market.

---

### Question 2:
**What skills are most in demand, and which skills are frequently listed together?**

Another important area of inquiry is skill demand. Using the job_skills.csv and mappings/skills.csv tables, we can measure which technical and soft skills are most frequently requested in postings, and further break them down by industry or seniority level (formatted_experience_level). This will help reveal whether certain industries prioritize specialized technical stacks while others emphasize general professional skills. In addition, we are interested in analyzing which skills are most frequently listed together in job postings, since co-occurrence patterns reveal natural bundles of skills—such as Python, SQL, and AWS—that employers value as complementary. Visualizing these relationships allows us to uncover clusters of in-demand skills that go beyond individual counts, offering practical insights for professionals who need to prioritize not just single skills, but sets of capabilities that align with employer expectations.

---

### Question 3:
**How do salaries differ across industries, company size, and company state?**

We also want to examine how salaries differ across industries, company size, and company location. The dataset provides salary details (jobs/salaries.csv with min, median, and max salary fields) as well as company attributes such as employee_counts.csv and geographic details in companies.csv. By linking salaries to these attributes, we can compare compensation trends across industries to see which sectors consistently offer higher pay, while also analyzing whether larger companies tend to provide more competitive salaries than smaller firms. Incorporating company location, such as state or country, further allows us to assess how salaries scale across regions with different cost-of-living levels. This analysis will reveal disparities and trends in compensation, providing insight into how factors such as industry domain, organizational scale, and geography shape pay expectations in the current job market.

---

### Question 4
**How does the availability of high-paying, remote jobs spread geographically over time? Does it start in tech hubs and then "wave out" to other metropolitan areas? This investigates the diffusion of modern work culture.**

We want to investigate how high-paying, remote job opportunities spread across different cities and states over time. The goal is to see whether these opportunities first appear in major tech hubs like San Francisco or New York and gradually expand to secondary metropolitan areas, reflecting the diffusion of modern remote work culture. Key attributes include location, remote_allowed, normalized_salary, and listed_time. This analysis could reveal patterns in the adoption of remote work and highlight geographic inequalities in access to high-paying remote jobs.

---

### Question 5 
**Is there a relationship between a company's popularity and its willingness to disclose salary information in postings? Do well-known companies rely on their brand to attract applicants without being transparent?**

This question explores whether a company’s popularity affects its willingness to disclose salary information. The hypothesis is that well-known companies may rely on their brand reputation to attract applicants without providing salary transparency. Important attributes include company_id, salary_disclosed (or a derived boolean), and followers_count or other popularity indicators. Understanding this trade-off can shed light on corporate strategies and candidate behavior when evaluating jobs.

---

### Question 6 
**For the same job title, what is the salary premium for requiring a higher experience level? And does this premium vary significantly across different industries?**

We want to examine how salary increases with experience level for the same job title and how this premium varies across industries. For example, an entry-level “Data Analyst” may earn significantly less than a director-level analyst, and this gap might differ in Tech versus Finance. Relevant attributes include title, experience_level, min_salary, max_salary, and industry. Investigating this helps uncover whether compensation growth is consistent and fair across industries and experience levels.

---

## Task 3: Task abstractions

### Question 1: How have job postings trended over time in 2023 and 2024? Which industries are generating the most postings?

**Temporal Trends in Job Postings**  
- **Task Type:** Trend Analysis  
- **Action:** Summarize and analyze the number of job postings by aggregating them into months and years  
- **Target:** Identify temporal patterns in hiring activity, such as seasonal peaks, dips, or year-over-year changes, helping users understand the rhythms of the job market and when opportunities are most abundant.  

**Industry Trends Volume Over Time**  
- **Task Type:** Correlation Analysis  
- **Action:** Explore the relationship between time and industry by plotting posting trends for individual industries over months and years.  
- **Target:** Discover which industries are growing, stable, or declining in job postings, helping to connect temporal hiring cycles with sector-specific labor demand.  

---

### Question 2: What skills are most in demand, and which skills are frequently listed together?

**Skill Demand Ranking**  
- **Task Type:** Distribution Analysis  
- **Action:** Identify the most frequently requested skills in job postings  
- **Target:** Highlight the most popular individual skills across all postings. This helps professionals and students prioritize which skills are most valuable in the market.  

**Skill Demand by Industry and Role**  
- **Task Type:** Comparison Analysis  
- **Action:** Compare how skill requirements vary across industries and experience levels  
- **Target:** Understand differences in skill demand between sectors (e.g., technology vs. healthcare) and roles (e.g., entry-level vs. senior), providing actionable insights into industry-specific expectations.  

**Skill Co-occurrence Patterns**  
- **Task Type:** Relationship Discovery  
- **Action:** Analyze skill pairs that appear together within the same job postings to uncover frequent bundles of skills.  
- **Target:** Identify complementary skill sets (e.g., Python + SQL + AWS for Data Engineer) that employers value together, guiding learners to focus on not just single skills but clusters of related competencies.  

---

### Question 3: How do salaries differ across industries, company size, and company state?

**Salary Ranking by Category**  
- **Task Type:** Ranking Analysis  
- **Action:** Rank industries and regions by median salaries, aggregating values and ordering them from highest to lowest.  
- **Target:** Reveal which industries and states consistently offer higher or lower salaries, supporting clearer comparisons for job seekers evaluating opportunities.  

**State-wise Comparison of Median Salary of Job Postings**  
- **Task Type:** Ranking Analysis
- **Action:** Aggregate job postings by U.S. state, compute median salaries for each state
- **Target:** Reveal geographic patterns in compensation, identifying states that consistently offer higher or lower pay, helping job seekers compare opportunities regionally.

---

### Question 4: How does the availability of high-paying, remote jobs spread geographically over time? Does it start in tech hubs  and then "wave out" to other metropolitan areas? This investigates the diffusion of modern work culture.

**Remote vs. Total Jobs by State**
- **Task Type:** Comparative Aggregation  
- **Action:** Summarize job postings by counting both total and remote jobs per state, then visualize them using side-by-side bar charts.  
- **Target:** Highlight which states contribute most to the overall job market and how remote opportunities align with state-level hiring volumes.  

**Remote Work Share with Market Size Context**
- **Task Type:** Proportion Analysis  
- **Action:** Calculate the percentage of remote postings in each state and encode results with bubble size (job market volume) and bubble color (remote share).  
- **Target:** Reveal where remote work is more prevalent relative to state size, allowing identification of states with disproportionately high remote adoption.

**Compositional Balance of Remote vs. On-Site Jobs**
- **Task Type:** Stacked Composition Analysis  
- **Action:** Express each state’s job market as a 100% stacked bar showing proportions of remote and on-site jobs.  
- **Target:** Compare the internal makeup of state job markets, clarifying which states are balanced versus heavily skewed toward one work style.  

---

### Question 5: Is there a relationship between a company's popularity and its willingness to disclose salary information in postings? Do well-known companies rely on their brand to attract applicants without being transparent?

**Salary Disclosure vs. Non-Disclosure Over Time**
- **Task Type:** Temporal Composition Analysis  
- **Action:** Aggregate job postings by month and plot disclosed vs. not disclosed salaries using a stacked area chart.  
- **Target:** Show the overall growth or decline of salary transparency over time while capturing both categories simultaneously.  

**Transparency Percentage Trend**
- **Task Type:** Temporal Trend Analysis  
- **Action:** Compute the percentage of postings with disclosed salaries each month and visualize the trend with a line chart.  
- **Target:** Highlight fluctuations and long-term trends in salary transparency adoption over time.  

**Seasonal and Yearly Transparency Patterns**
- **Task Type:** Heatmap Visualization of Temporal Patterns  
- **Action:** Pivot transparency percentages into a Year × Month grid and render as a heatmap.  
- **Target:** Detect recurring monthly or seasonal patterns in salary transparency and compare changes across different years.
  
---

### Question 6: For the same job title, what is the salary premium for requiring a higher experience level? And does this premium vary significantly across different industries?

**Average Min/Max Salary by Experience Level**
- **Task Type:** Comparative Aggregation  
- **Action:** Compute the mean of minimum and maximum salaries for each experience level and display results as grouped bar charts.  
- **Target:** Compare baseline and ceiling salaries across experience groups, highlighting structural pay differences.  

**Salary Distribution Spread by Experience Level**
- **Task Type:** Distribution Analysis  
- **Action:** Use box plots of median salary per experience level to capture spread, central tendency, and outliers.  
- **Target:** Reveal salary variability and detect skewness or anomalies within each experience group.  

**Salary Ranges as Progression Across Experience**
- **Task Type:** Range & Progression Visualization  
- **Action:** Plot average min-to-max salary ranges for each experience level using a slope graph.  
- **Target:** Illustrate the widening or narrowing of salary bands with increasing experience, showing how opportunity scales.

---

## Task 4: Visualization Sketches

## Question 1: How have job postings trended over time in 2023 and 2024? Which industries are generating the most postings?

### Sketch 1: Temporal Trends in Job Postings (Line Chart)

<img src="sketch/temporal-trends-in-job-postings.png" width="600" />

- **Main idea:** Track how total job postings change month by month, highlighting peaks and troughs in 2023 and 2024.  
- **Motivation:** A time-series view makes it easier to detect hiring cycles, sudden dips, or sustained growth over multiple months.  
- **What worked well:** Clear line progression reveals both seasonal variations and long-term upward growth patterns.  
- **What didn’t work so well:** The chart shows only aggregate postings, without breaking down by industry, so the causes of changes are not visible.  
- **Difference:** Unlike categorical comparisons, this emphasizes timing and temporal rhythm, helping us understand when hiring demand is strongest.  

### Sketch 2: Industry Trends Over Time (Multi-Line Chart)

<img src="sketch/temporal-trends-in-industry-postings.png" width="600" />

- **Main idea:** Show how job posting volumes vary across industries over time, revealing which sectors are expanding or contracting.  
- **Motivation:** Different industries follow different cycles (e.g., tech growth vs. healthcare decline), and a comparative chart makes this visible.  
- **What worked well:** Overlapping lines highlight diverging trends, making it clear which industries are driving overall market shifts.  
- **What didn’t work so well:** With multiple lines, the visualization may get cluttered, making it harder to isolate specific sectors without filtering.  
- **Difference:** Unlike the aggregate chart, this focuses on sector-level differentiation, explaining *which industries* fuel or dampen hiring trends.  

---

## Question 2: What skills are most in demand, and which skills are frequently listed together?

### Sketch 3: Skill Demand Ranking (Bar Chart)

<img src="sketch/skill-demand-ranking.png" width="600" />

- **Main idea:** Display the most requested individual skills by frequency across job postings.  
- **Motivation:** Ranking skills shows what employers value most, providing direct guidance for workforce preparation.  
- **What worked well:** Easy-to-read bar heights quickly reveal top skills like Python and SQL.  
- **What didn’t work so well:** The chart doesn’t reveal how skills interact or whether demand varies by industry or level.  
- **Difference:** Unlike relationship-based visuals, this isolates *individual skill popularity*.  

### Sketch 4: Skill Demand by Industry and Role (Multi-Line Chart)

<img src="sketch/skill-demand-by-industry.png" width="600" />

- **Main idea:** Compare how skill demand differs across industries and experience levels (entry, mid, senior).  
- **Motivation:** Employers may expect different skill sets for different contexts, and this visualization highlights those differences.  
- **What worked well:** It shows nuanced variations, such as SQL being steady in finance but Python rising in technology.  
- **What didn’t work so well:** With many overlapping lines, it can look noisy and require careful reading.  
- **Difference:** Unlike aggregate rankings, this emphasizes *contextual skill demand* based on industry and role.  

### Sketch 5: Skill Co-occurrence Network (Graph)

<img src="sketch/skill-cooccurance-network.png" width="600" />

- **Main idea:** Highlight which skills appear together in job postings, forming natural clusters of competencies.  
- **Motivation:** Many jobs require bundles of skills, and understanding these combinations is more valuable than looking at single skills.  
- **What worked well:** The network graph clearly shows strong ties (e.g., Python, SQL, and AWS forming a cluster).  
- **What didn’t work so well:** Exact frequencies of co-occurrences are harder to judge compared to bar charts.  
- **Difference:** Unlike frequency or ranking views, this focuses on *relationships between skills*, showing employers’ preferred combinations.  

---

## Question 3: How do salaries differ across industries, company size, and company state?

### Sketch 6: Salary Distributions by Industry (Box Plot) 

<img src="sketch/salary-distribution-by-industry.png" width="600" />

- **Main idea:** Compare the distribution of salaries across different industries to highlight pay differences.  
- **Motivation:** Box plots reveal not only median salaries but also variability, spread, and outliers, making them powerful for understanding disparities within and between industries.  
- **What worked well:** The visualization shows clear distinctions in pay levels—Technology has the highest median and spread, while Retail shows lower medians and more variability at the bottom.  
- **What didn’t work so well:** Box plots can be harder for general audiences to interpret compared to simpler bar charts, as they require familiarity with medians, quartiles, and outliers.  
- **Difference:** Unlike bar charts or rankings, this focuses on *salary distributions* within each industry, showing both central values and the range of compensation.  

### Sketch 7: US State Median Salaries (Choropleth Map)

<img src="sketch/us-states-median-salary-map.png" width="600" />

- **Main idea:** Display median salaries geographically across US states to highlight regional differences in compensation.  
- **Motivation:** A choropleth map makes it intuitive to see geographic pay disparities, showing how salaries cluster in tech hubs (CA, WA, NY) versus lower-paying states.  
- **What worked well:** The map provides a clear spatial pattern, emphasizing high-salary regions like the West Coast and Northeast. It also makes it easy to identify outliers such as DC.  
- **What didn’t work so well:** Choropleths emphasize area, so large states (e.g., Texas, California) visually dominate regardless of population or job density. Smaller states can be harder to see without zooming.  
- **Difference:** Unlike salary comparisons by industry or company size, this focuses on *regional variation*, enabling spatial analysis of compensation trends across the country.  

## Question 4: How does the availability of high-paying, remote jobs spread geographically over time? Does it start in tech hubs  and then "wave out" to other metropolitan areas? This investigates the diffusion of modern work culture.

### Sketch 8: Side-by-Side Bar Chart

 <img src="sketch/Q1a.png" width="600" />
 
- **Main idea:** Compare the number of total job postings vs. remote postings by state.  
- **Motivation:** Bar charts provide a straightforward comparison across categories, letting us see if remote opportunities scale with overall job demand.  
- **What worked well:** It clearly shows which states have more remote jobs in absolute terms.  
- **What didn’t work so well:** The visualization favors high-volume states (like CA, NY) while underrepresenting smaller states, making proportions harder to see.  
- **Difference:** Unlike proportional methods, this emphasizes absolute job volume rather than ratios.

### Sketch 9: Bubble Plot Proxy Map

<img src="sketch/Q1b.png" width="600" /> 

- **Main idea:** Encode total jobs as bubble size and remote percentage as bubble color, laid out as a proxy map.  
- **Motivation:** This introduces spatial thinking, mimicking how remote adoption might spread across the U.S.  
- **What worked well:** Dual encoding of size and color highlights states with both large markets and high remote percentages.  
- **What didn’t work so well:** The linear layout loses true geography, making it less intuitive than a real map.  
- **Difference:** Compared to the bar chart, this balances both absolute and relative perspectives in one view.

---

## Question 5: Is there a relationship between a company's popularity and its willingness to disclose salary information in postings? Do well-known companies rely on their brand to attract applicants without being transparent?

### Sketch 10: Line Chart of Salary Transparency Over Time

<img src="sketch/Q2a.png" width="600" /> 

- **Main idea:** Show monthly percentage of jobs with disclosed salaries over time.  
- **Motivation:** Line charts are excellent for highlighting temporal trends, allowing us to detect increases or decreases in transparency.  
- **What worked well:** Trends and fluctuations across months are visible, with peaks and troughs easy to spot.  
- **What didn’t work so well:** Outliers or single-month spikes may distract from the longer-term pattern.  
- **Difference:** Focuses purely on temporal evolution, unlike heatmaps which emphasize seasonality.

### Sketch 11: Heatmap of Salary Transparency by Year/Month

<img src="sketch/Q2b.png" width="600" />

- **Main idea:** Grid-based heatmap showing percentage transparency by month across years.  
- **Motivation:** Captures both seasonality (within year) and year-over-year changes.  
- **What worked well:** Easy to detect recurring seasonal dips (e.g., summer, end of year).  
- **What didn’t work so well:** Numbers inside cells risk clutter and may overwhelm the visual gradient.  
- **Difference:** Unlike the line chart, this emphasizes patterns across months, useful for periodicity detection.

---

## Question 6: For the same job title, what is the salary premium for requiring a higher experience level? And does this premium vary significantly across different industries?

### Sketch 12: Salary Distribution Line Plot by Experience Level

 <img src="sketch/Q3a.png" width="600" /> 
 
- **Main idea:** Display salary range (from min to max) using a line plot for each experience level. Each line represents the salary span for Entry, Mid, Senior, and Director roles.  
- **Motivation:** This sketch provides a **clean and direct** view of the min-to-max salary progression per experience level. It’s easy to compare ranges and see how different roles scale up in compensation.  
- **What worked well:** Very readable; immediately shows which experience levels have higher or wider salary ranges.  
- **What didn’t work so well:** Does not show the **distribution or concentration** of salaries—e.g., medians and quartiles are missing.  
- **Difference:** Compared to the box plot (Sketch 13), this line chart is simpler and more intuitive but lacks depth in statistical insight. It highlights **range**, while the box plot highlights **distribution and outliers**.

### Sketch 13:  Salary Distribution Box Plot by Experience Level

<img src="sketch/Q3b.png" width="600" />

- **Main idea:** Visualize the distribution of median salaries across four experience levels—Entry, Mid, Senior, and Director—using box plots to show variability, medians, and outliers.  
- **Motivation:** While line plots reveal ranges, they don't expose how salaries are distributed within those ranges. Box plots offer deeper statistical insight by showing spread, skew, and outliers, making it easier to analyze salary gaps and overlaps.  
- **What worked well:** Makes salary variability and outliers visible; good for comparing central tendency and spread between experience groups.  
- **What didn’t work so well:** Doesn’t show individual data points; can be less intuitive for audiences unfamiliar with statistical plots.  
- **Difference:** Compared to the line plot (Sketch 12), which shows only the min and max salary range, the box plot provides a **richer view of salary distribution**, highlighting medians, quartiles, and outliers. It's more useful when you want to understand how salaries are spread within each experience level—not just the extremes.

---

## Task 5: Summarizing  

In this project, we explored a wide range of visualization designs, experimenting with both conventional and more exploratory directions. Our design process involved creating multiple sketches for each task, comparing their ability to highlight patterns, and iterating on the ones that felt most intuitive. Some visualizations leaned on familiar encodings such as bar charts and line graphs, while others experimented with scatter plots, heatmaps, and even network-style layouts for skill co-occurrence. This diversity allowed us to see the same data from different angles, and in many cases, the act of sketching reshaped our understanding of what the domain questions were truly asking.  

Through this process, we learned that different designs reveal different aspects of the data. For example, line charts were effective for showing temporal trends and cycles, but they lacked explanatory depth when it came to highlighting categorical differences. On the other hand, bar charts provided clarity in ranking industries or states by salary, though they sometimes felt too simplistic and repetitive when applied across multiple tasks. Scatter plots gave us insight into correlation patterns, such as salary versus company size, but were harder to interpret at a glance compared to simpler charts. These explorations underscored that no single visualization was “best” on its own—the effectiveness depended on the specific domain question being asked.  

Readability was one of the strongest criteria we considered. Simpler visualizations like bars and lines excelled in readability and scalability, making them suitable for larger datasets and general audiences. However, they sometimes sacrificed originality and expressiveness. More experimental visualizations like network graphs felt fresh and original, and they offered new ways to understand relationships (e.g., between skills), but they risked overwhelming viewers with complexity. This tension between clarity and creativity became a recurring theme in our evaluations.  

As we compared our sketches, we also reflected on the process itself. Some directions felt generative, opening up new ways to think about the data and its relationships. For instance, trying network-style visualizations expanded our design space and made us reconsider how co-occurrence could be represented beyond simple counts. Other directions felt repetitive, such as redrawing categorical comparisons in different formats that ultimately conveyed similar insights. This mix of repetition and discovery was valuable: the repetitive sketches reinforced our confidence in certain designs, while the exploratory ones revealed new perspectives.  

A key part of our process was starting with a broad abstraction of the question before refining it through multiple sketches. For example, when tackling the salary vs. experience domain, our initial sketch was a simple line chart connecting minimum and maximum salaries per experience level. While clean and readable, it lacked insight into distribution. This realization led to a second sketch using box plots, which revealed medians, quartiles, and outliers—offering a more nuanced view of salary disparities. This iterative process—moving from general to specific, from simple to statistically rich—improved both the clarity and the depth of our visual storytelling. It demonstrated that abstraction and sketching are not separate phases but an evolving conversation between design and understanding.

Overall, the diversity of our visualizations contributed to answering the domain questions in complementary ways. Some designs emphasized clarity, while others emphasized discovery. Together, they created a toolkit of perspectives that gave us a richer and more nuanced understanding of the dataset than if we had relied solely on one type of visualization. In the end, the process of comparing, critiquing, and iterating across different visual directions was just as important as the final designs themselves.  

---

## Task 6: Collaboration Process  

For this assignment, our group placed strong emphasis on collaboration and iteration. We explored multiple types of visualizations for each task, brainstorming ideas before narrowing down to the ones that felt the most intuitive and impactful. Often, we tried out different directions, drew sketches, and improved upon them after review.  

We communicated mainly through daily in-person meetings and continuous online coordination. Every day, we met for one hour at the new CS building, which gave us the opportunity to brainstorm ideas, refine sketches, and discuss next steps. Outside of these sessions, we stayed connected through WhatsApp, where we shared updates on GitHub progress and quick clarifications. This balance of structured offline meetings and flexible online messaging kept us aligned and moving forward.  

To share and refine our artifacts, we used both offline and online methods. Sketches were initially exchanged in person, and later uploaded online for additional review. GitHub served as the central hub for our README, datasets, and evolving visualizations, ensuring that version control was transparent and accessible to all members. We also made it a practice to review each other’s work carefully, pointing out improvements and suggesting alternative directions, which often led to stronger and clearer visualizations.  

Task division was fluid rather than rigid. We brainstormed together as a group, then each member took responsibility for sketching different versions of visualizations. Afterward, we came back together to review, critique, and decide which versions best communicated our intended ideas. This rotation ensured that every member contributed not only to generating ideas but also to the iterative process of refining them.  

Looking back, the most effective part of our collaboration was the rhythm we established between structured offline meetings and quick online updates. This approach allowed us to stay productive without overwhelming anyone. The main challenge we encountered was time management, particularly for visualizations that required several rounds of refinement before they felt right. However, these challenges shaped our design process: each round of critique and improvement brought us closer to visualizations that were sharper, more intuitive, and more meaningful.  

In the end, our collaboration was not just about dividing work—it was about co-creating, reviewing, and continuously improving. The group process itself shaped the design outcomes, making the final results stronger than anything we could have achieved individually.  
