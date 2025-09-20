# 🖥️ LimaCharlie EDR Setup

This document explains how to set up LimaCharlie on a Windows VM for lab testing and portfolio creation.

---

## 1️⃣ Sign Up & Create an Organization
1. Go to [LimaCharlie](https://www.limacharlie.io/) and sign up for a free account.  
2. Create a new **Organization** in the console.  
3. Copy your **API Key** — you’ll need it to enroll agents.

---

## 2️⃣ Install the LimaCharlie Agent
1. From the console, download the **Windows Installer** for the agent.  
2. Install it inside your Windows 10 VM.  
3. Confirm that the device shows up as “Online” under **Sensors**.

---

## 3️⃣ Add the Sigma Extension (Recommended)
> To get immediate detections based on community-maintained rules, enable the free **Sigma** extension.

1. In the LimaCharlie console, open **Extensions → Add Extension**.  
2. Search for `Sigma` and install it.  
3. Apply it to your organization so its rules are available to your sensor.  
> 💡 Tip: You may have a few default rules installed, but most detections require the Sigma extension to trigger alerts.

---

## 4️⃣ Configure Policies
- Enable or verify these modules in your policy:
  - ✅ Real-time process & file monitoring  
  - ✅ Network and registry activity  
  - ✅ Behavioral detection  
- Assign the policy to your VM sensor.

---

## 5️⃣ Verify & Snapshot
1. Ensure the VM appears healthy and is sending telemetry.  
2. Take a **snapshot** of this clean state before testing any detections.  
> 💡 Tip: Maintaining a clean snapshot allows you to safely test malware or suspicious scripts repeatedly without contaminating the VM.

---

## 6️⃣ Additional Setup Tips
- Keep your lab **isolated** (Host-only network, no shared folders/clipboard).  
- Ensure **Real-time Scanning** and policy modules are enabled to detect behaviors.  
- Use safe test files first to validate that alerts trigger correctly before testing any real malware.  
- Document all configuration changes to include in your portfolio notes.

