# Log-threat-hunting-with-Azure
🛡️ Windows Security Log Threat Hunting with Azure Log Analytics & KQL
======================================================================

_A beginner-friendly SOC Analyst project using a Windows VM in Azure_

📌 Project Overview
-------------------

This project simulates a real SOC (Security Operations Center) workflow by collecting and analyzing Windows Security Event Logs in Microsoft Azure.

I deployed a Windows Virtual Machine, connected it to a Log Analytics Workspace using the Azure Monitor Agent (AMA), generated real security events (failed logins, account lockouts, new users), and performed threat hunting using **Kusto Query Language (KQL)**.

This project demonstrates:

*   Azure log ingestion
    
*   Windows security event monitoring
    
*   KQL querying and analysis
    
*   Threat hunting fundamentals
    
*   Building detections from raw event data
    

🧱 Architecture
---------------

+-------------------+        AMA        +---------------------------+  |   Windows VM      | ----------------> |   Log Analytics Workspace |  |  (SecurityEvents) |                   |   (Stores event logs)     |  +-------------------+                   +---------------------------+                 |                 | Generate activity (RDP)                 v       Attacker Simulation (Failed Logins)   `

🎯 Objectives
-------------

✔ Deploy a Windows VM in Azure✔ Enable log collection using the Azure Monitor Agent✔ Generate Windows security activity✔ Query logs using KQL✔ Identify indicators of brute-force attempts✔ Write a basic threat detection workflow✔ Build a mini SOC investigation report

🛠️ Tools & Skills Used
-----------------------

*   **Microsoft Azure**
    
    *   Virtual Machines
        
    *   Log Analytics Workspace
        
    *   Azure Monitor Agent (AMA)
        
    *   Azure Portal Logs
        
*   **Security**
    
    *   Windows Event Logs
        
    *   RDP login auditing
        
    *   Account lockout behavior
        
*   **KQL (Kusto Query Language)**
    
    *   Filtering
        
    *   Projection
        
    *   Sorting
        
    *   Event analysis
        
*   **Documentation**
    
    *   GitHub
        
    *   SOC-style incident logging
        

🧩 Step-by-Step Implementation
==============================

1️⃣ Create Azure Resources
--------------------------

### Resources created:

*   Resource Group
    
*   Windows Virtual Machine
    
*   Log Analytics Workspace
    
*   Data Collection Rule for SecurityEvent logs
    
*   Azure Monitor Agent (auto-installed via DCR)
    

2️⃣ Verify Log Ingestion
------------------------

Run this KQL query:

SecurityEvent  | take 5   `

If results appear → Security logs are flowing correctly.

3️⃣ Simulate Security Events (Attack Simulation)
------------------------------------------------

### 🔐 Failed Login Attempts

Using Microsoft Remote Desktop (Mac):

*   Attempted 10–20 failed RDP logins
    
*   Entered wrong username/password combinations
    

**Generated Event ID 4625**

### 🔓 Successful Login

Logged in once with the correct password.

**Generated Event ID 4624**

### 🔐 Account Lockout

Entered incorrect credentials repeatedly until the account locked.

**Generated Event ID 4740**

### 👤 New User Creation

Inside the VM → Created new user attacker01.

**Generated Event ID 4720**

### 🛑 Privilege Escalation

Added attacker01 to the Administrators group.

**Generated Event ID 4728**

🔎 Threat Hunting with KQL
==========================

📌 1. Failed RDP Login Attempts (Brute-Force Detection)
-------------------------------------------------------

SecurityEvent  | where EventID == 4625  | project TimeGenerated, Account, IPAddress=RemoteIpAddress, FailureReason  | order by TimeGenerated desc   `

📌 2. Successful Logins
-----------------------

SecurityEvent  | where EventID == 4624  | project TimeGenerated, Account, LogonType, IPAddress=IpAddress  | order by TimeGenerated desc   `

📌 3. Account Lockouts
----------------------

SecurityEvent  | where EventID == 4740   `

📌 4. New User Account Created
------------------------------

SecurityEvent  | where EventID == 4720   `

📌 5. New Administrator Added
-----------------------------

SecurityEvent  | where EventID == 4728   `

📊 Optional SOC Dashboard (Azure Workbook)
==========================================

Created a dashboard visualizing:

*   Failed login attempts
    
*   Successful logins
    
*   New user accounts
    
*   Privilege escalations
    

🧾 Incident Report Summary
==========================

**Incident Title:** RDP Brute-Force Simulation**Date:** _(Add your date)_**Severity:** Medium

### 🔍 Description

Multiple failed login attempts were performed to simulate a brute-force attack, followed by successful login, new user creation, and privilege escalation.

### 📌 Findings

*   **20 failed login attempts** (Event ID 4625)
    
*   **1 successful login** (4624)
    
*   **1 account lockout** (4740)
    
*   **User created:** attacker01 (4720)
    
*   **Privilege escalation:** attacker01 added to Administrators (4728)
    

### ✔ Conclusion

This environment successfully captured and logged suspicious and administrative activities, demonstrating how SOC analysts detect account-based attacks using Azure and KQL.
