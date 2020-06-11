# Assembly

There's a lot to learn here - it's a whole new language!  I've devoted a whole page to learning assembly here, but if you want a quick overview for the information that you need in CTF's, here they are:

Registers:

Registers are pieces of hardware, located near to the CPU. They hold instructions (or data) that are used in the execution of the program. The data is stored here, rather than RAM, as it's quicker for the CPU to access the instructions inside registers.
These instructions inside a program will store data in the different registers. They come in different sizes:
```
rax: ================================================================ (64 bit)
eax:                                 ================================ (32 bit)
ax:                                                  ================ (16 bit)
al:                                                          ======== (8 bits (lower))
ah:                                                  ========         (8 bits (upper))
```
We also have different registers for different purposes. These are:
```
rax - returns values
rbx - general purpose
rcx - general purpose
rdx - general purpose
```
We also have some other registers that are very important to note:
```
rsp - stack pointer
rbp - base pointer
rsi - source index
rdi - destination index
r8 - r15 - other registers
```
We'll dig into what these registers do in the page about assembly, found here (also at the top of the assembly partion on the page)
