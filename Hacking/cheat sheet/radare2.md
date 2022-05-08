https://scoding.de/uploadsd/r2_cs.pdf
![[Pasted image 20220204194811.png]]

Radare2 is a framework for reverse engineering binaries.

typical analysis:

```bash
aaa #analyze all
e asm.syntax=att #change the syntax ino at&t
afl #list function
pdf@main #print disassembly function @main
db 0x55ae52836612 # set break at the address
dc #start the program
dr #look at registries
ds #move instruction forward(1step)
```
