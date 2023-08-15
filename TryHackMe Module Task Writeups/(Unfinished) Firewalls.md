# Room Information 

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Banner%20-%20Firewalls.png)

Room link: https://tryhackme.com/room/redteamfirewalls

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 08/15/2023

# Room Tasks

## Evasion via Modifying Header Fields

```Bash
root@ip-10-10-216-160:~# nmap -ttl 2 10.10.21.218

Starting Nmap 7.60 ( https://nmap.org ) at 2023-08-02 18:51 BST
Nmap scan report for ip-10-10-21-218.eu-west-1.compute.internal (10.10.21.218)
Host is up (0.00029s latency).
Not shown: 997 filtered ports
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
3389/tcp open  ms-wbt-server
MAC Address: 02:21:EC:C8:1A:51 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 17.96 seconds
```

## Evasion via Modifying Header Fields

```Bash
root@ip-10-10-216-160:~# nmap --badsum 10.10.21.218

Starting Nmap 7.60 ( https://nmap.org ) at 2023-08-02 18:56 BST
Nmap scan report for ip-10-10-21-218.eu-west-1.compute.internal (10.10.21.218)
Host is up (0.000052s latency).
All 1000 scanned ports on ip-10-10-21-218.eu-west-1.compute.internal (10.10.21.218) are filtered
MAC Address: 02:21:EC:C8:1A:51 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 21.45 seconds
```

## Evasion Using Port Hopping 

```Bash
root@ip-10-10-216-160:~# nc -lvnp 21
Listening on [0.0.0.0] (family 0, port 21)
```

```Bash
root@ip-10-10-216-160:~# firefox http://10.10.87.92:8000
```

![]()

![]()

```Bash
Listening on [0.0.0.0] (family 0, port 21)
Connection from 10.10.87.92 38304 received!
```

## Evasion Using Port Tunneling

```Bash
root@ip-10-10-173-37:~# nc -lvnp 21
Listening on [0.0.0.0] (family 0, port 21)
```

![]()

![]()

```Bash
Listening on [0.0.0.0] (family 0, port 21)
Connection from 10.10.87.92 38426 received!
GET / HTTP/1.1
host: default

HTTP/1.1 200 OK
Server: nginx/1.18.0 (Ubuntu)
Date: Wed, 02 Aug 2023 20:00:35 GMT
Content-Type: text/html
Content-Length: 223
Last-Modified: Thu, 13 Jan 2022 14:44:03 GMT
Connection: keep-alive
ETag: "61e03ab3-df"
Accept-Ranges: bytes

<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Welcome to Our Local Server</title>
</head>
<body>
        THM{1298331956}
</body>
</html>
```

# Evasion Using Non-Standard Ports
