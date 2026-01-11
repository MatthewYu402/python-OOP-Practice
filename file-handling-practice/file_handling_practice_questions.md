# File Handling Practice — Questions (25)

**How to use**

- Each question has a title and a Python code stub.
- Use only the Python standard library unless the prompt says otherwise.
- Prefer small, testable functions.
- Most questions reference files in `file-handling-practice/data/`.
- You can do these in the notebook (`file-handling-practice.ipynb`) or in your own `.py` file.

---

## 1) Read a text file (whole file)

Read `data/notes.txt` and return the entire contents as a single string.

```python
from pathlib import Path

def q01_read_notes_text(data_dir: Path) -> str:
    """
    Return the full contents of notes.txt as a string.
    """
    # TODO
    raise NotImplementedError
```

---

## 2) Count lines and non-empty lines

Using `data/notes.txt`, return a tuple: `(total_lines, non_empty_lines)`.

```python
from pathlib import Path

def q02_count_lines(data_dir: Path) -> tuple[int, int]:
    """
    Return (total_lines, non_empty_lines) for notes.txt.
    """
    # TODO
    raise NotImplementedError
```

---

## 3) Extract emails from text

Extract all email addresses from `data/notes.txt`. Return **unique** emails sorted ascending.

Hint: a simple regex is fine (it doesn’t need to be perfect).

```python
from pathlib import Path

def q03_extract_emails(data_dir: Path) -> list[str]:
    """
    Return unique emails found in notes.txt, sorted.
    """
    # TODO
    raise NotImplementedError
```

---

## 4) Unicode and “non-ASCII” detection

Read `data/unicode.txt` (UTF-8) and return all lines that contain any non-ASCII character.

```python
from pathlib import Path

def q04_non_ascii_lines(data_dir: Path) -> list[str]:
    """
    Return lines containing any character with ord(c) > 127.
    Preserve original line text (no trailing newline).
    """
    # TODO
    raise NotImplementedError
```

---

## 5) Parse a messy delimited file (robustness)

Parse `data/messy_delimited.txt` which uses `|` as a delimiter.

- The first row is a header.
- Skip invalid rows (wrong number of fields).
- Convert `user_id` to `int`.
- Convert `age` to `int` when present, otherwise `None`.

Return a list of dicts like `{"user_id": 1001, "name": "...", "age": 29, "country": "US"}`.

```python
from pathlib import Path

def q05_parse_messy_delimited(data_dir: Path) -> list[dict]:
    """
    Parse messy_delimited.txt into a list of typed dicts.
    Skip bad rows safely.
    """
    # TODO
    raise NotImplementedError
```

---

## 6) Read CSV into dictionaries

Read `data/people.csv` using `csv.DictReader` and return a list of row dicts (strings only).

```python
from pathlib import Path
import csv

def q06_read_people_csv_raw(data_dir: Path) -> list[dict[str, str]]:
    """
    Return rows from people.csv exactly as DictReader provides them.
    """
    # TODO
    raise NotImplementedError
```

---

## 7) Convert CSV fields to types

From `people.csv`, create typed records:

- `user_id`: int
- `age`: int | None (blank means None)
- `signup_date`: `datetime.date`

Return a list of dicts.

```python
from pathlib import Path
import datetime as dt

def q07_people_typed(data_dir: Path) -> list[dict]:
    """
    Return typed people records.
    """
    # TODO
    raise NotImplementedError
```

---

## 8) Filter + sort (CSV)

From your typed people records, return the names of users in the US, sorted alphabetically.

```python
from pathlib import Path

def q08_us_people_names_sorted(data_dir: Path) -> list[str]:
    """
    Return US-based people's names sorted ascending.
    """
    # TODO
    raise NotImplementedError
```

---

## 9) Parse orders CSV + compute revenue

Read `data/orders.csv` and compute **total paid revenue** as a float.

Only include rows where `status == "paid"`.

```python
from pathlib import Path

def q09_total_paid_revenue(data_dir: Path) -> float:
    """
    Return total revenue from paid orders.
    """
    # TODO
    raise NotImplementedError
```

---

## 10) Group by user (aggregation)

Compute **paid revenue per user_id** from `orders.csv`.

Return `dict[int, float]` mapping `user_id -> total_paid`.

```python
from pathlib import Path

def q10_paid_revenue_by_user(data_dir: Path) -> dict[int, float]:
    """
    Return paid revenue by user_id.
    """
    # TODO
    raise NotImplementedError
```

---

## 11) Join people + revenue

Create a list of customer summaries:

`{"user_id": 1001, "name": "Alice Nguyen", "total_paid": 52.47}`

Include only users that appear in `people.csv` (ignore unknown user_ids).
Sort by `total_paid` descending, then by `user_id` ascending.

```python
from pathlib import Path

def q11_customer_revenue_summaries(data_dir: Path) -> list[dict]:
    """
    Join people.csv + orders.csv into customer revenue summaries.
    """
    # TODO
    raise NotImplementedError
```

---

## 12) Write a CSV report (output files)

Write the summaries from Q11 to `outputs/customer_revenue.csv` with columns:

`user_id,name,total_paid`

Format `total_paid` to 2 decimal places.

Return the output path.

```python
from pathlib import Path

def q12_write_customer_revenue_csv(data_dir: Path, output_dir: Path) -> Path:
    """
    Write outputs/customer_revenue.csv and return its path.
    """
    # TODO
    raise NotImplementedError
```

---

## 13) Read JSON + filter

Read `data/products.json` and return a list of `sku` values that are `in_stock == True`.

```python
from pathlib import Path

def q13_in_stock_skus(data_dir: Path) -> list[str]:
    """
    Return in-stock SKUs from products.json.
    """
    # TODO
    raise NotImplementedError
```

---

## 14) JSON aggregation (nested fields)

From `products.json`, compute average `price_usd` by department (e.g. `"Books"`, `"Home"`).

Return `dict[str, float]`.

```python
from pathlib import Path

def q14_avg_price_by_department(data_dir: Path) -> dict[str, float]:
    """
    Return average price_usd by department.
    """
    # TODO
    raise NotImplementedError
```

---

## 15) Normalize JSON fields

From `data/users.json`, return a dict mapping `user_id -> primary_email`.

Rule: primary email is the first entry in the `emails` list.

```python
from pathlib import Path

def q15_primary_email_by_user_id(data_dir: Path) -> dict[int, str]:
    """
    Return {user_id: primary_email}.
    """
    # TODO
    raise NotImplementedError
```

---

## 16) Write normalized JSON output

Read `users.json` and write `outputs/users_normalized.json` containing a list of objects:

`{"user_id": 1001, "name": "...", "country": "US", "emails": ["..."]}`

Return the output path.

```python
from pathlib import Path

def q16_write_users_normalized(data_dir: Path, output_dir: Path) -> Path:
    """
    Write normalized users JSON to outputs/users_normalized.json.
    """
    # TODO
    raise NotImplementedError
```

---

## 17) Read NDJSON (streaming)

Read `data/events.ndjson` line-by-line and return a dict counting events by type:

Example: `{"page_view": 5, "purchase": 3, ...}`

```python
from pathlib import Path

def q17_event_counts(data_dir: Path) -> dict[str, int]:
    """
    Return counts of each event type in events.ndjson.
    """
    # TODO
    raise NotImplementedError
```

---

## 18) NDJSON filtering + numeric sum

From `events.ndjson`, compute the sum of `total_usd` for `event == "purchase"`.

```python
from pathlib import Path

def q18_total_purchase_amount(data_dir: Path) -> float:
    """
    Return sum(total_usd) for purchase events.
    """
    # TODO
    raise NotImplementedError
```

---

## 19) Parse semi-structured log lines

Parse `data/server.log` lines into dicts with keys:

`ts, level, request_id, method, path, status, ms`

Convert `status` and `ms` to `int`.

```python
from pathlib import Path

def q19_parse_server_log(data_dir: Path) -> list[dict]:
    """
    Parse server.log into structured records.
    """
    # TODO
    raise NotImplementedError
```

---

## 20) Compute error rate from logs

Using your parsed log records, compute:

`error_rate = (count of status >= 500) / (total log lines)`

Return a float.

```python
from pathlib import Path

def q20_error_rate(data_dir: Path) -> float:
    """
    Return fraction of requests with status >= 500.
    """
    # TODO
    raise NotImplementedError
```

---

## 21) Parse INI config

Read `data/app.ini` using `configparser` and return a dict with:

- `app_name` (string)
- `data_dir` (string)
- `max_error_rate` (float)
- `enable_experimental` (bool)

```python
from pathlib import Path

def q21_read_ini_config(data_dir: Path) -> dict:
    """
    Parse app.ini and return selected config values with proper types.
    """
    # TODO
    raise NotImplementedError
```

---

## 22) Parse TSV and compute metrics

Read `data/metrics.tsv` and:

- compute `avg_conversion_rate` across all rows
- compute `max_sessions_by_user_id` (max sessions seen for each user)

Return `(avg_conversion_rate, max_sessions_by_user_id)`.

```python
from pathlib import Path

def q22_metrics_from_tsv(data_dir: Path) -> tuple[float, dict[int, int]]:
    """
    Return (avg_conversion_rate, max_sessions_by_user_id).
    """
    # TODO
    raise NotImplementedError
```

---

## 23) Parse XML into records

Read `data/sample.xml` and return a list of dicts:

`{"sku": "...", "name": "...", "department": "...", "price_usd": 29.99}`

```python
from pathlib import Path

def q23_products_from_xml(data_dir: Path) -> list[dict]:
    """
    Parse sample.xml into a list of product dicts.
    """
    # TODO
    raise NotImplementedError
```

---

## 24) Convert XML catalog to JSON output

Using Q23, write `outputs/catalog_from_xml.json` in this shape:

```json
{
  "source": "sample.xml",
  "product_count": 2,
  "products": [...]
}
```

Return the output path.

```python
from pathlib import Path

def q24_write_catalog_from_xml(data_dir: Path, output_dir: Path) -> Path:
    """
    Write outputs/catalog_from_xml.json and return its path.
    """
    # TODO
    raise NotImplementedError
```

---

## 25) Capstone: mini “pipeline” summary report

Create `run_pipeline()` that reads multiple file types and writes a summary JSON:

- Inputs:
  - `people.csv`
  - `orders.csv`
  - `events.ndjson`
- Compute:
  - `people_count`
  - `paid_revenue`
  - `top_spender` object: `{user_id, name, total_paid}`
  - `error_codes` (unique, sorted) from `events.ndjson` where `event == "error"`
- Output:
  - write `outputs/summary.json`
- Return the summary dict.

Hint: build this by reusing earlier functions (don’t copy/paste logic).

```python
from pathlib import Path

def q25_run_pipeline(data_dir: Path, output_dir: Path) -> dict:
    """
    Write outputs/summary.json and return the summary dict.
    """
    # TODO
    raise NotImplementedError
```

