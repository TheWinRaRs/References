# CTF Tips And Tricks

There are some things that I always use when doing anything before actually running the binary. This is called static analysis - analysing the binary or thing in question when it's not changing, or not running - being the same, or static.


The very first thing that I like to do is see what "low hanging fruit" we have available, or what information we have available without much effort.


Strings:
I like to first check what strings are in the binary. to do that, we run the 
```
> strings
or
> rabin2 -z <filename>
```

Just because we love radare2 and its family, we might as well get used to some of it's family members and their syntax. In the strings, we might find a flag, some username or passwords, anything that may help us investigate the binary further.


File type:


Next, we want to check what file type we're dealing with. We have two main file types that we find in CTF's:
```
ELF - Executable and Linkable format - a Linux executable
PE - Portable Executable - a Windows executable
```
We can find out what type of executable it is by running:
```
> file <filename>
or
> rabin2 -I <filename>
```
rabin2 will return a lot more information about the binary than just it's file type (if it's little or big endian, has a canary, full or partial pic etc.). It's mainly used when doing pwn from the amount of information that we get from it, but it's still a nice tool to have.
Trace program execution:
Here, we're seeing what is happening with the program - we're now moving to dynamic analysis, as we're executing the binary and seeing what steps it's taking.
To see a trace of the program, we can use:
```
> ltrace <filename>
> strace <filename>
```
The difference between these two tools is that strace intercepts system calls made by the glibc and other libraries  directly into the kernel. Ltrace intercepts library and system calls made by the application to C libraries such as glibc. They do display similar outputs, but it's good to know what each tool does before diving straight into it.
If we don't get anything from these, then we start to play around with the binary. We execute it, and see what prompt it asks us for. We can input any of these to see what output we get:
Characters: "A"
Strings: "Hello World"
Integers: 1
Floating point numbers: 1.5
Negative numbers: -1
Boolean: True or False
Format strings: %x
Buffer overflow: (Provide more characters than the buffer can accept)
Hex: 0xdeadc0de
And some more may come to me later :)
If none of those work, then I think that it's time to start using our tools.
