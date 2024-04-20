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
