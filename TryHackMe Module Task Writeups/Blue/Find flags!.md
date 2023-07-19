
```PowerShell
C:\Windows\system32>dir /s C:\*flag1*
dir /s C:\*flag1*
 Volume in drive C has no label.
 Volume Serial Number is E611-0B66

 Directory of C:\

03/17/2019  02:27 PM                24 flag1.txt
               1 File(s)             24 bytes

 Directory of C:\Users\Jon\AppData\Roaming\Microsoft\Windows\Recent

03/17/2019  02:26 PM               482 flag1.lnk
               1 File(s)            482 bytes

     Total Files Listed:
               2 File(s)            506 bytes
               0 Dir(s)  21,103,321,088 bytes free
```

```PowerShell
C:\Windows\system32>type C:\flag1.txt
type C:\flag1.txt
flag{access_the_machine}
```

```PowerShell
C:\Windows\system32>dir /s C:\*flag2*
dir /s C:\*flag2*
 Volume in drive C has no label.
 Volume Serial Number is E611-0B66

 Directory of C:\Users\Jon\AppData\Roaming\Microsoft\Windows\Recent

03/17/2019  02:30 PM               848 flag2.lnk
               1 File(s)            848 bytes

 Directory of C:\Windows\System32\config

03/17/2019  02:32 PM                34 flag2.txt
               1 File(s)             34 bytes

     Total Files Listed:
               2 File(s)            882 bytes
               0 Dir(s)  21,103,026,176 bytes free
```

```PowerShell
C:\Windows\system32>type C:\Windows\System32\config\flag2.txt
type C:\Windows\System32\config\flag2.txt
flag{sam_database_elevated_access}
```

```PowerShell
C:\Windows\system32>dir /s C:\*flag3*
dir /s C:\*flag3*
 Volume in drive C has no label.
 Volume Serial Number is E611-0B66

 Directory of C:\Users\Jon\AppData\Roaming\Microsoft\Windows\Recent

03/17/2019  02:32 PM             2,344 flag3.lnk
               1 File(s)          2,344 bytes

 Directory of C:\Users\Jon\Documents

03/17/2019  02:26 PM                37 flag3.txt
               1 File(s)             37 bytes

     Total Files Listed:
               2 File(s)          2,381 bytes
               0 Dir(s)  21,099,413,504 bytes free
```

```PowerShell
C:\Windows\system32>type C:\Users\Jon\Documents\flag3.txt
type C:\Users\Jon\Documents\flag3.txt
flag{admin_documents_can_be_valuable}
```
