
# NTLM Authenticated Services

1. Download this task's tasks files and unzip it

![]()

```Bash
┌──(kali㉿kali)-[~]
└─$ unzip ~/Downloads/passwordsprayer.zip 
Archive:  /home/kali/Downloads/passwordsprayer.zip
  inflating: ntlm_passwordspray.py   
  inflating: usernames.txt
```

```Bash
┌──(kali㉿kali)-[~]
└─$ python3 ntlm_passwordspray.py
Traceback (most recent call last):
  File "/home/kali/ntlm_passwordspray.py", line 4, in <module>
    from requests_ntlm import HttpNtlmAuth
ModuleNotFoundError: No module named 'requests_ntlm'
```

