# Autorecon
On standalone devices, run Autorecon
```shell
autorecon 192.168.177.151 192.168.177.152 192.168.177.153
```

# Nmap
Run initial Nmap scan to determine what ports are open
Full scan
```shell
nmap -Pn 192.168.177.161 -p-
```

Fast (may encounter missing ports)
```shell
nmap --min-rate 4500 --max-rtt-timeout 1500ms 192.168.211.52 -p-
```

After obtaining open port numbers, run enhanced scan on the ports
```shell
nmap -Pn 192.168.177.161 -p 80,135,139,445 -T4 -A -oN enum/nmap.txt
```

# Directory Enumeration
If any HTTP ports available, eg 80, 8000, 8080
## gobuster
```shell
gobuster -u http://<ip-addr>:<port> -w /usr/share/wordlists/dirb/common.txt
gobuster -u http://<ip-addr>:<port> -w /usr/share/wordlists/dirb/big.txt
gobuster -u http://<ip-addr>:<port> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

## feroxbuster
```shell
feroxbuster --url http://<ip-addr>:<port>
```

# Website Fingerprinting
## Whatweb
```shell
whatweb http://<ip-addr>:<port>
```

## View Page Source
On a webpage, right click to view the source code of the page

![[Pasted image 20240819165501.png]]

Look for relevant interesting info, eg software used, version, etc
# SMB enumeration
Upon seeing port 445 or any SMB port, try connecting using `smbclient`
```shell
# list shares
smbclient -L <ip-addr>

# connect to share
smbclient //<ip-addr>/<share-name>
```

# LDAP enumeration
Upon seeing classic Domain Controller ports, eg 88, 389, 3268 etc, try using `ldapsearch`
Reference: https://book.hacktricks.xyz/network-services-pentesting/pentesting-ldap
![[Pasted image 20240818152333.png]]
```shell
ldapsearch -x -H ldap://<DC-IP> -D '' -w '' -b "DC=<DC-Subdomain>,DC=<TLD>
```
There might be passwords or other interesting info 
# Random things to try
Try inputting the hostname everywhere, eg in website paths, as credentials etc