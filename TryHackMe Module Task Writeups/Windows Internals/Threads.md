# About This Module
Module link: https://tryhackme.com/room/windowslocalpersistence

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 07/13/2023

# Tasks

## What is the thread ID of the first thread created by notepad.exe?

1. Open up the Logfile.PML file using Procmon by first launching Procmon and then opening the Logfile.PML file

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt1.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt2.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt3.png)

2. Once we opened the Logfile.PML file using Procmon, we'll add a filter to show results for all process activity for notepad.exe

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt4.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt5.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Threads%20pt1.png)

3. Clicking on the second result opens up an Event Properties window in its Event tab, in which we can see this process' thread ID

![]()

## What is the stack argument of the previous thread?


