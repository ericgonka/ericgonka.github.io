![image](https://github.com/user-attachments/assets/b6e362c9-0bb2-495d-a442-ee52fe1caa4b)

# Offensive Security Penetration Test Report for OSCP Exam
**Version**: 3.2  
**OSID**: XXXXX  
**Email**: example@example.example  

![OSCP Logo](https://github.com/user-attachments/assets/d6b50208-9978-426d-bdd3-6cffcd6778c7)

> © All rights reserved to Offensive Security, 2014  
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
   - [4.1 Proof and Local Contents](#41-proof-and-local-contents)  
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
- **192.168.xx.xx (hostname)** – Initial exploit used  
- **192.168.xx.xx (hostname)** – Initial exploit used  
- **192.168.xx.xx (hostname)** – Initial exploit used  
- **192.168.xx.xx (hostname)** – Initial exploit used  
- **192.168.xx.xx (hostname)** – Buffer Overflow (BOF)

---

### 2.1 Recommendations
It is recommended that the identified vulnerabilities be patched immediately to prevent exploitation. These systems require regular updates and patches to avoid future breaches.

---

## 3. Methodologies

### 3.1 Information Gathering
Information gathering is critical in penetration testing. In this case, the scope of the test covered the following IP addresses:
- `192.168.xx.xx`
- `192.168.xx.xx`
- `192.168.xx.xx`
- `192.168.xx.xx`
- `192.168.57.112 (b0f-vic)`

---

### 3.2 Penetration

#### System IP: `192.168.xx.xx`

**Service Enumeration:**
- **Server IP Address:** 192.168.xx.xx  

| Protocol | Port(s)       |
|----------|---------------|
| TCP      | X, Y, Z       |
| UDP      | A, B, C       |

- **Nmap Scan Results:**

  ```
  (Insert Nmap output here)
  ```

##### Initial Shell Vulnerability Exploited  

- _Insert details here_  
- **Vulnerability Explanation:**  
  _Explanation here_  
- **Vulnerability Fix:**  
  _Fix info here_  
- **Severity:** Medium / High  
- **Proof of Concept Code:**  
  ```python
  # Example exploit code here
  ```

##### Local.txt Proof Screenshot

**Local.txt Contents:**
```
insert hash or content
```

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
  # Exploit code here
  ```

##### Proof Screenshot

**Proof.txt Contents:**
```
insert hash or content
```

---

_Repeat for other machines as needed_

#### Buffer Overflow – `192.168.57.112 (b0f-vic)`

##### Vulnerability Exploited: BOF

- _BOF notes here_  
- **Proof Screenshot:**

- **Completed Buffer Overflow Code:**

```c
// Insert your full BOF code here
```

---

### 3.3 Maintaining Access

Maintaining access ensures persistence post-exploitation. After exploiting, I ensured persistent access via reverse shells or backdoors (as needed) on critical systems.

---

### 3.4 House Cleaning

All artifacts from the assessment were removed, including shells, scripts, accounts, and tools. No lingering traces were left on the compromised systems.

---

## 4. Additional Items

### 4.1 Proof and Local Contents

| IP Address      | Hostname   | Local.txt         | Proof.txt         |
|-----------------|------------|-------------------|-------------------|
| 192.168.xx.xx   | Host1      | `hash1`           | `hash1_proof`     |
| 192.168.xx.xx   | Host2      | `hash2`           | `hash2_proof`     |
| 192.168.xx.xx   | Host3      | `hash3`           | `hash3_proof`     |
| 192.168.xx.xx   | Host4      | `hash4`           | `hash4_proof`     |
| 192.168.57.112  | b0f-vic    | `hash5`           | `hash5_proof`     |

---

**End of Report**
