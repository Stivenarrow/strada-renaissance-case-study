# Architecture

## Overview

STRADA Renaissance separates the legacy scientific workflow from the user interface, reporting and Windows distribution layers.

```text
Excel .xls input
      |
      v
+-----------------------+
| Scientific core       |
| - data validation     |
| - flow conversion     |
| - VESPA calculation   |
| - cross-correlation   |
| - classification      |
+-----------------------+
      |
      +--------------------+
      |                    |
      v                    v
+-------------+      +-------------+
| GUI         |      | CLI         |
| desktop app |      | command line|
+-------------+      +-------------+
      |                    |
      +---------+----------+
                |
                v
        +---------------+
        | Reports       |
        | TXT + PDF     |
        +---------------+
                |
                v
        +---------------+
        | Distribution  |
        | EXE / Portable|
        | Installer     |
        +---------------+
```

## Main layers

### Scientific core

Contains the calculation workflow independently from the graphical interface. Its behavior was compared with the available Python 2 implementation during regression testing.

### Input layer

Maintains compatibility with the historical Excel `.xls` workflow and validates the data required by the calculations.

### User interfaces

Two entry points were maintained:

- graphical desktop interface for normal use;
- command-line interface for compatibility, diagnostics and regression testing.

### Reporting

Calculation results can be exported automatically as TXT and PDF reports.

### Distribution

The Python application is packaged for Windows as standalone executables. A portable distribution and a conventional installer are generated from the same application baseline.

## Technology

| Area | Technology |
| --- | --- |
| Runtime | Python 3 |
| Numerical processing | NumPy |
| Legacy Excel input | xlrd |
| PDF reporting | ReportLab |
| Executable packaging | PyInstaller |
| Windows installer | Inno Setup |

## Design principle

The primary design rule of the modernization was **compatibility before redesign**: infrastructure and runtime components could be modernized, but established scientific behavior should remain unchanged unless a methodological change is independently validated.
