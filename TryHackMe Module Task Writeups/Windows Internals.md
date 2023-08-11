# Room Information

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Banner%20-%20Windows%20Internals.png)

Room link: [Windows Internals](https://tryhackme.com/room/windowsinternals)

# Task Writeups

## Processes

### What is the process ID of "notepad.exe"?

1. Open up the Logfile.PML file using Procmon by first launching Procmon, which is a Microsoft Sysinternals tool that monitors and logs system activities in real-time, and then opening the Logfile.PML (Process Monitor Log) file

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt1.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt2.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt3.png)

2. Once we opened the Logfile.PML file using Procmon, we'll add a filter to show results for all process activity for notepad.exe, for which the first result gives us its PID

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt4.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt5.png)


**TASK COMPLETED!**


### What is the parent process ID of the previous process?

3. Clicking on the first result opens up an Event Properties window in the Event tab, in which we can see this process' parent PID

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt6.png)


**TASK COMPLETED!**

### What is the integrity level of the process?

4. In the same Event Properties window, we'll go to the Process tab, in which we'll find the integrity level of this process

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt7.png)


**TASK COMPLETED!**

## Threads

### What is the thread ID of the first thread created by notepad.exe?

1. Open up the Logfile.PML file using Procmon by first launching Procmon, which is a Microsoft Sysinternals tool that monitors and logs system activities in real-time, and then opening the Logfile.PML (Process Monitor Log) file

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt1.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt2.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt3.png)

2. Once we opened the Logfile.PML file using Procmon, we'll add a filter to show results for all process activity for notepad.exe, for which the second top result shows us the ID of the first thread created by the notepad.exe process

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt4.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt5.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Threads%20pt1.png)


**TASK COMPLETED!**

### What is the stack argument of the previous thread?

4. Clicking on the first top result, which is the previous thread, opens up an Event Properties window in its Event tab, in which we can see this process' thread ID

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Threads%20pt2.png)


**TASK COMPLETED!**

## Virtual Memory

### What is the base address of "notepad.exe"?

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

## Dynamic Link Libraries

### What is the base address and the size of "ntdll.dll" loaded from "notepad.exe"?

1. Open up the Logfile.PML file using Procmon by first launching Procmon, which is a Microsoft Sysinternals tool that monitors and logs system activities in real-time, and then opening the Logfile.PML (Process Monitor Log) file

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt1.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt2.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt3.png)

2. Once we opened the Logfile.PML file using Procmon, we'll add a filter to show results for all process activity for notepad.exe

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt4.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Processes%20pt5.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Threads%20pt1.png)

3. Clicking on the third top result, which should be a log for the **Load Image** operation, will open the Event Properties window in the Event tab where we'll be able to see the base address of **ntdll.dll** that was loaded from **notepad.exe**, as well as its size

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/DLL%20pt1.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/DLL%20pt2.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/DLL%20pt3.png)


**TASK COMPLETED!**

### How many DLLs were loaded by "notepad.exe"?

4. We'll add another filter to show results for any DLL that were loaded by **notepad.exe** by searching for any paths with the **.dll** extension, which would allow to us to see how many DLLs were loaded by notepad.exe

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/DLL%20pt5.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/DLL%20pt6.png)


**TASK COMPLETED!**

## Portable Executable Format

### What is the entry point reported by DiE and what is the value of "NumberOfSections"??

1. We'll first launch DiE (Detect it Easy)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Portable%20Executable%20Format%20pt1.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Portable%20Executable%20Format%20pt2.png)

2. Once we successfully opened DiE, we'll go and analyze the **notepad.exe** executable file, which allowed us to see its entry point, which refers to a specific address or location within an executable file where the program's execution begins, as well as the value of NumberOfSections, which refers to the total number of sections or segments within the executable file 

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Portable%20Executable%20Format%20pt3.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Portable%20Executable%20Format%20pt4.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Portable%20Executable%20Format%20pt5.png)


**TASK COMPLETED!**

### What is the virtual address of ".data"?

3. Clicking clicking on the **PE** button will open a small window displaying more information about the executable file, including the virtual address of **.data**, which refers to a specific section within an executable file that is dedicated to storing initialized or pre-defined data used by the program 

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Portable%20Executable%20Format%20pt6.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Portable%20Executable%20Format%20pt7.png)


**TASK COMPLETED!**

### What string is located at the offset "0001f99c"?

4. In the small window we have opened, we'll go to the **Strings** section and go down until we find the string that we're looking for that's located at the specified offset

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Portable%20Executable%20Format%20pt8.png)


**TASK COMPLETED!**

## Interacting With Windows Internals

1. Changed to the compromised machine's root directory and searched for the `inject-poc.exe' executable

```PowerShell
C:\Users\THM-Attacker>cd /
```
```PowerShell
C:\>dir /s "inject-poc.exe"
Volume in drive C has no label.
Volume Serial Number is A8A4-C362
Directory of C:\Users\THM-Attacker\Desktop\Process Injection POC                                                                                                                                 01/16/2022  10:49 PM             9,728 inject-poc.exe                                                           1 File(s)          9,728 bytes

     Total Files Listed:
               1 File(s)          9,728 bytes
               0 Dir(s)  11,371,859,968 bytes free
```

2. Once we got the executable's location, we executed it, which ended up giving us this task's flag

```PowerShell
C:\>cd C:\Users\THM-Attacker\Desktop\Process Injection POC
```
```PowerShell
C:\Users\THM-Attacker\Desktop\Process Injection POC>inject-poc.exe                                                                                                                                   shellcode length: 282
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Interacting%20With%20Windows%20Internals%20pt2.png)


**TASK COMPLETED!**
