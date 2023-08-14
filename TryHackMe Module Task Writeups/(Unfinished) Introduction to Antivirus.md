# Room Information

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Banner%20-%20Introduction%20to%20Antivirus.png)

Room link: https://tryhackme.com/room/introtoav

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 08/14/2023

# Room Tasks

## AV Static Detection

### Using sigtool to Generate a Hash
1. Started up this task's machine
2. `cd \` from the compromised Windows machine to change to the machine's root directory from where we'll recursively search for the `sigtool` and `AV-Check` tools
```PowerShell
C:\Users\thm>cd \
```
3. `dir /s "sigtool.exe"` from the compromised Windows machine to search for the already downloaded sigtool tool
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

4. `dir /s "AV-Check.exe"` from the compromised Windows machine to search for the already downloaded AV-Check tool
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

5. `sigtool.exe --md5 AV-Check.exe` from the compromised Windows machine to generate an MD5 hash of the AV-Check.exe executable file, which ended up being the correct answer for this task
```PowerShell
C:\>"c:\Program Files\ClamAV\sigtool.exe" --md5 "C:\Users\thm\Desktop\Samples\AV-Check.exe"
f4a974b0cf25dca7fbce8701b7ab3a88:6144:AV-Check.exe
```

### Reading Human-Readable Strings From the AV-Check Binary

1. Started up this task's machine
2. `strings AV-Check.exe | findstr <STRING>` from the compromised Windows machine to display the lines within the AV-Check executable file containing the string "THM" onto our temrinal, which ended up displaying this task's flag
```PowerShell
C:\>strings C:\Users\thm\Desktop\Samples\AV-Check.exe | findstr "THM"
THM{Y0uC4nC-5tr16s}
```


**TASK COMPLETED!**
