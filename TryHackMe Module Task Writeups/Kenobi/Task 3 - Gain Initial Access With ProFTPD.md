
```Bash
root@ip-10-10-7-66:~# nc 10.10.156.18 21
220 ProFTPD 1.3.5 Server (ProFTPD Default Installation) [10.10.156.18]
```

```Bash
220 ProFTPD 1.3.5 Server (ProFTPD Default Installation) [10.10.156.18]
^C
root@ip-10-10-7-66:~# 
```

```Bash
root@ip-10-10-7-66:~# searchsploit ProFTPD 1.3.5
[i] Found (#2): /opt/searchsploit/files_exploits.csv
[i] To remove this message, please edit "/opt/searchsploit/.searchsploit_rc" for "files_exploits.csv" (package_array: exploitdb)

[i] Found (#2): /opt/searchsploit/files_shellcodes.csv
[i] To remove this message, please edit "/opt/searchsploit/.searchsploit_rc" for "files_shellcodes.csv" (package_array: exploitdb)

------------------------------------------------- ---------------------------------
 Exploit Title                                   |  Path
------------------------------------------------- ---------------------------------
ProFTPd 1.3.5 - 'mod_copy' Command Execution (Me | linux/remote/37262.rb
ProFTPd 1.3.5 - 'mod_copy' Remote Command Execut | linux/remote/36803.py
ProFTPd 1.3.5 - File Copy                        | linux/remote/36742.txt
------------------------------------------------- ---------------------------------
Shellcodes: No Results
```


