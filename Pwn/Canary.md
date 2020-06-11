# Canary

A canary is a random value placed on the stack to detect buffer overflow attacks. Before a function returns, the value is compared to the original, random value - if they are different then buffer overflow has taken place and the program crashes.There are two main ways to bypass canaries - using a format string vulnerability (if one exists) or brute-forcing it (which is not feasible on 64-bit machines).

<hr>

# Canary and Format String

Format string vulnerabilities allow us to leak arbitrary values off the stack. Since the canary is (by definition) on the stack, this allows us to read the value.
The key to finding which item on the stack is the canary, if you cannot be bothered to look at the disassembly, is to iterate through every offset several times. Canary values always end in 00 and are different every time, so they are fairly distinguishable. We advise turning ASLR off when testing for canaries to eliminate false positives (as ASLR randomises libc addresses every time the program is run).
The way stack canaries work is as follows:At the beginning of the function, a random value is moved into a local variable:
```C
mov rax, qword fs:[0x28]
mov qword [var_8h], rax
```
At the end of the function the value in this local variable (var_8h) is checked again with the random value qword fs:[0x28] and if they are different crashes the program:
```C
mov rax, qword [var_8h]
xor rax, qword fs:[0x28]
je [leave]
call sym.imp.__stack_chk_fail ; void __stack_chk_fail(void)
leave
```
var_8h is once again moved into rax, which then compares with the original random value. If they are equal, the program jumps to the leave instruction. If not, it calls __stack_chk_fail.
So, knowing the value through format string, we want to find the offset between the beginning of our input buffer and the stack value. Luckily we can use an RE tool to do this as the rax register is involved, and our preferred tool of choice is radare2.
Firstly, generate a De Bruijn pattern using ragg2:


```ragg2 -P 100 -rAAABAACAADAAEAAFAAGAAHAAIAAJAAKAALAAMAANAAOAAPAAQAARAASAATAAUAAVAAWAAXAAYAAZAAaAAbAAcAAdAAeAAfAAgAAh```


Then run the program in debug mode, input the sequence and set a breakpoint right after rax reads the variable value:


```r2 -d vuln> aaa> db 0x00400945> dc```


We then use wopO to work out the offset using the De Bruijn pattern.> wopO `dr eip`And this gives us the offset between the buffer and the canary.
