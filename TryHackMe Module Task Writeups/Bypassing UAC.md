
# UAC: GUI Based Bypasses

```PowerShell
root@ip-10-10-123-143:~# xfreerdp /v:10.10.174.61 /u:attacker /p:Password321

connected to 10.10.174.61:3389
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@           WARNING: CERTIFICATE NAME MISMATCH!           @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
The hostname used for this connection (10.10.174.61) 
does not match the name given in the certificate:
Common Name (CN):
	MYSERVER
A valid certificate for the wrong name should NOT be trusted!
Certificate details:
	Subject: CN = MYSERVER
	Issuer: CN = MYSERVER
	Thumbprint: e6:47:85:e4:6e:e6:2e:fa:b5:ea:c2:b3:fc:e8:45:9f:f9:03:01:f2
The above X.509 certificate could not be verified, possibly because you do not have the CA certificate in your certificate store, or the certificate has expired. Please look at the documentation on how to create local certificate store for a private CA.
Do you trust the above certificate? (Y/N) 
Do you trust the above certificate? (Y/N) 
Do you trust the above certificate? (Y/N) 
Do you trust the above certificate? (Y/N) 
Do you trust the above certificate? (Y/N) y
```

![]()

# UAC: Auto-Elevating Processes
