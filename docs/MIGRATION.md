# Migration: Python 2 to Python 3

## Objective

The objective of STRADA Renaissance was to modernize a legacy scientific desktop application written in Python 2 while preserving its historical calculation workflow and expected results.

The project was approached as a compatibility and preservation exercise rather than a redesign of the scientific methodology.

## Starting point

The legacy application combined scientific calculations, Excel input processing, cross-correlation analysis and vulnerability classification in a Python 2 desktop workflow.

The modernization had to address both language/runtime obsolescence and Windows distribution requirements.

## Main modernization work

### Python 2 to Python 3

The legacy codebase was migrated to Python 3, adapting syntax, runtime behavior, input handling and library compatibility where necessary.

### Data compatibility

Support for the existing `.xls` workflow was preserved so that historical input files could continue to be used without requiring a data-format migration.

### Scientific calculations

The calculation flow was preserved and regression-tested against the legacy implementation.

Particular care was taken not to change scientific behavior simply because a newer implementation appeared cleaner or more modern.

### Cross-correlation dependency

The legacy workflow relied on SciPy for the cross-correlation operation.

During packaging on a modern Windows environment, SciPy caused DLL/application-control problems. The specific correlation operation required by STRADA was therefore reproduced using NumPy and checked against the legacy results.

This reduced the runtime dependency footprint while preserving the observed behavior of the reference calculation.

### Desktop interface

A graphical interface was added for the modernized application while retaining a command-line version for compatibility and testing.

The GUI provides access to the same calculation workflow together with clearer input selection and result presentation.

### Reporting

The modernized version can generate result reports automatically in TXT and PDF formats.

### Windows packaging

The application was prepared for Windows distribution using:

- PyInstaller for standalone executables
- a portable ZIP distribution
- Inno Setup for a conventional Windows installer

Application-relative paths were introduced so that input files and generated reports behave correctly in compiled and portable environments.

## Regression validation

A reference dataset was used repeatedly during development to compare the modernized implementation with the historical Python 2 behavior.

Validation included:

- calculated VESPA index
- cross-correlation delays
- intermediate scoring
- final vulnerability classification
- generated protection-zone values

The objective was to detect unintended behavioral changes introduced by modernization.

## VESPA threshold investigation

During the work, a discrepancy was identified between the VESPA classification thresholds implemented in the historical STRADA source and thresholds contained in technical documentation examined during the project.

The historical software uses the following ranges:

| VESPA index | Classification |
| --- | --- |
| V <= 1 | Basso |
| 1 < V <= 10 | Medio |
| 10 < V <= 100 | Alto |
| V > 100 | Molto Alto |

The technical document examined uses:

| VESPA index | Classification |
| --- | --- |
| V <= 0.1 | Basso |
| 0.1 < V <= 1 | Medio |
| 1 < V <= 10 | Alto |
| V > 10 | Elevato |

An experimental variant implementing the documented ranges was created and tested.

However, because the historical Python 2 application is the established STRADA software reference, the modernization baseline preserves the original STRADA thresholds until the methodological difference can be independently validated.

This became an important engineering lesson from the project: modernization should preserve established scientific behavior unless there is sufficient evidence and authority to change it.

## Result

The project produced a modern Python 3 implementation capable of running on current Windows systems while preserving the workflow and behavior of the available legacy application.

The work also produced a repeatable build process, portable distribution, installer, reporting workflow and documented validation process.
