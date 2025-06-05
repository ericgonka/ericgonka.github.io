---
title: "Penetration Test Report Dorvack-Fles LAB 14"
author: ["ericgon28@proton.me", "OSID: XXXX"]
date: "2025-05-25"
subject: "Markdown"
keywords: [OSCP]
subtitle: "OSCP Exam Report"
lang: "en"
titlepage: true
titlepage-color: "DC143C"
titlepage-text-color: "FFFFFF"
titlepage-rule-color: "FFFFFF"
titlepage-rule-height: 2
book: true
classoption: oneside
code-block-font-size: \scriptsize
categories: 
  - OSCP
tags:
  - Difícil
  - CTF
image: "https://github.com/user-attachments/assets/5f10c3b9-55c2-4feb-bb82-0d979cea12d5"

---

![image](https://github.com/user-attachments/assets/0c2115d0-30f0-499e-a110-7e88e069a4d1)


#Penetration Test Report Dorvack-Teacher LAB 14
**Version**: 3.2  
**OSID**: XXXXX  
**Email**: ericgon28@proton.me 

![OSCP Logo](https://github.com/user-attachments/assets/d6b50208-9978-426d-bdd3-6cffcd6778c7)

> © All rights reserved to Offensive Security, 2025  
> No part of this publication, in whole or in part, may be reproduced, copied, transferred or any other right reserved to its copyright owner without prior written permission from Offensive Security.

---

## Table of Contents
1. [Offensive Security Exam Penetration Test Report](#1-offensive-security-exam-penetration-test-report)
   - [1.1 Introduction](#11-introduction)  
   - [1.2 Objective](#12-objective)  
   - [1.3 Requirements](#13-requirements)  
2. [High-Level Summary](#2-high-level-summary)  
   - [2.1 Recommendations](#21-recommendations)  
3. [Methodologies](#3-methodologies)  
   - [3.1 Information Gathering](#31-information-gathering)  
   - [3.2 Penetration](#32-penetration)  
---

## 1. Offensive Security Exam Penetration Test Report

### 1.1 Introduction
The Offensive Security Exam penetration test report outlines all actions and efforts taken during the Offensive Security exam. This report is graded based on the thoroughness, accuracy, and completion of each aspect of the exam. It ensures the student demonstrates an in-depth understanding of penetration testing methodologies as well as the technical knowledge required to earn the Offensive Security Certified Professional (OSCP) certification.

### 1.2 Objective
The objective is to perform an internal penetration test on the Offensive Security Exam network. The student must follow a systematic approach to achieving the assigned goals. This test simulates an actual penetration test, from start to finish, and includes a comprehensive report.

### 1.3 Requirements
The student is required to fill out this report fully, including the following sections:
- High-level summary and recommendations (non-technical)
- Detailed methodology outline
- Each finding with screenshots, walkthroughs, sample code, and proof.txt if applicable
- Additional items if not included in previous sections

---

## 2. High-Level Summary

### Compromised Hosts
- **192.168.1.134** – Dorvack-ERP Web
---

### 2.1 Recommendations
It is recommended that the identified vulnerabilities be patched immediately to prevent exploitation. These systems require regular updates and patches to avoid future breaches.

---

## 3. Methodologies

### 3.1 Information Gathering
Information gathering is critical in penetration testing. In this case, the scope of the test covered the following IP addresses:
- `192.168.1.134`

---

### 3.2 Penetration

#### System IP: `172.20.0.235` 
#### Intranet Dorvack-Teacher IP: `192.168.1.134`

**Service Enumeration:**
- **Dorvack-ERP IP Address:** 192.168.1.134

| Protocol | Port(s)       |
|----------|---------------|
| TCP      | 22, 80        |
| UDP      |               |


![image](https://github.com/user-attachments/assets/d38e78f7-753e-4ae8-91f2-a6032ea0c38d)

We see there's a website running Drupal, and judging by the URL, I suspect it could be vulnerable to SQL Injection.

![image](https://github.com/user-attachments/assets/8ec61648-31b1-4b5d-be44-0a9405f04ccd)


##### Initial Shell Vulnerability Exploited  

- _SQL Injection_  
- **Vulnerability Explanation:**  
  _The application fails to properly sanitize user input in SQL queries, allowing attackers to inject malicious SQL code. This can lead to unauthorized access to the database, data leakage, and potentially full system compromise if combined with other vulnerabilities. In this case, SQL injection was used to gain initial shell access to the target system._
  
- **Vulnerability Fix:**  
  _To mitigate this issue:
  
Use parameterized queries (prepared statements) instead of dynamic SQL.

Employ input validation to ensure user inputs conform to expected formats.

Apply the principle of least privilege to database accounts.

Regularly update and patch the database management system (DBMS) and application.

Conduct security code reviews and regular penetration testing.
_  
- **Severity:** Critical


We test with sqlmap.

![image](https://github.com/user-attachments/assets/8fa15285-44ca-479b-af17-c00b87fe3247)

Indeed, we can see it’s vulnerable and reveals two databases.

![image](https://github.com/user-attachments/assets/8d79d511-b069-443d-bb0a-108424544ee5)

We dump the users and passwords from the `d4db` database.

![image](https://github.com/user-attachments/assets/a5a4ad4c-b4d1-4d07-b5b1-dac37ca0950e)
![image](https://github.com/user-attachments/assets/3aeff805-4cd2-451e-87c2-019628eb5dba)

We copy the found hashes and crack them using John.

![image](https://github.com/user-attachments/assets/a988d58e-59d4-49b4-b2ab-da8b3d124c9d)

We find the password for user `john`.

![image](https://github.com/user-attachments/assets/0bb1bbc6-08a4-4074-9e3c-b6da83b3141f)

We log in to the web application.

![image](https://github.com/user-attachments/assets/bce86497-7d2d-4f93-a56c-63a8e18dd615)

After several hours of analysis, I discover we can inject PHP code into the contact form section.

![image](https://github.com/user-attachments/assets/448f3931-e57d-4318-b6b7-05e1e294476e)

We complete the form and upon submission, we receive a reverse shell.

![image](https://github.com/user-attachments/assets/1442968f-fc70-4b45-9c6f-0d694e8c9cc9)

We filter as usual by SUID permissions.

![image](https://github.com/user-attachments/assets/df4c8f1a-40a6-4d8e-8ce3-6df75d947bed)

##### Privilege Escalation Vulnerability Exploited  

- _Exim4 SUID_
- **Vulnerability Explanation:**  
  _Exim4, a mail transfer agent, was found to have a misconfigured or vulnerable SUID binary that could be exploited to execute arbitrary code with elevated (root) privileges. Attackers leveraged this flaw to escalate from a low-privileged shell to full root access. This often occurs when the Exim binary is improperly set with the SUID bit and contains a vulnerability such as command injection, buffer overflow, or improper input handling._
  
- **Vulnerability Fix:**  
  _To mitigate this issue:
Remove the SUID bit from the Exim4 binary unless explicitly required (chmod u-s /usr/sbin/exim4).

Ensure Exim is updated to the latest secure version, as older versions may contain known exploits (e.g., CVE-2019-10149).

Restrict access to the Exim binary using file permissions and AppArmor/SELinux profiles.

Monitor and audit SUID binaries regularly using tools like find / -perm -4000 -type f and validate necessity.

Apply principle of least privilege and use privilege separation wherever possible.
_  
- **Severity:** Critical


The Exim4 permission especially catches my attention, so I check the installed version.

![image](https://github.com/user-attachments/assets/7c6e33cc-7a79-4d8e-a8ae-ff9e1e573094)

We see there’s an exploit for Exim versions 4.87 to 4.91.

![image](https://github.com/user-attachments/assets/4475afbb-9a43-434c-b2fa-7fbfcbc6d657)

We upload it to the victim machine.

![image](https://github.com/user-attachments/assets/1df4318f-fe89-439f-877b-89743adb0841)

We grant execution permissions and run it.

![image](https://github.com/user-attachments/assets/fa0c2d04-ae8d-40c4-934f-3ab84a302642)

We see we are now root and can read the flag!

![image](https://github.com/user-attachments/assets/70c14912-8916-4313-9c54-1abb1bbb9e14)

![image](https://github.com/user-attachments/assets/8288f742-109b-4af1-8374-1c1f00806675)

**flag.txt Root Contents:**

```
flag:{Fj987akjs34590.ld}
```

**End of Report**
