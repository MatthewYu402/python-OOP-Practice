# Python Regular Expressions (Regex) Practice

This branch is a focused practice track for **Python regular expressions** using the built-in `re` module. The goal is to help you learn how regex works and how to use it to **search, extract, validate, transform**, and **parse** text (including file contents).

---

## What regex is (and isn’t)

Regex is a compact “pattern language” for matching text.

- **Great for**: finding patterns, extracting parts, validating formats, rewriting text, parsing semi-structured logs.
- **Not great for**: parsing fully nested structures (e.g., arbitrary nested parentheses/HTML) without careful constraints.

In Python, regex lives in the `re` module:

- `re.search()` – find **first** match anywhere in the string
- `re.match()` – match only at the **start** of the string
- `re.fullmatch()` – the pattern must match the **entire** string
- `re.findall()` – return **all** matches (often as a list of strings/tuples)
- `re.finditer()` – iterate match objects (better for positions + groups)
- `re.sub()` – replace matches
- `re.split()` – split by a pattern
- `re.compile()` – precompile patterns (faster + reusable)

---

## Core syntax you should know

### Literal characters
Most characters match themselves.

- `cat` matches `cat`
- To match special characters literally, escape them (e.g., `\.` to match a dot).

### “Any character”
- `.` matches any character except newline (unless `re.DOTALL` is used).

### Character classes
Match **one** character from a set:

- `[abc]` – `a` or `b` or `c`
- `[a-z]` – any lowercase letter
- `[^0-9]` – anything **except** a digit (`^` inside `[]` means negation)

Common shorthands:

- `\d` digit, `\D` non-digit
- `\w` “word char” (letters/digits/underscore), `\W` non-word
- `\s` whitespace, `\S` non-whitespace

### Quantifiers (how many)
Apply to the token right before them:

- `*` – 0 or more
- `+` – 1 or more
- `?` – 0 or 1
- `{m}` – exactly m
- `{m,}` – at least m
- `{m,n}` – between m and n

Greedy vs lazy:

- `+`, `*`, `{m,n}` are **greedy** by default (match as much as possible)
- Add `?` to make them **lazy**: `+?`, `*?`, `{m,n}?`

### Anchors (where)
Anchors don’t consume characters; they match positions.

- `^` start of string (or start of line with `re.MULTILINE`)
- `$` end of string (or end of line with `re.MULTILINE`)
- `\b` word boundary (transition between `\w` and `\W`)

### Alternation (“or”)
- `cat|dog` matches either `cat` or `dog`
- Use parentheses to control scope: `(?:cat|dog)s?`

### Groups and capturing
Parentheses create groups:

- `(abc)` captures text for later retrieval
- `(?:abc)` is a **non-capturing** group (grouping without capture)
- Named groups: `(?P<name>...)`

Access captures via:

- `match.group(1)`, `match.group("name")`

### Lookarounds (advanced but powerful)
Lookarounds assert context without consuming characters:

- `(?=...)` positive lookahead
- `(?!...)` negative lookahead
- `(?<=...)` positive lookbehind (fixed-width in Python)
- `(?<!...)` negative lookbehind (fixed-width in Python)

---

## Flags you’ll actually use

You can pass flags to most `re.*` calls:

- `re.IGNORECASE` (`re.I`) – case-insensitive matching
- `re.MULTILINE` (`re.M`) – `^`/`$` work per line
- `re.DOTALL` (`re.S`) – `.` matches newlines too
- `re.VERBOSE` (`re.X`) – whitespace + comments allowed (great for readability)

Example:

```python
import re

pattern = re.compile(r"""
    ^\s*              # optional leading whitespace
    (?P<key>[A-Z_]+)  # KEY
    \s*=\s*           # equals with optional spaces
    (?P<value>.+?)    # value (lazy)
    \s*$              # optional trailing whitespace
""", re.VERBOSE)

m = pattern.search("  USER_ID = 42  ")
assert m.group("key") == "USER_ID"
assert m.group("value") == "42"
```

---

## The `r"..."` (raw string) habit

Prefer raw strings for regex patterns:

- `r"\d+"` is easier than `"\\d+"`

Note: raw strings still process quotes and cannot end with a single backslash.

---

## Practical examples (not the same as the practice problems)

### Extract all hashtags from text

```python
import re

text = "Shipping #NextDay is now available in #NYC and #SF!"
tags = re.findall(r"#([A-Za-z]\w*)", text)
assert tags == ["NextDay", "NYC", "SF"]
```

### Validate a simple identifier (whole string)

```python
import re

def is_identifier(s: str) -> bool:
    return re.fullmatch(r"[A-Za-z_]\w*", s) is not None

assert is_identifier("snake_case_2")
assert not is_identifier("2fast")
```

### Rewrite repeated spaces into one space

```python
import re

clean = re.sub(r"[ \t]{2,}", " ", "Too    many\t\tspaces")
assert clean == "Too many spaces"
```

### Parsing files: a simple pattern-driven loop

Regex works best when you combine it with line-by-line reading:

```python
import re

line_re = re.compile(r"^\[(?P<level>[A-Z]+)\]\s+(?P<msg>.+)$")

with open("app.log", "r", encoding="utf-8") as f:
    for line in f:
        m = line_re.search(line.rstrip("\n"))
        if not m:
            continue
        level = m.group("level")
        msg = m.group("msg")
        # ... do something useful ...
```

---

## Common pitfalls (worth memorizing)

- **`re.match` vs `re.search`**: `match` is anchored at the start; `search` scans.
- **Greediness surprises**: prefer explicit patterns and consider lazy quantifiers.
- **Over-capturing**: use `(?:...)` when you don’t need captured groups.
- **Escaping**: remember special characters like `. ^ $ * + ? { } [ ] \ | ( )`
- **Validation**: use `re.fullmatch` when you mean “the entire string must conform”.

---

## Practice

- `regex_practice.ipynb` – ~30 problems from easy → medium → hard
- `regex_practice_solutions.md` – complete solutions (peek only after trying!)
