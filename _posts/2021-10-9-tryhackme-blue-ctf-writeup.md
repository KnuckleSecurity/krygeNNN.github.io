---
title: Tryhackme Blue Ctf Writeup
author: krygennn
date: 2021-10-9 00:59
categories: [Blogging,cyber-security]
image:
    src: /assets/img/posts/tryhackme-blue-ctf-writeup/blue1.jpg
    alt: TRYHACKME BLUE
tags: [ctf,pentesting]
---
# TRYHACKME BLUE - CTF WRITEUP 
Blue is a machine that specifically designed to cover EternalBlue (CVE-2017-0144), 
which allows remote attackers to execute arbitrary code via crafted packets, 
aka "Windows SMB Remote Code Execution Vulnerability." 


## Enumeration 
´´´
In order to enumerate the host, we will run Network Mapper (nmap) to discover opened ports and services.   

** -sV >  Probe open ports to determine service/version info 

**  -sC or --script=default >  Performs a script scan using the default set of scripts.

**  -O > Enable OS detection.

**  -T4 > T{0-5} Set scan speed, higher is faster.

**  -p- > Scan all 65536 ports.

Full command : ´´´ nmap {machine IP} -sV -sC -O -T4 -p- ´´´ 
