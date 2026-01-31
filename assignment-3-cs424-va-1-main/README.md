# Visual Analytics of LinkedIn Job Postings

<img width="795" height="400" alt="image" src="https://github.com/user-attachments/assets/31f64ee5-b04e-484d-9ccc-e292f2d1dc37" />

## Group Information  

| Name         | UIN       | Email             | Contribution |
|--------------|-----------|-------------------|--------------|
| Sunil Kuruba | 659375633 | skuru@uic.edu     | Visualization 1, 2 |
| Richa Rameshkrishna   | 651622805    | rrame11@uic.edu   | Visualization 3, 4, 5 |

---

## **Reproducibility**

- All the necessary datasets and preprocessed CSV files are included within this GitHub repository, ensuring that every visualization can be reproduced end-to-end without external dependencies.  
- Each Jupyter Notebook contains well-documented code cells for data cleaning, transformation, and visualization. The workflow follows a clear, linear execution order — running the notebooks sequentially will recreate all charts and GIFs shown in the README.  
- For rendering consistency on GitHub, the command  
  ```python
  alt.renderers.enable('svg')
   ```
has been enabled in all notebooks. This forces static SVG rendering, allowing plots to appear directly in GitHub previews even without interactive support.

**IMPORTANT**: If you wish to view the interactive Altair visualizations locally, simply comment out or remove that renderer line from the notebook. This will revert the charts to their default interactive Vega-Lite behavior in Jupyter, enabling full zoom, tooltip, and selection functionality.

- The project was tested on Python 3.11 with Altair 5.x and Vega-Lite 5.x across macOS and Linux systems to verify reproducibility and rendering consistency.

---

# **Visualization 1 — Skill Demand, Salary, and Geography (Tasks 1 – 3)**

## **Task 1 — Linked Views: Skill Demand vs Salary Distribution**

### **Question**
> Do the most in-demand technical skills by number of job postings also correspond to higher salaries?

This question explores whether job-market demand aligns with compensation or whether some highly popular skills are undervalued relative to others.

### **Data Attributes**
The visualization relies on three key attributes:  
- **Skill:** Extracted and normalized from the `skills_clean` column. Each posting may contain multiple skills separated by “|”. These were exploded into individual rows to compute frequency counts and enable linking across visualizations.  
- **num_postings:** The total number of job postings that mention each skill, serving as a measure of market demand.  
- **salary_mid:** A derived metric representing the midpoint between minimum and maximum salary estimates. This helps neutralize extreme salary outliers while preserving spread for boxplot visualization.

### **Views**
The visualization consists of two coordinated views displayed vertically.

1. **Top Skill Demand Bar Chart**  
   This chart highlights the *top 15 skills* ranked by the number of job postings.  
   The X-axis encodes the total posting count (`num_postings`), while the Y-axis lists skills (`skill`).  
   Each bar’s length reflects relative demand, making it easy to see which technical competencies dominate the market.  
   Labels, gridlines, and tooltips are carefully chosen to provide precise quantitative feedback on hover without overwhelming the viewer.

2. **Salary Distribution Boxplot**  
   The second chart presents a distribution of salaries for the selected skill using a boxplot.  
   Each box captures the interquartile range, median, and outliers of `salary_mid` values across postings that list that skill.  
   This encoding effectively summarizes spread and variability, providing a compact but informative representation of salary dispersion.  

Together, these two panels allow a user to compare how demand correlates with salary potential interactively.

### **Interaction**
A **point selection** (`alt.selection_point(fields=["skill"])`) is defined on the bar chart.  
When the user clicks on a bar, that selection filters the data shown in the boxplot through `transform_filter(selection)`.  
For example, selecting *Python* in the bar chart automatically updates the lower panel to display the salary distribution of all postings mentioning *Python*.  
This linking transforms static summaries into an exploratory experience where the user can test hypotheses about skills one by one.

### **Design Decisions**
The layout and encodings were chosen with clarity and usability in mind.  
- Skills are nominal and often lengthy, so positioning them on the Y-axis prevents label overlap and maintains legibility even for long names.  
- Boxplots were deliberately chosen over scatterplots or histograms because they reveal median trends and outliers without requiring aggregation bins.  
- The color palette remains consistent across both charts, reinforcing the sense of linkage and helping the eye connect the two views.  
- Interactive tooltips communicate precise numeric values and context, enabling the viewer to make quantitative assessments directly within the visualization.  
- The design adheres to the principle of minimal cognitive load by reducing unnecessary chart junk and emphasizing meaningful encodings.

### **Alternatives Explored**
Several alternative designs were tested before finalizing this approach.  
- A scatterplot mapping *average salary vs number of postings* was visually dense and exhibited heavy overplotting, making patterns hard to read.  
- Violin plots were also considered to show distribution shape but proved cluttered when many skills were compared simultaneously.  
- Mean salary bars provided less insight into spread and variability; they were replaced with boxplots that preserve data distribution fidelity.  

Through iteration, the current pairing achieved the best balance between interpretability and interactivity.

### **Key Findings**
This visualization revealed that *Python* and *Machine Learning* are by far the most commonly listed skills.  
However, they do not always yield the highest median salaries, suggesting that high demand does not directly imply higher pay.  
Skills such as *AWS* and *Deep Learning* appear less frequently but show wider salary ranges and higher medians, indicating niche specialization value.  
The dual-view design made it clear that understanding market trends requires looking beyond raw posting counts.

### **GIF Preview**
![Skill Demand vs Salary Distribution](visualizations/task1-v1-skill-vs-salary.gif)

---

## **Task 2 — Spatial Visualization: Top Skill by State**

### **Question**
> How do dominant technical skills vary across U.S. states, and what regional specialization patterns emerge?

### **Data Processing**
The dataset was aggregated at the `(state, skill)` level to compute posting counts for every skill in each state.  
For each state, the *top skill* (the most frequently occurring one) was identified using a mode operation.  
This derived dataset was then joined with a U.S. state **GeoJSON** boundary file to enable spatial encoding through Vega-Lite’s `geoshape` mark.

### **Visualization Description**
The resulting **choropleth map** depicts each U.S. state colored by its most dominant skill.  
Each color corresponds to a categorical skill label (e.g., *Python*, *Machine Learning*, *AWS*, *Database*, *Deep Learning*).  
Tooltips display the state name, the top skill, and the number of postings supporting that conclusion.  
The legend, positioned outside the map, allows easy identification and comparison of regional clusters.

### **Design Decisions**
- A *categorical color palette* optimized for colorblind accessibility was selected to differentiate up to ten unique skills without ambiguity.  
- Only the *top skill per state* is shown to simplify interpretation and maintain visual clarity. Showing proportions of all skills made the map unreadable in early experiments.  
- The legend is designed with succinct labels and color patches to guide immediate recognition, following best practices for qualitative encoding.  
- Subtle boundaries and neutral basemap styling keep the focus on color differences rather than geographic outlines.

### **Alternatives Explored**
During experimentation, a number of alternative representations were tested.  
A **heatmap** showing average salaries per state was visually effective but misled interpretation because a map would be better visually to identify the states.  
A **dot-density map** was attempted to plot each posting, but dense regions like California and New York quickly became saturated and unreadable. Not completely a bad choice though. 
A **bivariate map** encoding both skill and salary simultaneously was attractive conceptually but too complex for quick interpretation.  
Therefore, the categorical choropleth was retained for its simplicity and interpretive strength.

### **Key Findings**
The visualization demonstrates that *Python* overwhelmingly dominates most states, confirming its role as the de facto language of the data-science ecosystem.  
Western states such as *California*, *Colorado*, and *Washington* emphasize *Machine Learning* and *AWS* skills, aligning with their technology-heavy economies.  
Midwestern states like *Wisconsin* and *Minnesota* highlight *Database* and *Analytics* expertise, suggesting stronger traditional enterprise roles.  
These results show how geography correlates with industry specialization — cloud computing in tech hubs, databases in enterprise-driven regions, and analytics across urban centers.

### **GIF Preview**
![Top Skill by State](visualizations/task2-v1-top-skills-by-state.svg)

---

## **Task 3 — Linked Spatial Visualization**

### **Linked Views**
The dashboard unifies three coordinated views:
1. A **U.S. state map** visualizing each state’s dominant skill.  
2. A **bar chart** of the top 15 skills ranked by total postings.  
3. A **salary boxplot** showing the distribution of `salary_mid` for the selected subset.  

All three components are linked such that interactions in one view immediately update the others, forming a complete exploratory environment.

### **Interaction Mechanism**
Two Altair selection objects control linking:
- `state_select = alt.selection_point(fields=["state"], clear="true")` manages interactions from the map.  
- `skill_select = alt.selection_point(fields=["skill"], clear="true")` manages interactions from the bar chart.  

### **Design Decisions**
Creating a cohesive tri-view layout required balancing visual hierarchy and screen real estate.  
The map occupies the upper half to emphasize geographic context, while the bar and boxplot share the lower half for detail exploration.  
A consistent color encoding scheme ensures that *Python* appears as the same hue across all three panels, avoiding confusion.  
Each view includes clearly labeled axes, restrained gridlines, and explanatory titles to support independent comprehension even when viewed alone.  
The dashboard follows a “focus + context” paradigm — users see the big picture in the map and the fine-grained salary detail simultaneously.  

### **GIF Preview**
![Linked Spatial Visualization](visualizations/task3-v1-skill-vs-salary.gif)

---

# **Visualization 2 — Industry Landscape, Salary Distribution, and Skill Alignment (Tasks 1 – 3)**

## **Task 1 — Linked Views: Median Salary by State and Top Industries**

### **Question**
> How do median salaries vary across U.S. states, and which industries dominate overall job postings?

This task focuses on exploring the spatial variation of salaries alongside the nationwide distribution of job postings by industry.

### **Views**
1. **Median Salary Map (Top Panel)**  
   This choropleth map visualizes the median salary per state.  
   Each state polygon is colored on a sequential blue scale ranging from light (low median salary) to dark (high salary).  
   Tooltips display the *state name*, *median salary*, and *number of postings* within that state.  
   The map allows users to instantly see where compensation levels are highest and lowest across the country.

2. **Top Industries Bar Chart (Bottom Panel)**  
   The bar chart below summarizes the number of postings for each major industry.  
   The X-axis encodes the percentage of postings, while the Y-axis lists industries sorted by total job volume.  
   The Technology sector clearly dominates, followed by Finance and Healthcare, illustrating the uneven distribution of opportunities across sectors.

### **Interaction**
Hovering or clicking on any state updates the bar chart to highlight industries with strong representation in that region.  
Conversely, selecting an industry filters the map to display only the states containing that sector’s postings, providing bidirectional exploration of *“where industries pay the most.”*  
This linking reinforces spatial reasoning and encourages users to correlate salary trends with industrial composition.

### **Design Decisions**
- A **monochromatic blue palette** was chosen for the salary map to represent quantitative magnitude intuitively.  
  Darker hues correspond to higher salaries, following perceptual luminance progression.  
- The bar chart employs **horizontal orientation** for readability of long industry labels.  
- Shared fonts, grid alignment, and consistent axis styling tie both views into a single cohesive analytical unit.  
- **Tooltip interactivity** communicates exact values, allowing quantitative comparisons without visual clutter.

### **Alternatives Explored**
Early design attempts used a **bubble map** with circle size encoding salary, but overlapping bubbles obscured regional boundaries.  
A **stacked bar chart** was also tested to show salary and posting count simultaneously but confused viewers due to mixed scales.  
The current dual-view layout was selected for its clarity and cognitive separation between geography and quantity.

### **Key Findings**
- The **West Coast and Northeast** regions (California, Washington, New York, and Massachusetts) show the highest median salaries, often exceeding \$150,000.  
- States in the **Midwest and South** display lower salaries but a more even spread of industry presence.  
- The **Technology** and **Finance** sectors dominate total postings, whereas industries like **Education**, **Retail**, and **Logistics** appear less frequently but may offer localized opportunities.  
- This task highlights a structural divide between high-pay, tech-dense states and lower-pay, diversified economies.

### **GIF Preview**
![Median Salary and Top Industries](visualizations/task1-v2-salary-vs-industry.gif)

---

## **Task 2 — Heatmap: Skill vs Industry**

### **Question**
> What technical skills are most prevalent in each industry, and which sectors demonstrate diverse or specialized skill portfolios?


### **Visualization Description**
The **Skill vs Industry heatmap** uses a rectangular grid to represent co-occurrence frequency.  
- The *Y-axis* lists approximately 25 top skills (e.g., Python, AWS, TensorFlow, SQL).  
- The *X-axis* lists industries (Technology, Finance, Healthcare, Retail, etc.).  
- The *color intensity* encodes the number of job postings containing that skill in that industry.  
Darker cells indicate stronger demand within that pairing.

### **Design Decisions**
- A **sequential light-to-dark blue gradient** was chosen to emphasize magnitude while keeping the visualization aesthetically coherent with the salary map.  
- Axis labels were rotated and aligned to maximize legibility.  
- Gridlines were retained to help trace both dimensions clearly since users often scan horizontally (skills) and vertically (industries).  
- The chart was designed with sufficient white space to avoid color saturation in dense areas like Technology.

### **Alternatives Explored**
A clustered bar chart was initially attempted but became too cluttered given the large number of skills.  
Tree-maps and bubble matrices were also tested but failed to communicate two-dimensional relationships effectively.  
The heatmap was ultimately chosen because it supports both overview and pattern detection at a glance.

### **Key Findings**
- The **Technology industry** unsurprisingly shows the broadest skill coverage, with strong presence of *Python*, *Machine Learning*, *AWS*, and *SQL*.  
- **Finance** emphasizes analytical and database skills such as *R*, *SQL*, and *Tableau*, reflecting the data-driven nature of financial modeling.  
- **Healthcare** jobs highlight tools like *TensorFlow*, *Deep Learning*, and *Azure* — suggesting growing adoption of AI for medical data analysis.  
- **Retail** and **Education** display narrower skill portfolios but often overlap around *Data Visualization* and *Cloud Computing* competencies.  
Overall, the heatmap exposes clear industry specialization trends while emphasizing transferable skills across domains.

### **GIF Preview**
![Skill vs Industry](visualizations/task2-v2-skills-vs-industry.svg)

---

## **Task 3 — Linked Spatial Visualization: Salary + Industry + Skill Alignment**

### **Linked Views**
1. **Median Salary by State Map (Top-Left)**  
   Displays state-level salary variations using the same sequential scale as in Task 1.  
   Selecting a state highlights its top industries and relevant skills.  
2. **Top Industries Bar Chart (Bottom-Left)**  
   Shows industry-wise posting counts for the selected state or nationwide if no selection is active.  
   Bars are colored by industry category to link visually with the heatmap.  
3. **Skill vs Industry Heatmap (Right Panel)**  
   Reflects the intersection of the two previous selections — only displaying skills and industries relevant to the chosen state.  

### **Interaction Mechanism**
Two linked selection objects (`state_select` and `industry_select`) coordinate filtering among all views.  
- Clicking a **state** filters the bar chart and heatmap to display data relevant to that location.  
- Selecting an **industry** highlights corresponding skill clusters within the heatmap and shades relevant states on the map.  
- Clearing selections resets the view to a national overview.  
Hovering reveals tooltips containing median salary, top skills, and posting counts.

### **Design Decisions**
- The dashboard adopts a **balanced spatial-analytical layout**, placing the map above for context and the detailed charts below for explanation.  
- Industry colors remain consistent across the bar chart and heatmap legend to maintain visual unity.  
- Axis titles, legend placement, and font sizes were harmonized to achieve aesthetic coherence.  
- A neutral white background enhances contrast, while subtle gridlines help the user trace relationships without distraction.  
- Interaction latency was optimized by pre-aggregating counts using Pandas to ensure smooth performance in Altair.

### **Cross-View User Flow**
1. A viewer starts with the **map**, observing overall salary gradients.  
2. Upon selecting a state such as *California*, the **bar chart** updates to show that Technology overwhelmingly leads postings there.  
3. The **heatmap** then highlights the dominant skill sets within that state’s major industries — for example, *Python*, *TensorFlow*, and *AWS* under Technology.  
4. Alternatively, selecting the **Finance** industry emphasizes states like *New York* and *Illinois* while spotlighting analytical tools like *R* and *SQL* on the heatmap.  

### **Insights Enabled by Linking**
- States with **high median salaries** (e.g., California, Washington, New York) are predominantly driven by the **Technology industry**, confirming its premium compensation.  
- **Finance-centric states** such as New York and Illinois show competitive but slightly lower salary medians, aligning with established cost-of-living patterns.  
- Linking revealed that **Healthcare and Education sectors**, while smaller in posting volume, still show high specialization in AI-related skills, signaling domain transformation.  
- Users can observe that the same skill (e.g., *Python*) yields different pay ranges depending on the industry and state, illustrating the complex interplay between geography, domain, and skill value.

### **GIF Preview**
![Linked Industry Dashboard](visualizations/task3-v2-salary-vs-industry.gif)

---
# **Visualization 3 — Brush Selection - Salary vs. Engagement Analysis (Task 1 & 3)**

## **Task 1 — Linked View Visualizations**
### **Question**
> How does salary relate to job engagement rates across different experience levels, and what is the distribution of work types for jobs within specific salary and engagement ranges?
This task focuses on exploring the relationship between compensation and applicant engagement while examining how different work arrangements (full-time, contract, etc.) distribute across these dimensions.

**Demo:**

![Brush Selection Interaction](visualizations/Task_1a_richa.gif)

#### **Views**
1. **Scatter Plot (Top Panel)**  
   This visualization maps salary (X-axis) against engagement rate (Y-axis), with each point representing a job posting.
   - **X-axis**: Annual salary ranging from $20,000 to $500,000 with dollar formatting
   - **Y-axis**: Engagement rate (0-100%) representing the applies-to-views ratio
   - **Color**: Experience level encoded using Tableau10 categorical color scheme
   - **Size**: Fixed at 80 pixels with 0.7 opacity to balance visibility and overplotting
   - **Sample**: 10,000 jobs randomly sampled to avoid overplotting
   
2. **Bar Chart (Bottom Panel)**  
   The bar chart displays the distribution of work types filtered by the brush selection above.
   - **X-axis**: Count of jobs (quantitative)
   - **Y-axis**: Work type categories, sorted by frequency
   - **Color**: Category20 scheme for work type differentiation

#### **Interaction**
**Primary Interaction: Brush Selection (Manipulating View + Filtering)**
- Users can click and drag on the scatter plot to select a rectangular region of interest
- Selected points retain their experience level colors while unselected points turn light gray
- The bar chart below updates in real-time to show only work types within the brushed region
- This enables the user flow: Overview → Filter → Details on Demand

**Secondary Interaction: Tooltips (Details on Demand)**
- Hovering over points reveals: job title, salary, engagement rate, experience level, work type, and remote status
- Provides detailed context without cluttering the visualization

#### **Design Decisions**
- **Scatter plot encoding**: Position encoding leverages our strongest perceptual ability for exploring correlations between two quantitative variables (Cleveland & McGill, 1984)
- **Color scheme**: Tableau10 provides clear distinction between 6 experience level categories with good color discrimination
- **Opacity setting**: 0.7 opacity allows users to see overlapping points while maintaining individual point visibility
- **Vertical layout**: Creates a stronger visual connection between views and is more space-efficient
- **Sorting**: Bar chart sorted by frequency to emphasize dominant work types

#### **Alternatives Explored**

**Alternative 1: Single Point Selection**
- **Approach**: Used `alt.selection_point()` to select individual points
- **Rejected**: Too granular for exploring patterns; users would need to click many points to understand trends. Brush selection allows selecting regions of interest more efficiently.

**Alternative 2: Color by Work Type**
- **Approach**: Encoded work type with color in the scatter plot instead of experience level
- **Rejected**: With 7 work type categories, color discrimination became difficult, especially with overplotting. Experience level (6 categories) provided better visual separation and more meaningful insights about career progression.

**Alternative 3: Linked Histogram Instead of Bar Chart**
- **Approach**: Salary histogram filtered by brush selection
- **Rejected**: Less informative than work type distribution; users can already see salary information in the scatter plot. Work type distribution provides complementary insights.

**Alternative 4: Side-by-Side Layout**
- **Approach**: Horizontal arrangement of scatter plot and bar chart
- **Rejected**: Vertical stacking creates a stronger visual connection between views and is more space-efficient for wide visualizations.

#### **Key Findings**
- **Salary-Engagement Paradox**: Higher salaries don't always correlate with higher engagement; mid-range positions ($75K-$125K) often show stronger engagement rates
- **Experience Level Patterns**: Entry-level positions cluster in lower salary ranges with variable engagement, while senior positions show more consistent engagement regardless of salary
- **Work Type Distribution**: Brushing reveals that contract positions dominate certain salary bands, while full-time positions are more evenly distributed across all ranges
- **Full-time dominance**: Approximately 80% of all job postings are full-time positions, with contract work being the second most common type

---

## **Task 3 — Linked Spatial Visualization: Integrated Dashboard**

### **Question**
> How do geographic location, salary ranges, engagement patterns, and work type distributions interact across the U.S. job market?

This task integrates the spatial visualization from Task 2 with the linked views from Task 1 to create a comprehensive dashboard that enables cross-view exploration and reveals insights impossible to discover in isolated views.

### **Integrated Geographic and Job Market Dashboard**
**Demo:**

![Linked Dashboard Interaction](visualizations/Task_3_richa.gif)

#### **Dashboard Components**
1. **Choropleth Map (Top Panel)** - 900×450px  
   Geographic overview showing average salary by state with interactive state selection
   
2. **Scatter Plot (Middle Panel)** - 800×350px  
   Salary vs. engagement rate with brush selection, filtered by selected state
   
3. **Bar Chart (Bottom Panel)** - 800×200px  
   Work type distribution filtered by both state selection and scatter plot brush

#### **Cross-View Interactions and User Flow**

**Interaction 1: State Selection → Global Filter (Geographic → Detail)**
- **Mechanism**: `alt.selection_point(fields=['state'])` on choropleth map
- **User Flow**: Geographic context → Detailed exploration
- **Behavior**:
  1. User clicks any state on the choropleth map
  2. Selected state highlights with full opacity and 3px black stroke
  3. Unselected states fade to 0.3 opacity (maintaining geographic context)
  4. Scatter plot filters to show only jobs from the selected state
  5. Bar chart updates to show work type distribution for that state
- **Design Intent**: Start with familiar geographic overview, then drill down to specific state patterns
- **Visual Feedback**: 
  - Strong opacity contrast (1.0 vs 0.3) on map
  - Bold stroke on selected state
  - Coordinated updates across all linked views

**Interaction 2: Brush Selection → Work Type Filter (Detail → Detail)**
- **Mechanism**: `alt.selection_interval()` on scatter plot
- **User Flow**: Explore salary-engagement patterns → Filter work types
- **Behavior**:
  1. After selecting a state, user brushes rectangular region on scatter plot
  2. Selected points retain experience level colors
  3. Unselected points turn light gray
  4. Bar chart filters to show only work types within brushed region
- **Design Intent**: Allow exploration of specific salary/engagement segments within a state
- **Cascading Filters**: Both state selection AND brush selection apply simultaneously

**Interaction 3: Coordinated Tooltips (Details on Demand)**
- **Mechanism**: Hover interactions across all views
- **Behavior**: 
  - Map: State name, job count, average salary, remote percentage
  - Scatter plot: Job title, state, salary, engagement rate, experience level, work type
  - Bar chart: Work type, count, average salary
- **Design Intent**: Provide context without permanent screen real estate

#### **Intended User Flows**

**Scenario A: Geographic Exploration (Top-Down)**
1. **Overview**: User scans choropleth to identify high-salary states
2. **Select**: Clicks California to investigate West Coast tech market
3. **Explore**: Scatter plot reveals CA salary range ($20K-$500K) and engagement patterns
4. **Filter**: Brushes high-engagement, mid-salary region ($80K-$120K)
5. **Discover**: Bar chart shows these are predominantly full-time positions

**Scenario B: Job Seeker Workflow (Goal-Oriented)**
1. **Context**: User interested in remote work opportunities
2. **Identify**: Hovers over states to find high remote percentage (tooltip)
3. **Select**: Clicks state with good remote percentage and high salary (e.g., Colorado)
4. **Analyze**: Examines scatter plot to find desired salary range with good engagement
5. **Refine**: Brushes target region to see which work types are available
6. **Decision**: Discovers remote-friendly full-time positions in desired salary band

**Scenario C: Comparative State Analysis (Research)**
1. **Compare**: Click Texas ($91K avg), note scatter plot distribution patterns
2. **Observe**: Texas shows concentration in $60K-$100K range with moderate engagement
3. **Switch**: Click California ($104K avg), compare scatter patterns
4. **Discover**: CA has more high-salary outliers and different engagement distribution
5. **Analyze**: Brush equivalent salary ranges ($80K-$100K) in both states
6. **Insight**: Compare work type mixes - CA shows more full-time tech, TX shows more contract positions

#### **Technical Implementation Decisions**

**Challenge 1: Data Size Management**
- **Problem**: 22,785 spatial jobs too large for smooth interactive performance
- **Solution**: Sampled 4,172 jobs for scatter plot while maintaining state coverage (all 51 states represented)
- **Trade-off**: Lost some granular detail but gained smooth, responsive interaction
- **Result**: Interactions render in <100ms, maintaining fluid user experience

**Challenge 2: Filter Coordination**
- **Problem**: Multiple selections need to work together (state AND brush) without conflicts
- **Solution**: Cascade `.transform_filter()` calls in proper order:
```python
  scatter.transform_filter(state_click)
  bar_chart.transform_filter(brush).transform_filter(state_click)
```
- **Result**: Intuitive filter stacking where state filter is primary, brush filter is secondary

**Challenge 3: Visual Hierarchy**
- **Problem**: Three views competing for attention, risk of cognitive overload
- **Solution**: 
  - Map at top (largest, 900×450px) establishes geographic context first
  - Scatter plot in middle (800×350px) for detailed exploration
  - Bar chart at bottom (800×200px, most compact) for summary statistics
  - Consistent 800px width for detail views creates visual alignment
  - Vertical flow follows natural reading pattern (top to bottom)
- **Result**: Clear visual hierarchy guides exploration from overview to details

**Challenge 4: Performance Optimization**
- **Strategy 1**: Pre-aggregated statistics for bar chart (not computed on-the-fly)
- **Strategy 2**: Sampled data for scatter plot (4,172 vs 22,785 jobs)
- **Strategy 3**: Pre-computed state statistics table (51 rows vs thousands of jobs)
- **Result**: Dashboard loads quickly and interactions are responsive

#### **Design Decisions for Visual Consistency**

**Color Scheme Differentiation:**
- **Map**: Blues sequential (salary magnitude)
- **Scatter plot**: Tableau10 categorical (experience levels)
- **Bar chart**: Category20 categorical (work types)
- **Rationale**: Different color schemes prevent confusion about what attributes are encoded where
- **Consistency**: Blues theme ties together the overall dashboard aesthetic

**Selection Visual Language:**
- **Map selection**: Opacity (1.0 vs 0.3) + stroke (bold black) = semi-permanent state
- **Scatter brush**: Color vs. gray = temporary selection within state
- **Rationale**: Different visual treatments match different interaction persistence levels
- **Clarity**: Users can easily see both state selection (bold) and brush selection (color) simultaneously

**Typography and Spacing:**
- **Font sizes**: Titles (14px), axis labels (11px), legends (12px)
- **Spacing**: 20px between views for clear visual separation
- **Alignment**: Left-aligned titles create consistent vertical edge
- **White space**: Sufficient padding prevents claustrophobic feeling

#### **Concrete Example: Insight Only Possible Through Linking**

**Discovery: California's High-Salary Jobs Show Unexpected Low Engagement**

1. **Click California** on the map → Scatter plot filters to CA jobs only
2. **Observe**: CA shows jobs spanning $20K-$500K, but high-salary positions ($200K+) cluster at only 5-15% engagement
3. **Brush the $200K+ region** → Bar chart reveals these are predominantly full-time positions
4. **Compare**: Click Texas and brush same salary range → Different pattern emerges with more contract positions

**The Insight:**
California's premium positions ($200K+) are mostly full-time roles with surprisingly low engagement rates compared to mid-tier jobs. This suggests highly specialized positions that only attract qualified candidates, rather than broad applicant pools.

**Why linking matters:**
- Choropleth alone shows CA's high average salary but hides within-state variation
- Scatter plot without state filter mixes all states together, obscuring CA-specific patterns  
- Bar chart without salary filtering doesn't reveal how work types distribute across salary segments
- Only by combining state selection (map) + salary filtering (brush) can we see work type differences in specific market segments


### **Visualization 4: Click Selection - Salary Heatmap with Detail View (Task 1)**
**Demo:**

![Click Selection Interaction](visualizations/Task_1b_richa.gif)

#### **Question**
> How do average salaries vary across combinations of experience level and work type, and what is the detailed salary distribution for specific experience levels?

This visualization provides both an aggregated overview of salary patterns and detailed distribution analysis through coordinated views.

#### **Views**
1. **Heatmap (Top Panel)**  
   A 2D matrix showing average salary by experience level and work type.
   - **X-axis**: Work type categories (Full-time, Contract, Part-time, etc.)
   - **Y-axis**: Experience levels ordered by seniority (Entry level → Executive)
   - **Color**: Mean salary using Blues sequential scheme ($40K-$170K range)
   - **Opacity & Stroke**: Visual feedback for selection state (selected cells are fully opaque with black stroke, others fade to 0.4 opacity)

2. **Histogram (Bottom Panel)**  
   Detailed salary distribution for the selected experience level.
   - **X-axis**: Binned salary ranges (20 bins for optimal granularity)
   - **Y-axis**: Count of jobs in each salary bin
   - **Color**: Steelblue for visual consistency

#### **Interaction**
**Primary Interaction: Click Selection (Manipulating Data + Navigation)**
- Users click any cell in the heatmap to select an experience level
- Selected row becomes fully opaque with a bold 3-pixel black stroke
- Unselected rows fade to 0.4 opacity to maintain context
- Histogram updates to show detailed salary distribution for the selected experience level
- User flow: Overview (aggregated heatmap) → Select → Details (distribution histogram)

**Secondary Interaction: Tooltips**
- Heatmap tooltips show: experience level, work type, average salary, and job count
- Histogram tooltips show: salary range bins and job counts for precise values

#### **Design Decisions**
- **Sequential color scheme**: Blues scheme is appropriate since salary has a natural zero baseline (not comparing deviations from a central value) (Brewer, 1994)
- **Heatmap ordering**: 
  - Experience levels ordered from entry to executive (natural career progression)
  - Work types ordered by frequency (full-time first) to facilitate pattern recognition
- **Histogram binning**: 20 bins provides good granularity without excessive noise, balancing detail and interpretability
- **Visual feedback**: Strong opacity contrast (1.0 vs 0.4) and bold stroke clearly distinguish selected from unselected states
- **Click vs. hover**: Click selection provides stable, intentional exploration rather than flickering updates from hover

#### **Alternatives Explored**

**Alternative 1: Hover-Based Selection**
- **Approach**: `alt.selection_point(on='mouseover')` for hover interaction
- **Rejected**: Too sensitive; histogram would flicker constantly as users explored the heatmap. Click selection provides stable, intentional exploration.

**Alternative 2: Diverging Color Scheme**
- **Approach**: Red-Blue diverging scheme with median as midpoint
- **Rejected**: Sequential scheme is more appropriate since salary has a natural zero baseline and we're not comparing deviations from a central value.

**Alternative 3: Box Plot Instead of Histogram**
- **Approach**: Box plot showing quartiles and outliers for selected experience level
- **Rejected**: While box plots efficiently show summary statistics, histograms better reveal distribution shapes, multiple modes, and gaps in salary ranges that are important for job seekers.

**Alternative 4: Bi-directional Selection**
- **Approach**: Allowing selection from either heatmap or histogram
- **Rejected**: Added complexity without clear benefit. The hierarchical overview-to-detail flow is more intuitive.

#### **Key Findings**
- **Executive Premium**: Executive positions command significantly higher salaries across all work types, with full-time executive roles averaging over $170K
- **Contract Work Patterns**: Contract positions often pay comparable or higher rates than full-time for senior levels, showing bimodal distributions suggesting both high-end consulting and lower-end temporary work
- **Distribution Shapes**: 
  - Entry-level positions show right-skewed distributions (concentrated at lower end with long high-salary tail)
  - Executive positions show more uniform distributions at higher salary ranges
- **Mid-Senior sweet spot**: Mid-Senior level positions represent the largest segment (12,710 jobs) with salaries ranging from $60K-$200K

---

### **Visualization 5: Spatial Visualization: Geographic Salary Distribution(Task 2)**

### **Question**
> How does average job salary vary geographically across the United States? Are there regional patterns in compensation that reflect cost of living, industry concentration, or other geographic factors?

This task leverages spatial information from the dataset to explore how compensation varies across state boundaries and identify regional economic patterns.

### **Visualization: Choropleth Map - Average Salary by State**
**Demo:**

![Map Interaction](visualizations/Task_2_richa.gif)

#### **Design Description**
The choropleth map visualizes average salary across U.S. states using a sequential color encoding.
- **Geographic projection**: AlbersUSA (preserves area relationships for accurate state comparison)
- **Color encoding**: Blues sequential scheme mapping average salary ($40K-$150K range)
- **Stroke**: White borders (1px) for clear state delineation
- **Background**: Light gray (#f0f0f0) for states with no data
- **Tooltips**: Display state name, job count, average salary, and percentage of remote positions

#### **Data Preparation**
- Extracted state codes from the location field (e.g., "San Francisco, CA" → "CA")
- Filtered to 22,785 jobs with valid 2-letter state codes across 51 states
- Aggregated statistics per state: job count, mean salary, mean engagement rate, remote work percentage
- Mapped state abbreviations to FIPS codes for geographic joining with topographic data

#### **Comparison of Encoding Alternatives**

**Alternative 1: Graduated Symbols (Proportional Circles)**
- **Approach**: Circles on state centroids sized by average salary
- **Advantages**: 
  - Preserves geographic accuracy better than choropleth
  - Doesn't imply uniform distribution within states
- **Disadvantages**: 
  - Overlapping symbols in dense East Coast region
  - Harder to compare values visually (area perception is less accurate than color intensity)
- **Trade-off**: While theoretically more accurate, practical usability suffered

**Alternative 2: Dot Density Map**
- **Approach**: Individual dots for job postings, colored by salary range
- **Advantages**: Shows individual job locations and distribution patterns within states
- **Disadvantages**: 
  - Requires precise lat/lon coordinates (not available in our dataset)
  - Severe overplotting in urban centers like NYC, SF, and Seattle
  - Individual salaries less meaningful than aggregated patterns
- **Trade-off**: Would be ideal with precise coordinates, but state-level aggregation is more interpretable for current data granularity

**Alternative 3: Cartogram (Area Distortion)**
- **Approach**: Distort state sizes based on job count or average salary
- **Advantages**: Emphasizes states with more data or higher compensation
- **Disadvantages**: 
  - Distorted geography is cognitively demanding
  - Unfamiliar representation reduces quick recognition
  - Doesn't align with users' mental maps of U.S. geography
- **Trade-off**: Novel but sacrifices geographic intuition that users rely on

**Alternative 4: Bivariate Choropleth**
- **Approach**: Encode both salary (color hue) and job count (color intensity) simultaneously
- **Advantages**: Shows two variables without requiring multiple maps
- **Disadvantages**: 
  - Complex legend requires user training
  - Color discrimination becomes difficult with 2D color scales
  - Cognitive load too high for general audience
- **Trade-off**: More information but significantly reduced interpretability

**Alternative 5: Hexagonal Binning (Hex Tile Map)**
- **Approach**: Replace states with equal-area hexagons arranged geographically
- **Advantages**: Equal visual weight for each region, better for comparing small states
- **Disadvantages**: 
  - Loses actual geographic shapes and boundaries
  - Less familiar to general audiences
  - Our data naturally aligns with state boundaries
- **Trade-off**: Better for equal comparison but worse for geographic recognition

#### **Final Choice Justification**

**Why Choropleth Won:**
1. **Cognitive Efficiency**: Users have strong mental models of U.S. state geography (Harrower & Brewer, 2003)
2. **Data Alignment**: Our data naturally aggregates at the state level (extracted from location field)
3. **Color Effectiveness**: Sequential color schemes are well-understood for quantitative data
4. **Scalability**: Performs well with 51 states without overplotting or symbol overlap
5. **Tooltip Enhancement**: Interactive tooltips provide additional context (job count, remote %) without visual encoding complexity

**How It Improves Interpretability:**
1. **Immediate Pattern Recognition**: West Coast and Northeast salary clusters are instantly visible
2. **Regional Comparisons**: Adjacent states can be easily compared (e.g., California vs. Nevada, New York vs. Pennsylvania)
3. **Scale Consistency**: Domain set to $40K-$150K captures meaningful variation without outlier distortion
4. **Professional Aesthetic**: Blues scheme aligns with business/financial visualization conventions and is colorblind-friendly
5. **Geographic Context**: Leverages users' existing knowledge of U.S. geography to provide immediate spatial understanding

#### **Technical Implementation**
- **Geographic data source**: Vega datasets US topography (us_10m.url) provides state boundaries
- **Projection**: AlbersUSA conic equal-area projection with inset Alaska and Hawaii
- **Data transformation**: Lookup join connects state statistics to geographic features via FIPS codes
- **Color scale**: Linear mapping from $40K (light blue) to $150K (dark blue)

#### **Key Geographic Insights**

**High-Salary Regions:**
- **California**: $104,668 average (4,423 jobs) - Tech hub dominance
- **Washington**: $103,208 average (1,012 jobs) - Seattle/Amazon effect
- **Massachusetts**: $103,918 average (671 jobs) - Boston biotech and finance
- **New York**: $101,841 average (2,354 jobs) - Finance and media industries
- **New Jersey**: $101,310 average (691 jobs) - NYC metro spillover

**Regional Patterns:**
- **West Coast Premium**: CA and WA show 20-30% higher salaries than national average
- **Northeast Cluster**: NY, MA, NJ, CT form high-salary corridor
- **Southeast Lower Range**: FL ($81,851), GA, NC generally 10-20% below national average
- **Midwest Variation**: IL ($94,628) outperforms neighbors due to Chicago metro area
- **Tech Hub Effect**: States with major technology centers consistently show elevated salaries

**Data Density:**
- California dominates with 4,423 jobs (19% of all postings with state data)
- New York: 2,354 jobs (10%)
- Texas: 1,447 jobs (6%)
- Top 10 states account for approximately 60% of all job postings

---
