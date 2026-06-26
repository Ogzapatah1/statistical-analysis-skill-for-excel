# Statistical Analysis Skill for ChatGPT in Excel

> An adaptation of a Claude statistical analysis skill, tuned for business analysts and financial professionals working inside ChatGPT in Excel.

---

## Background

The original `statistical-analysis` skill was well-structured and statistically sound. It covered descriptive statistics, trend analysis, outlier detection, and hypothesis testing with solid methodology. However, when deployed in a **ChatGPT in Excel** environment, a few gaps became apparent:

- Output format was not always structured for direct use in a spreadsheet context
- The hypothesis testing section produced technically correct answers but lacked the decision-oriented, business-readable output that analysts need to act on
- The skill was written for a general data science context rather than the specific scenarios that come up in day-to-day FP&A and commercial analysis work

This repository tracks the adaptation of that skill to close those gaps.

---

## Adaptation Goals

### 1. Excel-first usability

The skill is designed for use inside a spreadsheet environment. Excel formula references are included in the hypothesis testing section, but only surfaced when the user is explicitly computing results in a spreadsheet — not when asking interpretive or conceptual questions. This avoids the noise of getting formula output when you just need an explanation.

### 2. A more structured Hypothesis Testing section

The hypothesis testing workflow was redesigned as a six-step framework. The most significant addition is **Step 6: Structured Output**, which instructs the model to consistently deliver:

1. The business question being tested
2. The statistical test selected and why
3. H0 and H1 stated in plain language
4. Assumption checks, p-value, and effect size
5. A clear decision (Reject / Fail to Reject H0)
6. A business interpretation of the result
7. A business recommendation

This structured output is designed to be directly usable in analytical reports, executive summaries, and Excel-based dashboards — not just readable in chat.

### 3. Business user orientation

As a Data and Financial Analyst, the core use case is not statistical research — it is answering business questions with the right method and communicating the result clearly to stakeholders. The skill is tuned around the most common analytical scenarios in that context:

- **A/B test analysis**: Did the variant outperform the control?
- **Before/after comparisons**: Did a campaign, training program, or product change move the metric?
- **Segment comparisons**: Do Enterprise and SMB customers behave significantly differently?
- **Categorical association**: Is there a relationship between two categorical variables (e.g., acquisition channel and plan type)?

---

## Key Changes from the Original Skill

All modifications are in the `## Hypothesis Testing` section of the skill file.

**Structured reasoning before test selection**
The original skill described which test to use for which scenario. This version adds explicit reasoning steps — deducing H0 and H1 from the business question, clarifying one-tailed vs. two-tailed directionality, and checking test assumptions — before any test is selected or run.

**Targeted clarifying question logic**
The skill now asks clarifying questions only when something genuinely cannot be inferred from context. It prioritizes the single most critical unresolved question rather than front-loading a checklist, which is important in a tool like Excel where back-and-forth conversation has a higher friction cost.

**Step 6: Structured output block**
This is the most impactful addition for the Excel environment. It produces a consistently formatted result that covers methodology, hypotheses, statistical output, decision, interpretation, and business recommendation in a predictable, reusable structure.

**Statistical vs. practical significance**
The skill explicitly separates p-value (statistical significance) from effect size and business impact (practical significance). This distinction is the one that actually drives decisions — a result can be statistically significant but too small to justify any action, and the skill is designed to flag that clearly.

---

## Repository Structure

```
statistical-analysis-skill/
│
├── README.md                  # This file
├── CHANGELOG.md               # Version history
├── NEXT.md                    # Planned development roadmap
├── LICENSE
├── .gitignore
│
├── skill/
│   └── statistical-analysis.md         # Adapted skill file
│
├── tests/
│   ├── statistical_skill_test_workbook.xlsx         # Test dataset
│   └── statistical_skill_test_workbook_solved.xlsx  # Expected outputs and results
│
└── docs/
    └── methodology-notes.md   # Design decisions and test case rationale
```

---

## File Descriptions

| File | Purpose |
|---|---|
| `README.md` | Explains the project purpose, adaptation goals, repository structure, and how to use the skill and test materials. |
| `CHANGELOG.md` | Tracks each meaningful update to the skill, test workbook, documentation, or project structure with a date and one-line summary. |
| `NEXT.md` | Tracks planned improvements, backlog items, and acceptance criteria for future development. |
| `LICENSE` | Defines the project license. This repository uses the MIT License. |
| `.gitignore` | Prevents temporary Excel files, local system files, and other repo clutter from being committed. This is especially important because Excel creates temporary files that start with `~$` when a workbook is open. |
| `skill/statistical-analysis.md` | The adapted ChatGPT-in-Excel statistical analysis skill. This is the core file being improved and tested. |
| `tests/statistical_skill_test_workbook.xlsx` | The unsolved testing workbook containing datasets, prompts, and scenarios used to evaluate how the skill behaves. |
| `tests/statistical_skill_test_workbook_solved.xlsx` | The solved testing workbook containing implemented calculations, expected outputs, and analysis results for comparison. |
| `docs/methodology-notes.md` | Captures the reasoning behind the test design, including deliberate traps, ambiguity checks, and statistical judgment points such as one-tailed vs. two-tailed testing and Welch's variance handling. |

---

## How to Use

1. Open `skill/statistical-analysis.md` and copy the full content
2. Paste it into the ChatGPT in Excel custom instructions or system prompt field
3. Open `tests/statistical_skill_test_workbook.xlsx` to run the test scenarios
4. Use `tests/statistical_skill_test_workbook_solved.xlsx` to compare against expected outputs
5. The `Prompt_Bank` sheet in the workbook contains curated prompts calibrated to test specific skill behaviors — including edge cases like ambiguous directionality, paired vs. independent structure, and multiple comparisons

Refer to `docs/methodology-notes.md` for the reasoning behind each test scenario and what correct model behavior looks like for each case.

---

## Statistical Coverage

| Area | Methods |
|---|---|
| Descriptive Statistics | Mean, median, IQR, percentiles, distribution shape, data quality flags |
| Trend Analysis | Moving averages, period-over-period comparisons, seasonality detection, basic forecasting |
| Outlier Detection | Z-score, IQR method, time series deviation flagging |
| Hypothesis Testing | Independent t-test, Welch's t-test, paired t-test, z-test for proportions, ANOVA, Mann-Whitney U, chi-squared test of independence |

---

## Notes on the Skill Adaptation

This project reflects a practical interest in how AI behavioral specifications — variously called skills, system prompts, or custom instructions depending on the platform — can be ported and tuned across different model environments. The original skill was written for Claude (Anthropic). Adapting it for ChatGPT in Excel required understanding not just the statistical content, but how output format, question-handling logic, and context assumptions differ between the two environments.

The changes here are incremental rather than a full rewrite — the statistical methodology of the original was sound. The adaptation is primarily about output structure, business orientation, and making the skill behave consistently in a tool context where the user is working in a spreadsheet, not a chat interface.

Version history is tracked in `CHANGELOG.md`.

---

## Invitation to Participate

Contributions and feedback are welcome from people who agree with the goals described in this README and in `docs/methodology-notes.md`.

This project sits at the intersection of business analysis, statistical reasoning, spreadsheet workflows, and AI skill development. Useful contributions may come from data analysts, financial analysts, business analysts, statisticians, Excel users, prompt engineers, or software developers interested in practical AI tooling.

Please use standard GitHub best practices and etiquette when participating:

- Open an issue before proposing large changes.
- Keep pull requests focused and easy to review.
- Explain the business or statistical reason behind each change.
- Update `CHANGELOG.md` when modifying the skill, workbook, or documentation.
- Add or update test cases when changing skill behavior.
- Be respectful and constructive in comments and reviews.

The purpose of this repository is not only to store a skill file. It is to show an iterative, testable, and documented approach to adapting an AI skill from one environment to another: from a Claude-style statistical analysis skill into a ChatGPT-in-Excel workflow that is more useful for real business analysis.
