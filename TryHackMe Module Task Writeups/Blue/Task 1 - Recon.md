
```Bash
root@ip-10-10-95-15:~# nmap -sC -sV 10.10.84.232

Starting Nmap 7.60 ( https://nmap.org ) at 2023-07-19 00:29 BST
Nmap scan report for ip-10-10-84-232.eu-west-1.compute.internal (10.10.84.232)
Host is up (0.00036s latency).
Not shown: 991 closed ports
PORT      STATE SERVICE       VERSION
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds  Windows 7 Professional 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)
3389/tcp  open  ms-wbt-server Microsoft Terminal Service
| ssl-cert: Subject: commonName=Jon-PC
| Not valid before: 2023-07-17T23:29:02
|_Not valid after:  2024-01-16T23:29:02
|_ssl-date: 2023-07-18T23:31:49+00:00; +1s from scanner time.
49152/tcp open  msrpc         Microsoft Windows RPC
49153/tcp open  msrpc         Microsoft Windows RPC
49154/tcp open  msrpc         Microsoft Windows RPC
49158/tcp open  msrpc         Microsoft Windows RPC
49159/tcp open  msrpc         Microsoft Windows RPC
MAC Address: 02:AB:F6:F6:A9:2F (Unknown)
Service Info: Host: JON-PC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_nbstat: NetBIOS name: JON-PC, NetBIOS user: <unknown>, NetBIOS MAC: 02:ab:f6:f6:a9:2f (unknown)
| smb-os-discovery: 
|   OS: Windows 7 Professional 7601 Service Pack 1 (Windows 7 Professional 6.1)
|   OS CPE: cpe:/o:microsoft:windows_7::sp1:professional
|   Computer name: Jon-PC
|   NetBIOS computer name: JON-PC\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2023-07-18T18:31:49-05:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2023-07-19 00:31:49
|_  start_date: 2023-07-19 00:29:00

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 119.09 seconds
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Blue/Assets/Blue%20pt1.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Blue/Assets/Blue%20pt2.png)
