---
title: "Penetration Test Report Dorvack-Fles LAB 13"
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
image: "https://github.com/user-attachments/assets/d2511954-8b56-4fda-a562-4aa5e3e53dd1"

---

![image](https://github.com/user-attachments/assets/0c2115d0-30f0-499e-a110-7e88e069a4d1)


#Penetration Test Report Dorvack-Teacher LAB 12
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
- **192.168.1.121 (formacion.dorvack.corp)** – Dorvack-Teacher Web
---

### 2.1 Recommendations
It is recommended that the identified vulnerabilities be patched immediately to prevent exploitation. These systems require regular updates and patches to avoid future breaches.

---

## 3. Methodologies

### 3.1 Information Gathering
Information gathering is critical in penetration testing. In this case, the scope of the test covered the following IP addresses:
- `192.168.1.123`

---

### 3.2 Penetration

#### System IP: `172.20.0.235` 
#### Intranet Dorvack-Teacher IP: `192.168.1.121`

**Service Enumeration:**
- **Dorvack-Teacher IP Address:** 192.168.1.121

| Protocol | Port(s)       |
|----------|---------------|
| TCP      | 80            |
| UDP      |               |


Configuramos el DNS en el BurpSuite ya que nos redirige al subdominio `formacion.dorvack.corp`

![image](https://github.com/user-attachments/assets/cd5a4ab0-cb4c-479e-8f03-08e17eb03be6)

We connect to the web to see what it is about.

![image](https://github.com/user-attachments/assets/8b898e55-26a7-414a-8290-8f91ab6a1cd4)

##### Initial Shell Vulnerability Exploited  

- _Moodle 3.9 - Remote Code Execution (RCE) (Authenticated)_  
- **Vulnerability Explanation:**  
  _This vulnerability allows an authenticated user with specific permissions (such as a teacher or manager) to execute arbitrary PHP code on the underlying server. The issue arises due to improper validation of user-supplied input in certain Moodle components (such as quiz or file upload plugins), allowing malicious payloads to be injected and executed on the server. Once exploited, this can lead to full server compromise._
  
- **Vulnerability Fix:**  
  _To mitigate this issue:
To mitigate this issue:

Upgrade to the latest supported version of Moodle, as the vulnerability has been patched in newer releases.

Apply any available security patches from the official Moodle security advisories.

Restrict user roles and permissions to the minimum necessary, especially those with file upload or quiz configuration capabilities.

Monitor logs for unusual activity from authenticated users.
_  
- **Severity:** Critical


After some time researching, we found a script for Moodle version 3.9 that is quite interesting, as it indicates it’s an authenticated RCE, and we have valid username and password credentials.

![image](https://github.com/user-attachments/assets/0b411672-c6d9-419d-963d-295e65a36d89)

We run the script, but it doesn’t work, as it needs to add our user to the `manager` group in order to access the admin panel and insert the reverse shell.

![image](https://github.com/user-attachments/assets/7fdfcd90-a68b-4233-95a6-5c35fd0236f6)

After heavily modifying the script based on the server’s responses and removing what wasn’t necessary, it seems to have worked, and we now have access to the admin panel.

![image](https://github.com/user-attachments/assets/a80a8258-f2a3-4b81-8526-9a7951be97f8)

We see that we can upload a zip file pretending we are installing a new module, but in reality, it’s to execute commands.

![image](https://github.com/user-attachments/assets/88e3bc4f-8ef6-4120-8291-e96d9ab62189)

We go to the section for **Instalar módulos externos** and install the zip.

![image](https://github.com/user-attachments/assets/54cf1830-4fe2-4fb8-9223-a9b19d957079)

It validates it and we proceed with the installation.

![image](https://github.com/user-attachments/assets/d7e45ad5-35c6-46a8-9798-b02b035014bc)

It gives us information regarding the installation and plugin verification.

![image](https://github.com/user-attachments/assets/a0cae167-107b-43ac-985c-8ae18fdf420b)

![image](https://github.com/user-attachments/assets/971965cc-ea26-44b6-80ad-793b0e8ee472)

We try to execute a simple `id` command and see that we do, in fact, have command execution capabilities.

![image](https://github.com/user-attachments/assets/fb989c0b-f6bd-4f40-8290-d15c624de85a)

We send ourselves a URL-encoded reverse shell.

![image](https://github.com/user-attachments/assets/e3ff835e-d9fc-4bba-b035-c3c4ca0f4e9b)

We start listening and gain access to the system.

![image](https://github.com/user-attachments/assets/b1786e81-d20a-4b81-978b-0210802c1ea2)

We check the available users on the system, and since we see `noelia`, we proceed to migrate using the password we already have.

![image](https://github.com/user-attachments/assets/8ed8aabc-685d-418b-b363-0b3641465bf1)

##### Privilege Escalation Vulnerability Exploited  

- _SUID Polkit's Pkexec_  
- **Vulnerability Explanation:**  
  _CVE-2021-4034 (nicknamed PwnKit) is a local privilege escalation vulnerability found in pkexec, a SUID-root program that is part of Polkit. The vulnerability exists because pkexec does not properly validate the number of arguments passed to it. This allows an attacker to manipulate environment variables—specifically GCONV_PATH—to load a malicious shared library and execute arbitrary code as root. Since pkexec is installed by default on many Linux distributions and is SUID-root, this can lead to full system compromise._
  
- **Vulnerability Fix:**  
  _To mitigate this issue:
Update to the latest version of Polkit where the vulnerability has been patched.

Remove the SUID bit from pkexec if it's not needed:

chmod 0755 /usr/bin/pkexec

Monitor user activity and audit logs for unusual privilege escalation behavior.

Consider deploying intrusion detection tools to flag exploitation attempts.
_  
- **Severity:** Critical

We check the SUID permissions and `pkexec` particularly catches our attention.

![image](https://github.com/user-attachments/assets/ea09b933-4c64-448c-85be-de82bf124479)

There’s a way to escalate privileges using an exploit called `Polkit's Pkexec`.

![image](https://github.com/user-attachments/assets/a38b4f92-1124-4bda-b17f-9806a5302d18)

We just need to transfer the `polkit_pwkit.py` script to the target machine and execute it.

![image](https://github.com/user-attachments/assets/a93b4eaa-2393-4bb5-a038-f01a743ad877)

And we would now be **root** and able to read the flag.

![image](https://github.com/user-attachments/assets/2170ec29-54c3-42b8-b6b0-4b08a75832fd)


**flag.txt Root Contents:**

```
Flag:{FYh73aoplksT990ss}
```


**End of Report**
