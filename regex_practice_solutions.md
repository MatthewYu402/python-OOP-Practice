# Regex Practice Solutions

This file contains reference solutions for the problems in `regex_practice.ipynb`.

> Tip: Try solving first without looking. When you do look, compare **your pattern choices** and **why they work**.

---

## Easy Set 1 (Problems 1–3)

### 1) Extract integers

```python
import re
from typing import List

def extract_integers(text: str) -> List[int]:
    return [int(x) for x in re.findall(r"[+-]?\d+", text)]
```

### 2) Validate a ZIP code

```python
import re

def is_valid_zip(zip_code: str) -> bool:
    return re.fullmatch(r"\d{5}(?:-\d{4})?", zip_code) is not None
```

### 3) Find capitalized words

```python
import re
from typing import List

def capitalized_words(text: str) -> List[str]:
    return re.findall(r"\b[A-Z][a-z]+\b", text)
```

---

## Easy Set 2 (Problems 4–6)

### 4) Extract email domains

```python
import re
from typing import List

def extract_email_domains(text: str) -> List[str]:
    return re.findall(r"[A-Za-z0-9._%+-]+@([A-Za-z0-9.-]+\.[A-Za-z]{2,})", text)
```

### 5) Squash repeated exclamation marks

```python
import re

def squash_exclamations(text: str) -> str:
    return re.sub(r"!{2,}", "!", text)
```

### 6) Split a loose CSV line

```python
import re
from typing import List

def split_loose_csv(line: str) -> List[str]:
    s = line.strip()
    # Split on commas with optional surrounding whitespace.
    parts = re.split(r"\s*,\s*", s)
    return parts
```

---

## Easy Set 3 (Problems 7–9)

### 7) Detect a hex color

```python
import re

def contains_hex_color(text: str) -> bool:
    # (?![0-9A-Fa-f]) prevents matching extra hex digits after a valid length.
    return re.search(r"#[0-9A-Fa-f]{3}(?:[0-9A-Fa-f]{3})?(?![0-9A-Fa-f])", text) is not None
```

### 8) Extract YYYY/MM/DD dates

```python
import re
from typing import List

def extract_slash_dates(text: str) -> List[str]:
    pattern = r"\b\d{4}/(?:0[1-9]|1[0-2])/(?:0[1-9]|[12]\d|3[01])\b"
    return re.findall(pattern, text)
```

### 9) Find long runs of a character

```python
import re
from typing import List, Tuple

def find_long_runs(text: str) -> List[Tuple[str, int]]:
    out: List[Tuple[str, int]] = []
    for m in re.finditer(r"(.)\1{2,}", text):
        run = m.group(0)
        out.append((run[0], len(run)))
    return out
```

---

## Easy Set 4 (Problems 10–12)

### 10) Normalize phone numbers

```python
import re
from typing import List

def normalize_phone_numbers(text: str) -> List[str]:
    pattern = re.compile(r"\b(?:\((\d{3})\)\s*|(\d{3})[- ]?)(\d{3})[- ]?(\d{4})\b")
    out: List[str] = []
    for m in pattern.finditer(text):
        area = m.group(1) or m.group(2)
        out.append(area + m.group(3) + m.group(4))
    return out
```

### 11) Extract @mentions

```python
import re
from typing import List

def extract_mentions(text: str) -> List[str]:
    return re.findall(r"@([A-Za-z][A-Za-z0-9_]{1,15})\b", text)
```

### 12) Strip simple HTML tags

```python
import re

def strip_simple_html(text: str) -> str:
    return re.sub(r"<[^>]*>", "", text)
```

---

## Medium Set 5 (Problems 13–15)

### 13) Parse simple logs

```python
import re
from typing import List, Dict

def parse_simple_logs(text: str) -> List[Dict[str, str]]:
    pattern = re.compile(r"^(INFO|WARN|ERROR|DEBUG)\s+(\d{4}-\d{2}-\d{2})\s+(.*)$", re.MULTILINE)
    out: List[Dict[str, str]] = []
    for level, date, msg in pattern.findall(text):
        out.append({"level": level, "date": date, "message": msg})
    return out
```

### 14) Extract double-quoted strings (with escapes)

```python
import re
from typing import List

def extract_double_quoted(text: str) -> List[str]:
    return re.findall(r"\"((?:\\.|[^\"\\])*)\"", text)
```

### 15) Validate IPv4

```python
import re

def is_valid_ipv4(addr: str) -> bool:
    octet = r"(?:25[0-5]|2[0-4]\d|1?\d?\d)"
    return re.fullmatch(rf"{octet}\.{octet}\.{octet}\.{octet}", addr) is not None
```

---

## Medium Set 6 (Problems 16–18)

### 16) Parse semicolon key/value pairs

```python
import re
from typing import Dict

_kv_re = re.compile(r'''
    (?P<key>[A-Za-z_]\w*)          # key
    \s*=\s*
    (?P<val>
        "(?:\\.|[^"\\])*"         # quoted value with escapes
        |
        [^;]*                     # unquoted value up to ';'
    )
    (?:\s*;\s*|$)                 # separator or end
''', re.VERBOSE)

def parse_semicolon_kv(line: str) -> Dict[str, str]:
    out: Dict[str, str] = {}
    for m in _kv_re.finditer(line):
        key = m.group("key")
        raw = m.group("val").strip()
        if raw.startswith('"') and raw.endswith('"'):
            raw = raw[1:-1]
            raw = raw.replace(r"\\", "\\").replace(r"\"", "\"")
        out[key] = raw.strip()
    return out
```

### 17) Swap "Last, First" → "First Last"

```python
import re

def swap_name(name: str) -> str:
    return re.sub(r"^\s*([^,]+)\s*,\s*([^,]+)\s*$", r"\2 \1", name)
```

### 18) Detect repeated consecutive words

```python
import re
from typing import List

def repeated_words(text: str) -> List[str]:
    out: List[str] = []
    pattern = re.compile(r"\b(\w+)\b(?:\s+\1\b)", re.IGNORECASE)
    for m in pattern.finditer(text):
        out.append(m.group(1))
    return out
```

---

## Medium Set 7 (Problems 19–21)

### 19) Extract Markdown links

```python
import re
from typing import List, Tuple

def extract_markdown_links(md: str) -> List[Tuple[str, str]]:
    return re.findall(r"\[([^\]\n]+)\]\(([^)\s]+)\)", md)
```

### 20) Find Python function definitions

```python
import re
from typing import List

def python_def_names(code: str) -> List[str]:
    pattern = re.compile(r"^(?!\s*#)\s*(?:async\s+)?def\s+([A-Za-z_]\w*)\s*\(", re.MULTILINE)
    return pattern.findall(code)
```

### 21) Mask card-like digit sequences

```python
import re

_card_re = re.compile(r"\b(?:\d[ -]?){12,15}\d\b")

def mask_card_numbers(text: str) -> str:
    def repl(m: re.Match) -> str:
        s = m.group(0)
        digits = sum(ch.isdigit() for ch in s)
        keep = 4
        to_mask = max(0, digits - keep)
        out = []
        for ch in s:
            if ch.isdigit() and to_mask > 0:
                out.append("*")
                to_mask -= 1
            else:
                out.append(ch)
        return "".join(out)
    return _card_re.sub(repl, text)
```

---

## Hard Set 8 (Problems 22–24)

### 22) Strong password check

```python
import re

def is_strong_password(pw: str) -> bool:
    pattern = r"^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[!@#$%^&*])\S{8,}$"
    return re.fullmatch(pattern, pw) is not None
```

### 23) Extract US currency amounts

```python
import re
from typing import List

def extract_us_currency(text: str) -> List[str]:
    # For this exercise, 4+ digits must use commas (so "$1234.56" is rejected).
    pattern = r"\$(?:\d{1,3}(?:,\d{3})+|\d{1,3})(?:\.\d{2})?"
    return re.findall(pattern, text)
```

### 24) Replace a standalone word

```python
import re

def replace_standalone_word(text: str, word: str, repl: str) -> str:
    return re.sub(rf"\b{re.escape(word)}\b", repl, text)
```

---

## Hard Set 9 (Problems 25–27)

### 25) Parse ISO-8601 datetime

```python
import re
from typing import Optional, Dict

_iso_re = re.compile(
    r"^(?P<year>\d{4})-(?P<month>\d{2})-(?P<day>\d{2})"
    r"[T ](?P<hour>\d{2}):(?P<minute>\d{2})(?::(?P<second>\d{2}))?"
    r"(?P<tz>Z|[+-]\d{2}:\d{2})$"
)

def parse_iso8601(dt: str) -> Optional[Dict[str, str]]:
    m = _iso_re.fullmatch(dt)
    if not m:
        return None
    d = m.groupdict()
    if d["second"] is None:
        d["second"] = "00"
    return {
        "year": d["year"],
        "month": d["month"],
        "day": d["day"],
        "hour": d["hour"],
        "minute": d["minute"],
        "second": d["second"],
        "tz": d["tz"],
    }
```

### 26) Words that start and end with the same letter

```python
import re
from typing import List

def words_same_ends(text: str) -> List[str]:
    pattern = re.compile(r"\b([A-Za-z])[A-Za-z]*\1\b", re.IGNORECASE)
    return [m.group(0) for m in pattern.finditer(text)]
```

### 27) Overlapping 3-character palindromes

```python
import re
from typing import List

def overlapping_pal3(text: str) -> List[str]:
    pattern = re.compile(r"(?=(([A-Za-z])([A-Za-z])\2))")
    return [m.group(1) for m in pattern.finditer(text)]
```

---

## Hard Set 10 (Problems 28–30)

### 28) Parse `Name <email>`

```python
import re
from typing import Optional, Tuple

_name_email_re = re.compile(
    r'^\s*(?:(?:"(?P<qname>[^"]+)"|(?P<name>[^<"]+?))\s*)?'
    r'<(?P<email>[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,})>\s*$'
)

def parse_name_email(s: str) -> Optional[Tuple[str, str]]:
    m = _name_email_re.fullmatch(s)
    if not m:
        return None
    name = (m.group("qname") or m.group("name") or "").strip()
    return (name, m.group("email"))
```

### 29) Extract http/https URLs

```python
import re
from typing import List

def extract_urls(text: str) -> List[str]:
    raw = re.findall(r"https?://\S+", text)
    return [u.rstrip(".,)]!?:;") for u in raw]
```

### 30) Validate Roman numerals (1–3999)

```python
import re

_roman_re = re.compile(r"^M{0,3}(CM|CD|D?C{0,3})(XC|XL|L?X{0,3})(IX|IV|V?I{0,3})$")

def is_valid_roman(s: str) -> bool:
    return bool(s) and _roman_re.fullmatch(s) is not None
```

