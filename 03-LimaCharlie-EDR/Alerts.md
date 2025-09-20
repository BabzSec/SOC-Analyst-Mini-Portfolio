# 🛡️ LimaCharlie EDR Alerts

This document outlines the key alert scenarios tested in the LimaCharlie lab environment.  
All tests are performed in an **isolated VM** using safe or lab-generated events.

---

## 1️⃣ Failed Login Attempts (Brute-Force)

Detects multiple failed logins which could indicate password guessing or brute-force attacks.

- **Trigger Method:** Attempt 3–5 failed logins on the VM.
- **Sigma Rule:** `login_failure`  
- **Notes:** Threshold for testing is **3 attempts in 3 minutes**. Real scenario might use **5–10 attempts**.
- **Screenshot:**  
![Failed Login Alert](04-Screenshots/Alerts/Failed_Login_Alert.png)

---

## 2️⃣ PowerShell Script Execution

Detects suspicious PowerShell activity on the endpoint.

- **Trigger Method:** Run a benign PowerShell test snippet flagged by Sigma:
```powershell
# Example safe test
$cmd = 'Write-Output "Test LimaCharlie Detection"'
Invoke-Expression $cmd
```
- **Sigma Rule:** `powershell_suspicious`  
- **Notes:** Real scenario could include malicious script execution or encoded commands.
- **Screenshot:**  
![PowerShell Alert](04-Screenshots/Alerts/PowerShell_Alert.png)

---

## 3️⃣ Privilege Escalation (Admin / Special Privileges)

Detects when accounts are granted administrative privileges.

- **Trigger Method:** Log in as a user with administrative rights (EventCode 4672).  
- **Sigma Rule:** `privilege_escalation`  
- **Notes:** SYSTEM account logins are typically ignored to reduce noise.
- **Screenshot:**  
![Privilege Escalation Alert](04-Screenshots/Alerts/Privilege_Escalation_Alert.png)

---

## 4️⃣ Malicious / Suspicious File Execution

Detects execution of potentially harmful files.

- **Trigger Method:** Execute a safe test file, e.g., **EICAR** or a lab-generated PUA.  
- **Sigma Rule:** `malicious_file_execution`  
- **Notes:** Always use **isolated VMs**; do not run actual malware in your main environment.  
- **Screenshot:**  
![Malicious File Alert](04-Screenshots/Alerts/Malicious_File_Alert.png)

---

## ⚡ Tips for Portfolio

- Add **captions** for each screenshot explaining what it shows.  
- Include **before/after views** of alerts to show detection flow.  
- Highlight **key fields**: filename, hash, process, user, timestamp.  
- Mention **Sigma rules** used for detection to show hands-on knowledge.  

> Keep your lab clean and snapshots ready to revert after testing.
