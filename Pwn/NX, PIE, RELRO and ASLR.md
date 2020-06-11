# NX, PIE, RELRO and ASLR

Whilst stack canaries mainly exist to defend against stack overflow, NX, PIE, RELRO and ASLR fight against many attacks. 


NX - No-execute. It means that pages like the stack and the heap are made unexecutable, and the pages that must be executable are made unwritable.


PIE - Position Independent Executable. (Please note this isn't just a protection, sometimes it is required for shared libraries and such). It means that the binary base of the application is different each run, so code within the binary like gadgets, or the PLT and the GOT and such are not in a constant location.


RELRO - Relocation Read-only. Partial RELRO means that the GOT is relocated to a place which we will not be able to reach via overflow. Full RELRO means the GOT is relocated, and read only, preventing GOT overwrite attacks.


ASLR - Unlike other protections, this one lives in the kernel, not a binary. There's no compiler option to enable or disable this. ASLR is much like PIE, but for imported things and other pages of the memory like libraries and the stack, randomising their base offset.

<hr>

# NX

One of the simplest buffer overflow attacks is that on the stack, overwriting the return address to be the address of shellcode and using shellcode (and sometimes NOP sleds) to spawn a shell. NX defends against this, making the stack, heap and other places in memory non-executable. Generally, it's very hard to bypass this, however it is possible using a function called mprotect. Mprotect allows you to change the protections of certain areas in memory. Under specific circumstances, like if we had the ability to control pages or knew the address of the stack, would allow an attacker to use mprotect to make the stack executable again, and execute a classic buffer overflow attack.

<hr>

# PIE

By itself, PIE isn't too large of a problem. When combined with other things, however, it gets very annoying. For example, one solution to the NX problem is to use something called ROP - return oriented programming. Or it's cousin JOP - jump oriented programming. These involve creating chains of "gadgets", addresses of pieces of code placed inside of the binary that can be used to carry out specific tasks, like control registers or execute system calls. With PIE, this becomes much harder, as the addresses of these gadgets will not be static.


Another technique that PIE mitigates would be ret2libc. This is where an attacker uses functions inside of the standard c library to create a shell. With ASLR(which will be explained shortly), this can be quite difficult. However, given buffer overflow, things inside the binary like the PLT and the GOT can be used to leak the libc base, giving an attacker the power to perform ret2libc. PIE means that the PLT and GOT won't be static.


So, how can we bypass this? One way is a format string attack. If the binary is vulnerable to a format string attack, we can leak addresses in the binary. If we leak an address in the binary, we can subtract a specific offset from it to leak the binary base. Say there's a global variable, var1. This variable var1 is stored inside of the binary at the offset 0x567. On the stack is the address of var1. If we leak the address of var1, we can subtract 0x567 to get the binary base.


Other possibilities include the ability to brute force values like the RIP/EIP byte by byte due to forking and variable length inputs. More on this later.


Generally, however, we leak the binary base by leaking the address of something in the binary, then subtracting the needed offset.

<hr>

# Relro

In some rare cases, buffer overflow to a global variable can allow an attacker to overflow and overwrite the GOT. Partial RELRO relocates the GOT to a place that cannot be reached via overflow, and makes certain parts of it read-only. However, this still opens up the avenue for a GOT overwrite. Full RELRO completely mitigates GOT attacks by making the entire GOT read-only after resolution. This is almost unbypassable, however still vulnerable to the mprotect attack I mentioned earlier in the context of NX.


<hr>


# ASLR

SLR randomises the position of many places in memory, like libraries and the stack. This helps to prevent against attacks like ret2libc, which rely on knowing the exact location of things inside of the libc. The libc base or stack address can be leaked via a format string attack, similar to PIE. We can also use the PLT and the GOT to leak the addresses of certain functions in the libc, like so:


Remember, after a dynamic function was called once in the PLT, it is resolved so that it's GOT entry holds the actual address. Any form of arbitrary read will allow us to read the GOT and get the address of one of the libc functions. With the address of a libc function, all that is required is to subtract it's appropriate offset in the libc to get the libc base, defeating ASLR.


An example is using the PLT.  We can call upon the plt entry of a function to emulate the actual function. For example, say puts has been called before. We can call upon puts@plt, using puts@got as an argument. The program would then print the value of puts@got(the address of puts in the libc!) as a string, giving us the ability to calculate the libc base and defeat ASLR.


Generally, ASLR is defeated if you can accurately get the address of something inside of libc, like a variable or a function.


ASLR is a kernel protection. Whether or not ASLR is on is determined by the system and not the binary. You can control it with ```echo <0,1,2> | sudo tee /proc/sys/kernel/randomize_va_space.``` 0 means ASLR is off, 1 means partial, 2 means full.
