# Computer-Architecture-and-the-Hardware-Software-Interface
uploaded for my fb/irl friend Nicolas Sadirac


To understand a program you must become both the
machine and the program.
Alan Perlis, Epigrams


As software engineers, we study computer architecture to be able to
understand how our programs ultimately run. Our immediate reward
is to be able to write faster, more memory-efficient and more secure
code.
Longer term, the value of understanding computer architecture may
be even greater. Every abstraction between us and the hardware
is leaky, to some degree. This course aims to provide a set of first
principles from which to build sturdier mental models and reason
more effectively.
We’ll start mostly on the hardware side of the hardware/software
interface, developing our understanding of how the machine works
and writing assembly language programs to explore a typical instruction set architecture.


We’ll primarily use MIPS as our
example architecture, and introduce
x86-64 towards the end of the course
as a point of comparison. We chose
MIPS because of its relative simplicity:
it was one of the reduced instruction
set computer (RISC) architectures, and
has an order of magnitude smaller
instruction set than x86-64.

MIPS is no longer a popular
architecture for general purpose
computing, but was used in many
game consoles including the Nintendo
64, PlayStation and PlayStation 2, as
well as the Mars rover. In many ways,
MIPS inspired ARM, which is now the
most popular computer architecture in
the world by virtue of it being used in
most phones.


With a better understanding of program 2 We’ll primarily use MIPS as our
example architecture, and introduce
x86-64 towards the end of the course
as a point of comparison. We chose
MIPS because of its relative simplicity:
it was one of the reduced instruction
set computer (RISC) architectures, and
has an order of magnitude smaller
instruction set than x86-64.
MIPS is no longer a popular
architecture for general purpose
computing, but was used in many
game consoles including the Nintendo
64, PlayStation and PlayStation 2, as
well as the Mars rover. In many ways,
MIPS inspired ARM, which is now the
most popular computer architecture in
the world by virtue of it being used in
most phones.
execution at a low level, we’ll move on to higher level considerations
like C language programming and the compile-assemble-link-load
pipeline, the basic responsibilities of an operating system as well as
one of the most important performance consequences of modern
architecture: CPU cache utilization.

ps: Moore’s Law gave some programmers
an excuse to ignore computer
architecture, by relying on faster
underlying hardware each year. That
era is over, and as John Hennessy
argues in the talk from which the above
graph is sourced, much of the burden
for progress from this point is shifting
to software systems.

# Recommended Resources