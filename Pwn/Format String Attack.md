# Format String Attack

A format string vulnerability is caused when printf, sprintf, or other functions that use formatting are called directly on user input.

For example,
```c
char input[20];
fgets(input, 20, stdin);
printf(input); // This is horrible coding practice!
printf("%s", input) // This is good coding practice!
```
What is string formatting?

String formatting allows you to put integer values, character values, pointer values, string values and much more within a string to be printed, scanned to a file, scanned to a variable, and much more.

| Specifier  | Purpose |
| ------------- | ------------- |
| %d  | Decimal number  |
| %p  | Hexadecimal pointer. Reads 4 bytes on 32-bit and 8 bytes on 64-bit |
| %x  | Hexadecimal integer. (Reads 4 bytes on most systems) |
| %s  | Read data as a pointer to a string |
| %n  | Take the value as an address. Write the number of characters previously printed to this address. |


So, why does this matter? How is it vulnerable? To understand this, we must think about how the CPU knows where to get arguments. On a 32-bit system, arguments are commonly placed on the stack. First argument on the top, second argument is the next value down, etc.

On 64-bit systems, the arguments are stored on RDI, RSI, RDX, RCX, R8, and R9. If there's any more arguments, they are stored on the stack. Now think. How does the CPU know a value on the stack was supposed to be an argument and isn't just a random value? Spoiler: it doesn't. Thus, if our input is printed directly, e.g `printf(input)`, We get a vulnerability.

As it's first argument, printf expects a string format. This means we can use format specifiers to read values off of the stack. To explain further: Imagine we type %x into our input on a 32-bit binary. The binary then calls printf(input);. Given the %x, it will go take the first argument off of the stack and print this back. In reality, it's just the first value on the stack.

Thus, we get the ability to read values off the stack. This can be incredibly useful in order to leak things like the binary base, libc base, or canary in order to bypass PIE, ASLR and canaries respectively. We can leak instruction pointers, libc symbols, and much more.As well as a read vulnerability, we get an arbitrary write with format strings. Remember %n? It takes the argument as an address, and then writes the number of characters previously written to that address.

In order to make use of this, we must think of some special things we can do to format specifiers. %\<number>\<specifier> pads the output of the specifier to be a specific number of bytes, whilst %\<number>$\<specifier> goes to the numberth argument(1st argument if number is 1, 2nd argument if number is 2 etc) and uses that specifier on it.So, we can use something like %\<number>c to make sure printf prints the number of characters we want before %n.

The problem is, we need the address to be stored somewhere on the stack for this to work. If your input's on the heap, you aren't in luck. But if we've got something like this:
```c
char input[20];
fgets(input,20,stdin);
printf(input);
```
Our input will be on the stack! All we have to do is find out how far our input is along the stack, and we can use it. Remember: we can use %\<number>$\<specifier> to jump to a certain value on the stack. With this same logic, we can arbitrarily read values from memory using %s, as %s will read the value on the stack as a pointer to the string and print said string.

Creating format string payloads can be tedious manually, especially if they need to be dynamic, or overwrite multiple values. With pwntools, we can use a few special things. First of all, let's look at the `FmtStr` object type.

With the FmtStr object type, we can dynamically calculate how far our input is along the stack. Take this example:

```python
from pwn import *
e = ELF("./sample_elf")
def write_fmt(data):
  p = e.process()
  p.recvline()
  p.sendline(data)
  output = p.recv()
  p.close()
  return output
obj = FmtStr(execute_fmt = write_fmt)
...
```
The above code generates a pwntools FmtStr object. The pwntools FmtStr object takes an argument function that allows it to execute format string and get the output. With this, it automatically calculates the offset of the input on the stack via leaks and cyclic patterns.
Now, let's look at generating payloads.
```python
from pwn import *
e = ELF("./sample_elf")
def write_fmt(data):
  p = e.process()
  p.recvline()
  p.sendline(data)
  output = p.recv()
  p.close()
  return output
obj = FmtStr(execute_fmt = write_fmt)
writes = {e.got['puts']: e.plt['system']} # Here we supply a dictionary of form {address: value to write}. In this case, we're executing a GOT overwrite, overwriting puts@got with system@plt.
payload = fmtstr.fmtstr_payload(obj.offset,writes)
```
The above code gets the `offset` attribute of the FmtStr object, containing the offset of the input on the stack. It then uses the pwntools function fmtstr_payload to generate a payload given a dictionary of writes and an input offset. There's a lot more to format string attacks using pwntools, you can read about it here https://docs.pwntools.com/en/stable/fmtstr.html
