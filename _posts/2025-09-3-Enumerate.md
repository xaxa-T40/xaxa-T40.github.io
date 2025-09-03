---
title: "defhawk"
# date: 2019-04-18T15:34:30-04:00
categories:
  - blog
tags:
  - ctf
  - web
  - defhawk
---

### Description ###
 There’s a website behaving strangely. Maybe it's including files without proper checks? Try visiting the playground link and injecting a payload that can help you read sensitive system files. If you find the right path, it may just lead you to the flag!

![alt text](/assets/images/deffhack/enumerate-1.png)
web interface tested using Node.js.
![alt text](/assets/images/deffhack/enumerate-2.png)
There is one input button that could be a point of vulnerability for file inclusion. Test it first to get the parameters. Use Burp Suite to make it easier.
![alt text](/assets/images/deffhack/enumerate-3.png)
I entered the word “test” and the result was “file not found.” Why is the file not found? It could be that the input is searching for a file.
![alt text](/assets/images/deffhack/enumerate-4.png)
![alt text](/assets/images/deffhack/enumerate-5.png)
in the term parameter, no file inclusion vulnerability was found ```/search?term=../../../../etc/passwd&sample=Sample1 ```
![alt text](/assets/images/deffhack/enumerate-6.png)
The existing vulnerability parameter was found in the sample.```/search?term=test&sample=../../../../etc/passwd``` 
lanjutkan untuk mencari flagnya
![alt text](/assets/images/deffhack/enumerate-7.png)
```
/search?term=test&sample=../../../../usr/src/app/flag.txt
```
and the flag was successfully obtained
Flag: CRAC{So_Y0u_List3d_Th3_Secret}

