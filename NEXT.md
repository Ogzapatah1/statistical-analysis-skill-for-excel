# Next Development Steps

This file tracks planned improvements for the statistical analysis skill. The project remains intentionally lightweight, but development follows an iterative backlog: define the feature, test it in Excel, document the results, and record the change.

## Completed Feature Work

### Correlation Analysis

Correlation analysis has been added to the skill and tested in Excel. The feature supports Pearson and Spearman correlation, guidance for binary-numeric and categorical cases, data-quality and scatterplot checks, correlation-versus-causation safeguards, and direction to regression when the business objective is prediction or driver analysis.

The correlation test workbooks include common business cases and solved outputs to validate the expected behavior.

### Regression Analysis

Regression analysis has been added to the skill and tested in Excel. The feature supports simple and multiple linear regression for continuous outcomes, Excel-native calculation guidance, business-readable main reports, technical analysis worksheets, model classification, and safeguards for causation claims.

The regression test workbooks cover standard business cases and deliberate risks: influential outliers, multicollinearity, nonlinear relationships, binary outcomes, and data leakage. The solved workbook confirms that the skill can identify the appropriate method, communicate material limitations, and distinguish models that are ready for decision support from those requiring caution or revision.

## Planned Follow-Up Work

- Document the rationale for regression test cases in `docs/methodology-notes.md`.
- Update README coverage and file descriptions for the regression feature and test workbooks.
- Refine technical-summary worksheet naming so each regression analysis receives a unique Excel-compatible worksheet name.

## Development Backlog

| Priority | Item | Status |
|---|---|---|
| Complete | Add correlation analysis to `skill/statistical-analysis-xls.md`. | Complete |
| Complete | Create unsolved and solved correlation test workbooks. | Complete |
| Complete | Add regression analysis to `skill/statistical-analysis-xls.md`. | Complete |
| Complete | Create regression test cases for standard and high-risk business scenarios. | Complete |
| Complete | Add solved workbook outputs for regression tests. | Complete |
| Medium | Document regression test-case rationale in `docs/methodology-notes.md`. | Planned |
| Low | Update README coverage and file descriptions for regression. | Planned |
| Medium | Refine unique, Excel-compatible regression technical-summary worksheet names. | Planned |

## Acceptance Criteria

- The regression section helps the model select a method before calculating anything.
- It distinguishes association, prediction, explanation, and causal claims.
- Regression outputs are business-readable, not only statistical.
- Excel functions appear only when the user is computing in a spreadsheet.
- New scenarios include deliberate traps, such as outlier-driven relationships, multicollinearity, or a relationship that disappears after segmentation.
- The feature is tested in an unsolved workbook and checked against documented expected outputs.

## Versioning Note

Each completed change should be logged in `CHANGELOG.md` with the date and a concise one-line description.
