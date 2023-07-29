```PowerShell
C:\>dir /s "sigtool.exe"
 Volume in drive C has no label.
 Volume Serial Number is 9855-3AC5

 Directory of C:\Program Files\ClamAV

01/10/2022  07:37 PM           279,552 sigtool.exe
               1 File(s)        279,552 bytes

     Total Files Listed:
               1 File(s)        279,552 bytes
               0 Dir(s)  22,689,230,848 bytes free
```

```PowerShell
C:\>dir /s "AV-Check.exe"
 Volume in drive C has no label.
 Volume Serial Number is 9855-3AC5

 Directory of C:\Users\thm\Desktop\Samples

03/18/2022  06:44 AM             6,144 AV-Check.exe
               1 File(s)          6,144 bytes

 Directory of C:\Users\thm\source\repos\AV-Check\AV-Check\obj\Debug

03/18/2022  06:49 AM             6,144 AV-Check.exe
               1 File(s)          6,144 bytes

 Directory of C:\Users\thm\source\repos\AV-Check\AV-Check\obj\x64\Debug

03/17/2022  03:24 AM             5,632 AV-Check.exe
               1 File(s)          5,632 bytes

     Total Files Listed:
               3 File(s)         17,920 bytes
               0 Dir(s)  22,689,230,848 bytes free
```

```PowerShell
C:\>"c:\Program Files\ClamAV\sigtool.exe" --md5 "C:\Users\thm\Desktop\Samples\AV-Check.exe"
f4a974b0cf25dca7fbce8701b7ab3a88:6144:AV-Check.exe
```

```PowerShell
C:\>strings C:\Users\thm\Desktop\Samples\AV-Check.exe | findstr "THM"
THM{Y0uC4nC-5tr16s}
```
