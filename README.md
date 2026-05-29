# Food Inequality

An exploration of food inequality in the United States and the driving forces.

## Food Insecurity, Poverty, and Health Outcomes in America

### A Contradiction Worth Investigating

The United States produces more wealth than any country on earth. By GDP, by agricultural output, by sheer economic capacity, nothing comes close. And yet food insecurity remains a persistent condition for millions of Americans every year. The contradiction is what pulled us in: how does a country with this much abundance still have communities where people can't reliably afford to eat? What's actually driving it?

We started this project as a class assignment, expecting an academic exercise. As we got into the data, the patterns we found stopped feeling academic. The communities carrying the heaviest food insecurity were also the poorest, the most racially marginalized, and the ones suffering the worst health outcomes — all at once, often in the same counties. The contradiction had an answer: food insecurity in a wealthy country isn't a problem of national scarcity. It's a problem of how that wealth is distributed.

That shift in understanding also changed who the work was for. Policymakers, public health officials, and people with the authority to allocate budget and resources to underserved communities became the natural audience. The question we'd started with — why does this exist at all? — turned into a more useful one: where should intervention be prioritized?

## Hypotheses We Started With

- **Low-income and low-access communities will show higher rates of obesity and diabetes** than communities with higher income and better access. Reduced access to supermarkets may limit affordable fresh food, contributing to worse diet-related outcomes over time.

- **Food access problems will be more common in communities with lower median household income** and higher poverty rates. Food access inequality isn't random — it's connected to broader socioeconomic inequality.

- **Rural communities will show stronger distance-based food access barriers** than urban communities. Rural areas are more geographically spread out, so physical distance to supermarkets may matter more there.

- **Communities with lower vehicle availability may face compounded food access barriers**, even when stores aren't extremely far away. Distance alone doesn't capture access — transportation is part of the story.

- **Food access disparities may cluster geographically by region and state** rather than being evenly distributed across the country. Policy, infrastructure, and regional development patterns shape food access conditions.

## How the Analysis Unfolds

The project follows a deliberate progression — each step setting up the next.

### Step 1: Where is food insecurity in America?

We start with the national picture. Food insecurity isn't distributed evenly across the country; it clusters in specific regions, specific states, and specific counties. The opening visuals establish that geographic reality before any explanatory variable gets introduced.

### Step 2: How is income distributed across racial groups?

Before connecting income to food insecurity, we look at how income itself is distributed. Median household income varies significantly across racial groups, and that gap is the foundation for almost everything that follows. This is where the contradiction sharpens — it's not just that some communities have less; it's that "less" tracks consistently along racial lines.

### Step 3: How does income translate into food insecurity?

With income disparity established, we connect it to food insecurity directly. Counties with lower median incomes show higher food insecurity rates, and the relationship is strong enough to be visible at the state level, the county level, and within demographic subgroups. Black and Hispanic populations show disproportionately higher food insecurity rates — not because of anything intrinsic, but because they're disproportionately represented in the lower-income brackets we just mapped.

### Step 4: How does food insecurity translate into worse health?

The final step traces the consequence. Counties facing high poverty and high food insecurity show worse outcomes across obesity, high cholesterol, and short sleep — and lower rates of routine preventive care. The relationships hold individually for each factor, and they compound when both are present. The same counties appear again and again across the rankings: highest poverty, highest food insecurity, worst health outcomes, largest food budget shortfalls. By the end of the analysis, the answer to "where should intervention be prioritized?" has effectively named itself.

## Data Sources

| Dataset | Source | Role |
|---------|--------|------|
| 500 Cities: Local Data for Better Health (2019) | CDC | City-level health outcomes |
| Food Access Research Atlas | USDA | County-level food access metrics |
| Map the Meal Gap (2017) | Feeding America | County-level food insecurity rates and budget shortfalls |
| U.S. Census Dataset | U.S. Census Bureau | Population and demographic data |
| Healthcare Spend Inequality | Supplementary | Healthcare expenditure by income |
| Population Distribution | Supplementary | Population by race/ethnicity |

The 500 Cities dataset alone contains over 800,000 rows. All sources were used at meaningful depth rather than as cosmetic additions.

## Process & Cleaning

Most of the real work happened before any chart was drawn. A few of the larger problems we worked through:

### Data Alignment Across Sources

The 500 Cities health data is reported at the city level, while the Food Access Research Atlas is at the county level. We resolved the mismatch by left-joining food access metrics onto cities by matching on (State, County), preserving city-level granularity for the health outcomes while attaching the county's food access context. Each city ends up labeled with the food access profile of the county it sits inside — a defensible approximation but one that loses within-county variation, especially in larger counties.

### County Name Normalization

County names varied across sources in frustrating ways: suffixes like "County," "Parish," "Borough," "Census Area," and "Municipality" appeared inconsistently, along with mixed casing and whitespace. We built a normalization function that strips suffixes and standardizes case before joining, which caught most cases. A handful of independent cities and edge cases (Baltimore, St. Louis, D.C., several Alaska boroughs, Louisiana parishes) needed manual attention.

### Data Type Standardization

Several fields arrived as strings dressed up for human reading — $, commas, and % symbols embedded in numeric columns. These were stripped and cast to proper numeric types before any analysis ran. The final merged dataset was kept in long format (one row per city × health concern) rather than pivoted wide, which made per-concern filtering in Tableau substantially cleaner.

### Tools Used

Python (pandas) handled the cleaning, merging, and computation of derived metrics — per-person food budget shortfall, food access pressure composites, and z-scored rankings for surfacing outliers. Tableau handled the visualization layer. Excel was used for early inspection and for the source datasets that arrived as workbooks. Final visuals were exported from Tableau as SVGs and polished in Inkscape — annotations, leader lines, color refinement, and typography cleanup, so each chart can stand alone without a presenter.

## On LLM Use

We used LLM assistants (Claude and ChatGPT) throughout the project as a coding and writing collaborator. Concretely, that meant scaffolding and debugging pandas code for the cleaning and merging steps, iterating on chart titles and captions, talking through analytical framings (how to define "compounding" rigorously, what ranking metrics best surfaced the story), and editing this writeup. The analytical decisions — which questions to ask, what audience to write for, what the patterns meant — were ours. The LLM was a tool, not the author.

## Limitations

### Correlation vs. Causation

The relationships shown in this analysis are correlational, not causal. We can show that poverty, food insecurity, and poor health outcomes co-occur consistently and strongly, but we can't claim from this data alone that any one of them causes the others. The mechanism is almost certainly bidirectional and mediated by structural factors the data doesn't directly capture.

### Temporal Misalignment

The datasets come from different years — food insecurity from 2017, health outcomes from a 2019 CDC release based on earlier survey waves, food access and Census data from yet other years. We treated them as roughly contemporaneous, which is a defensible simplification but worth naming.

### Data Quality & Granularity Issues

The 500 Cities health metrics rely on self-reported survey data (BRFSS), which carries known biases around recall and social desirability. The geographic-unit mismatch between city-level health data and county-level food access data smooths over within-county variation. Per-person food budget shortfall figures can become noisy in counties with very small food-insecure populations, since the denominator is small enough to amplify outliers.

### Jurisdictional Edge Cases

A handful of jurisdictions — independent cities, Alaska boroughs, Louisiana parishes — required special handling during the county join. Most were caught by the normalization function, but a small number of unmatched rows remain.

### Impact on Interpretation

None of these limitations change the broad picture the data tells. They're worth noting because anyone using this analysis to make actual resource decisions should understand where the edges are.

## Design Approach

Every visualization in this project was built to work without narration. Chart titles state the conclusion, not the chart type — "Counties Facing Both Crises See the Worst Health Outcomes" rather than "2×2 Matrix of Poverty and Food Insecurity." A consistent palette runs through every artifact, with a reserved accent color used only for highlighting the "worse outcome" or high-priority condition, and neutral colors for baseline or protective indicators. Final polish was done in Inkscape — callouts, annotations, leader lines, and typography adjustments that significantly improved standalone readability.
