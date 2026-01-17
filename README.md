# 🛡️ Windows 11 STIG Hardening: Security Remediation Project

## 📝 Project Overview
This project demonstrates a real-world **Vulnerability Management Workflow** focused on system hardening. Using an Azure VM (Host: `Kaddy`), I conducted a comprehensive security audit against **DISA Windows 11 STIG benchmarks**.  
![Alt text for the image](https://github.com/ksgassama-lab/DISA-Windows-11-STIG-remediation-with-PowerShell/blob/main/Tenable%20Scan%20.png?raw=true)

Key highlights:  
- Identified **152 failed security controls**  
- Prioritized **10 high-impact misconfigurations**  
- Deployed **automated registry-based fixes** to harden the system against unauthorized access and privilege escalation
-  

---

## 🔐 The Problem
Security vulnerabilities often persist even on updated systems because standard **Windows Updates** do not always address deep-level configuration misalignments.  

In this environment, several critical features—such as **Windows Installer privileges** and **Kernel DMA Protection**—were left in an insecure state, creating potential vectors for:  
- Local privilege escalation (LPE)  
- Data exfiltration  

---

## 🚨 The Discovery
I ran a **Tenable scan** on my Azure VM (`Kaddy`).  

**Scan results:**  
- **152 failed audit items**  
- Multiple **High severity vulnerabilities**  

**Example:**  
- **WN11-CC-000315** – The system was configured to allow the Windows Installer to **“Always install with elevated privileges”**.  

---

## 🔍 Investigation
I verified flagged controls using **PowerShell**.  

**Example for WN11-CC-000315:**  

powershell
# Checking if elevated privileges for Windows Installer are enabled
Get-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\Installer" -Name AlwaysInstallElevated

Result:
The check confirmed the policy was misconfigured (set to 1 or missing), allowing standard users to execute MSI files with SYSTEM-level authority.

---

🛠️ Remediation 

Instead of manually editing the Registry (RegEdit), I used a PowerShell script to apply fixes across 10 critical STIG IDs.

Advantages:

Faster

Repeatable

Scalable for enterprise environments

---

Control ID	Finding Name	Mitigation Action.
- WN11-AC-000010	Account Lockout Duration	Set duration to 15+ minutes

- WN11-AU-000050	Detailed Tracking - Process Creation	Enable success/failure auditing

- WN11-AU-000560	Other Logon/Logoff Events	Enable auditing of logon/logoff events

- WN11-CC-000005	Activity Feed	Disable to prevent data leakage

- WN11-CC-000090	GPO Reprocessing	Force refresh even if GPO hasn't changed

- WN11-CC-000197	Microsoft Consumer Experiences	Disable to reduce attack surface

- WN11-CC-000255	Windows Hello Hardware Device	Mandate TPM/Security device use

- WN11-CC-000315	Elevated Installer Privileges	Disable to prevent privilege escalation

- WN11-EP-000310	Kernel DMA Protection	Enable to block unauthorized DMA

- WN11-SO-000220	NTLM SSP Session Security	Enforce minimum security requirements

✅ Audit & Verification

After running the script, I re-ran the Tenable verification scan.

Outcome:

Targeted controls, including WN11-CC-000315, transitioned from FAILED → PASSED

💡 Key Takeaways & Lessons Learned

Beyond Patching: Not all vulnerabilities are fixed by “Windows Update.” Hardening requires active management of registry keys and Group Policy Objects (GPOs).

Prioritization is Key: Focusing on high-impact findings like WN11-CC-000315 immediately closed a major path for local privilege escalation.

Registry Hardening: Mastering HKLM via PowerShell is essential for security professionals managing systems at scale, ensuring consistent compliance across cloud infrastructure.
