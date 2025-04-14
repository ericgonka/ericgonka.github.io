---
title: "Jack WP - Professional Exam Report (OSCP Style)"
author: ["ericgon28@proton.me", "OSID: XXXX"]
date: "2025-04-14"
subject: "Markdown"
keywords: [OSCP, Wordpress]
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
  - TryHackMe
tags:
  - Difícil
  - CTF
  - Wordpress
image: "https://github.com/user-attachments/assets/a5356a21-0b2a-42d3-89ee-3e0d540acd32"
---
# Jack WP - Professional Exam Report (OSCP Style)

## Introduction

This machine is a great way to learn about the different ways to enumerate and exploit WordPress.
To gain access, we must enumerate the server with WPScan; however, we will only get enough information to fully compromise it by performing an additional enumeration with Nmap scripts.
Finally, before we begin, we add "jack.thm" to the hosts file (/etc/hosts) as needed for name resolution.

## Objective

The goal of the machine is for us to have access and subsequently escalate privileges to have full control.

## Requirements

We have to have the following requirements:

- Nmap
- WPScan

**Exam Network**

10.10.97.82

# Independent Challenges

## Target #1 - 10.10.97.82

### Service Enumeration

**Port Scan Results**

Server IP Address | Ports Open
------------------|----------------------------------------
10.10.97.82       | **TCP**: 22,80

**Nmap Scan Results:**

The Nmap scan reveals that Port 22 and Port 80 is open and running OpenSSH 7.2p2 and Apache 2.4.18 respectively.

![{DE40A305-E741-4BB4-80B0-053008D4FAC3}](https://github.com/user-attachments/assets/33425e15-b076-4c3e-8ab8-f72f58ffeb9a)


**SSH Enumeration**

From experience, we know that SSH is rarely vulnerable, and especially in this version, we searched for vulnerabilities with "searchsploit" and found a Python script that allows us to list existing users.
We'll leave it at that for now, as we don't have a list of possible valid users.

![{D2BF5A41-E380-4967-8F99-EBC9A5EE59B1}](https://github.com/user-attachments/assets/4c1f1cdd-c793-4661-b902-29437000055a)

**HTTP Enumeration**

We add the jack.thm domain referencing the machine's IP (10.10.97.82) in /etc/hosts.

![{462A6FF3-E1E5-464D-B4C3-CF0E06576281}](https://github.com/user-attachments/assets/9d462718-6131-43f3-9425-a7dc3b9d1976)

The server is running Jack’s personal blog.

![{5CD26E73-DFDF-4301-9FB7-B3F995F98876}](https://github.com/user-attachments/assets/1232d4d8-9053-4936-9596-9c1d9dbf4df8)

With the wappalyzer extension, we see information about the content manager and its version, which is interesting since we could search for any vulnerabilities associated with that version.

![{4271C40B-EDBD-4735-900F-8582F43DF37C}](https://github.com/user-attachments/assets/d45ef1a4-7575-40ad-9963-6dc4298a1762)

Whatweb also tells us the same thing but through the console.

![{0F342F98-756F-4436-A751-D6DC270B5A75}](https://github.com/user-attachments/assets/0b7403c2-e93c-48bd-99d2-7c7bba8a1307)

Now that we know the site is running Wordpress 5.3.2 we can start to enumerate some more specific information such as Plugins, Themes, Users, Config Backups, DB Exports and more. My tool of choice for Wordpress enumeration is WPScan.

We can use WPScan to enumerate the above items.

ap = All Plugins
at = All Themes
u = Enumerate Users

![{2AD6DF69-3C9F-4234-BD1B-B560B35DD2B4}](https://github.com/user-attachments/assets/b6c5bfb5-4024-44ec-b4cb-1f31cddba8c3)


### Explotation

Our results show that we have 3 users of value but very little else in terms of vulnerable plugins or themes.

![{BC9B5831-1AE1-4D3B-90FA-4F47A20D119A}](https://github.com/user-attachments/assets/72102aa0-a1ff-4e71-bf18-8549781ea97f)

![{BFC1CDDB-F3EF-4C1B-BA54-16BA0B39DD11}](https://github.com/user-attachments/assets/ce2d3d48-8953-4f82-a71d-c59091d38246)


Our next step will be to see if we can get a valid password for one of the three users. From our WPScan we can see that XML-RPC is accessible so we can use WPScan to execute the bruteforce attack.

![{D12AE816-5531-44B9-87F9-AA10C19B9233}](https://github.com/user-attachments/assets/e11f26eb-ef75-44ca-aa32-775a9d574199)

In this case, it worked and I quickly got the password for one of the accounts.

User              | Password
------------------|----------------------------------------
Wendy             | changelater

![{93463B0A-29C8-49FB-96BF-9E3CA3BE5C53}](https://github.com/user-attachments/assets/22419621-671c-46bf-aaa1-de03e7817010)




### Initial Access - WordPress Plugin User Role Editor < 4.25 - Privilege Escalation

**Vulnerability Explanation:** Ability Server 2.34 is subject to a buffer overflow vulnerability in STOR field.
Attackers can use this vulnerability to cause arbitrary remote code execution and take completely control over the system.

**Vulnerability Fix:** The publishers of the Ability Server have issued a patch to fix this known issue.
It can be found here: http://www.code-crafters.com/abilityserver/

**Severity:** Critical

**Steps to reproduce the attack:** The operating system was different from the known public exploit.
A rewritten exploit was needed in order for successful code execution to occur. Once the exploit was rewritten, a targeted attack was performed on the system which gave John full administrative access over the system.

**Proof of Concept Code Here:** Modifications to the existing exploit was needed and is highlighted in red.

```python

```

**Proof Screenshot:**

IMATGE

\newpage

### Privilege Escalation - MySQL Injection

**Vulnerability Explanation:** After establishing a foothold on target, John noticed there were several applications running locally, one of them, a custom web application on port 80 was prone to SQL Injection attacks.
Using Chisel for port forwarding, John was able to access the web application.
When performing the penetration test, John noticed error-based MySQL Injection on the taxid query string parameter.
While enumerating table data, John was able to successfully extract the database root account login and password credentials that were unencrypted that also matched username and password accounts for the administrative user account on the system and John was able to log in remotely using RDP.
This allowed for a successful breach of the operating system as well as all data contained on the system.

**Vulnerability Fix:** Since this is a custom web application, a specific update will not properly solve this issue.
The application will need to be programmed to properly sanitize user-input data, ensure that the user is running off of a limited user account, and that any sensitive data stored within the SQL database is properly encrypted.
Custom error messages are highly recommended, as it becomes more challenging for the attacker to exploit a given weakness if errors are not being presented back to them.

**Severity:** Critical

**Steps to reproduce the attack:**

**Proof of Concept Code Here:**

```sql
SELECT * FROM login WHERE id = 1 or 1=1 AND user LIKE "%root%"
```

### Post-Exploitation

**System Proof Screenshot:**

IMATGE
