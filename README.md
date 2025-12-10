# Power Outages: Regression Analysis of Outage Duration
A public website with my analysis of a power outages dataset.


## Introduction

Power outages impact millions of individuals and businesses every year, having widespread effects such as economic losses, safety risks, and public inconvenience. By analyzing outages, I aim to uncover the factors that lead to longer outages and build predictive models to inform utility repair personnel and policymakers respond more effectively. 

This [dataset](https://engineering.purdue.edu/LASCI/research-data/outages) is accessed from Purdue University's Laboratory for Advancing Sustainable Critical Infrastructure. Throughout this project, I analyzed major power outages witnessed by different states in the continental U.S. between January 2000 and July 2016. A major power outage refers to one that impacted at leaset 50,000 customers or caused an unplanned firm load loss of at least 300 MW. 

The provided **outages** dataset contains 1540 rows and 57 columns by default. 

### Key Dataset Columns

- **YEAR** – Year of the outage.
- **MONTH** – Month of the outage.
- **CLIMATE.REGION** – Regional climate classification.
- **ANOMALY.LEVEL** – Measure of climate anomaly.
- **CLIMATE.CATEGORY** – Broad climate type.
- **CAUSE.CATEGORY** – Primary cause of the outage (e.g., severe weather, equipment failure).
- **OUTAGE.DURATION** – Duration of the outage in hours.
- **DEMAND.LOSS.MW** – Electricity demand lost due to the outage (MW).
- **CUSTOMERS.AFFECTED** – Total customers affected.
- **OUTAGE.START** – Timestamp when the outage began.
- **OUTAGE.RESTORATION** – Timestamp when power was restored.

The question that this project centers around is:
**“Which factors influence the duration of power outages, and can we predict outage length from these features?”**



## Data Cleaning and Exploratory Data Analysis

### Data Cleaning Steps

To prepare the dataset for analysis, I performed the following cleaning steps:

- **Removed initial rows and irrelevant columns**:
The first few rows of the dataset contained metadata rather than actual observations, so I removed them using iloc. I also dropped the first column and many columns related to economic statistics, customer percentages, and sales that were not relevant to outage analysis.

- **Renamed columns and set index**:
I renamed the columns using the first valid row after removing metadata and set "OBS" as the index for clarity.

- **Processed outage timestamps**:
The dataset included separate date and time columns for outage start and restoration. I combined these into single datetime columns, OUTAGE.START and OUTAGE.RESTORATION, using pd.to_datetime, so that the dataframe has proper datetime formatting for future analysis.

- **Dropped redundant date/time columns**:
After creating the combined datetime columns, the original separate date and time columns were dropped to simplify the dataset.

These cleaning steps reduced noise, standardized timestamps, and removed irrelevant information, allowing for more accurate modeling and visualization of outage durations.

<div style="overflow-x:auto;">
  <table>
    <thead>
      <tr>
        <th>YEAR</th>
        <th>MONTH</th>
        <th>U.S._STATE</th>
        <th>POSTAL.CODE</th>
        <th>NERC.REGION</th>
        <th>CLIMATE.REGION</th>
        <th>ANOMALY.LEVEL</th>
        <th>CLIMATE.CATEGORY</th>
        <th>CAUSE.CATEGORY</th>
        <th>CAUSE.CATEGORY.DETAIL</th>
        <th>HURRICANE.NAMES</th>
        <th>OUTAGE.DURATION</th>
        <th>DEMAND.LOSS.MW</th>
        <th>CUSTOMERS.AFFECTED</th>
        <th>RES.CUSTOMERS</th>
        <th>COM.CUSTOMERS</th>
        <th>IND.CUSTOMERS</th>
        <th>TOTAL.CUSTOMERS</th>
        <th>POPULATION</th>
        <th>OUTAGE.START</th>
        <th>OUTAGE.RESTORATION</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>2011</td><td>7</td><td>Minnesota</td><td>MN</td><td>MRO</td><td>East North Central</td><td>-0.3</td><td>normal</td><td>severe weather</td><td>nan</td><td>nan</td><td>3060</td><td>nan</td><td>70000</td><td>2308736</td><td>276286</td><td>10673</td><td>2595696</td><td>5348119</td><td>2011-07-01 17:00:00</td><td>2011-07-03 20:00:00</td>
      </tr>
      <tr>
        <td>2014</td><td>5</td><td>Minnesota</td><td>MN</td><td>MRO</td><td>East North Central</td><td>-0.1</td><td>normal</td><td>intentional attack</td><td>vandalism</td><td>nan</td><td>1</td><td>nan</td><td>nan</td><td>2345860</td><td>284978</td><td>9898</td><td>2640737</td><td>5457125</td><td>2014-05-11 18:38:00</td><td>2014-05-11 18:39:00</td>
      </tr>
      <tr>
        <td>2010</td><td>10</td><td>Minnesota</td><td>MN</td><td>MRO</td><td>East North Central</td><td>-1.5</td><td>cold</td><td>severe weather</td><td>heavy wind</td><td>nan</td><td>3000</td><td>nan</td><td>70000</td><td>2300291</td><td>276463</td><td>10150</td><td>2586905</td><td>5310903</td><td>2010-10-26 20:00:00</td><td>2010-10-28 22:00:00</td>
      </tr>
      <tr>
        <td>2012</td><td>6</td><td>Minnesota</td><td>MN</td><td>MRO</td><td>East North Central</td><td>-0.1</td><td>normal</td><td>severe weather</td><td>thunderstorm</td><td>nan</td><td>2550</td><td>nan</td><td>68200</td><td>2317336</td><td>278466</td><td>11010</td><td>2606813</td><td>5380443</td><td>2012-06-19 04:30:00</td><td>2012-06-20 23:00:00</td>
      </tr>
      <tr>
        <td>2015</td><td>7</td><td>Minnesota</td><td>MN</td><td>MRO</td><td>East North Central</td><td>1.2</td><td>warm</td><td>severe weather</td><td>nan</td><td>nan</td><td>1740</td><td>250</td><td>250000</td><td>2374674</td><td>289044</td><td>9812</td><td>2673531</td><td>5489594</td><td>2015-07-18 02:00:00</td><td>2015-07-19 07:00:00</td>
      </tr>
    </tbody>
  </table>
</div>


### Univariate Analysis
<iframe
  src="assets/outageshistogram.html"
  width="650"
  height="400"
  frameborder="0"
></iframe>

The histogram shows the distribution of outage durations across all recorded events. Most outages are relatively short, but there are a few extreme events with much longer durations, indicating a right-skewed distribution.

### Bivariate Analysis
<iframe
  src="assets/outagesboxplot.html"
  width="650"
  height="400"
  frameborder="0"
></iframe>

The box plot displays how outage duration varies by cause category. Fuel supply emergency and severe weather tend to result in longer outages, while equipment failures and islanding usually have shorter durations. The IQR for fuel supply emergencies is considerably larger than other causes, reflecting greater variability in the data.


### Interesting Aggregate

| CLIMATE.REGION     |   OUTAGE.DURATION |
|:-------------------|------------------:|
| Central            |          2701.13  |
| East North Central |          5448.26  |
| Northeast          |          2991.66  |
| Northwest          |          1284.5   |
| South              |          2846.1   |
| Southeast          |          2217.69  |
| Southwest          |          1566.14  |
| West               |          1628.33  |
| West North Central |           696.562 |

The pivot table summarizes average outage duration by climate region. It highlights that some regions, like East North Central, experience longer outages on average, which can guide utilities in allocating resources and preparing contingency plans for high-risk areas. On the other hand, regions like Northwest and West North Central have lower outage durations, which could either reflect a lack of appropriate coverage, a lack of potential outage causes (severe weather, equipment failure, etc) , or robust power systems in those regions.

## Assessment of Missingness
## Hypothesis Testing
## Framing a Prediction Problem
## Baseline Model
## Final Model
## Fairness Analysis