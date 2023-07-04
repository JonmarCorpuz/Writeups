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
# Your PowerShell script content goes here
Write-Host 'Hello, World!'
"@
Set-Content -Path $scriptPath -Value $scriptContent
`

# Hijacking File Associations
