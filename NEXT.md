# Next Development Steps

This file tracks planned improvements for the statistical analysis skill. It is intentionally lightweight: the project is small, but the work should still be organized as an iterative development backlog.

## Planned Feature Work

### Add Correlation and Regression Coverage

The next major skill enhancement is to add a dedicated section for correlation and regression analysis. This is not currently covered in the original statistical analysis skill, so it represents a net-new feature rather than a small adaptation.

Planned scope:

- Add guidance for choosing between correlation and regression based on the business question.
- Distinguish correlation from causation and require cautious interpretation.
- Cover Pearson correlation, Spearman correlation, simple linear regression, and basic multiple regression.
- Add assumptions and diagnostics such as linearity, outliers, multicollinearity, residual patterns, and sample size considerations.
- Include Excel-oriented formulas and workflows where useful, such as `CORREL`, `RSQ`, `SLOPE`, `INTERCEPT`, `LINEST`, scatterplots, trendlines, and regression output interpretation.
- Add structured output for regression results, including the business question, model selected, variables used, coefficient interpretation, R-squared, limitations, and business recommendation.

## Development Backlog

| Priority | Item | Status |
|---|---|---|
| High | Draft the new Correlation and Regression section in `Skill/statistical-analysis.md`. | Planned |
| High | Create test cases in the workbook for correlation, simple regression, and misleading correlation scenarios. | Planned |
| Medium | Add solved workbook outputs for correlation and regression tests. | Planned |
| Medium | Document the reasoning behind the new test cases in `docs/methodology-notes.md`. | Planned |
| Low | Refactor README coverage table after the new section is added. | Planned |

## Acceptance Criteria

- The new section should help the model select the right method before calculating anything.
- The skill should explicitly warn when a correlation is being interpreted too causally.
- Regression outputs should be business-readable, not only statistical.
- Excel functions should appear only when the user is computing inside a spreadsheet.
- New test scenarios should include at least one deliberate trap, such as an outlier-driven correlation or a relationship that disappears after segmentation.

## Versioning Note

Each completed change should be logged in `CHANGELOG.md` with the date and a concise one-line description.
