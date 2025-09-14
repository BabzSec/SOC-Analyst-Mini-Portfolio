# 🔍 Windows Security Event ID Cheat Sheet
> by **BabzSec** — part of [SOC-Analyst-Mini-Portfolio](https://github.com/BlueWardRix/SOC-Analyst-Mini-Portfolio)

> 🎯 Quick reference for common Windows Event IDs — perfect for Splunk, Microsoft Defender, or any SOC investigation.

---

## 🔑 Logon & Authentication
| 🆔 Event ID | 📌 Description | 📝 Notes |
|-------------|---------------|----------|
| **4624** | ✅ Successful logon | Shows who logged in & from where (local, RDP, network) |
| **4625** | ❌ Failed logon | Wrong password or unauthorized access attempt |
| **4634** | 👋 Logoff | User signed out, session ended |
| **4647** | 🚪 User initiated logoff | Graceful logoff initiated by user |
| **4648** | 👤 Logon with explicit credentials | "Run as different user" or service authentication |
| **4672** | 👑 Special privileges assigned | Admin or privileged accounts logging in |
| **4776** | 🔐 NTLM authentication | Domain logins using NTLM; check for unusual patterns |

---

## 🖥️ Process & Command Execution
| 🆔 Event ID | 📌 Description | 📝 Notes |
|-------------|---------------|----------|
| **4688** | ▶️ New process created | Detect suspicious processes, scripts, or PowerShell commands |
| **4689** | ⏹ Process exited | Correlate with 4688 for process lifecycle |
| **4104** | 📜 PowerShell script block logging | Logs executed PowerShell commands; look for suspicious scripts |

---

## 🛠️ Services & Scheduled Tasks
| 🆔 Event ID | 📌 Description | 📝 Notes |
|-------------|---------------|----------|
| **7045** | 🛠 New service installed | Can indicate persistence or malware installation |
| **4697** | ⚙️ Service installed (alt) | Alternative service installation event; monitor changes |
| **4698** | ⏲ Scheduled task created | Often used for automated tasks or malware persistence |

---

## 📂 Object & File Access
| 🆔 Event ID | 📌 Description | 📝 Notes |
|-------------|---------------|----------|
| **4663** | 📁 File or folder accessed | Requires Object Access auditing; detect sensitive file access |
| **4656** | 🗝 Handle to object requested | Precedes 4663; shows access request before action occurs |

---

## 👥 User & Group Management
| 🆔 Event ID | 📌 Description | 📝 Notes |
|-------------|---------------|----------|
| **4720** | ➕ User account created | Check for unauthorized or test accounts |
| **4722** | 🔓 User account enabled | Watch for unexpected account activations |
| **4723** | 🔑 Password change attempt | Indicates user password update activity |
| **4724** | 🔄 Password reset attempt | Admin or recovery reset; watch for suspicious resets |
| **4728** | 👥 Added to a global group | Could escalate privileges; monitor new group memberships |
| **4732** | 👤 Added to local group | Check for unauthorized privilege escalation |
| **4738** | ✏️ User account changed | Monitor for changes in user attributes or privileges |

---

## 🌐 Remote Desktop & Network
| 🆔 Event ID | 📌 Description | 📝 Notes |
|-------------|---------------|----------|
| **4624** *(Type 10)* | 💻 Successful RDP login | Remote Desktop login; check source IP and account |
| **4625** *(Type 10)* | 🚫 Failed RDP login | Detect brute-force attempts or unauthorized remote access |
| **1149** *(TerminalServices)* | 🔎 RDP session attempt | TerminalServices login attempt; useful for RDP monitoring |

---

## 🔒 Security Policy & Audit
| 🆔 Event ID | 📌 Description | 📝 Notes |
|-------------|---------------|----------|
| **1102** | 🧹 Audit log cleared | Indicates log tampering; watch for deleted audit trails |
| **4719** | 🛡 System audit policy changed | Changes in auditing settings; potential attacker activity |

---

### 💡 Splunk Quick Tip
```spl
index=wineventlog EventCode=4625
| stats count by Account_Name, Workstation_Name
| sort -count
