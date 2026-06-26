# Methodology Notes

## Pain Point

The original `statistical-analysis` skill was good, but it was not fully optimized for my working environment: ChatGPT in Excel.

The original skill had a solid statistical foundation, but when tested inside a spreadsheet workflow, a few limitations became clear. The output was not always structured in a way that was easy to use directly in Excel, and the hypothesis testing guidance did not consistently produce the kind of decision-ready business output that I need as a Data and Financial Analyst.

In my environment, the skill needs to do more than explain statistical concepts. It needs to help identify the correct test, explain why that test is appropriate, support Excel-based calculation when needed, and translate results into practical business interpretation.

## Main Goal

The purpose of this project is to adapt the statistical analysis skill for a more practical business analytics workflow inside ChatGPT in Excel.

The main goals are:

1. Make the skill easier to use in Excel.
2. Make the skill more powerful, starting with the hypothesis testing section.
3. Produce more standardized output that can be displayed, reviewed, or reused in Excel.
4. Make the skill more useful for business users, especially analysts working with operational, commercial, or financial data.

In analytical work, the goal is not just to run calculations. The more important skill is knowing which statistical method fits the business question. For my work, that means being able to identify the right statistical test for common business cases such as A/B tests, before-and-after comparisons, segment comparisons, conversion-rate analysis, and categorical relationship testing.

## Methodology

I started from an existing Claude statistical analysis skill because the original version already had a strong baseline. This avoided starting from zero and allowed me to focus on adaptation, testing, and iteration.

The development process followed a lightweight software-development workflow:

1. Review the original skill and identify the sections most relevant to Excel-based business analysis.
2. Test the original skill inside ChatGPT in Excel using practical analyst scenarios.
3. Identify gaps between statistically correct answers and business-ready answers.
4. Modify the skill in small, targeted iterations.
5. Build a project folder with supporting files for version control, testing, documentation, and future development.
6. Create test workbooks to validate the skill against realistic spreadsheet scenarios.
7. Track changes in `CHANGELOG.md` so updates are documented over time.

The first development focus was hypothesis testing because this is where method selection matters most. A common mistake in analytical work is treating every comparison as a t-test. The adapted skill is designed to slow down that decision point and first ask: what kind of business question is being answered?

The test workbook includes deliberate scenarios and traps, such as:

- Ambiguous one-tailed vs. two-tailed questions.
- Independent vs. paired sample structure.
- Unequal variance considerations that may require Welch's t-test.
- Proportion testing for conversion-rate analysis.
- Chi-squared testing for categorical association.
- Data quality issues and business-context outliers.

These tests are intended to validate not only whether the skill can calculate an answer, but whether it can reason correctly before calculating.

## Current State and Future Development

I am satisfied with this first iteration. The project now has a clearer hypothesis testing workflow, a test workbook, a solved workbook, documentation, a changelog, a `.gitignore`, and a roadmap for future development.

The current version should be treated as an initial release rather than a finished product. The next planned improvement is to add a new Correlation and Regression section, which is not currently part of the original skill. That enhancement is documented in `NEXT.md`.

Future work may include:

- Expanding the test workbook with correlation and regression cases.
- Adding more examples for financial analysis, forecasting, and business performance diagnostics.
- Improving output formatting for Excel display.
- Adding more explicit acceptance criteria for each test scenario.
- Reviewing the skill for consistency, clarity, and maintainability.

## Invitation to Participate

Contributions and feedback are welcome from people who agree with the goals described in the README and these methodology notes.

This project sits at the intersection of business analysis, statistical reasoning, spreadsheet workflows, and AI skill development. Useful contributions may come from data analysts, financial analysts, business analysts, statisticians, Excel users, prompt engineers, or software developers interested in practical AI tooling.

Please use standard GitHub best practices and etiquette when participating:

- Open an issue before proposing large changes.
- Keep pull requests focused and easy to review.
- Explain the business or statistical reason behind each change.
- Update `CHANGELOG.md` when modifying the skill, workbook, or documentation.
- Add or update test cases when changing skill behavior.
- Be respectful and constructive in comments and reviews.

The purpose of this repository is not only to store a skill file. It is to show an iterative, testable, and documented approach to adapting an AI skill from one environment to another: from a Claude-style statistical analysis skill into a ChatGPT-in-Excel workflow that is more useful for real business analysis.
