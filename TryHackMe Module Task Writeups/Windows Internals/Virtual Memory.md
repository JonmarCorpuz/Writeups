# About This Module
Module link: https://tryhackme.com/room/windowslocalpersistence

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 07/13/2023

# Tasks

## What is the base address of "notepad.exe"?

1. Open up the Logfile.PML file using Procmon by first launching Procmon, which is a Microsoft Sysinternals tool that monitors and logs system activities in real-time, and then opening the Logfile.PML (Process Monitor Log) file

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt1.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt2.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt3.png)

2. Once we opened the Logfile.PML file using Procmon, we'll add a filter to show results for all process activity for notepad.exe

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt4.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt5.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Threads%20pt1.png)

3. Clicking on the third top result will open up the Event Properties window in the Event tab, which shows us this process' base address

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Virtual%20Memory%20pt1.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Virtual%20Memory%20pt2.png)


**TASK COMPLETED!**
