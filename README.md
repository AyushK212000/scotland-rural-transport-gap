Scotland's Rural Transport Gap

How long does it take to reach a GP without a car? An analysis of public transport and driving times to essential services across all 6,976 neighbourhoods (data zones) in Scotland, built with Python (pandas, matplotlib) and Tableau.

View the interactive dashboard on Tableau Public

Dashboard

Key findings
#	Question	Finding
1	How does the access gap change from urban to rural areas?	Without a car, reaching a GP takes a median 4.9 extra minutes in large urban areas but 11.0 in remote rural areas. The gap jumps between small towns and rural areas rather than rising steadily.
2	Which councils are most affected?	16.0% of Scotland's population live more than 15 minutes from a GP by public transport. The share is highest in Na h-Eileanan Siar (57.2%), Shetland (54.1%) and Aberdeenshire (45.0%). By number of people, Aberdeenshire (~118,000) is the most affected council.
3	Are deprived rural areas doubly disadvantaged?	Not at neighbourhood level. In rural areas, only 11.7% of people in the most income-deprived zones are more than 15 minutes from a GP, compared with 69.3% in the least deprived. However, 110,110 people live in zones that are both among the 40% most deprived and more than 15 minutes from a GP, concentrated in North Lanarkshire, East Ayrshire, Renfrewshire and South Ayrshire.

Two measures, two stories. Remote rural residents lose more than twice as many extra minutes as large urban residents, yet the ratio of public transport to driving time is slightly lower (2.7× vs 3.0×), because driving is also slow in rural areas. The rural disadvantage is mainly in absolute time: a median 18.4-minute public transport journey to a GP, against 7.5 minutes in large urban areas.

Charts
Gap by rurality	Councils over 15 minutes
Gap by rurality	Councils
Data
Dataset	Source	Level
SIMD 2020v2 indicators (drive and public transport times, population, income deprivation)	Scottish Government	6,976 data zones (2011)
Urban Rural Classification 2020	Scottish Government, via Public Health Scotland open data	6,976 data zones (2011)

Travel times are in minutes and are population-weighted averages within each data zone. Public transport times are available for GP surgeries, post offices and retail centres.

Approach
Exploration (notebooks/01_data_exploration.ipynb): identified the data sheet, checked types, suppressed values and outliers, and confirmed every data zone code matches between the two files.
Cleaning (notebooks/02_data_cleaning.ipynb): selected 14 of 37 columns, converted income deprivation to numeric, joined the urban-rural classification (one-to-one, validated), created gap (minutes) and ratio measures for each service, and flagged unusual values. assert checks confirm row counts, unique zones and complete travel times.
Analysis (notebooks/03_analysis.ipynb): medians by urban-rural class, population-weighted shares by council, and income-deprivation quintiles crossed with rurality.
Dashboard (tableau/): Tableau calculated fields, reference lines and a heat table. Every dashboard figure was checked against the notebook results.

Medians are used throughout because journey times are right-skewed: a few very remote zones pull the mean upwards.

Data quality decisions
Issue	Decision
3 zones have suppressed income data (*)	Kept for travel analysis; excluded only from deprivation comparisons
5 island zones in Orkney and Shetland have public transport to retail at exactly 190 minutes, likely a cap	Kept and flagged (flag_pt_retail_capped); the true time is at least 190 minutes
3 remote zones have public transport faster than driving (largest: Argyll and Bute, Highland), likely where ferry or public transport routes are shorter than the road route	Kept and flagged (flag_negative_gap)
SIMD uses 2011 data zones	Used the 2011 version of the urban-rural lookup; the 2022 version uses different codes

No rows were removed.

Limitations
SIMD 2020v2 travel times reflect services and timetables from a few years before 2020.
The analysis is at neighbourhood level. It cannot show the circumstances of deprived individuals living in less deprived zones, who may be among those most affected by poor public transport.
Travel times do not reflect service frequency, journey cost or accessibility needs.

Possible extension: add council-level car availability from the Scottish Household Survey to examine car dependency.

Repository structure
scotland-rural-transport-gap/
├── data/
│   ├── raw/          # Original SIMD and Urban Rural files
│   └── processed/    # Cleaned data (not committed; created by the notebooks)
├── notebooks/
│   ├── 01_data_exploration.ipynb
│   ├── 02_data_cleaning.ipynb
│   └── 03_analysis.ipynb
├── tableau/          # Tableau workbook
├── images/           # Charts and dashboard screenshot
└── requirements.txt
How to reproduce
Clone the repository and install the libraries:
bash
   pip install -r requirements.txt
Run the notebooks in order (01, 02, 03). Notebook 03 also creates data/processed/tableau_data.csv for the dashboard.
Open the workbook in tableau/ with Tableau Public, or view the published dashboard.

Ayush Kadam · MSc Data Science for Business, University of Stirling

Data: Scottish Government, Scottish Index of Multiple Deprivation 2020v2; Urban Rural Classification 2020. Contains public sector information licensed under the Open Government Licence v3.0.
