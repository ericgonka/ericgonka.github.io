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
- **192.168.1.123 (Dorvack-Files)** – Dorvack-Files Web
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
#### Intranet Dorvack-Files IP: `192.168.1.123`

**Service Enumeration:**
- **Dorvack-Files IP Address:** 192.168.1.123

| Protocol | Port(s)       |
|----------|---------------|
| TCP      | 80, 3306      |
| UDP      |               |


- **Nmap Scan Results:**

We scan with nmap and we get these results.

![{8E0EF2D0-BB42-49D1-A413-33C51971ADFB}](https://github.com/user-attachments/assets/9e36b115-b9ea-429d-bb73-b6d47bae2b98)

We connect to the web to see what it is about.

![image](https://github.com/user-attachments/assets/0df4d47a-bc6b-44e8-b3a3-f60e42d4fcef)

Since I didn't have valid passwords, I understood that I had to get some kind of credentials to access the website.
The "?page=" parameter caught my attention in the URL and I tried a Local File Inclusion without visible results.

![{499AF8EB-DF11-4B30-B9DC-2C258C3D2768}](https://github.com/user-attachments/assets/059f0374-cae0-4920-9120-cdfd1164b07d)

Nikto gives us information about a config.php file on the website.

![{02C09580-A733-4816-9DE0-6B280AA24317}](https://github.com/user-attachments/assets/e73a8a23-b135-4d8f-9787-dda096540dd7)

When I look at the index, without the php extension, it makes me think that the server is implementing the ". .php" extension.

![{ED35474D-379E-4787-8406-C5242C99DD81}](https://github.com/user-attachments/assets/2a78b04c-8f12-4271-8be1-9cc206423776)

##### Initial Shell Vulnerability Exploited  

- _Local File Inclusion with Wrappers_  
- **Vulnerability Explanation:**  
  _Certain PHP-based web applications improperly sanitize user input when including files, allowing attackers to exploit Local File Inclusion (LFI) vulnerabilities. When wrappers such as php://filter are allowed, the impact can escalate beyond simple file inclusion. This may lead to:

Source code disclosure (php://filter/convert.base64-encode/resource=...)_  

- **Vulnerability Fix:**  
  _To mitigate this issue:

Always validate and sanitize user-supplied input.

Disable unused PHP wrappers in php.ini (e.g., allow_url_include, allow_url_fopen).

Implement allowlists for file paths and avoid dynamic file inclusion when possible.

Keep software and dependencies up to date to benefit from upstream security patches.
_  
- **Severity:** Critical
- **Proof of Concept Code:**
  
```bash
http://target.com/index.php?page=php://filter/convert.base64-encode/resource=config.php
 ```

I'm going to implement the wrappers, let's see if I can use an LFI to read the config file that Nikto found for us and decode it into base64.
We capture the request and go to the burpsuite repeater to run the tests.

![image](https://github.com/user-attachments/assets/9a5ac5d7-3a0d-4000-82e9-5f460091df68)

And indeed we see a base64 string in the response.

![image](https://github.com/user-attachments/assets/127c54cc-901b-41a5-95bb-682bc8bcc788)

We decode and see a user and a password from the User database.

![image](https://github.com/user-attachments/assets/c04ceb57-bceb-4229-abff-027c6d452902)

As we have seen previously, the open mysql port, we are going to connect with the obtained credentials.

![image](https://github.com/user-attachments/assets/c752c9b6-bf2f-4e9d-9b3c-6e9db4de5c5e)

We accessed the database and saw the users and passwords.

![image](https://github.com/user-attachments/assets/888ae1af-d754-486a-bb46-fd569681d715)

![image](https://github.com/user-attachments/assets/c7260148-27aa-4aaf-98ed-01e2eaca3c58)

![image](https://github.com/user-attachments/assets/a42fd31f-aadc-4cd2-be46-2559fba71dd2)

We decode and log in to the web.

![image](https://github.com/user-attachments/assets/bd0e45a2-2e15-48bf-8ddd-883d43bf46cc)

![image](https://github.com/user-attachments/assets/4e20fcda-36dd-40dc-aa5b-1d00d0a33675)

We tried uploading a hidden PHP shell with the allowed extensions, and modified the header so that it is detected as a GIF.

![{B17AF97E-7F07-4664-87C2-99D29A21787F}](https://github.com/user-attachments/assets/69f1ad06-a932-4a7f-be59-c3c1ea423793)

When uploading it we get this error, but we have successfully uploaded it to the /upload/ directory.

![{6F829296-7405-432A-BE76-F7379798D7D3}](https://github.com/user-attachments/assets/d9f2763a-d223-480a-b377-8d4948567a36)

After hours and hours of searching, we see that in the index.php file that we downloaded using the previous LFI, there is another "include" that refers to "/lang+cookie".

![image](https://github.com/user-attachments/assets/ebc7f4e4-5acf-4db6-bc1d-e953a4cf9a2f)

We make the relevant modifications and point to the gif that contains the reverse shell.

![image](https://github.com/user-attachments/assets/2ad6f64d-76e6-4f7f-a330-cbb7b3571883)

We gained access to the server.

![image](https://github.com/user-attachments/assets/8a95becd-449c-4ce6-8791-ef6a9208a4f4)

We become Noelia and read the flag.

![image](https://github.com/user-attachments/assets/dbf76131-6a08-4336-8a80-a3acd7baf15a)

![image](https://github.com/user-attachments/assets/b3335526-a114-4740-919d-56965bbb847e)


**flag.txt Contents:**

```
flag{30afT3klsan:elo}
```

We read with strings the program we found on Noelia's home page.

![image](https://github.com/user-attachments/assets/aa9c75da-38a1-49f4-b146-bff79d161243)

We see that it uses cat in a relative path, we create our cat that contains /bin/bash, and we modify the path so that it executes the one we have in Noelia's home directory.

![image](https://github.com/user-attachments/assets/13ecb6da-2f8f-42ca-ab49-c9210d772b6b)

We run and become Mike.

![image](https://github.com/user-attachments/assets/e1c6c784-a140-4607-9834-771db5e3ab52)

We watch another program.

![image](https://github.com/user-attachments/assets/c84739c6-65ad-405a-b0fa-7cd4bc6aa10f)

Same procedure, we see the program with strings.

![image](https://github.com/user-attachments/assets/ff0dcbd9-4270-4d2a-95da-8fa1d0c40399)

We run the script and with commands we copy the bash to /tmp/, we run it and we see that we are now root.

![image](https://github.com/user-attachments/assets/ff6e765f-1ca2-42c1-9e7a-de33b96f7940)

We see the root flag.

![image](https://github.com/user-attachments/assets/cec5974c-7d12-4931-82cc-48a25658c34b)


**flag.txt Root Contents:**

```
flag{F1L0m3n4_forEver}
```


**End of Report**
