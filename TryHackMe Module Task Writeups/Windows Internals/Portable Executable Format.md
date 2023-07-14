# About This Module
Module link: https://tryhackme.com/room/windowslocalpersistence

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 07/14/2023

# Tasks

## What is the entry point reported by DiE and what is the value of "NumberOfSections"??

1. We'll first launch DiE (Detect it Easy)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Portable%20Executable%20Format%20pt1.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Portable%20Executable%20Format%20pt2.png)

2. Once we successfully opened DiE, we'll go and analyze the **notepad.exe** executable file, which allowed us to see its entry point, which refers to a specific address or location within an executable file where the program's execution begins, as well as the value of NumberOfSections, which refers to the total number of sections or segments within the executable file 

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Portable%20Executable%20Format%20pt3.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Portable%20Executable%20Format%20pt4.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Portable%20Executable%20Format%20pt5.png)


**TASK COMPLETED!**


## What is the virtual address of ".data"?

3. Clicking clicking on the **PE** button will open a small window displaying more information about the executable file, including the virtual address of **.data**, which refers to a specific section within an executable file that is dedicated to storing initialized or pre-defined data used by the program 

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Portable%20Executable%20Format%20pt6.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Portable%20Executable%20Format%20pt7.png)


**TASK COMPLETED!**


## What string is located at the offset "0001f99c"?

4. In the small window we have opened, we'll go to the **Strings** section and go down until we find the string that we're looking for that's located at the specified offset

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Portable%20Executable%20Format%20pt8.png)


**TASK COMPLETED!**
