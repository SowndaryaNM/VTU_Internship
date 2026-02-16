# LOG ANALYZER

The application server generates a log file daily:

* Thousands of lines
* Mixed INFO, ERROR, WARNING
* Occasional malformed entries
* Different users triggering events

Management wants:

* Total errors
* Error breakdown
* Top 3 most active users
* Exportable summary file
* Fail-safe behavior (no crash on bad logs)

This is classic DevOps automation.

---

# 📁 PROJECT STRUCTURE

```
log_analyzer/
│
├── main.py
├── logs/
│   └── app.log
├── modules/
│   ├── __init__.py
│   ├── parser.py
│   ├── analyzer.py
│   └── report.py
└── output/

```
# Sample Log Format

Use something like this:

2026-02-16 10:23:11 INFO User:101 Login successful

2026-02-16 10:25:19 ERROR User:203 Payment failed

2026-02-16 10:27:44 WARNING User:101 Slow response detected

2026-02-16 10:30:10 INFO User:305 Logout successful

BAD LINE WITHOUT FORMAT
```


# parser.py – Structured Extraction

## 🎯 Responsibility

Convert raw text lines → structured dictionary records

# analyzer.py – Core Analytics Engine

## 🎯 Responsibility

Business logic and metrics calculation.

# report.py – Output Generation

## 🎯 Responsibility

Convert results → human-readable report.

# main.py – Orchestration Layer

This is integration logic only.


# Example Final Output (log_summary.txt)

=== LOG ANALYSIS REPORT ===

Log Level Summary:
INFO: 12
ERROR: 4
WARNING: 3

Top Users:
User 101: 8 actions
User 203: 5 actions
User 305: 2 actions

Total Errors: 4
```

# Edge Cases You Should look out

* Empty log file
* File not found
* Corrupt lines
* Case-insensitive levels
* Huge file performance considerations
