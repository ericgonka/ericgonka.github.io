![image](https://github.com/user-attachments/assets/b6e362c9-0bb2-495d-a442-ee52fe1caa4b)
---
title: Offensive Security Exam Report
author: example@example.example
osid: XXXXX
---

# Offensive Security  
## Penetration Test Report for OSCP Exam  
**Version:** 3.2  
**OSID:** XXXXX  
**Email:** example@example.example  

![image](https://github.com/user-attachments/assets/d6b50208-9978-426d-bdd3-6cffcd6778c7)

© All rights reserved to Offensive Security, 2014

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
   - Appendix 1 - Proof and Local Contents  
   - Appendix 2 - Metasploit/Meterpreter Usage  
   - Appendix 3 - Completed Buffer Overflow Code  

---

## 1. Offensive Security Exam Penetration Test Report

### 1.1 Introduction

The Offensive Security Exam penetration test report contains all efforts that were conducted in order to pass the Offensive Security exam. This report will be graded from a standpoint of correctness and fullness to all aspects of the exam. The purpose of this report is to ensure that the student has a full understanding of penetration testing methodologies as well as the technical knowledge to pass the qualifications for the Offensive Security Certified Professional.

### 1.2 Objective

The objective of this assessment is to perform an internal penetration test against the Offensive Security Exam network. The student is tasked with following a methodical approach in obtaining access to the objective goals. This test should simulate an actual penetration test and how you would start from beginning to end, including the overall report.

### 1.3 Requirements

The student will be required to fill out this penetration testing report fully and to include the following sections:

- Overall High-Level Summary and Recommendations (non-technical)  
- Methodology walkthrough and detailed outline of steps taken  
- Each finding with included screenshots, walkthrough, sample code, and proof.txt if applicable  
- Any additional items that were not included  

---

## 2. High-Level Summary

I was tasked with performing an internal penetration test towards Offensive Security Exam. An internal penetration test is a dedicated attack against internally connected systems. The focus of this test is to perform attacks, similar to those of a hacker and attempt to infiltrate Offensive Security’s internal exam systems – the `THINC.local` domain. My overall objective was to evaluate the network, identify systems, and exploit flaws while reporting the findings back to Offensive Security.

When performing the internal penetration test, there were several alarming vulnerabilities that were identified on Offensive Security’s network. When performing the attacks, I was able to gain access to multiple machines, primarily due to outdated patches and poor security configurations. During the testing, I had administrative level access to multiple systems. All systems were successfully exploited and access granted.

### Compromised Hosts

- `192.168.xx.xx (hostname)` – Name of initial exploit  
- `192.168.xx.xx (hostname)` – Name of initial exploit  
- `192.168.xx.xx (hostname)` – Name of initial exploit  
- `192.168.xx.xx (hostname)` – Name of initial exploit  
- `192.168.xx.xx (hostname)` – Buffer Overflow (BOF)

---

### 2.1 Recommendations

I recommend patching the vulnerabilities identified during the testing to ensure that an attacker cannot exploit these systems in the future. These systems require frequent patching and should remain on a regular patch program to protect against future vulnerabilities.

---

## 3. Methodologies

### 3.1 Information Gathering

The information gathering portion of a penetration test focuses on identifying the scope of the penetration test. During this penetration test, I was tasked with exploiting the exam network. The specific IP addresses were:

- `192.168.xx.xx`
- `192.168.xx.xx`
- `192.168.xx.xx`
- `192.168.xx.xx`
- `192.168.57.112` (b0f-vic)

---

### 3.2 Penetration

_Repeat for each system as shown below:_

#### System IP: 192.168.xx.xx

##### Service Enumeration

- **Server IP Address:** 192.168.xx.xx  
- **Ports Open:**  
  - TCP:  
  - UDP:  
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

### Appendix 1 - Proof and Local Contents

| IP Address      | Hostname   | Local.txt         | Proof.txt         |
|-----------------|------------|-------------------|-------------------|
| 192.168.xx.xx   | Host1      | `hash1`           | `hash1_proof`     |
| 192.168.xx.xx   | Host2      | `hash2`           | `hash2_proof`     |
| 192.168.xx.xx   | Host3      | `hash3`           | `hash3_proof`     |
| 192.168.xx.xx   | Host4      | `hash4`           | `hash4_proof`     |
| 192.168.57.112  | b0f-vic    | `hash5`           | `hash5_proof`     |

---

### Appendix 2 - Metasploit/Meterpreter Usage

I used my Metasploit/Meterpreter allowance on the following machine:

- `192.168.xx.xx`

---

### Appendix 3 - Completed Buffer Overflow Code

```c
// Insert your BOF code here
```

---

**End of Report**
