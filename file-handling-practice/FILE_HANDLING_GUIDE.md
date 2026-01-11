# File Handling & Parsing Guide (Python)

Parsing files is one of the most common “real work” skills in programming: almost every system communicates through files (or file-like streams) at some point—exports, logs, configs, backups, data lakes, and ETL pipelines.

This folder includes:

- `data/`: realistic sample files to parse
- `file_handling_practice_questions.md`: 25 progressively harder practice problems
- `file_handling_practice_solutions.md`: full solutions + extra notes
- `file-handling-practice.ipynb`: the same problems in notebook form, with commented-out tests

## Why file parsing matters

- **Interoperability**: different tools prefer different formats.
  - Excel exports often become **CSV**.
  - Web services commonly produce **JSON** (or NDJSON for streaming).
  - Servers and apps emit **log files** (semi-structured text).
- **Automation**: instead of manually opening files and eyeballing them, parsing lets you automatically:
  - validate data quality
  - compute metrics
  - build dashboards/reports
  - move data between systems
- **Reliability & security**: real files are messy.
  - missing fields, weird quoting, inconsistent types
  - encoding issues (UTF‑8 vs latin‑1)
  - partial writes, corrupted lines, unexpected schemas
  - untrusted inputs that must be validated/sanitized

## Why these formats exist (and when they’re used)

### `.txt` (plain text)
- **What it optimizes for**: simplicity and human readability.
- **Downside**: no standard schema; parsing requires conventions.
- **Real-world examples**:
  - meeting notes
  - exported “reports” from legacy systems
  - README/checklists

### `.csv` (Comma-Separated Values)
- **What it optimizes for**: tabular data interchange (spreadsheets, databases, analytics tools).
- **Why it looks like it does**: rows/columns are easy to stream line-by-line.
- **Downside**: types are not native (everything is text until you convert it); quoting/commas/newlines can be tricky.
- **Real-world examples**:
  - invoices, transactions, customer lists
  - BI exports and quick data extracts

### `.json` (JavaScript Object Notation)
- **What it optimizes for**: structured, nested data with clear types (numbers, booleans, null, objects, arrays).
- **Why it looks like it does**: maps cleanly to in-memory objects; great for APIs.
- **Downside**: can be large; a single huge JSON array is not friendly to streaming without loading it all.
- **Real-world examples**:
  - API responses
  - configuration payloads
  - “documents” in document databases

### `.ndjson` (Newline Delimited JSON)
- **What it optimizes for**: streaming and log-like append-only data.
- **Why it looks like it does**: each line is a complete JSON object; you can process line-by-line.
- **Real-world examples**:
  - event streams (“clickstream”)
  - data lake ingestion (one JSON record per line)
  - server/app telemetry

### `.log` (application logs)
- **What it optimizes for**: fast, append-only debugging/observability.
- **Downside**: not a universal format; often “key=value” text with conventions.
- **Real-world examples**:
  - incident response (identify spikes in 500s)
  - performance tuning (latency percentiles)

### `.ini` (config files)
- **What it optimizes for**: human-editable configuration with sections/keys.
- **Downside**: limited types; you must convert strings to numbers/booleans.
- **Real-world examples**:
  - app configuration for local/dev environments
  - feature flags or thresholds

### `.tsv` (Tab-Separated Values)
- **What it optimizes for**: tabular data where commas appear inside values more often; common in research.
- **Downside**: same typing and schema issues as CSV.
- **Real-world examples**:
  - exported analytics tables
  - dataset interchange in science/ML

### `.xml`
- **What it optimizes for**: structured documents with attributes and nested tags (historically popular in enterprise).
- **Downside**: verbose; many variations; needs careful parsing.
- **Real-world examples**:
  - legacy integrations (SOAP, old ERPs)
  - document-based formats (some feeds/exports)

## Common parsing pitfalls (the stuff that breaks real pipelines)

- **Encoding issues**: a file might not be UTF‑8; you may need to detect or specify encodings.
- **Missing or invalid fields**: blanks, `NULL`, `N/A`, malformed rows.
- **Inconsistent schemas over time**: columns added/removed, renamed keys, new JSON fields.
- **Floating point money**: store cents as integers or use `decimal.Decimal` if you need exactness.
- **Large files**: avoid `read()` for massive inputs; stream line-by-line.
- **Validation and error handling**: decide whether to “fail fast” or “skip bad records and log them”.

## What you’ll practice here

- Reading/writing files with `pathlib.Path`
- Parsing:
  - text + regex
  - CSV/TSV with `csv`
  - JSON with `json`
  - NDJSON line-by-line
  - INI with `configparser`
  - XML with `xml.etree.ElementTree`
- Building small, testable functions that power a mini “pipeline”

