# LimaCharlie EDR Alerts

### 📊 Detection Dashboard Overview
Shows alert trends and event counts in real time for key rules.

![Detection Dashboard](../04-Screenshots/LimaCharlie/Dashboards/Dashboard_Overview.png)

---

### 📜 Sigma Rules Overview
After installing the **Sigma Extension**, over 1,500 community-maintained rules were enabled in LimaCharlie.  
These serve as the baseline for detections, covering PowerShell abuse, privilege escalation, file execution, and more.

![Sigma Rules Enabled](../04-Screenshots/LimaCharlie/Dashboards/Sigma_Rules.png)

> 💡 **Tip:** Keep the Sigma extension updated to benefit from the latest community rules, and disable any that create excessive noise in your lab.

---

### 1️⃣ Malicious / Suspicious File Execution

LimaCharlie detected suspicious or potentially malicious files executed on the isolated lab VM.  

> For a **detailed analysis, investigation, and full screenshot workflow**, see [Malware Detection & Analysis](Malware/Malware_Detection.md)

---

## 2️⃣ PowerShell Encoded Command Alert

Detects when PowerShell is launched with an encoded payload on the command line.

- **Rule Used:** *Suspicious Execution of PowerShell with Base64*  
- **Description:** Watches for `powershell.exe` or `pwsh.exe` started with flags such as `-e`, `-enc`, `-EncodedCommand`, indicating a possible obfuscated script execution.  
- **Severity:** Medium  
- **References:**  
  - [Atomic Red Team – T1059.001](https://github.com/redcanaryco/atomic-red-team/blob/f339e7da7d05f6057fdfcdd3742bfcf365fee2a9/atomics/T1059.001/T1059.001.md#atomic-test-20---powershell-invoke-known-malicious-cmdlets)  
  - [Palo Alto – EncodedCommand Attacks](https://unit42.paloaltonetworks.com/unit42-pulling-back-the-curtains-on-encodedcommand-powershell-attacks/)

### 💻 Lab Simulation

Run this harmless one-liner inside your **isolated** Windows VM:

```powershell
powershell.exe -NoProfile -NoExit -EncodedCommand VwByAGkAdABlAC0ATwB1AHQAcAB1AHQAIAAiAFQAZQBzAHQAIABEAGUAdABlAGMAdABpAG8AbgAiAA==
```

This launches PowerShell with an encoded string (“Test Detection”).  
The rule should trigger and produce an alert in LimaCharlie.

### 📸 Screenshot

Capture:

- The detection in **Detections → All Detections** (showing `powershell.exe` and `-EncodedCommand`).
- The process tree or event details.
- Optionally, the rule page highlighting that it fired.

![PowerShell Encoded Command Alert](../04-Screenshots/LimaCharlie/Alerts/PowerShell_Encoded_Command_Alert.png)

> 💡 **Tip:** If no alert appears, confirm the rule is enabled, scope includes your host, and NEW_PROCESS events are reaching the cloud.



---

## 3️⃣ Privilege Escalation Alert (Admin / Special Privileges)

Detects when commands or processes are executed with elevated privileges using “RunAs”-like flags or administrative accounts.  

- **Sigma Rule Used:** `Suspicious RunAs-Like Flag Combination`  
- **Description:** Detects suspicious command-line flags that allow setting a target user and command (e.g., PsExec-like tools).  
- **Triggered By:** Actions such as running commands with `-u SYSTEM`, `-u NT`, `-u administrator`, or using `-c cmd / powershell / wscript / cscript`.  

- **Notes:**  
  - SYSTEM account logins are typically ignored.  
  - False positives are possible; this rule is medium severity.  
  - References: [Florian Roth / Nextron Systems](https://www.trendmicro.com/en_us/research/22/k/hack-the-real-box-apt41-new-subgroup-earth-longzhi.html)

- **Screenshot:**  
![Privilege Escalation Alert](../04-Screenshots/LimaCharlie/Alerts/Privilege_Escalation_Alert.png)

- **Lab Tip:**  
  - Simulate detection using a safe elevated command:  
    ```powershell
    Start-Process powershell -ArgumentList "-c whoami"
    ```

---

## 4️⃣ Investigation & Response

Demonstrates a SOC workflow after detection and highlights real-world incident handling skills.

- **Triggered By:** Any alert from your lab (e.g., PowerShell encoded command, malware execution, privilege escalation).

- **Description:**  
  This section focuses on how a SOC analyst investigates and responds to alerts. The goal is to show a complete workflow from detection to containment, emphasizing critical thinking and decision-making.

- **Investigation Steps:**  
  1. **Open Alert Details:**  
     - Review the detection metadata: process name, command line, executing user, host, and timestamp.  
     - Note any unusual patterns or unexpected processes.  
  2. **Analyze Process Tree:**  
     - Explore parent-child relationships of processes to understand the origin and propagation of the suspicious activity.  
     - Identify which processes were spawned by the malicious or suspicious event.  
  3. **Check File & System Context:**  
     - Look at file paths, hashes, registry keys, or network connections involved.  
     - Determine whether any sensitive areas (Program Files, System32) were accessed or modified.  
  4. **Correlate with Other Alerts:**  
     - Compare the alert to other logged events on the same host or user to detect broader attack patterns.  
     - Identify lateral movement or privilege escalation attempts.  

- **Response Actions:**  
  1. **Containment:**  
     - Isolate the affected host from the network to prevent spread.  
     - Quarantine malicious files or scripts.  
  2. **Mitigation:**  
     - Terminate suspicious processes if safe to do so in the lab environment.  
     - Block or restrict suspicious scripts or executables using EDR policies.  
  3. **Documentation:**  
     - Record all observed behaviors, actions taken, and timestamps.  
     - Include notes on false positives or anomalies for learning purposes.  

- **Lab Tip:**  
  - Even when using safe files or simulated attacks, demonstrate the **full workflow**: detection → investigation → analysis → response.  
  - Highlight critical fields such as process name, user, command line, timestamp, and actions taken.  
  - Emphasize how you trace and contain the threat while minimizing impact on the lab environment.


