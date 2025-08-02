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
![alt text](/assets/images/hacktrace/multishop/image3.png)
That's it, nothing interesting, try looking into the url
![alt text](/assets/images/hacktrace/multishop/image4.png)
It looks like there is no upload file, I tried view-page:source
![alt text](/assets/images/hacktrace/multishop/image5.png)
There is a name parameter, this can indicate input validation, let's try to change the value 
```
http://10.1.2.140/bio/?name=../../../etc/passwd
```
![alt text](/assets/images/hacktrace/multishop/image6.png)
It turns out that we can do file inclusion, seen from the response given. 
Referensi file inclusion :
`https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/File%20Inclusion/README.md`
after reading through it turns out that file inclusion using wrappers (php: //filter) can be entered, we create the payload using the php_filter_chain_generator.py tool 
```
python3 php_filter_chain_generator.py --chain '<?php phpinfo();?>'
```
later the payload will come out and paste it in the parameter name, if you want the book to continue.
![alt text](/assets/images/hacktrace/multishop/image7.png)
And it does, we can continue to rce, but we have to create a file and live python3 -m http.server, the steps are as follows:
1.	Create a file with the name l (fiel that contains revshel bash) why is the name l and does not contain extensions? because so that the payload is not too long, because it is usually an error, the length of url
 ![alt text](/assets/images/hacktrace/multishop/image8.png)

