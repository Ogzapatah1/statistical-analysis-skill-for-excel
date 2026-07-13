# Next Development Steps

This file tracks planned improvements for the statistical analysis skill. The project remains intentionally lightweight, but development follows an iterative backlog: define the feature, test it in Excel, document the results, and record the change.

## Completed Feature Work

### Correlation Analysis

Correlation analysis has been added to the skill and tested in Excel. The feature supports Pearson and Spearman correlation, guidance for binary-numeric and categorical cases, data-quality and scatterplot checks, correlation-versus-causation safeguards, and direction to regression when the business objective is prediction or driver analysis.

The correlation test workbooks include common business cases and solved outputs to validate the expected behavior.

## Planned Feature Work

### Add Regression Coverage

The next major enhancement is a dedicated regression-analysis section. Correlation is now available for measuring association; regression will extend the skill to explain, estimate, and predict an outcome variable.

Planned scope:

- Add guidance for choosing simple or multiple regression based on the business question.
- Cover coefficient interpretation, R-squared, adjusted R-squared, confidence intervals, and practical business impact.
- Add assumptions and diagnostics including linearity, residual patterns, outliers, multicollinearity, and sample-size considerations.
- Include Excel-oriented workflows where useful, including `SLOPE`, `INTERCEPT`, `LINEST`, scatterplots, trendlines, and regression-output interpretation.
- Add structured output for regression results: business question, model selected, variables used, coefficient interpretation, model fit, limitations, and recommendation.

## Development Backlog

| Priority | Item | Status |
|---|---|---|
| Complete | Add correlation analysis to `skill/statistical-analysis-xls.md`. | Complete |
| Complete | Create unsolved and solved correlation test workbooks. | Complete |
| High | Draft a regression section in `skill/statistical-analysis-xls.md`. | Planned |
| High | Create Excel test cases for simple regression, multiple regression, and misleading relationships. | Planned |
| Medium | Add solved workbook outputs for regression tests. | Planned |
| Medium | Document regression test-case rationale in `docs/methodology-notes.md`. | Planned |
| Low | Update README coverage and file descriptions after regression is implemented. | Planned |

## Acceptance Criteria

- The regression section helps the model select a method before calculating anything.
- It distinguishes association, prediction, explanation, and causal claims.
- Regression outputs are business-readable, not only statistical.
- Excel functions appear only when the user is computing in a spreadsheet.
- New scenarios include deliberate traps, such as outlier-driven relationships, multicollinearity, or a relationship that disappears after segmentation.
- The feature is tested in an unsolved workbook and checked against documented expected outputs.

## Versioning Note

Each completed change should be logged in `CHANGELOG.md` with the date and a concise one-line description.
