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
## Assessment of Missingness
## Hypothesis Testing
## Framing a Prediction Problem
## Baseline Model
## Final Model
## Fairness Analysis