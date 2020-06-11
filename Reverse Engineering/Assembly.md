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

<hr>

# Syntax

 Assembly comes in two different flavours, or rather, two different syntax's. One syntax is Intel, the other being AT&T. You do have to note which syntax you're analysing, as it can become quite frustrating moving from Intel to AT&T and vice-versa.
 
 
Intel:


This syntax, in CTF's, is the most likely syntax that you'll see. So it's good to get to know how Intel syntax is displayed.


```
<operand> <destination>, <source>
```
This is Intel syntax. It's asked to require the operand first (eg. mov), then asks for the destination of where the data will be going, then ask for the source - where the data will be provided from.


`mov eax, 0`


The operand here is mov, the destination here is eax, and the source is 0. This instruction is saying "move the constant 0 into the eax register"


AT&T:


This syntax is less seen in CTF's, but it still appears sometimes
```<operand> <source>, <destination>```
This is AT&T syntax. Notice the difference?

We have the destination and the source mixed around so that it's the other way! Whyyyyyyy... who knows. The intel command that we just showed, in AT&T syntax, is
`movl $0, %eax`


So here we've got a tiny bit more to dissect. AT&t uses movl as the operand (as opposed to mov), then has the source as $0 (this being the constant 0), then the destination as %eax. The $ sign means that a value is a constant, and the % means that the value is a register.


Fortunately, a program has to stick to one syntax throughout execution, so lucky us for not having to convert each time!
Some actual coding of assembly:


So, now that we've got that out the way, let's take a look at some actual code for assembly. All of this will be explained in more detail here, but just as a general overview, here are the most important commands needed for CTF's (I'm going to use Intel syntax for easier reading):

```
; - comment
mov  <destination>, <source> ; move data from the source to the destination
cmp <register>, <register / value> ; compares the register to the register / value, and depending on the outcome, will normally come a jump instruction to take a different path.
jmp <label> ; loads the value of the label (a function or memory address) into the rip register, which the program will then execute those commands
call <label> ; jump to the function or memory address with a return function - basically calls a subroutine.
push <register / value> ; push the register or value on to the stack to reserve it
pop <register> ; pops the top value off of the stack, and stores it in the register
pop [register] ; (the square brackets are necessary) - pops the value off of the stack, and saves it into a position in memory, such as ebp-0x8
test <register>, <register / value> - performs a bitwise AND operation on the register by the value
<math function> <register>, <register / value> - as it says on the tin - provides a math operation (add, subtract eg.) to the register by the register/value
```

So there you have some actual code of assembly under your belt! Welcome to the reversing world!
