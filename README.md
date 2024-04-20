# Computer-Architecture-and-the-Hardware-Software-Interface
uploaded for my fb/irl friend Nicolas Sadirac


"To understand a program you must become both the machine and the program." - Alan Perlis, Epigrams


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
Please endeavor to complete all of the prework for each class.
Doing so will help us cover more content overall, and to spend
more time on the kind of interactive activities that we can only do in person.
I’ve done my best to keep the prework short, interesting
and relevant, so please let me know if there’s anything that seems off
topic or unreasonably long or uninteresting.

“P&H” below refers Patterson and Hennessy’s Computer Organization and Design—a classic text, very commonly used in undergraduate computer architecture courses. Chapter references are for the
5th edition, but older editions should be close in content.

Computer Organization and Design
is one of the most successful, lasting
textbooks in all of computer science.
The authors are living legends, having
pioneered RISC and created MIPS.
David Patterson now works as a
researcher at Google after 40 years
as a Professor at UC Berkeley, and
John Hennessy was most recently
President of Stanford before becoming
Chairman of Alphabet.

For those who prefer video-based courseware, my recommended
supplement to my own course is the Spring 2015 session of Berkeley’s 61C course “Great ideas in computer architecture” available on
the Internet Archive.
For students who have some extra time and would like to do some
more project-based preparatory work, I recommend the first half
of The Elements of Computing Systems (a.k.a. Nand2Tetris) which is
available for free online.
For those with no exposure to C, I strongly recommend working
through some of The C Programming Language (a.k.a “K&R C”)
before the course commences. I will have one class covering C,
but the more familiar you are, the better.
Finally, an alternative textbook which we like is Computer Systems: A Programmer’s Perspective. If you find the P&H book too
hardware-focused, CS:APP may be worth a try. It uses a different
architecture (a simplified version of x86) but the book is good
enough that the extra translation effort may be worthwhile.

#The Fetch-Decode-Execute Cycle
My first class will aim to help you develop a high level understanding
of the major components of a modern computer system—including
those within the CPU itself—and how each is involved in the execution of a program.