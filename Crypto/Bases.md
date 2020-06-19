# Bases
A base is a number system that assigns characters to values.\
The most common numbering systems found in computer science are:
* Base-2 (more commonly known as **binary**)
    * Uses `0s and 1s` to represent data
* Base-10 (also known as **denary**)
    * Uses the numbers `0-9` to represent data.
* Base-16 (also known as **hexadecimal**)
    * Uses `0-9` and the letters `A-F` to represent data.
    
There's also:
* Base-8 (also known as **octal**)
    * Uses the numbers `0-7`
* Base32 
    * Uses `A-Z`, `2-7` and `=`
* Base 64
    * Uses `A-Z`, `a-z`, `0-9`, `+ - =`
* Base85
    * Uses `ASCII values 33-117`
    
In normal, everyday use, we commonly use base 10 to represent numbers,
as we don't often deal with large numbers on a day-to-day basis.

We can also represent values in different number systems, which can end up making some numbers look very odd to the untrained eye.\
For example: 
* `255` in *denary*
* `FF` in *hexadecimal*
* `1111 1111` in *binary*. 

To show you all of these, I will now encode the message: {Hello! We are The WINRaRs} in
Base 2, Base 8, Base 16, Base 32, Base 64, and Base 85.
* Base 2
```
01111011 01001000 01100101 01101100 01101100 01101111 00100001 00100000 01010111 01100101 00100000 01100001 01110010 01100101 00100000 01010100 01101000 01100101 00100000 01010111 01001001 01001110 01010010 01100001 01010010 01110011 01111101
```
* Base 8
```
173 110 145 154 154 157 41 40 127 145 40 141 162 145 40 124 150 145 40 127 111 116 122 141 122 163 175
```
* Base 16
```
7b 48 65 6c 6c 6f 21 20 57 65 20 61 72 65 20 54 68 65 20 57 49 4e 52 61 52 73 7d
```
* Base 32
```
PNEGK3DMN4QSAV3FEBQXEZJAKRUGKICXJFHFEYKSON6Q====
```
* Base 64
```
e0hlbGxvISBXZSBhcmUgVGhlIFdJTlJhUnN9
```
* Base 85
```
HUq^aCi:I>=(NL_Eb-@mBOr;f8PW/l;KI6
```
Notice how as we go along, the encoded strings get shorter? That's because we have more available slots to assign characters to.

You can even try and decrypt these messages here:\
https://gchq.github.io/CyberChef/#input=e0hlbGxvISBXZSBhcmUgVGhlIFdJTlJhUnN9 
