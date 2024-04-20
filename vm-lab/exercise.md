#Building a Virtual Machine

In this exercise, you will write a program that simulates the behavior of computer hardware. Our primary objective is to understand the fetch-decode-execute cycle of a physical computer, by simulating one.

In addition, we hope you’ll start to appreciate how virtual machines such as the CPython interpreter, Java Virtual Machine or V8 JavaScript engine work.

The CPython interpreter is very readable, as the maintainers prefer to keep the implementation simple and accessible to newcomers. The core virtual machine is implemented in ceval.c, and if you look closely you should be able to make out the main loop as well as the fetch, decode and execute components.

To be able to simulate a computer architecture in the time available, we’ve created a fictional and highly simplistic instruction set architecture. What follows is a description of the computer architecture that your virtual machine should simulate.

#The Computer

The device we’re simulating is much simpler than a modern CPU; it has:

-20 bytes of memory, simulated by an array with length 20
-3 registers: 2 general purpose register and 1 for the “program counter”
-5 instructions: load word, store word, add, subtract and halt

#Memory

Here is one way to picture our memory:

00 01 02 03 04 05 06 07 08 09 10 11 12 13 14 15 16 17 18 19
__ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __
All of the instructions and all of the data for a particular “program” must fit within these 20 “bytes” worth of data.

It’s more work than it’s worth to have our VM enforce that each address only contains a single byte of data—instead we’ll write test cases that satisfy that requirement by hand.

In addition to a fixed size, our computer follows a convention for organizing memory. Memory is divided into 3 sections: instructions, input, and output. The instructions occupy the first 14 bytes, followed by 2 bytes for output and 4 bytes for two separate 2 byte inputs:

00 01 02 03 04 05 06 07 08 09 10 11 12 13 14 15 16 17 18 19
__ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __
INSTRUCTIONS ---------------------------^ OUT-^ IN-1^ IN-2^

#Aside: Hexadecimal Notation
Decimal is base 10, binary is base 2, hexadecimal is base 16.
If you’re entirely unfamiliar with hexadecimal notation, see the suggested resources for the “Binary Representations” class.


Counting from 0 to 16 in hex looks like this:

0 1 2 3 4 5 6 7 8 9 a b c d e f 10

f is 15, and 10 is 16 (one 16 and zero ones). It is common to identify hexadecimal values by prefixing them with a 0x (and binary with a 0b); for example, 0x23 is the number 35 (two 16s and three 1s), and we know it is a hex value because of the leading 0x. Hex is frequently used as a kind of “binary shorthand” because it’s hard to read long strings of 1s and 0s, and because each hex digit can be mapped uniquely to a sequence of four binary digits:

0: 0000
1: 0001
2: 0010
3: 0011
4: 0100
5: 0101
6: 0110
7: 0111
8: 1000
9: 1001
a: 1010
b: 1011
c: 1100
d: 1101
e: 1110
f: 1111
Therefore, we can write the binary value 0b11111101 as 0xfd. A single byte is always represented by 2 hex digits. The two byte value 0x0103 becomes 0b0000 0001 0000 0011 (spaces added for clarity in reading). From here on out, we’ll use hexadecimal values in place of integer values. This will help us write test cases where we are sure that our data can fit in a single byte, because we will use 2 or fewer hex digits!

Lets rewrite our memory indexes in hexadecimal:

00 01 02 03 04 05 06 07 08 09 0a 0b 0c 0d 0e 0f 10 11 12 13
__ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __
INSTRUCTIONS ---------------------------^ OUT-^ IN-1^ IN-2^


Instruction Format
The first 13 bytes are reserved for instructions for an executing program. Our architecture has a very limited instruction set; there are 5 instructions, and we can map each to a byte value:

With just 5 instruction, we could have encoded them using just 3 bits, but we decided to use an entire byte to simplify implementation.

load_word   0x01
store_word  0x02
add         0x03
sub         0x04
halt        0xff
Furthermore, each of the instructions has “parameters” that need to be supplied. The values of these parameters are often called “operands”, “op args” or just “arguments”.

load_word  reg (addr)  # Load value at given address into register
store_word reg (addr)  # Store the value in register at the given address
add reg1 reg2          # Set reg1 = reg1 + reg2
sub reg1 reg2          # Set reg1 = reg1 - reg2
halt
For simplicity, we’ve decided to represent each “parameter” with a single byte. This means that all the instructions except halt will take three bytes—and halt, just one—to encode. The reg parameters may only take one of two values, because our architecture only has 2 general purpose registers. We can choose any single byte value to identify the registers, we’ve chosen the values 0x01 and 0x02 (reserving value 0x00 for the program counter).

Now we have enough information to write a program. The “assembly”:

load_word r1 (0x10)   # Load input 1 into register 1
load_word r2 (0x12)   # Load input 2 into register 2
add r1 r2             # Add the two registers, store the result in register 1
store_word r1 (0x0E)  # Store the value in register 1 to the output device
halt

When translated line by line to our “machine code”:
Don’t forget that machine code values are ultimately in binary; we only use a hexadecimal representation here to aid readability.

0x010110
0x010212
0x030102
0x02010e
0xff
And into our visualization of memory:
Note that there is one byte of “unused” memory at 0x0d.

00 01 02 03 04 05 06 07 08 09 0a 0b 0c 0d 0e 0f 10 11 12 13
01 01 10 01 02 12 03 01 02 02 01 0e ff __ __ __ __ __ __ __
INSTRUCTIONS ---------------------------^ OUT-^ IN-1^ IN-2^
This program should add the two input values, whatever they are, and store the result into the output location.

Input and Output Formats
The two bytes of output always represent one 2-byte number. The two inputs each also always represent 2-byte numbers. Our computer is a “little endian” system, meaning the “least significant byte” of our inputs and outputs occupy the smaller array index location. Consider the following input:

byte 0x12: 10100001
byte 0x13: 00010100

# or as hex
byte 0x12: 0xa1
byte 0x13: 0x14
Byte 0x12 corresponds to the binary digits for 20 through 27. It has the value 27 + 25 + 20 = 161. Byte 0x13 corresponds to the binary digits for 28 through 215 and has the value 210 + 212 = 5120. Therefore, these two bytes represent the number 5120 + 161 = 5281.

Another useful way to think about this is to label the device like this:

byte 0x12: 10100001 x 1
byte 0x13: 00010100 x 256
Thinking about it this way, we have the binary number 161 on top, and the binary number 20 on bottom. This yields: 1 * 161 + 256 * 20. We’re relieved to discover that 256 * 20 = 5120 so our interpretations are indeed equivalent.

In the above scenario, the user encoded the decimal number 5120 by dividing it by 256 and entering 14 in binary on the bottom row, then they entered the remainder 161 on the top row. Unfortunately, the device interface is a little confusing, but it cannot be changed.

Note that this construction means that the “little end” of the number (161 x 1) comes first in memory (byte 0x12) and the “big end” (20 x 256) comes after (byte 0x13). Intel computers encode multi-byte numbers in this order, so we are in good company.

Now we have enough information to encode a complete program including input values. Here is a program to compute the result of 5281 + 12:

00 01 02 03 04 05 06 07 08 09 0a 0b 0c 0d 0e 0f 10 11 12 13
01 01 10 01 02 12 03 01 02 02 01 0e ff __ __ __ a1 14 0c 00
INSTRUCTIONS ---------------------------^ OUT-^ IN-1^ IN-2^
Or as an array in a number of high level languages:

program = [
  0x01, 0x01, 0x10,
  0x01, 0x02, 0x12,
  0x03, 0x01, 0x02,
  0x02, 0x01, 0x0e,
  0xff,
  0x00,
  0x00, 0x00,
  0xa1, 0x14,
  0x0c, 0x00
]
Two final notes about numbers: firstly, you should assume that the registers can hold 16 bit (2 byte) numbers directly. This means that you need to wrestle with endianness only when you load/store values from/to memory. When running the add or subtract command you may use your language’s + and - operators directly to perform the operation. Secondly, start by supporting only positive numbers. Later, if you have time, stretch yourself by making your input and output representations 16 bit two’s complement numbers.

#The Exercise

Write a “virtual computer” function that takes as input a reference to main memory (an array of 20 bytes), executes the stored program by fetching and decoding each instruction until it reaches halt, then returns. This function shouldn’t return anything, but should have the side-effect of mutating “main memory”.

Your VM should follow the fetch-decode-execute format, which you can model in a loop. The program counter should always contain the address of the “next instruction” (and so should start at 0). Fetch the current instruction by getting all of the relevant information from memory, decode the instruction to find out what operation should be performed using which registers/memory-addresses, then execute the instruction and update the program counter appropriately.

Your virtual computer should have one piece of internal state, an array of three registers: two general purpose registers and a program counter. Main memory is considered external state because it is passed in as an argument. Write some tests for your virtual computer function using different stored programs and different input values. Your test inputs should just be array literals representing main memory. After calling your VM function on the memory, test that its output section contains the correct computed value.

While most virtual machines are implemented in a low level language, we suggest that you write yours in whichever language is most familiar to you, in order to focus more on the mental model than on the implementation details.

#Stretch Goals
-Add a “branch if equal” instruction, which changes the program counter to a given address if the given register values are equal. Depending on your approach, this may require handling a 4-byte instruction, as well as expanding the overall memory size.
-Add an “add immediate” instruction, which adds a constant number (encoded within the instruction itself) to a given register.
-Support negative numbers using 16-bit twos complement.
-Consider porting your implementation to a lower level language, or using lower-level primitives in your existing language, to more easilly encode your programs as true arrays (contiguous in memory) as as bytes in binary-encoded files.
