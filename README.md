# Norsk-Tipping-Data-Analysis
R Markdown analysis for a Data Analyst case study (Norsk Tipping): first-week onboarding behavior of new customers. Covers feature engineering, EDA, k-means segmentation, and PCA to identify customer segments and early risk indicators for responsible gambling follow-up.

# New Customer Onboarding First-Week Insights (Norsk Tipping Case Study)

An R Markdown analysis built for a Data Analyst case study (Norsk Tipping), exploring new customers' behavior in their first week of digital play (app/web) to support early, responsible onboarding.

## Case background

> **Case:** Give new customers a good onboarding and  a relevant, entertaining customer experience within safe limits.
>
> The dataset covers new customers in the first week after placing their first digital bet. Norsk Tipping wants to quickly understand customers' interests, needs, and any early signs of risky play, so it can follow up promptly and provide a good onboarding experience, relevant and entertaining content, and safe play limits (problem gambling prevention).
>
> The task: use the dataset to find insight about customers, e.g., identify segments of customers with similar needs, and assess how each segment could be followed up, for example, through tailored CRM communication, personalized in-app content, or other channels.

### Dataset

Anonymized data for new customers over their first 7 days of play, including:
- Row number, gender, age
- Turnover (stake) per play day (1–7), split by casino, sports, lottery, and instant games
- GGR (Gross Gaming Revenue:  Norsk Tipping's net revenue after prizes) per play day, split by the same game types. From the customer's perspective, a negative amount means a net win (prizes exceed stakes); a positive amount means a net loss (stakes exceed prizes).

### Known risk factors (used to guide feature engineering)

- Younger men (18–25) have a higher risk of developing gambling problems
- Men generally have a higher risk than women
- Lottery has a low risk of gambling problems
- Sports betting and instant games carry moderate risk
- Casino games carry high risk
- High spending and high play frequency are associated with gambling problems
- Young men who play in casinos and sports also often play with multiple operators (including foreign operators)

## What the analysis does

1. **Import & cleaning** — reads the anonymized Excel export, translates the original column-naming pattern to English (`PLAYDAY_<day>_<game type>_<TURNOVER|LOSS>`), parses monetary columns with locale-aware number parsing, and drops rows with missing values in key day/game-specific fields.
2. **Feature engineering** — aggregates daily turnover/loss into per-customer totals, breaks turnover down by game type (casino, sports, lottery, instant games), computes each game type's *share* of total turnover, counts active days, and flags early risk indicators (top-decile turnover, high casino share, young male).
3. **Exploratory data analysis** — age and gender distribution, game-type preference summaries, and visualizations: log-scale turnover distribution, game-type share violin plots, age vs. lottery/sports share scatter plots, and casino share by gender/age group.
4. **Customer segmentation** — standardizes key behavioral features, uses the elbow method to select *k*, and runs k-means (k = 4) to produce four segments, profiled and labeled by activity level, casino exposure, age, and gender mix, with an assigned risk level (Low/Moderate/High).
5. **PCA visualization** — projects the clustering features onto two principal components to visualize how the four segments separate in behavioral space.

## Requirements

```r
install.packages(c(
  "tidyverse", "readxl", "janitor", "skimr", "readr",
  "ggplot2", "purrr", "knitr", "kableExtra"
))
```

## Data input

This repository does **not** include the underlying data file. 

## Notes

- `Sys.setlocale("LC_ALL", "nb_NO.UTF-8")` is kept in the setup chunk to correctly parse special characters in the source file before column names are translated to English; remove it if running on a system without the Norwegian locale installed, or if your source file already uses English/ASCII column names.
- Segment labels and risk levels (`Segment_name`, `Risk`) are assigned by inspecting cluster characteristics after running k-means — if you re-run with different data, the cluster numbers may not correspond to the same labels, so review and relabel as needed.
- This is a take-home case study exercise; conclusions and segment definitions reflect this specific dataset and are not official Norsk Tipping findings or policy.
