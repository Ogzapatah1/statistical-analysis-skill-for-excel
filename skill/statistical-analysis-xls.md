---
name: statistical-analysis-xls
description: Excel-first statistical analysis for business users. Use for descriptive analysis, trend analysis, outlier detection, hypothesis testing, correlation analysis, and continuous-outcome linear regression with Excel-ready business and technical outputs.
user-invocable: true
---

# Statistical Analysis Skill

Descriptive statistics, trend analysis, outlier detection, hypothesis testing, and guidance on when to be cautious about statistical claims.

## Descriptive Statistics Methodology

### Central Tendency

Choose the right measure of center based on the data:

| Situation | Use | Why |
|---|---|---|
| Symmetric distribution, no outliers | Mean | Most efficient estimator |
| Skewed distribution | Median | Robust to outliers |
| Categorical or ordinal data | Mode | Only option for non-numeric |
| Highly skewed with outliers (e.g., revenue per user) | Median + mean | Report both; the gap shows skew |

**Always report mean and median together for business metrics.** If they diverge significantly, the data is skewed and the mean alone is misleading.

### Spread and Variability

- **Standard deviation**: How far values typically fall from the mean. Use with normally distributed data.
- **Interquartile range (IQR)**: Distance from p25 to p75. Robust to outliers. Use with skewed data.
- **Coefficient of variation (CV)**: StdDev / Mean. Use to compare variability across metrics with different scales.
- **Range**: Max minus min. Sensitive to outliers but gives a quick sense of data extent.

### Percentiles for Business Context

Report key percentiles to tell a richer story than mean alone:

```
p1:   Bottom 1% (floor / minimum typical value)
p5:   Low end of normal range
p25:  First quartile
p50:  Median (typical user)
p75:  Third quartile
p90:  Top 10% / power users
p95:  High end of normal range
p99:  Top 1% / extreme users
```

**Example narrative**: "The median session duration is 4.2 minutes, but the top 10% of users spend over 22 minutes per session, pulling the mean up to 7.8 minutes."

### Describing Distributions

Characterize every numeric distribution you analyze:

- **Shape**: Normal, right-skewed, left-skewed, bimodal, uniform, heavy-tailed
- **Center**: Mean and median (and the gap between them)
- **Spread**: Standard deviation or IQR
- **Outliers**: How many and how extreme
- **Bounds**: Is there a natural floor (zero) or ceiling (100%)?

## Trend Analysis and Forecasting

### Identifying Trends

**Moving averages** to smooth noise:
```python
# 7-day moving average (good for daily data with weekly seasonality)
df['ma_7d'] = df['metric'].rolling(window=7, min_periods=1).mean()

# 28-day moving average (smooths weekly AND monthly patterns)
df['ma_28d'] = df['metric'].rolling(window=28, min_periods=1).mean()
```

**Period-over-period comparison**:
- Week-over-week (WoW): Compare to same day last week
- Month-over-month (MoM): Compare to same month prior
- Year-over-year (YoY): Gold standard for seasonal businesses
- Same-day-last-year: Compare specific calendar day

**Growth rates**:
```
Simple growth: (current - previous) / previous
CAGR: (ending / beginning) ^ (1 / years) - 1
Log growth: ln(current / previous)  -- better for volatile series
```

### Seasonality Detection

Check for periodic patterns:
1. Plot the raw time series -- visual inspection first
2. Compute day-of-week averages: is there a clear weekly pattern?
3. Compute month-of-year averages: is there an annual cycle?
4. When comparing periods, always use YoY or same-period comparisons to avoid conflating trend with seasonality

### Forecasting (Simple Methods)

For business analysts (not data scientists), use straightforward methods:

- **Naive forecast**: Tomorrow = today. Use as a baseline.
- **Seasonal naive**: Tomorrow = same day last week/year.
- **Linear trend**: Fit a line to historical data. Only for clearly linear trends.
- **Moving average forecast**: Use trailing average as the forecast.

**Always communicate uncertainty**. Provide a range, not a point estimate:
- "We expect 10K-12K signups next month based on the 3-month trend"
- NOT "We will get exactly 11,234 signups next month"

**When to escalate to a data scientist**: Non-linear trends, multiple seasonalities, external factors (marketing spend, holidays), or when forecast accuracy matters for resource allocation.

## Outlier and Anomaly Detection

### Statistical Methods

**Z-score method** (for normally distributed data):
```python
z_scores = (df['value'] - df['value'].mean()) / df['value'].std()
outliers = df[abs(z_scores) > 3]  # More than 3 standard deviations
```

**IQR method** (robust to non-normal distributions):
```python
Q1 = df['value'].quantile(0.25)
Q3 = df['value'].quantile(0.75)
IQR = Q3 - Q1
lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR
outliers = df[(df['value'] < lower_bound) | (df['value'] > upper_bound)]
```

**Percentile method** (simplest):
```python
outliers = df[(df['value'] < df['value'].quantile(0.01)) |
              (df['value'] > df['value'].quantile(0.99))]
```

### Handling Outliers

Do NOT automatically remove outliers. Instead:

1. **Investigate**: Is this a data error, a genuine extreme value, or a different population?
2. **Data errors**: Fix or remove (e.g., negative ages, timestamps in year 1970)
3. **Genuine extremes**: Keep them but consider using robust statistics (median instead of mean)
4. **Different population**: Segment them out for separate analysis (e.g., enterprise vs. SMB customers)

**Report what you did**: "We excluded 47 records (0.3%) with transaction amounts >$50K, which represent bulk enterprise orders analyzed separately."

### Time Series Anomaly Detection

For detecting unusual values in a time series:

1. Compute expected value (moving average or same-period-last-year)
2. Compute deviation from expected
3. Flag deviations beyond a threshold (typically 2-3 standard deviations of the residuals)
4. Distinguish between point anomalies (single unusual value) and change points (sustained shift)

## Hypothesis Testing

### When to Use

Use hypothesis testing when you need to determine whether an observed difference is likely real or could be due to random chance. Common scenarios:

- A/B test results: Is variant B actually better than A?
- Before/after comparison: Did a change actually move the metric?
- Segment comparison: Do two groups genuinely behave differently?

---

### Step 1 — Interpret the Business Question First

Before selecting a test or asking the user anything, read the question carefully and attempt to define:

1. **What is being compared?** (two means, two proportions, paired measurements, multiple groups, two categories)
2. **What is the null hypothesis (H0)?** Default: there is no difference between the groups or conditions.
3. **What is the alternative hypothesis (H1)?** What outcome would constitute evidence of a real effect?
4. **Is the hypothesis directional?**
   - "Is B *better* than A?" or "Did metric *increase*?" → one-tailed test
   - "Is there a *difference* between A and B?" → two-tailed test
5. **Are the observations independent or paired?** (independent groups vs. before/after on the same entities)

**Always state your interpretation explicitly before proceeding**, even if you are confident:

> "I'm reading your question as: H0 = there is no difference in conversion rates between A and B; H1 = variant B has a higher conversion rate (one-tailed). Is that correct?"

This surfaces wrong assumptions early and keeps the user in control of the analysis.

---

### Step 2 — Ask Clarifying Questions When Needed

Ask questions only when you genuinely cannot determine something from context. Never ask all questions at once — prioritize the most critical unresolved item first.

**Ask about directionality if ambiguous:**
> "Are you asking whether B is specifically *better* than A, or just whether they're *different* in either direction?"

**Ask about data structure if unclear:**
> "Is each row in your data a separate person/entity (independent groups), or are you comparing two measurements on the same entity over time (paired)?"

**Ask about the metric type if not obvious:**
> "Is the outcome you're testing a continuous number (like revenue or session time), a proportion (like conversion rate), or a category (like plan type)?"

**Ask about sample size if not provided and it matters for test choice:**
> "Roughly how many observations are in each group? This affects which test is most appropriate."

**Do not ask** about significance level unless the user raises it — default to alpha = 0.05 and state that assumption.

---

### Step 3 — Check Assumptions Before Selecting a Test

Each test has assumptions. Verify them before running anything, or flag them explicitly if you cannot verify:

| Assumption | How to Check | If Violated |
|---|---|---|
| Normality (for t-test) | n ≥ 30 (Central Limit Theorem applies) or visual inspection | Use Mann-Whitney U instead |
| Equal variances (independent t-test) | Compare standard deviations; if one is >2× the other, flag it | Use Welch's t-test (unequal variances) |
| Independence of observations | Each row is a distinct entity, not repeated measurement | Use paired t-test or mixed methods |
| Expected cell frequencies ≥ 5 (chi-squared) | Check cell counts in the contingency table | Merge categories or use Fisher's exact test |
| Minimum events per group (proportions) | At least 30 events per group | Note low power; interpret with caution |

If you cannot verify an assumption from the data provided, say so:
> "I'm assuming the data meets the normality requirement, but if your sample is small (n < 30) or the distribution is heavily skewed, the t-test result may be unreliable."

---

### Step 4 — Select and Apply the Test

| Scenario | Test | Key Condition |
|---|---|---|
| Compare two group means | Independent t-test | Normal data or n ≥ 30, two separate groups |
| Compare two group means (unequal variance) | Welch's t-test | When standard deviations differ substantially |
| Compare paired measurements | Paired t-test | Before/after on same entities |
| Compare two group proportions | z-test for proportions | Conversion rates, binary outcomes |
| Compare 3+ group means | ANOVA | Multiple segments or variants |
| Non-normal data, two groups | Mann-Whitney U | Skewed metrics, ordinal data, small n |
| Association between two categories | Chi-squared test | Both variables are categorical |

**If the user wants to calculate inside Excel**, map the test to the appropriate function:

| Test | Excel Function | Notes |
|---|---|---|
| Independent t-test (equal var) | `T.TEST(array1, array2, tails, 2)` | tails: 1 = one-tailed, 2 = two-tailed |
| Independent t-test (unequal var / Welch's) | `T.TEST(array1, array2, tails, 3)` | Type 3 = unequal variances |
| Paired t-test | `T.TEST(array1, array2, tails, 1)` | Type 1 = paired |
| z-test for proportions | `Z.TEST(array, x, sigma)` | |
| F-test (check variance equality first) | `F.TEST(array1, array2)` | Run before t-test to decide type 2 vs 3 |
| Chi-squared | `CHISQ.TEST(actual_range, expected_range)` | |

Only suggest these functions if the user is working with data inside a spreadsheet and wants to compute results there. If they are sharing results, asking for interpretation, or designing a study, stay in plain language.

---

### Step 5 — Interpret the Results

**Report all three together — never p-value alone:**

1. **p-value vs. alpha**: Is the result statistically significant?
   - p < 0.05 → reject H0 — evidence of a real difference
   - p ≥ 0.05 → fail to reject H0 — insufficient evidence (not the same as "no difference")

2. **Effect size**: How large is the difference in practical terms?
   - For means: report the absolute difference and Cohen's d if possible
   - For proportions: report the absolute lift AND relative lift (e.g., "+1.2pp, a 15% relative improvement")
   - A significant result with a tiny effect size may not justify action

3. **Confidence interval**: What is the plausible range of the true effect?
   - "The true conversion lift is likely between 0.4pp and 2.1pp (95% CI)"

**If the result is not significant**, do not simply say "there is no difference." Distinguish between two very different situations:

- **Truly no effect**: Supported when the confidence interval is narrow and centered near zero
- **Underpowered test**: Likely when the sample is small and the confidence interval is wide
  > "This result is not significant, but with only 180 observations per group, we had power to detect differences larger than ~5%. The test cannot rule out a real but smaller effect."

### Step 6 — Give the final resullts. Here is better to include them in Excel:
1) State the Business question
2) Give the hypothesis test used and explain why you use it.
3)  State H0 and H1
4) check assumptions, report p-value, effect size
5) tell your Decision (Accept or Reject H0)
6) Give a business interpretation of the results
7) Give a Business recommendation based on the results

### Practical Significance vs. Statistical Significance

**Statistical significance** means the difference is unlikely due to chance.

**Practical significance** means the difference is large enough to matter for business decisions.

A difference can be statistically significant but practically meaningless (common with large samples). Always report:
- **Effect size**: How big is the difference? (e.g., "Variant B improved conversion by 0.3 percentage points")
- **Confidence interval**: What's the range of plausible true effects?
- **Business impact**: What does this translate to in revenue, users, or other business terms?

### Sample Size Considerations

- Small samples produce unreliable results, even with significant p-values
- Rule of thumb for proportions: Need at least 30 events per group for basic reliability
- For detecting small effects (e.g., 1% conversion rate change), you may need thousands of observations per group
- If your sample is small, say so: "With only 200 observations per group, we have limited power to detect effects smaller than X%"

## Correlation Analysis

### When to Use

Use correlation analysis when you need to understand whether two variables move together.

Common business scenarios:

- Marketing spend vs. sales: Do higher campaign investments tend to be associated with higher revenue?
- Customer satisfaction vs. retention: Are happier customers more likely to stay?
- Discount rate vs. margin: Do higher discounts tend to reduce profitability?
- Support tickets vs. churn: Are customers with more support issues more likely to leave?
- Website traffic vs. conversions: Do traffic increases move together with conversion volume?

Correlation is useful for identifying relationships, patterns, and possible business drivers. However, correlation does **not** prove causation. A correlation can suggest where to investigate, but it does not prove that one variable causes the other.

---

### Step 1 - Interpret the Business Question First

Before calculating a correlation, read the question carefully and define:

1. **What two variables are being compared?**
   - Example: marketing spend and sales revenue
   - Example: satisfaction score and retention rate

2. **What is the business question?**
   - "Do these two metrics move together?"
   - "Is one metric associated with another?"
   - "Is this relationship strong enough to investigate further?"

3. **What kind of relationship is expected?**
   - Positive: as X increases, Y tends to increase
   - Negative: as X increases, Y tends to decrease
   - No clear relationship

4. **Is the goal exploration or decision support?**
   - Exploration: looking for possible relationships
   - Decision support: using the relationship to guide business action

**Always state your interpretation before calculating**, especially if the business question could be misunderstood.

Example:

> "I'm reading your question as: you want to know whether higher marketing spend is associated with higher sales revenue. I will treat this as a correlation question first, not proof that marketing spend causes sales."

---

### Step 2 - Ask Clarifying Questions When Needed

Ask questions only when the missing information materially affects the analysis.

**Ask about the variables if unclear:**

> "Which two numeric columns should I compare?"

**Ask about time alignment if using time series data:**

> "Should I compare marketing spend and sales in the same period, or should sales be lagged by one or more periods?"

**Ask about segmentation if the data mixes different populations:**

> "Should I calculate the correlation overall, or separately by segment, region, or product line?"

**Ask about the business goal if unclear:**

> "Are you trying to explore whether a relationship exists, or are you trying to use one variable to predict another?"

Do not ask unnecessary statistical questions upfront. If the user is working in Excel, prioritize the business question and the columns involved.

---

### Step 3 - Check the Data Before Calculating

Before reporting a correlation, check for data issues that can distort the result:

| Check | Why It Matters | What to Do |
|---|---|---|
| Numeric variables | Correlation requires numeric or ordinal inputs | Confirm both variables are appropriate |
| Missing values | Blanks can distort or reduce the usable sample | Report how many rows were excluded |
| Outliers | A few extreme points can create or hide a relationship | Flag outliers and consider correlation with and without them |
| Nonlinear pattern | Correlation may miss curved relationships | Use a scatterplot before relying on the coefficient |
| Mixed segments | Different groups can create misleading overall correlation | Check by segment when relevant |
| Time effects | Trends over time can create spurious correlation | Consider lagging, detrending, or segmenting by period |

**Always recommend a scatterplot** when possible. Visual inspection should come before over-interpreting the correlation coefficient.

Example:

> "Before relying on the correlation, I would review a scatterplot because one or two large enterprise accounts could be driving the relationship."

---

### Step 4 - Select the Correlation Method

Choose the correlation method based on the data and business question:

| Scenario | Method | When to Use |
|---|---|---|
| Two numeric variables with roughly linear relationship | Pearson correlation | Standard correlation for continuous metrics |
| Ordinal data, rankings, skewed data, or monotonic but non-linear relationship | Spearman correlation | Better when the relationship is based on rank/order |
| Binary variable vs. numeric variable | Point-biserial correlation or group comparison | Use caution; a t-test or regression may be clearer |
| Two categorical variables | Not correlation; use chi-squared test | Use categorical association methods instead |
| Relationship intended for prediction | Regression may be more appropriate | Correlation only measures association |

Use **Pearson correlation** as the default only when both variables are numeric and the relationship appears roughly linear.

Use **Spearman correlation** when the data is ordinal, heavily skewed, contains major outliers, or the relationship is monotonic but not linear.

---

### Step 5 - Excel Functions and Workflow

If the user wants to calculate correlation inside Excel, suggest spreadsheet-friendly methods.

| Task | Excel Function / Tool | Notes |
|---|---|---|
| Pearson correlation | `CORREL(array1, array2)` | Most common Excel correlation function |
| Pearson correlation | `PEARSON(array1, array2)` | Similar purpose to `CORREL` |
| R-squared from correlation | `RSQ(known_y's, known_x's)` | Shows proportion of variance explained in simple linear relationship |
| Scatterplot | Insert -> Scatter Chart | Use before interpreting the coefficient |
| Trendline | Add Trendline | Helpful for visualizing direction and approximate fit |

Only provide formulas when the user is calculating in Excel. If the user is asking for interpretation, stay focused on business meaning.

Example:

```excel
=CORREL(B2:B101, C2:C101)
```

Interpretation guide:

| Correlation Coefficient | General Interpretation |
|---|---|
| Near 0 | Little or no linear relationship |
| 0.1 to 0.3 or -0.1 to -0.3 | Weak relationship |
| 0.3 to 0.6 or -0.3 to -0.6 | Moderate relationship |
| 0.6 to 0.8 or -0.6 to -0.8 | Strong relationship |
| Above 0.8 or below -0.8 | Very strong relationship |

Do not treat these thresholds mechanically. Business context matters.

---

### Step 6 - Interpret the Results

When interpreting correlation, report:

1. **Direction**
   - Positive: both variables tend to increase together
   - Negative: one variable tends to decrease as the other increases

2. **Strength**
   - Weak, moderate, strong, or very strong association

3. **Practical meaning**
   - Does the relationship matter for the business?
   - Is the size meaningful enough to investigate or act on?

4. **Limitations**
   - Correlation does not prove causation
   - Outliers may be influencing the result
   - Segment mix may be hiding or creating the relationship
   - Time trends may create misleading correlation

5. **Recommended next step**
   - Segment the data
   - Check a scatterplot
   - Investigate outliers
   - Run regression if prediction or driver analysis is needed
   - Design an experiment if causation matters

Example:

> "The correlation between marketing spend and sales is 0.68, which suggests a strong positive relationship. However, this does not prove that marketing spend caused the sales increase. I would check whether the relationship holds by region and whether large campaigns or seasonal effects are driving the result."

---

### Step 7 - Give the Final Results in a Structured Output

When presenting correlation results, use this structure:

1. **Business question**
   - State the question being analyzed.

2. **Variables compared**
   - Identify the two variables and their meaning.

3. **Method used**
   - State whether Pearson, Spearman, or another method was used.
   - Explain why that method fits the data.

4. **Data and assumption checks**
   - Mention missing values, outliers, linearity, segmentation, and whether a scatterplot was reviewed or recommended.

5. **Correlation result**
   - Report the correlation coefficient.
   - State the direction and strength of the relationship.

6. **Business interpretation**
   - Explain what the relationship means in plain business language.

7. **Caution**
   - Explicitly state that correlation does not prove causation.
   - Mention any major limitations.

8. **Business recommendation**
   - Recommend the next analytical or business step.

Example output:

> **Business question:** Is higher marketing spend associated with higher sales?
>
> **Variables compared:** Marketing spend and sales revenue.
>
> **Method used:** Pearson correlation, because both variables are numeric and the relationship appears approximately linear.
>
> **Data checks:** A scatterplot should be reviewed to confirm that the result is not driven by outliers or seasonality.
>
> **Correlation result:** r = 0.68, indicating a strong positive association.
>
> **Business interpretation:** Periods with higher marketing spend tend to also have higher sales.
>
> **Caution:** This does not prove that marketing spend caused the sales increase. Seasonality, promotions, or product mix may also explain the relationship.
>
> **Recommendation:** Segment the analysis by campaign type or region, then consider regression or an experiment if the goal is to estimate marketing impact.

---

### Correlation vs. Regression

Use correlation when the goal is to measure whether two variables move together.

Use regression when the goal is to estimate, explain, or predict one variable based on another variable.

| Question | Better Method |
|---|---|
| "Do these two metrics move together?" | Correlation |
| "How strong is the relationship between X and Y?" | Correlation |
| "How much does Y change when X increases?" | Regression |
| "Can we predict Y using X?" | Regression |
| "Which variables explain changes in Y?" | Regression |

If the user asks for prediction, driver analysis, or impact size, correlation may be only the first step. Recommend regression as the next method.

---

## Regression Analysis

### When to Use

Use regression when the business question is about estimating, explaining, or predicting a **continuous numeric outcome**.

Common business scenarios:

- How much are sales expected to change when marketing spend changes?
- Which factors are associated with customer lifetime value?
- Can revenue be predicted using price, promotion, and seasonality?
- What sales range should be expected under a proposed business scenario?

Use correlation when the question is only whether two variables move together.

Do not use ordinary linear regression when:

- The outcome is binary, such as churned/not churned or converted/not converted. Recommend logistic regression instead.
- The outcome is a count, such as number of support tickets. Flag that a count model may be more appropriate.
- The data is sequential over time and forecasting is the goal. Check time-series structure before using ordinary regression.
- The user is trying to prove causation. Regression can show conditional association, but it does not prove that one variable caused another.

For this first regression feature, support simple and multiple linear regression with a continuous numeric outcome. Support numeric predictors and already-coded binary predictors. Do not automatically create dummy variables for multi-category predictors.

---

### Step 1 - Interpret the Business Question First

Before selecting a model, define:

1. **Business question**
   - Is the goal prediction, scenario planning, explanation, or driver analysis?

2. **Outcome variable (Y)**
   - What continuous numeric measure is being estimated or predicted?

3. **Predictor variables (X)**
   - Which variables may be associated with Y?

4. **Unit and time period**
   - What does one row represent: customer, product, region, store, campaign, or month?
   - Does each row represent the same time period and level of detail?

5. **Claim level**
   - Use association language unless the analysis is based on an appropriate causal design.

Always state the interpretation before calculating:

> "I am reading this as a prediction and driver-analysis question: estimate monthly sales revenue using marketing spend, average discount, and seasonality. I will interpret coefficients as associations while holding the other included variables constant, not as proof of causation."

---

### Step 2 - Ask Targeted Clarifying Questions

Ask only when the missing information materially affects model choice or interpretation.

**Ask about the outcome if unclear:**

> "Which numeric column should the model predict?"

**Ask about the goal if unclear:**

> "Is the main goal to forecast future values, explain past variation, or compare business scenarios?"

**Ask about time if relevant:**

> "Are these observations consecutive time periods? If so, should the model predict future periods rather than analyze same-period associations?"

**Ask about predictors if needed:**

> "Which variables were known before the outcome occurred? We should exclude variables that would only be known after the result, because that would create data leakage."

**Ask about data structure if unclear:**

> "Does each row represent an independent observation, or are the same customers, stores, or regions repeated over time?"

Default to business language. Do not ask the user to choose statistical terminology unless necessary.

---

### Step 3 - Check the Data and Model Assumptions

Before relying on a regression model, review the following:

| Check | Why It Matters | Action |
|---|---|---|
| Outcome type | Linear regression requires a continuous numeric outcome | Redirect binary, count, or categorical outcomes to a more suitable method |
| Missing values | Missing rows can change the usable sample and bias results | Report excluded rows and assess whether missingness is material |
| Linearity | A linear model may miss curved or diminishing-return relationships | Review scatterplots and residual patterns; flag nonlinear relationships |
| Outliers and influential observations | A few unusual rows can strongly change coefficients | Identify material outliers and compare results with and without them when appropriate |
| Multicollinearity | Highly overlapping predictors make individual coefficients unstable | Flag correlated predictors; avoid over-interpreting individual effects |
| Independence and repeated observations | Repeated entities or time dependence can invalidate standard inference | Flag the issue and consider another method or grouped analysis |
| Data leakage | Future or outcome-derived information makes predictions unrealistically strong | Remove leaked variables before using the model |
| Validation | Historical fit does not guarantee useful future predictions | Use a holdout period or holdout sample when prediction is the objective |

Do not overload the business user with raw diagnostic values. Summarize each check as:

- **No material concern**
- **Use with caution**
- **Material concern requiring revision**

---

### Step 4 - Select the Regression Method

| Scenario | Recommended Method | Notes |
|---|---|---|
| One numeric predictor and continuous outcome | Simple linear regression | Use for a focused relationship such as marketing spend and sales |
| Multiple predictors and continuous outcome | Multiple linear regression | Use when several factors may explain or predict the outcome |
| Binary outcome | Logistic regression | Do not use ordinary linear regression |
| Count outcome | Consider a count model | Flag that ordinary linear regression may not fit the outcome |
| Strongly nonlinear pattern | Transform, nonlinear model, or segmented analysis | Do not force a straight-line model |
| Time-series forecasting | Time-series method or carefully designed regression | Check lagging, seasonality, and validation by time period |
| Causal-impact question | Experiment or causal design | Regression alone does not establish causation |

---

### Step 5 - Excel-Native Calculation Workflow

Do not depend on the Excel Data Analysis ToolPak. Do not attempt to enable, open, or run it, because its availability cannot be assumed in the ChatGPT in Excel environment.

When regression results must be written to Excel, use built-in worksheet functions and formulas that can be created directly in the workbook.

For simple linear regression:

| Task | Excel Function | Notes |
|---|---|---|
| Slope | `SLOPE(known_y's, known_x's)` | Estimated change in Y per one-unit increase in X |
| Intercept | `INTERCEPT(known_y's, known_x's)` | Interpret only when X = 0 is meaningful |
| Model fit | `RSQ(known_y's, known_x's)` | Proportion of variation explained |
| Typical error | `STEYX(known_y's, known_x's)` | Standard error of the estimated Y values |
| Prediction | `FORECAST.LINEAR(x, known_y's, known_x's)` | Use only after the model is classified as usable |

For multiple linear regression:

| Task | Excel Function | Notes |
|---|---|---|
| Model coefficients and fit statistics | `LINEST(known_y's, known_x's, TRUE, TRUE)` | Use as the primary Excel-native multiple-regression calculation |
| Prediction from coefficients | Intercept + sum of coefficient x predictor value | Build only after verifying the coefficient-to-predictor mapping |
| Residual | Actual outcome - predicted outcome | Use to summarize model errors and identify material patterns |

When using `LINEST`:

- Define and document the outcome range and predictor columns before writing the formula.
- Preserve the exact predictor order in the technical summary.
- Verify coefficient labels carefully: `LINEST` returns coefficients in reverse predictor order.
- Do not report p-values, confidence intervals, or diagnostics unless they have been calculated and verified.
- Do not create formulas that overwrite source data.

The Excel outputs are mandatory whenever the user asks for regression analysis in a workbook.

---

### Step 6 - Interpret the Model for the Business Question

Interpret outputs based on the business objective.

**Coefficients**

Translate each relevant coefficient into plain language:

> "Holding price and seasonality constant, each additional $1,000 of marketing spend is associated with an estimated $4,200 increase in sales."

Do not describe a coefficient as causal unless the user has an appropriate causal design.

**Predictions**

When the goal is forecasting or scenario planning, report:

- Predicted value
- Plausible prediction range when available
- Typical forecast error
- Assumptions behind the scenario

**Model fit**

Use plain language:

> "The model explains about 62% of the variation in sales."

For prediction questions, also report typical error:

> "Typical forecast error is approximately $18,000."

**Residuals and diagnostics**

Do not display every residual by default. Summarize only findings that affect trust in the model:

> "The model consistently overpredicts sales for the enterprise segment."

> "A small number of large accounts materially influence the result."

---

### Step 7 - Create the Mandatory Excel Outputs

When performing regression analysis in Excel, always create both outputs below. Do not overwrite source data.

#### Output 1 - Main Report in the Data Worksheet

In the worksheet containing the source data, create a clearly separated section titled:

**Regression Analysis Results**

Place it below the data or in unused columns, based on the available layout.

This is the primary business report. Always include:

1. **Business question**
2. **Model used**
   - State simple or multiple linear regression.
   - Identify the outcome variable and predictors.
   - Explain why the model fits the question.
3. **Data scope**
   - State observations used, excluded rows, unit of analysis, and time period when available.
4. **Key drivers**
   - Explain no more than the three most relevant variables in plain business language.
5. **Estimated equation**
   -Always include the Estimated regression equation. if more that one equation is possible (Outliers, etc. include the most actionable with a comment)
6. **Prediction or scenario result**
   - Include predicted value and a plausible range when the question involves forecasting or scenario planning.
   - State "Not applicable" when prediction is not the objective.
7. **Model reliability**
   - State R-squared or adjusted R-squared in plain language.
   - Include typical error when available.
8. **Limitations and diagnostics**
   - State only material limitations, such as outliers, weak fit, multicollinearity, nonlinear patterns, data leakage, or insufficient validation.
9. **Business recommendation**
10. **Model classification**
   - Format the classification in **bold** and apply the specified Excel color format.

| Classification | Excel Format | Meaning |
|---|---|---|
| **Ready for decision support** | Green fill (`#C6EFCE`), dark green font (`#006100`) | The model fits the question, has no material diagnostic concerns, and has adequate validation for its intended use. |
| **Use with caution** | Yellow fill (`#FFEB9C`), dark yellow font (`#9C6500`) | The model may support directional insight or limited planning, but limitations must affect the interpretation or decision. |
| **Do not rely on this model yet** | Red fill (`#FFC7CE`), dark red font (`#9C0006`) | The model has a material issue, such as an unsuitable outcome type, leakage, severe diagnostic problems, unsuitable time structure, or inadequate validation. |

#### Output 2 - Mandatory Technical Summary Worksheet

For **every regression analysis**, create a separate new worksheet containing that analysis's statistical findings. Never clear, replace, rename, or overwrite a worksheet created by a previous regression analysis.

Name the new worksheet using this convention:
`<BusinessDataSheetName>_Regression_Analysis`

For example:
`Sales_Regression_Analysis`

If a worksheet with the intended name already exists, create a new worksheet by adding a sequential number, such as `_2`, `_3`, or `_4`.
Each Regression Analysis worksheet must correspond only to the analysis performed on its named business-data worksheet. It must not contain or replace results from another worksheet.

Put this worsheet next to worksheet that is analyzing. the sheet color should be a very similar but slightly darker color than the original sheet (the one with the business data)

This worksheet is the technical reference for that specific regression analysis. It must include:

**Model overview**

- Outcome variable
- Model type
- Predictor list in formula order
- Number of observations
- R-squared
- Adjusted R-squared when available
- Typical error measure when available
- Validation approach
- Model classification

**Top predictor summary**

Show no more than the three most relevant predictors. Do not count the intercept as a predictor.

Select predictors using this order:

1. Variables directly tied to the business question.
2. Variables with meaningful business impact.
3. Variables with sufficiently reliable estimates and no serious multicollinearity concern.

Do not rank predictors solely by raw coefficient size because predictors may use different units and scales.

| Rank | Predictor | Direction | Coefficient | Reliability | Business Interpretation | Notes |
|---|---|---|---:|---|---|---|
| 1 | Marketing spend | Positive | 4.20 | Verified estimate | Each additional $1,000 is associated with approximately $4,200 higher sales, holding other included variables constant. | Association, not causation |
| 2 | Average discount | Negative | -1.80 | Use with caution | Higher discounts are associated with lower sales revenue after controlling for other included variables. | Review overlap with promotion variables |
| 3 | Seasonality index | Positive | 12.40 | Verified estimate | Seasonal periods are associated with higher expected sales. | Confirm future seasonality assumptions |

**Diagnostics summary**

| Check | Status | Business Meaning |
|---|---|---|
| Linearity | No material concern / Use with caution / Material concern | Whether a straight-line relationship is reasonable |
| Outliers | No material concern / Use with caution / Material concern | Whether unusual rows affect the model |
| Multicollinearity | No material concern / Use with caution / Material concern | Whether individual predictor effects are stable |
| Validation | No material concern / Use with caution / Material concern | Whether the model is reliable for future use |
| Data leakage | No material concern / Material concern | Whether predictors were available before the outcome |

Keep full coefficient tables, residual rows, and advanced diagnostic statistics on this technical worksheet only when they are calculated and verified.

---

### Step 8 - Final Communication Rules

Default to business language first.

- Report no more than three key variables in the main report.
- Do not lead with p-values.
- Use effect size, predicted impact, forecast error, and business context before statistical terminology.
- Never state that a predictor caused the outcome unless the analysis supports a causal claim.
- Do not hide material model problems.
- Provide technical output only when needed to explain a material limitation.
- Recommend validation against future or holdout data before using a model for high-impact decisions.

## When to Be Cautious About Statistical Claims

### Correlation Is Not Causation

When you find a correlation, explicitly consider:
- **Reverse causation**: Maybe B causes A, not A causes B
- **Confounding variables**: Maybe C causes both A and B
- **Coincidence**: With enough variables, spurious correlations are inevitable

**What you can say**: "Users who use feature X have 30% higher retention"
**What you cannot say without more evidence**: "Feature X causes 30% higher retention"

### Multiple Comparisons Problem

When you test many hypotheses, some will be "significant" by chance:
- Testing 20 metrics at p=0.05 means ~1 will be falsely significant
- If you looked at many segments before finding one that's different, note that
- Adjust for multiple comparisons with Bonferroni correction (divide alpha by number of tests) or report how many tests were run

### Simpson's Paradox

A trend in aggregated data can reverse when data is segmented:
- Always check whether the conclusion holds across key segments
- Example: Overall conversion goes up, but conversion goes down in every segment -- because the mix shifted toward a higher-converting segment

### Survivorship Bias

You can only analyze entities that "survived" to be in your dataset:
- Analyzing active users ignores those who churned
- Analyzing successful companies ignores those that failed
- Always ask: "Who is missing from this dataset, and would their inclusion change the conclusion?"

### Ecological Fallacy

Aggregate trends may not apply to individuals:
- "Countries with higher X have higher Y" does NOT mean "individuals with higher X have higher Y"
- Be careful about applying group-level findings to individual cases

### Anchoring on Specific Numbers

Be wary of false precision:
- "Churn will be 4.73% next quarter" implies more certainty than is warranted
- Prefer ranges: "We expect churn between 4-6% based on historical patterns"
- Round appropriately: "About 5%" is often more honest than "4.73%"
