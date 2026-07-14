# Statistical Analysis Skill for ChatGPT in Excel

> An adaptation of a Claude statistical analysis skill, tuned for business analysts and financial professionals working inside ChatGPT in Excel.

---

## Summary

This project adapts an existing statistical analysis skill into an Excel-first tool for business analysis. The original skill had a strong statistical foundation, but it was not designed for how financial and data analysts work inside ChatGPT in Excel. The aim is to make it easier to use in spreadsheets, more useful for business decisions, and clearer about selecting the right statistical method for a given question.

The first iteration focused on hypothesis testing. The valuable work is not only calculating a p-value, but choosing the appropriate test, stating hypotheses, checking assumptions, interpreting the result, and translating it into a business recommendation. The skill now guides that workflow for common tests such as independent, Welch's, and paired t-tests, proportion tests, and chi-squared tests.

Correlation analysis is the second completed and tested feature. It helps business users choose Pearson or Spearman correlation, handles binary-numeric and categorical cases appropriately, recommends scatterplots and data-quality checks, and separates a useful association from an unsupported causal claim. When the business objective is prediction, scenario planning, or driver analysis, the skill directs the user toward regression instead of presenting correlation as a complete answer.

Regression analysis builds on that correlation workflow when the question requires an estimated outcome, a quantified business driver, or a planning scenario. The feature provides Excel-native guidance for simple and multiple linear regression, translates coefficients and model fit into business language, and requires both a main report beside the source data and a technical analysis worksheet. Its test cases include standard sales-planning models as well as influential outliers, multicollinearity, nonlinear relationships, binary outcomes, and data leakage, so the model is evaluated on its judgment as well as its calculations.

Development is iterative: the original Claude skill was tested in ChatGPT in Excel with realistic business scenarios, then revised to improve Excel usability, standardized output, and business interpretation. The test materials include deliberate traps, such as one-tailed versus two-tailed ambiguity, paired versus independent samples, unequal variance, categorical association, and practical versus statistical significance. The repository supports this process with skill versions, test and solved workbooks, methodology notes, a changelog, a roadmap, and Git workflow guidance.

The broader goal is to demonstrate a disciplined approach to developing an AI-assisted analytical tool: test it, identify weaknesses, improve it, document the reasoning, version the changes, and publish the work transparently on GitHub.

---

## Background

The original `statistical-analysis` skill was well-structured and statistically sound. It covered descriptive statistics, trend analysis, outlier detection, and hypothesis testing. In a **ChatGPT in Excel** environment, however, several gaps became apparent:

- Output was not always structured for direct spreadsheet use.
- Technically correct hypothesis-test answers did not always provide the decision-oriented interpretation analysts need.
- The skill targeted a general data science context rather than common FP&A and commercial analysis scenarios.

This repository tracks the adaptation intended to close those gaps.

The scope is practical rather than academic. Each change is evaluated by whether it helps an analyst identify the appropriate method, produce a defensible result in Excel, and explain its business meaning without overstating what the data supports. The workbooks provide an auditable way to compare the skill's behavior with expected reasoning and results over time. It also makes design decisions easier to review, reproduce, communicate, and improve as the skill evolves across future releases.

---

## Adaptation Goals

### 1. Excel-first usability

The skill is designed for spreadsheet work. Excel formulas are surfaced when the user is computing results in a workbook, but not for purely interpretive or conceptual questions. This keeps an explanation focused when calculation support is not needed, while still making the skill practical for users who need to reproduce a result in Excel.

### 2. A more structured Hypothesis Testing section

The hypothesis-testing workflow uses a six-step framework. Its key addition, **Step 6: Structured Output**, directs the model to provide:

1. The business question
2. The selected test and rationale
3. H0 and H1 in plain language
4. Assumption checks, p-value, and effect size
5. A decision: Reject or Fail to Reject H0
6. Business interpretation
7. A recommendation

This output is intended to be reusable in Excel, analytical reports, and executive summaries. It separates the statistical analysis from the final business-ready explanation so that stakeholders can see both the reasoning and the action implied by the result.

### 3. Correlation analysis for business questions

The correlation feature turns an ambiguous request to "see whether two variables are related" into a disciplined workflow: define the variables and business purpose, select the appropriate correlation method, check the relationship visually, and communicate the result with appropriate cautions. It is designed to help analysts use correlation for exploratory and decision-support work without confusing association with causation or treating a correlation coefficient as a forecast.

### 4. Regression analysis for decision support

The regression feature extends the skill from measuring association to estimating a continuous business outcome using one or more predictors. It guides model selection, documents the data scope, produces an estimated equation and practical prediction information when relevant, and uses diagnostics to identify material risks before recommending action. The required color-coded classification keeps the final message proportionate to the evidence: ready for decision support, use with caution, or do not rely on this model yet.

### 5. Business user orientation

The skill is tuned to business questions commonly handled by data and financial analysts:

- **A/B test analysis**: Did the variant outperform the control?
- **Before/after comparisons**: Did a campaign, training program, or product change affect a metric?
- **Segment comparisons**: Do Enterprise and SMB customers behave significantly differently?
- **Categorical association**: Is there a relationship between acquisition channel and plan type?

---

## Key Changes from the Original Skill

**Structured reasoning before test selection**
The skill now derives H0 and H1 from the business question, clarifies directionality when needed, and checks assumptions before selecting or running a test. This is particularly important when a business prompt does not explicitly say whether the samples are paired, whether a directional hypothesis was specified in advance, or whether equal variance can reasonably be assumed.

**Targeted clarifying question logic**
It asks for the single most important missing detail only when it cannot be inferred, reducing unnecessary back-and-forth in Excel. For example, it should resolve whether the same customers were observed before and after a campaign before treating the data as independent samples.

**Step 6: Structured output block**
The final response follows a predictable format covering methodology, hypotheses, results, decision, interpretation, and recommendation.

**Statistical vs. practical significance**
The skill distinguishes p-value from effect size and business impact, helping prevent statistically significant but operationally immaterial results from driving decisions.

**Correlation analysis**
The skill now includes a structured correlation workflow: define the business question, check data quality and the relationship visually, select Pearson or Spearman as appropriate, and interpret the coefficient with appropriate cautions. It also handles binary-numeric relationships and identifies categorical cases where correlation is not the right method.

The correlation feature is designed to answer association questions without overstating the evidence. It distinguishes correlation from causation, highlights outliers and nonlinear patterns that can mislead interpretation, and redirects prediction or driver-analysis questions toward regression. The Excel test workbooks include both unsolved business scenarios and solved expected outputs to validate the feature in ChatGPT in Excel.

**Regression analysis**

The regression feature adds a structured route for continuous-outcome business questions such as estimating sales, evaluating potential drivers, and building planning scenarios. It requires the skill to assess whether linear regression fits the question and data, communicate coefficients and reliability in business terms, and surface only diagnostics that materially affect a decision. The solved regression workbook demonstrates this approach across usable models and intentional failure cases, including a high-leverage campaign, overlapping marketing metrics, diminishing returns, a binary churn outcome, and a leaked month-end variable.

---

## Repository Structure

```
statistical-analysis-skill/
|
|- README.md
|- CHANGELOG.md
|- NEXT.md
|- LICENSE
|- .gitignore
|
|- skill/
|  `- statistical-analysis-xls.md
|
|- tests/
|  |- Original test/
|  |  |- statistical_skill_test_workbook.xlsx
|  |  `- statistical_skill_test_workbook_solved.xlsx
|  |- Correlation/
|  |  |- statistical_skill_correlation_test_workbook.xlsx
|  |  `- statistical_skill_correlation_test_solved_workbook.xlsx
|  `- Regression/
|     |- Book1.xlsx
|     |- Regression Test.xlsx
|     `- Regression Test_Solved.xlsx
|
`- docs/
   |- methodology-notes.md
   |- git-github-workflow.md
   `- Social Media/
```

---

## File Descriptions

| File | Purpose |
|---|---|
| `README.md` | Project purpose, goals, structure, and usage guidance. |
| `CHANGELOG.md` | Dated record of meaningful changes to the skill, workbooks, documentation, or project structure. |
| `NEXT.md` | Roadmap, backlog items, and acceptance criteria for future development. |
| `LICENSE` | MIT License for the project. |
| `.gitignore` | Excludes Excel temporary files, local drafts, and system clutter from commits. |
| `skill/statistical-analysis-xls.md` | The adapted ChatGPT-in-Excel statistical analysis skill being tested and improved. |
| `tests/Original test/statistical_skill_test_workbook.xlsx` | Unsolved hypothesis-testing datasets, prompts, and scenarios. |
| `tests/Original test/statistical_skill_test_workbook_solved.xlsx` | Hypothesis-testing calculations, expected outputs, and results. |
| `tests/Correlation/statistical_skill_correlation_test_workbook.xlsx` | Unsolved correlation scenarios for testing the correlation feature. |
| `tests/Correlation/statistical_skill_correlation_test_solved_workbook.xlsx` | Solved correlation workbook with expected outputs and results. |
| `tests/Regression/Book1.xlsx` | Initial generated regression test fixture with business scenarios and prompts. |
| `tests/Regression/Regression Test.xlsx` | Unsolved regression workbook used to test the feature in ChatGPT in Excel. |
| `tests/Regression/Regression Test_Solved.xlsx` | Solved regression workbook with business reports, technical analysis sheets, and diagnostic results. |
| `docs/methodology-notes.md` | Test-design rationale, deliberate traps, and statistical judgment points. |
| `docs/git-github-workflow.md` | Practical Git and GitHub workflow guidance for this project. |
| `docs/Social Media/` | Materials used to communicate the project publicly. |

---

## How to Use

1. Open `skill/statistical-analysis-xls.md` and copy its full content.
2. Paste it into the ChatGPT in Excel custom instructions or system-prompt field.
3. Run scenarios in the relevant unsolved workbook.
4. Compare results with its solved workbook.
5. Use the `Prompt_Bank` sheet to test targeted behaviors and edge cases.

Refer to `docs/methodology-notes.md` for the rationale and expected behavior behind each scenario.

---

## Statistical Coverage

| Area | Methods |
|---|---|
| Descriptive Statistics | Mean, median, IQR, percentiles, distribution shape, data quality flags |
| Trend Analysis | Moving averages, period-over-period comparisons, seasonality detection, basic forecasting |
| Outlier Detection | Z-score, IQR method, time series deviation flagging |
| Hypothesis Testing | Independent t-test, Welch's t-test, paired t-test, z-test for proportions, ANOVA, Mann-Whitney U, chi-squared test of independence |
| Correlation Analysis | Pearson, Spearman, binary-numeric association, and guidance on when correlation is not appropriate |

---

## Notes on the Skill Adaptation

AI behavioral specifications may be called skills, system prompts, or custom instructions depending on the platform. The original version was written for Claude (Anthropic); adapting it for ChatGPT in Excel required preserving sound statistical methodology while changing output structure, question-handling logic, and spreadsheet assumptions.

This is an incremental adaptation rather than a full rewrite: the original statistical methodology was retained where it was already sound, while the workflow was adjusted for an Excel-based business setting. The repository is also intended to show the development process behind the artifact, including test design, documented decisions, version history, and future work. Version history is maintained in `CHANGELOG.md`.

---

## Invitation to Participate

Contributions and feedback are welcome from people who share the goals described here and in `docs/methodology-notes.md`. Relevant perspectives may come from data, financial, and business analysts; statisticians; Excel users; prompt engineers; and software developers working on practical AI tools.

Please follow standard GitHub practices:

- Open an issue before proposing a large change.
- Keep pull requests focused and easy to review.
- Explain the business or statistical rationale behind each change.
- Update `CHANGELOG.md` when changing the skill, workbooks, or documentation.
- Add or revise test cases when skill behavior changes.
- Keep comments and reviews respectful and constructive.

The project documents a testable, versioned approach to adapting a Claude-style statistical skill into a ChatGPT-in-Excel workflow for business analysis.
