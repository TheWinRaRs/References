# pwntools

pwntools is an incredibly powerful python library which greatly simplifies your life when it comes to binary exploitation. It provides a much simpler interface with the program, includes a host of in-built functions for common operations and allows smooth transitioning between local and remote exploits. 


<hr>

# Setting it all up

### Importing pwntools

Unlike most python packages, with pwntools we generally import everything and don't bother about efficiency.
```python
from pwn import *
```


### The ELF class

The ELF class is the class you will be using most often for your Linux exploits; it allows you to instantiate an object that directly handles the executable itself.


```e = ELF("./executable")```


### Processes

When we want to interface with a running executable we use a process. Processes can either be made by themselves or be linked to a specific ELF object (if you want to further interact with the process' object outside of the process).
```python
process = process("/bin/sh")
process = elf.process()			# elf is an ELF object
```


You can also create a remote process to interface with executables on a network:
```python
p = remote("host.hosty.net", 31337)	# takes in a host and a port
```

<hr>


# Communicating with the Executeable

*Note: In these examples, __p__ is a process object.*

### p.sendline(text)

Sends the text to the executable. Generally the main way you respond to input prompts and deliver payloads.

### p.recvline()
Quite literally what it sounds like. Receives data until the next newline characters (\n). Returns a string (the data read) in case you want to print it.

*Note: 	An incorrect number of these can cause your program to seem to fail when it doesn't but is rather just waiting for input. This should be the first thing you check if everything else seems to make sense.*



### p.clean()

Generally receives all data sent to it. Make sure there isn't an error here before you move on to other sections of your exploit.


### p.recvuntil(string)

Receives all data until it encounters the string argument, after which it stops receiving.


<hr>

# Packing Integers

pwntools makes packing integers easy. While you would normally use the ```struct``` module and the ```struct.pack()``` function, these functions have a host of options that we simply do not need or are hard to remember in which case we need which arguments. This is where pwntools comes in.



### p32(integer)

`p32()` takes in an integer and returns the representation of that integer as a series of bytes, much like `struct.pack()`.
```p32(0xdeadc0de) == b"\xde\xc0\xad\xde"```

### u32(bytes)

`u32()` does quite the opposite - takes in a series of bytes and returns the integer representation of them, much like `struct.unpack()`.
```u32(b"\xde\xc0\xad\xde") == 0xdeadc0de```

#### p64() and u64()

These have the same function as `p32()` and `u32()` but are for 64-bit binaries.

<hr>

# Logging

### log.info(text)

Prints text to the screen in a special format to note that it's a log. Useful for distinguishing between text produced by the program and your own debugging. There are other functions for various levels of warning.


### Context

Context allows you to set some defaults to prevent you from having to repeat yourself.
For example,	```context.endian = 'little'``` makes all future `p32()` and `u32()` functions (as well as other similar ones) use little endian, meaning you don't have to specify every time you use the function.
