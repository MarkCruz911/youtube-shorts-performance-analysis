# YouTube Shorts Performance Analysis

An exploratory analysis of 346 YouTube Shorts using Python, pandas, and Tableau Public to investigate audience reach, estimated revenue, viewing behavior, and content category performance.

This project follows the PACE framework — Plan, Analyze, Construct, and Execute — and transforms an Excel export into a documented analytical dataset and an interactive Tableau story.

**Author:** Marco Antonio Cruz Rojas  
**Project status:** Exploratory analysis and Tableau category story completed. SQL analysis and a portfolio website are planned extensions.

## Interactive Tableau Story

[Explore the YouTube Shorts Performance Analysis on Tableau Public](https://public.tableau.com/app/profile/marco.antonio.cruz.rojas/viz/YouTubeShortsPerformanceAnalysis/YouTubeShortsKeyFindings)

The published story contains four sections:

1. **Overall Performance:** Views, video count, and estimated revenue across the exported videos.
2. **Category Comparison:** Median accumulated views by content category.
3. **Publication Year: 2025:** Category performance within a selected publication-year cohort.
4. **Recommendations & Limitations:** Proposed content experiments and the constraints of the analysis.

The dashboard includes a publication-year filter that updates the summary indicators and category comparison.

The Tableau story focuses on content categories. Additional analyses of audience metrics, revenue ratios, CTR, and viewing duration are documented in the notebook.

## Executive Presentations

Two presentations summarize the analysis for stakeholders:

### Overall Performance and Audience Findings

Findings on exported video performance, audience reach, estimated revenue,
CTR, and average viewing duration, with recommendations and limitations.

[Download the executive report — PowerPoint](reports/YouTube_Shorts_Executive_Report.pptx)

### Content Category Findings

Category comparisons using median views, publication-year breakdowns,
sample sizes, and priorities for future content testing.

[Download the category executive summaries — PowerPoint](reports/YouTube_Shorts_Category_Executive_Summaries.pptx)

## 1. Project Overview

CEO needs to understand which topics attract viewers and whether apparent performance differences are consistent enough to inform future publishing decisions.

A category with the most total views may also have the most published videos. A category with a high average may be influenced by a small number of unusually successful videos.

This project therefore examines both overall performance and typical performance per video, using medians, category sample sizes, publication-year comparisons, and exploratory correlations.

The analysis supports decisions about which topics deserve further testing. It does not establish that a particular topic, CTR, or viewing-duration metric causes higher views.

## 2. Business Questions

The project investigates the following questions:

- How many views and how much estimated revenue are represented by the exported Shorts?
- Which videos have unusual revenue relative to their view counts?
- How strongly are views associated with reported new viewers?
- How are impressions CTR and average viewing-duration percentage associated with views?
- Which content categories have the highest median views per video?
- Do category rankings change when videos are grouped by publication year?
- How do category sample sizes affect the interpretation of performance?
- Which findings can inform future content experiments?

## 3. Dataset and Scope

The source is an Excel export of YouTube video performance data:

`data/raw/TODOS LOS SHORTS CON DATOS.xlsx`

The analytical dataset contains **346 videos**, with one row per video.

The videos have publication dates spanning **2023 through 2026**. Publication year describes when a video was released; it does not identify the year in which all of its views occurred.

Views are analyzed as accumulated counts reported in the export, rather than as performance measured over an equal number of days after publication.

### Main analytical fields

| Area | Fields |
|---|---|
| Video identification | `video_id`, `video_title` |
| Publication information | `publish_date`, `publish_year`, `publish_month`, `publish_day_of_week` |
| Video length | `video_duration_seconds` |
| Audience | `new_viewers`, `returning_viewers`, `casual_viewers`, `regular_viewers` |
| Reach | `views`, `impressions`, `impressions_ctr_percent` |
| Viewing duration | `average_view_duration_seconds`, `average_view_duration_percent` |
| Revenue | `estimated_revenue_usd`, `estimated_revenue_per_1000_views_usd` |
| Classification | `content_category`, `review_status` |

The Tableau export contains **20 fields**.

### Reporting scope

All project-wide totals refer to the 346 exported video rows.

The source summary reported 138,204,991 views, while the video-level rows summed to 120,143,235 views. The difference of 18,061,756 views was not reconciled in this analysis.

Accordingly, the video-level total is used consistently, and the findings should not be presented as a reconciled total for the entire channel.

## 4. Methodology — PACE

### Plan

Defined the analytical questions around content performance, audience reach, and estimated revenue.

The intended audience includes content strategists, video editors, and stakeholders evaluating future publishing priorities.

The analysis was designed to distinguish:

- Total reach from typical performance per video.
- Associations from causal explanations.
- Publication-year cohorts from calendar-year viewing activity.
- Completed analyses from proposed future experiments.

### Analyze

Audited and prepared the source data, then explored:

- Data types, missing values, and video identifiers.
- Distributions of views and estimated revenue.
- Videos with unusual revenue-to-view ratios.
- Relationships between views and audience metrics.
- Relationships between views, impressions CTR, and viewing duration.
- Content categories and their classification status.
- Category performance overall and by publication year.

### Construct

Created derived metrics and reusable analytical outputs:

- Estimated revenue per 1,000 views.
- Average viewing duration as a percentage of video length.
- Publication date attributes.
- Content category assignments and review information.
- Category summaries with video counts and median views.
- A CSV dataset for Tableau.
- An interactive dashboard and a four-point Tableau story.

This phase focused on analytical tables and visualizations. No predictive machine-learning model was trained.

### Execute

Communicated the results through:

- Notebook explanations and visualizations.
- Executive summaries.
- A published Tableau story.
- Recommendations for future content tests.
- Documentation of data and interpretation limitations.

The recommendations are proposed next steps; their business impact has not yet been measured.

## 5. Data Preparation and ETL

The workflow follows an ETL process:

**Excel export → Python preparation and analysis → Categorized CSV → Tableau Public**

### Extract

Loaded the Excel source into pandas for inspection and analysis.

### Transform

Prepared the analytical data by:

- Standardizing column names for use in Python.
- Preparing publication dates and numeric fields.
- Expressing duration fields in seconds.
- Checking missing values and identifier uniqueness.
- Creating publication year, month, and day-of-week fields.
- Calculating revenue and viewing-duration ratios.
- Reviewing unusual observations against the source data.
- Joining content classifications to video records using `video_id`.
- Checking that category integration preserved the video-level dataset.

### Load

Exported the prepared dataset to:

`data/processed/youtube_shorts_tableau.csv`

This CSV provides the data used in Tableau Public.

PostgreSQL is not part of the implemented pipeline at this stage.

## 6. Derived Metrics

### Estimated revenue per 1,000 views

Calculated for videos with positive view counts:

`estimated_revenue_usd / views * 1,000`

This metric makes revenue comparable relative to view volume.

It is a project-derived ratio using the exported fields. It should not be presented as an independently verified match to YouTube's official Shorts RPM metric.

### Average viewing-duration percentage

Calculated for videos with positive video duration:

`average_view_duration_seconds / video_duration_seconds * 100`

This expresses average viewing duration relative to the length of the video.

A value above 100% means the reported average viewing duration exceeds the video's length. It is not the percentage of viewers who completed the video.

The calculation alone cannot establish the underlying viewing behavior responsible for that value.

## 7. Exploratory Analysis and Findings

### 7.1 Overall performance

| Metric | Result |
|---|---:|
| Exported videos | 346 |
| Total views | 120,143,235 |
| Total estimated revenue | USD 6,624.59 |
| Aggregate estimated revenue per 1,000 views | USD 0.055139 |
| Mean video-level revenue per 1,000 views | USD 0.054819 |
| Median video-level revenue per 1,000 views | USD 0.053508 |

The aggregate revenue ratio divides total revenue by total views.

The mean video-level ratio gives each video's ratio equal weight. These calculations answer different questions and should not be used interchangeably.

### 7.2 Revenue anomaly review

One video had:

- **761,693 views**
- **USD 2.045 estimated revenue**
- Approximately **USD 0.002685 per 1,000 views**

This observation was flagged for review because its estimated revenue was unusually low relative to its view count.

The data identifies the discrepancy but does not establish its cause. A low ratio alone is insufficient evidence to remove a video or change its reported revenue.

### 7.3 Views and audience acquisition

Views and reported new viewers had a strong positive association:

| Analysis | Pearson | Spearman |
|---|---:|---:|
| All videos | 0.872 | 0.908 |
| Excluding the most-viewed video | 0.918 | 0.907 |

The sensitivity check indicates that the strong association was not dependent on the single most-viewed video.

However, this relationship does not establish causal direction. Both measures also reflect the scale of a video's audience reach.

### 7.4 Comparing associations with views

| Metric compared with views | Pearson | Spearman |
|---|---:|---:|
| Reported new viewers | 0.872 | 0.908 |
| Impressions CTR (%) | 0.122 | 0.389 |
| Average viewing-duration percentage | 0.256 | 0.308 |

Pearson measures linear association, while Spearman measures association between ranked values.

Reported new viewers had the strongest association with views among these measures.

CTR and average viewing-duration percentage showed weaker associations. Neither should be treated as a sufficient standalone explanation for a video's view count.

These are descriptive correlations, not causal estimates or validated predictive models.

### 7.5 Viewing-duration values above 100%

Of the 346 videos, **202** had a calculated average viewing-duration percentage above 100%.

| Statistic | Percentage |
|---|---:|
| Mean | 106.67% |
| Median | 106.02% |
| Minimum | 59.76% |
| Maximum | 244.44% |

The highest value corresponded to a 9-second video with a reported average viewing duration of 22 seconds.

The original source record confirmed those duration values. This check supported the arithmetic and conversion used in the notebook, but did not independently explain the viewing behavior behind the measurement.

## 8. Content Classification

The original analytical data did not contain the thematic categories needed for this comparison.

A classification workflow was therefore added:

1. Export video IDs and titles for review.
2. Assign proposed thematic categories.
3. Identify ambiguous or insufficiently descriptive titles.
4. Generate video URLs to support further review.
5. Use supplied content summaries to resolve additional assignments.
6. Preserve classification notes and review status.
7. Merge the category information into the analytical dataset.

Classification used video titles and supplied summaries. It was not based on independent viewing of every video.

Some assignments remain marked as proposed. A populated category should therefore not be interpreted as proof that the classification has been fully validated.

### Classification coverage

| Status | Videos |
|---|---:|
| Assigned to a content category | 343 |
| Unclassified | 3 |
| Total | 346 |

The three unclassified videos remain in overall totals but are excluded from category comparisons.

The classified videos span **20 content categories**.

## 9. Category Performance

### Why median views?

Total views are useful for measuring a category's contribution to overall reach, but they are also affected by how many videos were published.

Median views provide a comparison of typical performance per video and are less sensitive to unusually high-performing observations than the mean.

However, medians do not remove differences in video age, category size, or other factors. They are interpreted alongside video counts.

### Leading categories overall

| Category | Videos | Median views |
|---|---:|---:|
| Liver & Gallbladder Health | 9 | 347,597 |
| Kidney & Urinary Health | 17 | 279,013 |
| Heart & Circulation | 10 | 246,630 |
| Food, Herbs & Supplements | 112 | 238,851.5 |
| Eye & Ear Health | 10 | 211,791 |

Liver & Gallbladder Health had the highest median views, while Food, Herbs & Supplements provided a substantially larger sample.

These results identify categories worth investigating further. They do not establish that the highest-ranked category will outperform the others in future publications.

### Publication-year comparisons

Category rankings changed when videos were grouped by publication year.

For videos published in 2025:

| Category | Videos | Median views |
|---|---:|---:|
| Eye & Ear Health | 2 | 319,896 |
| Liver & Gallbladder Health | 6 | 317,212.5 |
| Heart & Circulation | 5 | 243,497 |
| Food, Herbs & Supplements | 43 | 189,720 |

The leading category in this cohort had only two videos, which limits how broadly its result can be generalized.

The complete 2025 cohort contained:

- **163 videos**
- **36,261,873 accumulated views**
- **USD 2,064.98 estimated revenue**

These figures describe videos published in 2025. They do not represent views or revenue earned exclusively during calendar year 2025.

Grouping by publication year provides additional context but does not fully equalize the time each video had to accumulate views.

## 10. Recommendations

Based on the observed patterns:

1. **Test additional videos in promising categories.**  
   Liver & Gallbladder Health, Kidney & Urinary Health, and Heart & Circulation are candidates for further testing.

2. **Include a category with a larger historical sample.**  
   Food, Herbs & Supplements provides a useful comparison group, although differences in content and publication timing still need to be considered.

3. **Compare future videos over equal observation windows.**  
   For example, evaluate each video's first 28 days after publication rather than comparing accumulated views across videos of different ages.

4. **Report sample sizes alongside category performance.**  
   A high median based on two videos should be interpreted differently from a median based on more than one hundred videos.

5. **Evaluate several performance dimensions together.**  
   Reach, reported new viewers, viewing duration, and estimated revenue provide complementary information.

6. **Continue reviewing ambiguous classifications.**  
   More consistent labels would improve the reliability of future category comparisons.

These are proposed analytical and content-testing actions, not demonstrated performance improvements.

## 11. Limitations

- **Export coverage:** Video-level totals differ from the source summary total; the difference remains unresolved.
- **Unequal observation periods:** Older videos may have had more time to accumulate views.
- **Unequal category sizes:** Several category-year groups contain very few videos.
- **Classification uncertainty:** Categories rely on titles and supplied summaries, and some remain provisional.
- **Unclassified content:** Three videos are excluded from category comparisons.
- **Estimated revenue:** The revenue field contains estimates rather than independently verified final payments.
- **Audience overlap:** Summing video-level viewer counts does not establish a deduplicated channel audience.
- **Metric interpretation:** Viewing-duration percentage is not a viewer completion rate.
- **Correlation versus causation:** Observed relationships do not identify causal effects.
- **Snapshot analysis:** The project does not track performance at consistent intervals after each video's publication.

## 12. Repository Guide

| Path | Purpose |
|---|---|
| `notebooks/01_data_audit.ipynb` | Data preparation, exploratory analysis, metric calculations, category analysis, and export workflow |
| `notebooks/video_titles_for_review.csv` | Video IDs and titles exported for classification review |
| `data/raw/` | Source data directory |
| `data/processed/video_categories_draft.csv` | Initial classification output |
| `data/processed/video_categories_reviewed.csv` | Classification review output |
| `data/processed/youtube_shorts_tableau.csv` | Prepared video-level dataset used in Tableau |
| `images/` | Directory for project visuals |
| `sql/` | Directory reserved for the planned SQL extension |
| `tableau/` | Directory reserved for Tableau project artifacts |
| `web/` | Directory reserved for the planned portfolio website |

The published Tableau story is accessible through the link at the beginning of this README.

## 13. Tools and Skills Demonstrated

### Tools

- **Excel:** Source data format.
- **Python and pandas:** Data preparation, filtering, joins, aggregation, and export.
- **SciPy:** Support for Spearman correlation calculations.
- **Matplotlib:** Exploratory visualizations.
- **Jupyter Notebook in VS Code:** Analysis and documentation.
- **Tableau Public:** Interactive dashboard and data storytelling.
- **Git and GitHub:** Version control and project publication.

### Analytical skills

- Data auditing and cleaning.
- ETL workflow development.
- Feature and metric engineering.
- Exploratory data analysis.
- Outlier investigation.
- Pearson and Spearman correlation analysis.
- Sensitivity analysis.
- Content classification and review tracking.
- Grouped comparisons using medians and sample sizes.
- Stakeholder communication and documentation of limitations.

## 14. How to Explore the Project

1. Open the Tableau story for an interactive overview of category performance.
2. Read `notebooks/01_data_audit.ipynb` for the analytical process and supporting results.
3. Inspect `data/processed/youtube_shorts_tableau.csv` for the prepared dataset.
4. Review the classification files for additional context about category assignments.

To rerun the analysis, use a Python environment with the notebook's required dependencies and execute the cells in order.

Check the notebook's source-file paths before running it. The raw Excel file must be available at the expected location.

A pinned dependency file and a fully documented reproducibility setup are planned improvements.

## 15. Planned Extensions

- Load the prepared dataset into PostgreSQL.
- Reproduce selected aggregations and validation checks in SQL.
- Document the SQL queries in the repository.
- Add the Tableau story to a portfolio website.
- Improve the reproducibility documentation and dependency setup.
- Collect comparable post-publication observation windows for future analysis.
- Refine the category taxonomy and review remaining uncertain assignments.

## Author

**Marco Antonio Cruz Rojas**

[GitHub](https://github.com/MarkCruz911)  
[Interactive Tableau Story](https://public.tableau.com/app/profile/marco.antonio.cruz.rojas/viz/YouTubeShortsPerformanceAnalysis/YouTubeShortsKeyFindings)