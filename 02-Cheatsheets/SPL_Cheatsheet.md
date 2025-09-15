# 📘 Splunk SPL (Search Processing Language) Cheatsheet

A quick reference guide for using Splunk SPL effectively. Perfect for security analysts, engineers, and developers who want fast, actionable queries for logs, metrics, and events.

---

## 🔍 Basic Search Syntax

```spl
index=main                             # Search in 'main' index
sourcetype=access_combined            # Filter by sourcetype
host=webserver01                      # Filter by host
status=404                            # Filter for HTTP 404 errors
"login failed"                        # Exact phrase match
field=value*                          # Wildcard match
search error OR fail                  # Boolean logic (OR, AND, NOT)
```

---

## 🧰 Common SPL Commands

| Command     | Description                           |
|-------------|---------------------------------------|
| `stats`     | Aggregations (count, avg, sum)        |
| `timechart` | Time-series visualizations            |
| `top`       | Most frequent values                  |
| `rare`      | Least frequent values                 |
| `table`     | Display results in tabular form       |
| `fields`    | Include or exclude specific fields    |
| `sort`      | Sort results by field                 |
| `eval`      | Create or modify fields               |
| `where`     | Filter results based on conditions    |
| `dedup`     | Remove duplicate events               |
| `rename`    | Rename fields                         |

---

## 📊 Aggregation Examples

```spl
| stats count by status
| stats avg(bytes) by host
| stats sum(bytes) as total_bytes by uri_path
```

---

## ⏱ Time-Based Searches

```spl
index=web earliest=-24h latest=now
```

```spl
| timechart count by status
| timechart avg(duration) span=1h
```

---

## 🧮 Field Manipulation with `eval`

```spl
| eval total=price*quantity
| eval status=if(response=200, "OK", "ERROR")
| eval user=coalesce(user1, user2)
```

---

## 📋 Table & Fields

```spl
| table host, status, bytes
| fields - _raw, _time         # Exclude fields
| fields + user, ip_address    # Include fields
```

---

## 🧹 Deduplication & Filtering

```spl
| dedup user_id
| where duration > 5
```

---

## 📈 Visualization & Reporting

```spl
| top 5 uri_path
| rare user
| chart count by product, region
```

---

## 🔎 Subsearch Example

```spl
index=main user=[ search index=auth action=fail | top limit=1 user | return user ]
```

---

## 🔐 Security & Audit Use-Cases

```spl
index=windows sourcetype=WinEventLog:Security EventCode=4624
| stats count by Account_Name, host
```

```spl
index=firewall action=blocked
| timechart count by src_ip
```

---

## 🧠 Useful Functions

| Function            | Description                          |
|---------------------|--------------------------------------|
| `coalesce()`        | First non-null field                 |
| `if()`              | Inline conditional                   |
| `case()`            | Multi-conditional logic              |
| `substr(field, x)`  | Extract substring                    |
| `lower()`, `upper()`| Convert case                         |

---

## 🔄 Joins, Lookups, Transactions

```spl
| lookup user_info user_id OUTPUT full_name
| join user_id [ search index=logs error=1 ]
| transaction session_id startswith="login" endswith="logout"
```

---

## 🕒 Time Modifiers

| Modifier         | Meaning                            |
|------------------|------------------------------------|
| `earliest=-15m`  | Last 15 minutes                    |
| `earliest=-7d@d` | Beginning of 7 days ago            |
| `latest=now()`   | Current time                       |

---

## 📦 Output & Export

```spl
| outputcsv results.csv
| outputlookup my_lookup.csv
```

---

## ⚡ Pro Tips

- Use `tstats` for faster searches on indexed fields:
  ```spl
  | tstats count where index=* by host
  ```

- Use `eventstats` to enrich original events with aggregates:
  ```spl
  | eventstats avg(duration) as avg_duration
  ```

- Use `btool` for configuration troubleshooting (CLI tool).

---

> 📘 **Note:** This cheatsheet is intended as a concise reference for day-to-day use and team knowledge sharing. Feel free to adapt it to your specific Splunk environment or operational use cases.

---

## 🧑‍💻 Author

Made for personal use and public sharing. Contributions welcome!
