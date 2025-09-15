# 📊 Splunk Queries – Windows Events Dashboard

This file contains all SPL queries used in the “Windows Events” dashboard, with notes and tips for working with them in Splunk.

---

## 1️⃣ Top Security Event Codes
Shows which Windows Security Event Codes appear most frequently.

```spl
index=* source="WinEventLog:Security" host="DESKTOP-VCQQ1AG"
| stats count by EventCode
| sort -count
```

---

## 2️⃣ Failed Login Attempts Over Time
Displays failed login attempts (EventCode `4625`) as a time-series trend.

```spl
index=* EventCode=4625
| timechart span=1s count by Account_Name
| top limit=10 _time
| sort 0 _time
| rename _time as "Time"
| rename count as "Count of Events"
| rename percent as Percentage
| eval Time=strftime(Time, "%Y-%m-%d %H:%M:%S")
```

---

## 3️⃣ Admin / Privileged Actions
Lists accounts that received **Special Privileges Assigned** (EventCode `4672`).

```spl
index=main EventCode=4672
| stats count by Account_Name
| sort -count
```

---

## 4️⃣ PowerShell Activity Over Time
Shows the volume of PowerShell commands (EventCodes `4103` & `4104`) executed per host.

```spl
index=wineventlog 4104 OR 4103
| timechart span=1h count by host
```

---

### 💡 Splunk Usage Tips
- 🔍 **Time Range Picker:** Always confirm the correct time window (e.g., “Last 24 hours”) — otherwise you might miss recent events.  
- 📁 **Specify Indexes:** Instead of `index=*`, set `index=wineventlog` or `index=main` to improve speed and accuracy.  
- ⏱️ **Optimize Span:** For `timechart`, match `span` to your data volume (e.g., `span=5m` for busy environments, `span=1h` for low volume).  
- 🧹 **Filter Noise:** Add `Account_Name!="-"` or `NOT user="ANONYMOUS LOGON"` to remove irrelevant entries.  
- 📝 **Save Searches:** Save each SPL as a *Report* or *Dashboard Panel* so you don’t have to retype it.  
- ⚡ **Performance:** Use `tstats` for faster searches on indexed fields if your dataset grows large.  
- 📌 **Documentation:** Add inline comments in your SPL using `/* comment */` to remind yourself what a line does.

---
