---
layout: post
title: "TryHackMe: Simple CTF Walkthrough"
date: 2022-01-02
categories: tryhackme
---

## RECON
As always, the first step to any engagement is to do a simple recon on the target machine. This can be done by using nmap which should already come preinstalled on Kali or the AttackBox if using the tryhackme resources. Use the _-O_ and _-sV_ switches to check the Operating System and Service Version respectively. The answers to the first questions can be found from the results.

![nmap_scan](https://user-images.githubusercontent.com/20043220/147867985-ae5cba49-946f-4c75-bf98-2ae7b59f8825.png)

The next question asks asks to find a CVE that can be used to attack the machine. Given the results from the nmap scan, the first thing is to check out the server on port 80. Doing so shows that there is an apache server being hosted on the machine. To get more information on the server, GoBuster can be used to enumerate if there are any other directories.

![gobuster](https://user-images.githubusercontent.com/20043220/147872232-e90f88bf-a31c-45ca-b084-5a021e4e4acd.png)

As the results show, there is a directory named _simple_. Accessing it will give us the version number which can be used to find the CVE number. Reading the CVE details will also explain what type of vulnerability it is.

![cve](https://user-images.githubusercontent.com/20043220/147868619-6b960ba8-51e5-43c2-9300-772c7f49c94e.png)

## INITIAL EXPLOIT
Copy the exploit details provided and save them in a file called _exploit.py_. There may be some syntax issues if you try to run the python file. As of writing this, the file needed opening and closing brackets added to all the print statements. Once modified, the _best110.txt_ file can be used as a wordlist. The file can be downloaded from https://github.com/danielmiessler/SecLists/blob/master/Passwords/Common-Credentials/best110.txt. The script can be run using the following syntax: _domain/ip -c -w worldlist_. Once the username and password have been cracked, those credentials can be used to try and ssh into the machine as enumerated from the initial nmap scan. Once ssh is successful, the first flag can be found in the root directory. 

![user_txt](https://user-images.githubusercontent.com/20043220/147872090-b6ceb43f-0cd6-4b6f-9e92-97e723dd95b9.png)

## PRIVILEDGE EXCALATION
At this point, the current user cannot access the root flag. In order to find the current user's permissions, the following command is used: _sudo -l_. This will be the attack vector used to excalate the priviledge. Star vim with _sudo vim_. This will open vim which will be used to trick the sytem into running commands wit sudo priviledges. Typing _:!bash_ will prompt vim to open a bash session. Since vim in currently running with root priviledge, the command will also open bash with similar priviledges. Now simply cd into the root directory and the final flag will be accessible.

![final](https://user-images.githubusercontent.com/20043220/147872528-2bd35925-5756-432c-a732-da437a8463dd.png)
