# 🚨 Splunk Alerts – Windows Events Monitoring

This file documents the alerts configured in Splunk for the **Windows Events** lab.  
Each alert is designed to demonstrate how a SOC analyst can automate detections and quickly respond to suspicious activity.

---

## 1️⃣ Brute-Force Login Detection

Detects repeated failed login attempts, which could indicate password guessing or brute-force attacks.

- **SPL (Lab Version):**
```spl
index=* EventCode=4625
```

- **Alert Settings (Lab Version):**
  - **Time Range:** Last 3 minutes  
  - **Schedule:** Run every 1 minute  
  - **Trigger Condition:** Number of Results > 0 (i.e., any failed login detected)  
  - **Notes:** Threshold is effectively 3 attempts in 3 minutes for testing purposes — avoids locking Windows accounts.

- **Recommended Real-World Settings:**
  - **SPL:** Same as above  
  - **Time Range:** Last 10–15 minutes  
  - **Schedule:** Run every 5–15 minutes (cron: `*/5 * * * *`)  
  - **Trigger Condition:** Account/IP with ≥5–10 failed logins  
  - **Notes:** Adjust threshold and time window based on environment size and security policy.  

- **Actions:** Add to *Triggered Alerts*, optionally send email.  
- **Tips:**  
  - Exclude known safe accounts or service accounts to reduce false positives.  
  - Use `stats count by Account_Name, src_ip | where count >= 5` in production for precise counting.

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
