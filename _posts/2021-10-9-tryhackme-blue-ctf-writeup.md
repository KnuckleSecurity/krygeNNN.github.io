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
[**Solve Yourself >>**](https://www.tryhackme.com/room/blue)

# TRYHACKME BLUE - CTF WRITEUP 

Blue is a machine that specifically designed to cover EternalBlue (CVE-2017-0144), 
which allows remote attackers to execute arbitrary code via crafted packets, 
aka "Windows SMB Remote Code Execution Vulnerability." 


## Enumeration 

In order to enumerate the host, we will run Network Mapper (nmap) to discover opened ports and services.   


| Param                        | Function          | 
|:-----------------------------|:-----------------|
|-sV |Probe open ports to determine service/version info|
|-sC or --script=default | Performs a script scan using the default set of scripts|
|-O  | Enable OS detection|
|-T4 | T{0-5} Set scan speed, higher is faster|
|-p- | Scan all 65536 ports|


Full command : `nmap {machine IP} -sV -sC -O -T4 -p-`
![Window Shadow](/assets/img/posts/tryhackme-blue-ctf-writeup/blue2.jpg){: .shadow width="1220" height:"1373" style="max-width: 80%" .left}
It seems like plenty of services are enabled in the system.However one of them catches the eye,

`445 /tcp open microsoft-ds Windows 7 Professional 7601 Service Pack 1 microsoft-ds (workgroup : WORKGROUP)` 

After went through the web searching phase for that specific service, we got this

[**CVE-2017-0144 - EternalBlue SMB Remote Code Execution (MS17-010)**](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-0144)

## Exploitation 

Let's use `Metasploit` framework
![Window Shadow](/assets/img/posts/tryhackme-blue-ctf-writeup/blue3.jpg){: .shadow .left}
And than use `MS17-010`
![Window Shadow](/assets/img/posts/tryhackme-blue-ctf-writeup/blue4.jpg){: .shadow .left}

Six modules are showed up, two of them are auxiliary modules and the rest are exploiting scripts.In a real-life scenario it is not that obvious that those pre-arranged scripts would work.It is highly possible that target system would crash if you ran any careless exploit to target system or you are not sure host is vulnerable to it. To check this we can run `auxiliary module`.

If you are not familiar with what an auxiliary module is, it is some kind of script that helps us to verify if the target system is vulnerable to our actual exploit or not, without crashing or corrupting the system.
More technical description would be as following; An auxiliary module does not execute a payload. It can be used to perform arbitrary actions that may not be directly related to exploitation. Examples of auxiliary modules include scanners, fuzzers, and denial of service attacks. 
 
In this case Metasploit database offers us those two auxiliary files, lets use `auxiliary/scanner/smb/smb_ms17_010` ,than set RHOSTS  as IP address of  our target machine, than run it.
