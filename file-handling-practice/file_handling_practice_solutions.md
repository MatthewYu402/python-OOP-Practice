# File Handling Practice — Solutions

These solutions use only the Python standard library and are written to be readable and “real-world robust” (basic validation, types, and safe parsing).

If you want a harder challenge, try:

- adding stricter validation (raise on malformed rows instead of skipping)
- using `decimal.Decimal` for currency instead of float
- making the parsers streaming-only (avoid reading whole files)

---

## Shared helpers (used by multiple solutions)

```python
from __future__ import annotations

from pathlib import Path
import csv
import datetime as dt
import json
import re
from collections import Counter, defaultdict
import configparser
import xml.etree.ElementTree as ET
```

---

## 1) Read a text file (whole file)

```python
from pathlib import Path

def q01_read_notes_text(data_dir: Path) -> str:
    return (data_dir / "notes.txt").read_text(encoding="utf-8")
```

---

## 2) Count lines and non-empty lines

```python
from pathlib import Path

def q02_count_lines(data_dir: Path) -> tuple[int, int]:
    lines = (data_dir / "notes.txt").read_text(encoding="utf-8").splitlines()
    non_empty = sum(1 for l in lines if l.strip())
    return len(lines), non_empty
```

---

## 3) Extract emails from text

```python
from pathlib import Path
import re

def q03_extract_emails(data_dir: Path) -> list[str]:
    text = (data_dir / "notes.txt").read_text(encoding="utf-8")
    # Simple, practical regex for learning purposes.
    emails = re.findall(r"[\\w.+'-]+@[\\w.-]+\\.[A-Za-z]{2,}", text)
    return sorted(set(emails))
```

---

## 4) Unicode and “non-ASCII” detection

```python
from pathlib import Path

def q04_non_ascii_lines(data_dir: Path) -> list[str]:
    lines = (data_dir / "unicode.txt").read_text(encoding="utf-8").splitlines()
    out: list[str] = []
    for line in lines:
        if any(ord(c) > 127 for c in line):
            out.append(line)
    return out
```

---

## 5) Parse a messy delimited file (robustness)

```python
from pathlib import Path

def q05_parse_messy_delimited(data_dir: Path) -> list[dict]:
    path = data_dir / "messy_delimited.txt"
    lines = path.read_text(encoding="utf-8").splitlines()
    if not lines:
        return []

    header = lines[0].split("|")
    expected = len(header)

    rows: list[dict] = []
    for line in lines[1:]:
        parts = line.split("|")
        if len(parts) != expected:
            continue
        raw = dict(zip(header, parts))

        user_id = int(raw["user_id"])
        age_raw = (raw.get("age") or "").strip()
        age = int(age_raw) if age_raw else None

        rows.append(
            {
                "user_id": user_id,
                "name": raw["name"],
                "age": age,
                "country": raw["country"],
            }
        )

    return rows
```

---

## 6) Read CSV into dictionaries

```python
from pathlib import Path
import csv

def q06_read_people_csv_raw(data_dir: Path) -> list[dict[str, str]]:
    path = data_dir / "people.csv"
    with open(path, newline="", encoding="utf-8") as f:
        return list(csv.DictReader(f))
```

---

## 7) Convert CSV fields to types

```python
from pathlib import Path
import csv
import datetime as dt

def q07_people_typed(data_dir: Path) -> list[dict]:
    path = data_dir / "people.csv"
    out: list[dict] = []
    with open(path, newline="", encoding="utf-8") as f:
        for row in csv.DictReader(f):
            age_raw = (row.get("age") or "").strip()
            out.append(
                {
                    "user_id": int(row["user_id"]),
                    "name": row["name"],
                    "email": row["email"],
                    "signup_date": dt.date.fromisoformat(row["signup_date"]),
                    "country": row["country"],
                    "age": int(age_raw) if age_raw else None,
                }
            )
    return out
```

---

## 8) Filter + sort (CSV)

```python
from pathlib import Path

def q08_us_people_names_sorted(data_dir: Path) -> list[str]:
    people = q07_people_typed(data_dir)
    return sorted([p["name"] for p in people if p["country"] == "US"])
```

---

## 9) Parse orders CSV + compute revenue

```python
from pathlib import Path
import csv

def q09_total_paid_revenue(data_dir: Path) -> float:
    path = data_dir / "orders.csv"
    total = 0.0
    with open(path, newline="", encoding="utf-8") as f:
        for row in csv.DictReader(f):
            if row["status"] == "paid":
                total += float(row["total_usd"])
    return total
```

---

## 10) Group by user (aggregation)

```python
from pathlib import Path
import csv
from collections import defaultdict

def q10_paid_revenue_by_user(data_dir: Path) -> dict[int, float]:
    path = data_dir / "orders.csv"
    totals: dict[int, float] = defaultdict(float)
    with open(path, newline="", encoding="utf-8") as f:
        for row in csv.DictReader(f):
            if row["status"] != "paid":
                continue
            user_id = int(row["user_id"])
            totals[user_id] += float(row["total_usd"])
    return dict(totals)
```

---

## 11) Join people + revenue

```python
from pathlib import Path

def q11_customer_revenue_summaries(data_dir: Path) -> list[dict]:
    people = q07_people_typed(data_dir)
    paid_by_user = q10_paid_revenue_by_user(data_dir)

    name_by_id = {p["user_id"]: p["name"] for p in people}

    out: list[dict] = []
    for user_id, total_paid in paid_by_user.items():
        if user_id not in name_by_id:
            continue
        out.append({"user_id": user_id, "name": name_by_id[user_id], "total_paid": total_paid})

    out.sort(key=lambda r: (-r["total_paid"], r["user_id"]))
    return out
```

---

## 12) Write a CSV report (output files)

```python
from pathlib import Path
import csv

def q12_write_customer_revenue_csv(data_dir: Path, output_dir: Path) -> Path:
    output_dir.mkdir(parents=True, exist_ok=True)
    out_path = output_dir / "customer_revenue.csv"

    rows = q11_customer_revenue_summaries(data_dir)
    with open(out_path, "w", newline="", encoding="utf-8") as f:
        writer = csv.DictWriter(f, fieldnames=["user_id", "name", "total_paid"])
        writer.writeheader()
        for r in rows:
            writer.writerow(
                {
                    "user_id": r["user_id"],
                    "name": r["name"],
                    "total_paid": f"{r['total_paid']:.2f}",
                }
            )
    return out_path
```

---

## 13) Read JSON + filter

```python
from pathlib import Path
import json

def q13_in_stock_skus(data_dir: Path) -> list[str]:
    data = json.loads((data_dir / "products.json").read_text(encoding="utf-8"))
    return [p["sku"] for p in data["products"] if p.get("in_stock") is True]
```

---

## 14) JSON aggregation (nested fields)

```python
from pathlib import Path
import json
from collections import defaultdict

def q14_avg_price_by_department(data_dir: Path) -> dict[str, float]:
    data = json.loads((data_dir / "products.json").read_text(encoding="utf-8"))

    sums: dict[str, float] = defaultdict(float)
    counts: dict[str, int] = defaultdict(int)

    for p in data["products"]:
        dept = p["category"]["department"]
        sums[dept] += float(p["price_usd"])
        counts[dept] += 1

    return {dept: sums[dept] / counts[dept] for dept in sums}
```

---

## 15) Normalize JSON fields

```python
from pathlib import Path
import json

def q15_primary_email_by_user_id(data_dir: Path) -> dict[int, str]:
    data = json.loads((data_dir / "users.json").read_text(encoding="utf-8"))
    out: dict[int, str] = {}
    for u in data["users"]:
        emails = u.get("emails") or []
        out[int(u["user_id"])] = emails[0] if emails else ""
    return out
```

---

## 16) Write normalized JSON output

```python
from pathlib import Path
import json

def q16_write_users_normalized(data_dir: Path, output_dir: Path) -> Path:
    output_dir.mkdir(parents=True, exist_ok=True)
    out_path = output_dir / "users_normalized.json"

    data = json.loads((data_dir / "users.json").read_text(encoding="utf-8"))
    normalized = []
    for u in data["users"]:
        normalized.append(
            {
                "user_id": int(u["user_id"]),
                "name": u["name"],
                "country": u["address"]["country"],
                "emails": u.get("emails") or [],
            }
        )

    out_path.write_text(json.dumps(normalized, indent=2, ensure_ascii=False) + "\n", encoding="utf-8")
    return out_path
```

---

## 17) Read NDJSON (streaming)

```python
from pathlib import Path
import json
from collections import Counter

def q17_event_counts(data_dir: Path) -> dict[str, int]:
    counts: Counter[str] = Counter()
    with open(data_dir / "events.ndjson", encoding="utf-8") as f:
        for line in f:
            line = line.strip()
            if not line:
                continue
            obj = json.loads(line)
            counts[obj["event"]] += 1
    return dict(counts)
```

---

## 18) NDJSON filtering + numeric sum

```python
from pathlib import Path
import json

def q18_total_purchase_amount(data_dir: Path) -> float:
    total = 0.0
    with open(data_dir / "events.ndjson", encoding="utf-8") as f:
        for line in f:
            line = line.strip()
            if not line:
                continue
            obj = json.loads(line)
            if obj.get("event") == "purchase":
                total += float(obj.get("total_usd", 0.0))
    return total
```

---

## 19) Parse semi-structured log lines

```python
from pathlib import Path

def q19_parse_server_log(data_dir: Path) -> list[dict]:
    records: list[dict] = []
    with open(data_dir / "server.log", encoding="utf-8") as f:
        for line in f:
            line = line.strip()
            if not line:
                continue
            parts = line.split()
            ts, level = parts[0], parts[1]

            kv = {}
            for token in parts[2:]:
                if "=" in token:
                    k, v = token.split("=", 1)
                    kv[k] = v

            records.append(
                {
                    "ts": ts,
                    "level": level,
                    "request_id": kv["request_id"],
                    "method": kv["method"],
                    "path": kv["path"],
                    "status": int(kv["status"]),
                    "ms": int(kv["ms"]),
                }
            )
    return records
```

---

## 20) Compute error rate from logs

```python
from pathlib import Path

def q20_error_rate(data_dir: Path) -> float:
    recs = q19_parse_server_log(data_dir)
    if not recs:
        return 0.0
    err = sum(1 for r in recs if r["status"] >= 500)
    return err / len(recs)
```

---

## 21) Parse INI config

```python
from pathlib import Path
import configparser

def q21_read_ini_config(data_dir: Path) -> dict:
    cp = configparser.ConfigParser()
    cp.read(data_dir / "app.ini", encoding="utf-8")
    return {
        "app_name": cp["app"]["name"],
        "data_dir": cp["paths"]["data_dir"],
        "max_error_rate": float(cp["thresholds"]["max_error_rate"]),
        "enable_experimental": cp.getboolean("features", "enable_experimental"),
    }
```

---

## 22) Parse TSV and compute metrics

```python
from pathlib import Path
from collections import defaultdict

def q22_metrics_from_tsv(data_dir: Path) -> tuple[float, dict[int, int]]:
    path = data_dir / "metrics.tsv"
    lines = path.read_text(encoding="utf-8").splitlines()
    if len(lines) <= 1:
        return 0.0, {}

    max_sessions: dict[int, int] = defaultdict(int)
    crs: list[float] = []

    for line in lines[1:]:
        date_s, user_id_s, sessions_s, cr_s = line.split("\\t")
        user_id = int(user_id_s)
        sessions = int(sessions_s)
        cr = float(cr_s)
        crs.append(cr)
        if sessions > max_sessions[user_id]:
            max_sessions[user_id] = sessions

    return sum(crs) / len(crs), dict(max_sessions)
```

---

## 23) Parse XML into records

```python
from pathlib import Path
import xml.etree.ElementTree as ET

def q23_products_from_xml(data_dir: Path) -> list[dict]:
    root = ET.fromstring((data_dir / "sample.xml").read_text(encoding="utf-8"))
    out: list[dict] = []
    for p in root.findall("product"):
        out.append(
            {
                "sku": p.attrib["sku"],
                "name": p.findtext("name") or "",
                "department": p.findtext("department") or "",
                "price_usd": float(p.findtext("price_usd") or 0.0),
            }
        )
    return out
```

---

## 24) Convert XML catalog to JSON output

```python
from pathlib import Path
import json

def q24_write_catalog_from_xml(data_dir: Path, output_dir: Path) -> Path:
    output_dir.mkdir(parents=True, exist_ok=True)
    out_path = output_dir / "catalog_from_xml.json"

    products = q23_products_from_xml(data_dir)
    payload = {"source": "sample.xml", "product_count": len(products), "products": products}
    out_path.write_text(json.dumps(payload, indent=2, ensure_ascii=False) + "\\n", encoding="utf-8")
    return out_path
```

---

## 25) Capstone: mini “pipeline” summary report

```python
from pathlib import Path
import json

def q25_run_pipeline(data_dir: Path, output_dir: Path) -> dict:
    output_dir.mkdir(parents=True, exist_ok=True)
    out_path = output_dir / "summary.json"

    # Reuse earlier building blocks
    people = q07_people_typed(data_dir)
    paid_by_user = q10_paid_revenue_by_user(data_dir)

    name_by_id = {p["user_id"]: p["name"] for p in people}

    # top spender among known people
    spender_candidates = [
        (user_id, total) for user_id, total in paid_by_user.items() if user_id in name_by_id
    ]
    top_user_id, top_total = max(spender_candidates, key=lambda kv: kv[1]) if spender_candidates else (None, 0.0)

    # error codes from NDJSON
    error_codes = set()
    with open(data_dir / "events.ndjson", encoding="utf-8") as f:
        for line in f:
            line = line.strip()
            if not line:
                continue
            obj = json.loads(line)
            if obj.get("event") == "error" and obj.get("code"):
                error_codes.add(obj["code"])

    summary = {
        "people_count": len(people),
        "paid_revenue": q09_total_paid_revenue(data_dir),
        "top_spender": {
            "user_id": top_user_id,
            "name": name_by_id.get(top_user_id, "") if top_user_id is not None else "",
            "total_paid": top_total,
        },
        "error_codes": sorted(error_codes),
    }

    out_path.write_text(json.dumps(summary, indent=2, ensure_ascii=False) + "\\n", encoding="utf-8")
    return summary
```

