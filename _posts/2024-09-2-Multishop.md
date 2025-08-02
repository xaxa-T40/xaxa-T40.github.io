---
title: "Multishop"
# date: 2019-04-18T15:34:30-04:00
categories:
  - blog
tags:
  - ctf
  - hacktrace-ranges
  - retired-mechines
---


### Affected Endpoint ###
Scope
```
     host/url:10.1.2.140
    Description: Access using hacktrace vpn
```
working time: 30 agustus - 2 september 2024

### Description ###
Background Story: The more you read, the more things you will know. The more you learn, the more places you will visit. 

vulnerability: file inclusion and Misuse of Linux Capabilities.

### Impact ###
Impact File Inclusion & Misuse of Linux Capabilities is:
- Information Disclosure.
- Code Execution.
- Privilege Escalation.
- System Integrity Compromise.

### Step To Reproduce ###
The steps in finding  vulnerability are as follows:
- Information Gathering.
- Enumeration.
- Exploit.
- Privilege escalation.
- Reporting.

### PoC ###
#### Information Gathering ####
Just as usual, we start off with the Nmap scan:
```
nmap -sCV -p- --min-rate=1000 -oN nmap -Pn 10.1.2.140
```
![alt text](/assets/images/hacktrace/multishop/image1.png)
There are two ports open: 22 (SSH), 80 (HTTP). Not having any credentials for the SSH service service, we will start with enumerating port 80. 
Scan directory using dirsearch
``` 
dirsearch -u http://10.1.2.140 -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -t 50
```
![alt text](/assets/images/hacktrace/multishop/image2.png)
Try scanning the directory again, this time it's interesting in the bio
```
dirsearch -u http://10.1.2.140/bio -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -t 50
```