# 🛡️ Sophos Intercept X with EDR – Setup Guide

This document explains how I deployed and tested **Sophos Intercept X with EDR** in my lab environment as part of my SOC Analyst mini-portfolio.

---

## 1️⃣ Create a Sophos Central Trial
- Go to [Sophos Central Free Trials](https://www.sophos.com/en-us/products/free-trials).
- Select **Intercept X with EDR** and register for a free trial.
- Verify your email and sign in to the **Sophos Central** console.

> 💡 Use a lab-dedicated email address for trials.

---

## 2️⃣ Prepare a Test Endpoint
- Use an isolated Windows 10/11 VM.
- Ensure it has internet access and create a snapshot before testing.

---

## 3️⃣ Download & Install the Agent
- From Sophos Central, navigate to **Protect Devices** → download the Windows installer.
- Run the installer on your VM and complete the setup.

---

## 4️⃣ Verify Device Registration
- Confirm the endpoint appears under **Devices** in Sophos Central.
- Ensure the status shows as **Protected**.

---

## 5️⃣ Generate Test Events
- Use the [EICAR test file](https://www.eicar.org/download-anti-malware-testfile/) to simulate malware detection.
- Run harmless PowerShell commands to trigger behavior detection.
- Observe alerts and detections in Sophos Central.

> ⚠️ Only run safe, lab-approved tests in an isolated VM.

---

## 6️⃣ Investigate & Respond
- Open an alert and review details such as process tree and recommended actions.
- Test remediation options (e.g., isolate endpoint, clean up files).

---

## 7️⃣ Wrap Up
- Confirm all alerts are resolved after testing.
- Save notes about each step and detection for your portfolio.

---

## 🖼️ Screenshots
All screenshots for this lab are stored in the [Screenshots](Screenshots) folder:
- Sophos Central Dashboard (after agent installation)
- Endpoint listed as protected
- Example detection/alert
- Investigation and remediation workflow

---

### ✅ Notes
- This setup was performed in a **non-production lab environment**.
- All detections used harmless, simulated test data.
- Sophos EDR provides strong visibility into endpoint activity and supports efficient threat investigations for SOC analysts.
