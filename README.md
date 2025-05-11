# Massachusetts Zip Lens

## Project Description

This project aims to create a data visualization dashboard that analyzes various regional data based on zip codes in Massachusetts, USA. The goal is to assign a comprehensive "City Score" to each zip code, reflecting its overall desirability or suitability based on collected and analyzed variables. The dashboard allows users to interactively explore the data and understand how different factors contribute to the scores.

## Dashboards

* **Massachusetts Zip Code Scores Map:** [MA Zip Lens Dashboard](https://public.tableau.com/views/Project2_17338642634420/Dashboard1?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)
* **Variable Distribution Charts:** [Metrics Dashboard](https://public.tableau.com/views/Project2_17338642634420/MetricsDashboard?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)

## State and Region

* **State:** Massachusetts
* **Country:** USA
* **Scope:** The analysis covers approximately 540 ZIP codes across Massachusetts.

## Data Collection

At least five different variables were collected for each ZIP code. All data was sourced from credible and publicly available databases.

The following metrics were used for the analysis:

* **Total Population:** The total population of the ZIP code area.
* **Median Household Income in $:** The median household income in USD.
* **Bachelors Degree or Higher %:** The percentage of the population with a Bachelor's Degree or higher.
* **Employment Rate %:** The percentage of the employed population.
* **Total Housing Units:** The number of housing units in the ZIP code area.
* **Insured Population %:** The percentage of the population with health insurance.
* **Total Households:** The number of families/households living in the ZIP code area.
* **Number of establishments:** Number of economic and business establishments.
* **Annual payroll ($1000):** Annual payroll amount for the ZIP code area (in thousands of USD).
* **Crime Score per Person (out of 100):** A calculated score representing crime levels, weighted by crime type.

## Score Calculation

A composite "City Score" is calculated for each ZIP code to reflect its overall desirability based on the chosen variables.

### Methodology

1. **Normalization:** All metric columns were normalized to have values between 0 and 1. This ensures that metrics with larger numeric values do not disproportionately influence the score.
   * The formula used for normalization is:
        `Normalized Value = (value - minimum) / (maximum - minimum)`
2. **Crime Score Normalization:** The "Crime Score per Person (out of 100)" was inversely normalized (i.e., `1 - normalized value`) because a lower crime score is generally more desirable.
3. **Weighting System:** Each of the 10 metrics can be assigned a weight from 0 to 10 (floating point values) using sliders on the dashboard. This allows for dynamic control over how much each metric contributes to the "City Score". Setting a weight to 0 for a metric means it is ignored in the "City Score" calculation.
4. **City Score Calculation:** The "City Score" is calculated by multiplying the normalized value of each metric by its assigned weight and then summing these weighted scores.
    `City Score = Σ (Normalized Metric Value * Metric Weight)`
    The final "City Score" is then scaled to a range of 0 to 100.

### Crime Score Calculation Details

The "Crime Score per Person (out of 100)" was calculated based on various crime types, each assigned a specific weight:

| Crime Category                       | Weight |
| :----------------------------------- | :----- |
| Violent crime                        | 0.30   |
| Murder and nonnegligent manslaughter | 0.15   |
| Rape                                 | 0.10   |
| Robbery                              | 0.10   |
| Aggravated assault                   | 0.10   |
| Property crime                       | 0.10   |
| Burglary                             | 0.05   |
| Larceny-theft                        | 0.05   |
| Motor vehicle theft                  | 0.03   |
| Arson                                | 0.02   |

The crime data for each category was first normalized before applying the weights.

## Dashboard Visualization

The dashboard provides an intuitive and user-friendly interface to explore the data:

* **Map Visualization:** A heatmap of Massachusetts ZIP codes, color-coded based on their "City Score". Darker shades indicate higher (more desirable) scores.
* **Variable-specific Charts/Graphs:** Histograms are provided for the different metrics considered in the "City Score" calculation, allowing users to see the distribution of each variable.
* **Interactivity:**
  * **Metric Weight Sliders:** Users can adjust the weights of the 10 metrics to dynamically recalculate and visualize the "City Score" based on their preferences.
  * **Top N Slider:** Users can filter the map to display only the Top N ZIP code areas based on the calculated "City Score".

## Data Processing and Files

The project involves several data processing steps managed primarily through a Jupyter Notebook (`analysis.ipynb`):

1. **Data Loading:** Datasets are loaded from CSV files.
2. **Data Cleaning:** Unnecessary rows and columns are dropped, and column names are standardized.
3. **Data Merging:** Multiple datasets are merged based on ZIP code area names (ZCTA5).
4. **Type Conversion & NA Handling:** Data types are converted as needed (e.g., numeric columns with commas). Missing values in metric columns are handled (e.g., filled with 0 before normalization for score calculation, as seen in the `calculate_city_scores` function in `analysis.ipynb`).
5. **Score Calculation:** As detailed in the "Score Calculation" section, the notebook implements the logic for calculating both the "Crime Score per Person" and the final "City Score".
6. **Output:** The final processed data, including raw metrics, normalized values, and the "City Score" for each ZIP code, is saved to `city_data_final.csv`.

### Key Files

* `analysis.ipynb`: Jupyter Notebook containing the Python code for data collection, cleaning, processing, and score calculation.
* `city_data_final.csv`: The final dataset output by `analysis.ipynb`, used for the dashboard.
* `datasets/`: This directory contains the raw CSV files used for the analysis:
  * `DECENNIALDHC2020.P1-Data.csv` (Population)
  * `ACSST5Y2022.S1901-Data.csv` (Income and Poverty)
  * `ACSST5Y2022.S1501-Data.csv` (Education)
  * `ACSDP5Y2022.DP03-Data.csv` (Employment)
  * `DECENNIALDHC2020.H1-Data.csv` (Housing)
  * `ACSST5Y2022.S2701-Data.csv` (Health - Insured Population)
  * `ACSDP5Y2022.DP02-Data.csv` (Family and Living Arrangements - Total Households)
  * `CBP2021.CB2100CBP-Data.csv` (Business and Economy)
  * `crime_data.csv` (Crime data, pre-processed from FBI source)

## Data Sources & Citations (APA Format)

* Federal Bureau of Investigation. (2019). *Crime in the United States, 2019: Table 8 - Offenses Known to Law Enforcement by State by City, 2019: Massachusetts*. U.S. Department of Justice. Retrieved from [https://ucr.fbi.gov/crime-in-the-u.s/2019/crime-in-the-u.s.-2019/tables/table-8/table-8-state-cuts/massachusetts.xls](https://ucr.fbi.gov/crime-in-the-u.s/2019/crime-in-the-u.s.-2019/tables/table-8/table-8-state-cuts/massachusetts.xls)
* U.S. Census Bureau. (n.d.). *Data from the U.S. Census Bureau for metrics by zip code area in Massachusetts*. Retrieved from [https://data.census.gov/](https://data.census.gov/)
