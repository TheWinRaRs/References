# The GOT and the PLT

There are many functions in c. Examples being ```fgets, system, fopen, dup, dup2```, etc. etc. All or many of these functions are included in the C standard library, which we call libc.

Libc is essentially a shared object file that stores all sorts of variables and functions a binary will need. Almost every binary imports this library at an address, usually this address is different every execution due to ASLR.


Note: not all binaries need libc. Some don't use libc functions at all, some are static and carry all of the libc functions they need along with them.Note 2: We're talking about libc here, but this applies for all dynamic libraries.


The problem arises: how can the binary know where the function it needs is located if the address at which libc is imported is constantly changing? That's where the GOT and the PLT come in.


GOT stands for Global Offset Table. It is a section existing in the binary that holds the real addresses of functions that are in dynamic libraries. For example, printf@got holds the address of printf.


PLT stands for Process Linkage Table. It holds PLT "stubs" - small pieces of code, of which their purpose is to jump to the correct function, or set things up.


When dealing with dynamic functions, the program will call upon stubs in the PLT, not the functions directly.


At the beginning of the program, the GOT may hold the addresses of the PLT instead of the actual addresses, or it may hold the addresses of resolution or identifier functions.


The first time running a dynamically linked function, the PLT will be jumped to. The PLT will fall into it's "default" nature, resolving the address of the actual function, updating the GOT to hold this address, then jump to the function.


Afterwards, anytime the PLT is called again, it will simply read the GOT to get the address of the function, then jump there.


If full relro is not on, the GOT is not read-only. This can lead to a GOT overwrite attack, in which we overwrite the GOT entry of a function with another address. Say, for example, puts is called on our input after a vulnerability that gave us an arbitrary write. We would be able to overwrite puts in the GOT with the address of system in the PLT perhaps, then input /bin/sh.

When puts@plt would run, it would check puts@got, which would hold the address of system, and then jump there, allowing us to control the program's execution.
