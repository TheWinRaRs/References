# Format String Attack

A format string vulnerability is caused when printf, sprintf, or other functions that use formatting are called directly on user input.

For example,
```char input[20];fgets(input, 20, stdin);printf(input); // This is horrible coding practice!
printf("%s", input) // This is good coding practice!
```
What is string formatting?

String formatting allows you to put integer values, character values, pointer values, string values and much more within a string to be printed, scanned to a file, scanned to a variable, and much more.


%d - decimal number

%p - pointer

%s - string

%n - take the value as an address. Write the number of characters previously printed to this address.


So, why does this matter? How is it vulnerable?To understand this, we must think about how the CPU knows where to get arguments. On a 32-bit system, arguments are commonly placed on the stack. First argument on the top, second argument is the next value down, etc.

On 64-bit systems, the arguments are stored on RDI, RSI, RDX, RCX, R8, and R9. If there's any more arguments, they are stored on the stack. Now think. How does the CPU know a value on the stack was supposed to be an argument and isn't just a random value. Spoiler: it doesn't.Thus, if our input is printed directly, e.g printf(input); We get a vulnerability.

As it's first argument, printf expects a string format. This means we can use format specifiers to read values off of the stack.To explain further.Imagine we type %x into our input on a 32-bit binary. The binary then calls printf(input);. Given the %x, it will go take the first argument off of the stack and print this back. In reality, it's just the first value on the stack.

Thus, we get the ability to read values off the stack. This can be incredibly useful in order to leak things like the binary base, libc base, or canary in order to bypass PIE, ASLR and canaries respectively.As well as a read vulnerability, we get an arbitrary write with format strings. Remember %n? It takes the argument as an address, and then writes the number of characters previously written to that address.

In order to make use of this, we must think of some special things we can do to format specifiers. %<number><specifier> pads the output of the specifier to be a specific number of bytes, whilst %<number>$<specifier> goes to the numberth argument(1st argument if number is 1, 2nd argument if number is 2 etc) and uses that specifier on it.So, we can use something like %<number>c to make sure printf prints the number of characters we want before %n.

The problem is, we need the address to be stored somewhere on the stack for this to work. If your input's on the heap, you aren't in luck. But if we've got something like this:char input[20];fgets(input,20,stdin);printf(input);Our input will be on the stack! All we have to do is find out how far our input is along the stack, and we can use it.Remember: we can use %<number>$<specifier> to jump to a certain value on the stack.With this same logic, we can arbitrarily read values from memory using %s, as %s will read the value on the stack as a pointer to the string and print said string.
