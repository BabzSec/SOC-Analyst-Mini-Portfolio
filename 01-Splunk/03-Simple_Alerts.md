# 🚨 Splunk Alerts – Windows Events Monitoring

This file documents the alerts configured in Splunk for the **Windows Events** lab.  
Each alert is designed to demonstrate how a SOC analyst can automate detections and quickly respond to suspicious activity.

---

## 1️⃣ Brute-Force Login Detection

Detects repeated failed login attempts, which could indicate password guessing or brute-force attacks.

- **SPL:**
  ```spl
  index=* EventCode=4625
  | stats count by Account_Name, src_ip
  | where count >= 5
  ```
- **Trigger Condition:** `count >= 5` failed logins for the same `Account_Name` or `src_ip` within 10 minutes.  
- **Actions:** Add to *Triggered Alerts* (optional email if SMTP is configured).  
- **Notes:**  
  - Adjust the threshold (`>=5`) based on environment size.  
  - Use a narrower `index` or add `host=` if you have multiple data sources.

---

## 2️⃣ PowerShell Script Execution

Alerts whenever a PowerShell script block is logged (EventCode `4104`).  
This helps spot suspicious script activity, such as encoded commands.

- **SPL:**
  ```spl
  index=wineventlog EventCode=4104
  ```
- **Trigger Condition:** Any result (1+ event) in the last 15 minutes.  
- **Actions:** Add to *Triggered Alerts*.  
- **Notes:**  
  - Requires PowerShell Script Block Logging enabled.  
  - Consider adding filters (e.g., `NOT CommandLine="Get-Help*"`) to reduce noise.

---

## 3️⃣ Privilege Escalation (Special Privileges Assigned)

Watches for EventCode `4672` (Special privileges assigned to new logon).  
Useful to detect when non-admin accounts receive elevated privileges.

- **SPL:**
  ```spl
  index=main EventCode=4672
  | search Account_Name!="SYSTEM"
  ```
- **Trigger Condition:** Any result (1+ event) in the last 15 minutes.  
- **Actions:** Add to *Triggered Alerts*.  
- **Notes:**  
  - Exclude built-in service accounts (`SYSTEM`, `LOCAL SERVICE`) to avoid noise.  
  - Combine with logon type events (4624) for deeper investigations.

---

### 💡 General Tips for Splunk Alerts
- ⏱️ **Set an Appropriate Time Window:** Match your search to how fast incidents can happen (e.g., 5–15 min).  
- 📥 **Delivery Options:** If email isn’t available, use “Triggered Alerts” and export results later.  
- 🔍 **Tune for False Positives:** Start with broad rules, then add filters to reduce noise.  
- 🧠 **Think Like an Attacker:** Consider what unusual events you’d create if you were trying to break in — then write an alert for that!

---

> These alerts use **lab-generated data** for learning purposes. In production, thresholds and filters should be tuned to the specific environment.
