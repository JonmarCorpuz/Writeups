# Assigning Group Memberships
1. Start this room's machine
2. `cd \` from the compromised Windows machine to ensure that we'll be executing the following commands starting from the root directory
3. `net localgroup “Backup Operators” thmuser1 /add` from the compromised Windows machine to add our target user (thmuser1) to the compromised system's **Backup Operators** group, which won't give our target user administrative privileges but it will allow them to read and write to files within its system regardless of the system's DACL configurations and any other set permissions that may prevent users from accessing those files
4. `net localgroup “Remote Management Users” thmuser1 /add` from the compromised Windows machine to add our target user (thmuser1) to the compromised system's **Remote Management Users** group in order to have our target user be able to connect back to the compromised machine using WinRM
5. `reg query HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\System /v /EnableLUA` from the compromised Windows machine to check if Microsoft's UAC is active on our compromised machine or not, which it was
6. `reg add HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\System /t REG_DWORD /v LocalAccountTokenFilterPolicy /d 1` from the compromised Windows machine to

# Assigning Specific Privileges Using Security Descriptors

# RID Hijacking
