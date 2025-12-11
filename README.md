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

These cleaning steps reduced noise, standardized timestamps, and removed irrelevant information, allowing for more accurate modeling, calculations, and visualization of outage durations.

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

Missing hurricane names are an example of NMAR (Not Missing At Random) because the missingness depends on the true classification of the storm. Storms that never reached naming intensity, dissipated early, or were not officially recorded will have missing names because the underlying true status itself caused the missingness. Since this mechanism depends on unobserved characteristics rather than other observed variables, the missingness is NMAR.

### Missingness Permutation Test

**Null hypothesis (H₀)**:
The missingness of CAUSE.CATEGORY.DETAIL is independent of the observed columns being tested.

**Alternative hypothesis (H₁)**:
The missingness of CAUSE.CATEGORY.DETAIL is associated with the observed column.

The permutation tests calculated the difference in mean missingness across groups for each observed column and compared it to a null distribution generated by random shuffling. Columns with p-values below 0.05 indicate a statistically significant relationship with missingness, as seen below.

| Column           | p-value (approx)|
|-----------------|---------|
| YEAR            | 0.001   |
| MONTH           | 0.001   |
| CLIMATE.REGION  | 0.016   |
| ANOMALY.LEVEL   | 0.045   |
| CLIMATE.CATEGORY| 0.031   |
| CAUSE.CATEGORY  | 0.001   |

The missingness of CAUSE.CATEGORY.DETAIL is strongly associated with the values of the columns in the table above. Since there is a relationship between CAUSE.CATEGORY.DETAIL's missingness and other columns, the former is MAR by definition. In context, this likely means that outages in certain years and months have systematically more missing detailed causes.

<div style="overflow-x: auto; width: 100%;">
  <iframe
    src="assets/detailmissingnesshist.html"
    width="1000"
    height="500"
    frameborder="0"
  ></iframe>
</div>

The histogram shows the distribution of CAUSE.CATEGORY for outages where the detailed cause is missing versus where it is recorded.
The orange bars show the values for outages with missing cause details, while the blue bars show the values for outages where the cause is recorded.

## Hypothesis Testing

### Mean Outage Duration

I am testing whether outages in our dataset tend to last longer than a full day.

**Null hypothesis (H₀)**: The mean outage duration is 24 hours. 

**Alternative hypothesis (H₁)**: The mean outage duration is greater than 24 hours.

I used a one-sample t-test because we are comparing the sample mean to a fixed benchmark and the population standard deviation is unknown. Additionally, this is a directional test.

The resulting test statistic was ~7.661, while the p-value is **1.66 × 10⁻¹⁴**, which is far below the significance level of **α = 0.05**. Thus, we reject the null hypothesis, meaning there is statistically significant evidence that the mean outage duration is greater than 24 hours.

## Framing a Prediction Problem

My model will try to predict OUTAGE.DURATION, the length of time (in minutes) that a major power outage lasts. This is a regression problem, since the value we are predicting is continuous. I chose this variable because outage duration is one of the most important operational outcomes for utilities and policymakers. Accurately predicting how long an outage will last can help improve planning, resource allocation, and emergency response.

To evaluate model performance, I am using the R² score. R² is appropriate for this task because it measures how much of the variability in outage duration is explained by the model. I chose it over regression metrics such as RMSE or MAE because R² directly reflects explanatory power rather than error magnitude, which makes it better for understanding how much useful predictive structure exists in the data.

Since the **outages** dataframe has other correlated variables, such as ANOMALY.LEVEL, CAUSE.CATEGORY, etc, we can reasonably use them to predict how long the outage will last. Additionally, I only train my model using features that would be known prior to or at the start of the outage, ensuring that no future information plays an unfair role in the prediction process.

## Baseline Model

I built a regression model to predict OUTAGE.DURATION, the length of power outages in hours.

The features in my model are:

- **CAUSE.CATEGORY (nominal)**: This describes the general cause of the outage, such as severe weather or intentional attacks. Because it is categorical, I applied one-hot encoding to convert it into numerical features.

- **ANOMALY.LEVEL (quantitative)**: This numeric feature captures the severity of the climate anomaly based on the ONI index, where values typically range between -2 and 2. A higher absolute value of anomaly level indicates a stronger intensity level. Since it's already numerical, I just passed it through without changing it.

I used a ColumnTransformer to handle preprocessing and included it in a Pipeline with a Linear Regression model. This is so that the categorical encoding is applied consistently during training and prediction.

On the test set, the model achieved an R² score of approximately 0.2597. This relatively low value indicates that while the model captures some variation in outage duration, much of the variability is unexplained. Therefore, the current model provides a basic baseline, but its predictive performance is limited and could likely be improved with additional features or more complex modeling approaches.

## Final Model

My final model predicts LOG.OUTAGE.DURATION using the features: ANOMALY.LEVEL, DEMAND.LOSS.MW, CUSTOMER.RATIO, CAUSE.CATEGORY, START.HOUR, and START.DAYOFWEEK. I selected these features because each has a correlation to the outage process:

- **ANOMALY.LEVEL (quantitative)**: Measures the severity of the climate anomaly, which can influence outage duration by making restoration more difficult.

- **DEMAND.LOSS.MW (quantitative)**: Represents the electricity demand lost during the outage, providing a sense of the outage’s scale.

- **CUSTOMER.RATIO (quantitative)**: The ratio of customers affected to population normalizes the impact across regions of different sizes, helping the model account for population density effects.

- **CAUSE.CATEGORY (nominal)**: Categorical cause of the outage, such as severe weather or intentional attacks, is key in distinguishing the underlying reasons that lead to different outage durations.

- **START.HOUR (quantitative) and START.DAYOFWEEK (quantitative)**: Capture temporal patterns in outage response and restoration, since outages occurring at night or on weekends may take longer to fix.

These features are available at the start of an outage and thus are valid predictors.

Before modeling, I applied a logarithmic transformation to OUTAGE.DURATION to reduce skewness in the distribution. This helps the model better capture relationships between features and outage duration, particularly for very long outages.

I used an XGBoost Regressor for the final model because it can capture nonlinear relationships and interactions between features, which are common in outage data. To optimize model performance, I applied GridSearchCV with 5-fold cross-validation over a hyperparameter grid for:

- n_estimators [200, 300]: The number of trees. I tested multiple values to ensure the model had enough trees to capture patterns but not so many that it overfits.

- max_depth [3, 5, 8]: The depth of each tree. Shallower trees reduce overfitting, while deeper trees allow the model to capture more complex relationships.

- learning_rate [0.01, 0.02, 0.1]: Determines how much each tree contributes. I experimented with small rates to ensure gradual learning and avoid overshooting minima.

- subsample [0.8, 1]: Controls the fraction of samples used for each tree. Using 0.8 introduces randomness to improve generalization.

- colsample_bytree [0.8, 1]: Controls the fraction of features used for each tree. Tuning this helps prevent overfitting on features that were more correlated, like CUSTOMER.RATIO and DEMAND.LOSS.MW.

By trying multiple values for each hyperparameter, the grid search allowed me to observe how different combinations affected cross-validated performance.

**The best hyperparameters found were:**
- model__colsample_bytree: 0.8
- model__learning_rate: 0.01
- model__max_depth: 5
- model__n_estimators: 300
- model__subsample: 0.8

On the training set, the model achieved **R² = 0.401**, while on the test set it achieved **R² = 0.548**, a substantial improvement over the baseline linear regression model (R² ≈ 0.259). This improvement happened due to the model’s ability to capture nonlinear relationships and interactions between features. Specifically, the effects of outage cause, temporal factors, and population-adjusted customer impact.

Overall, the final XGBoost model provides a more accurate and nuanced prediction of outage duration than the baseline model, while remaining interpretable in terms of the features driving the predictions.

## Fairness Analysis

Fairness Analysis: Severe Weather vs. Non-Weather Outages

**Groups:**
- **Group X (severe weather)**: Outages where CAUSE.CATEGORY is "severe weather"
- **Group Y (non-weather)**: Outages caused by all other categories

I chose these groups because the cause of an outage can influence its duration, and we want to ensure the model predicts OUTAGE.DURATION equally well across different causes. This is important for reliability and fairness in operational decision-making.

**Evaluation Metric:**

- RMSE (Root Mean Squared Error), since the target is continuous. RMSE measures how closely the model’s predicted durations match the true durations and allows us to compare prediction performance across groups.

**Null Hypothesis (H0):**
The model predicts outage durations equally well for severe weather and non-weather outages; any difference in RMSE is due to random chance.

**Alternative Hypothesis (H1):**
The model predicts outage durations differently for severe weather versus non-weather outages; the difference in RMSE is larger than would be expected by chance.

**Test Statistic:**
T_obs = RMSE_weather - RMSE_nonweather

A positive or negative value indicates whether the model performs better on one group relative to the other.

**Permutation Test:**
- I shuffled the WEATHER labels 5,000 times and computed the RMSE difference for each shuffle to generate an empirical null distribution.
- Significance Level: α = 0.05

**Results:**
- Observed RMSE difference: -0.315
- p-value: 0.9306

**Conclusion:**
Since the p-value is much greater than 0.05, we fail to reject the null hypothesis. This indicates there is no statistically significant difference in model performance between severe weather and non-weather outages. Essentially, the model predicts outage duration fairly across these two groups.