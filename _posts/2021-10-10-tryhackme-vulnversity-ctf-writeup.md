---
title: Tryhackme Vulnversity Ctf Writeup
author: krygennn
date: 2021-10-9 16:10
categories: [Blogging,cyber-security]
image:
    src: /assets/img/posts/tryhackme-vulnversity-ctf-writeup/vuln1.jpg
    alt: TRYHACKME BLUE
tags: [ctf,pentesting]
---
[**Solve Yourself >>**](https://www.tryhackme.com/room/vulnversity)

# TRYHACKME VULNVERSITY - CTF WRITEUP 
Vulnversity is a machine that combines reconnaissance, web attack vectors and privilege escalation methods together.
<br>
## Enumeration
Let's start with enumerating the host.I will use Network Mapper (`nmap`) tool in order to scan and probe opened ports and active services to find my attack vector. 
| Parameter  | Functionality                                      |
|:-----------|:---------------------------------------------------|
|-sS         |SYN Stealth Scan (Half TCP Connect scan)            |
|-sV         |Probe open ports to determine service/version info  |
|-O          |Enable OS detection.                                |
|-T4         |T{0-5} Set scan speed, higher is faster.            |
|-p-         |Scan all 65536 ports.                               |
|-oN         |Outputfile                                          |


Full command: nmap `{machine IP} -sV -sS -O -T4 -p- -oN vulnversity.nmap`
![Window Shadow](/assets/img/posts/tryhackme-vulnversity-ctf-writeup/vuln2.jpg){: .shadow style="max-width: 80%" .normal} 
<br>
As you can see from the results, we have got six different ports active to create our attack vector.If you do your research and investigate whether one of these services have any vulnerabilities or not, you will return to your home empty handed.You can try anonymous login to FTP protocol, or try to make a SSH connection but you would fail just like I did.None of these services have any vulnerabilities that would put us in the target system.
 
Other than that `"3333/tcp open  http   Apache httpd 2.4.18 ((Ubuntu))"` catches the eye, so lets start to investigate the webpage that the host is running on its port 3333.
![Window Shadow](/assets/img/posts/tryhackme-vulnversity-ctf-writeup/vuln3.jpg){: .shadow style="max-width: 80%" .normal} 
<br>
