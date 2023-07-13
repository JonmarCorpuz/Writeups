# About This Module
Module link: https://tryhackme.com/room/windowslocalpersistence

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 07/13/2023

# Tasks

## What is the process ID of "notepad.exe"?

1. Open up the Logfile.PML file using Procmon by first launching Procmon and then opening the Logfile.PML file

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt1.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt2.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt3.png)

2. Once we opened the Logfile.PML file using Procmon, we'll add a filter to show results for all process activity for notepad.exe, for which the first result gives us its PID

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt4.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt5.png)

## What is the parent process ID of the previous process?

3. Clicking on the first result opens up an Event Properties window in the Event tab, in which we can see this process' parent PID

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt6.png)

## What is the integrity level of the process?

4. In the same Event Properties window, we'll go to the Process tab, in which we'll find the integrity level of this process

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt7.png)


**TASK COMPLETED!**
