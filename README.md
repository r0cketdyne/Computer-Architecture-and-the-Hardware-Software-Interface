# Programming, Computer Architecture and the Hardware Software Interface
uploaded for my fb/irl friend Nicolas Sadirac (repo actively under construction)



"To understand a program you must become both the machine and the program." - Alan Perlis, Epigrams

### Programming

Most undergraduate computer science programs kick off with an introductory course on programming. These courses are designed not only for beginners but also to reintroduce foundational concepts and programming paradigms to those who may have missed them initially.

My go-to recommendation for this foundational content is the seminal work, _Structure and Interpretation of Computer Programs_ (SICP), which is freely available online as both a [book](https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book.html) and a series of [MIT video lectures](http://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-001-structure-and-interpretation-of-computer-programs-spring-2005/video-lectures/). However, for a more tailored approach to new students, I suggest [Brian Harvey’s SICP lectures](https://archive.org/details/ucberkeley-webcast-PL3E89002AA9B9879E?sort=titleSorter) from the 61A course at Berkeley, which offer a more refined and student-friendly presentation.

I'd recommend working through at least the first three chapters of SICP and complete the exercises for a solid foundation. For further practice, engaging with small programming problems on platforms like [exercism](http://exercism.io) can be beneficial.

For those who find SICP too challenging, _[How to Design Programs](http://www.htdp.org/)_ is recommended. Conversely, for those who find it too easy, _[Concepts, Techniques, and Models of Computer Programming](https://smile.amazon.com/Concepts-Techniques-Models-Computer-Programming/dp/0262220695/)_ is suggested.

For those who've never programmed before but yould like to: https://www.reddit.com/r/learnprogramming/wiki/faq/#wiki_getting_started

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
as a point of comparison. I chose
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
as a point of comparison. I chose
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
Finally, an alternative textbook which I like is Computer Systems: A Programmer’s Perspective. If you find the P&H book too
hardware-focused, CS:APP may be worth a try. It uses a different
architecture (a simplified version of x86) but the book is good
enough that the extra translation effort may be worthwhile.

#The Fetch-Decode-Execute Cycle
My first class will aim to help you develop a high level understanding
of the major components of a modern computer system—including
those within the CPU itself—and how each is involved in the execution of a program.

Broadly, the architecture of most modern computers is often but controversially called the “von Neumann architecture” after the prodigious mathematician and computer science pioneer John von Neumann. Specifically, von Neumann was a consultant on the EDVAC project and wrote a report about it that you could say went viral.

EDVAC was one of the first binary digital computers, and a successor to ENIAC which used decimal encodings but is considered by many to be the first digital computer.

Sadly, J. Presper Eckert and John Mauchly who in fact created the “von Neumann architecture” for the ENIAC are not as well known.

I will add a photo to this repo of jvn standing with J. Robert Oppenheimer in front of the IAS machine, built under von Neumann’s direction based on the design in the EDVAC report. IAS was built at the Institute of Advanced Studies at Princeton. Oppenheimer and von Neumann previously worked together on the Manhattan Project.

We will see that a useful model of program execution is that the CPU will repeatedly fetch an instruction from memory, decode it to determine which logic gates to utilize, then execute it and store the results. By the end of the class, you should be able to confidently describe this fetch-decode-execute process, including how the primary components of the system play a role in each step. You should also be able to simulate this process by implementing a very simple virtual machine, which will you start in class and finish as post-class work.

Along the way, I will discuss how computers come to be architected this way, how they have evolved to perform this basic function at tremendous speed, and how the binary encodings of both instructions and data are tightly related to hardware components. These are themes that we will continue to explore throughout the course.

#Pre-class Work
Prior to class, please do two things:

#1: 
A diagram you have drawn of the main components of a computer, and how they are connected; and,

#2: 
A paragraph or two of prose describing your understanding of the fetch-decode-execute cycle, and how the relevant components of the computer are involved in each step.

In both cases, please go into as much detail as possible! I hope that you will spend at least an hour or two researching the topic and pushing your understanding as you draw the diagram and write the description.

There are many resources that you could use as a starting point, but there are two in particular that I recommend: Richard Feynman’s introductory lecture (1:15 hr) and the article How Computers Work: The CPU and Memory.

Richard Feynman is of course better know for his contributions to physics, but he did spend some time thinking about computing, including a lecture series on computation referenced in the further readings for this class, and some work on the Connection Machine. Like von Neumann and Oppenheimer above, Feynman too worked on the Manhattan Project—he was 24 when he joined.

The first is very conceptual; the second is more concrete. Both are useful angles.

While watching the Feynman lecture, you may wish to ask yourself which actual physical components correspond to each of Feynman’s metaphors. For instance, what are the physical equivalents of the “cards” that Feynman describes, and what hardware is used to “file” as opposed to “process” these cards?

While reading the article, please look up any terms or concepts where your understanding is at all vague.

I hope that this reading takes you down a long path of discovery, but don’t forget to incorporate your discoveries into your diagram and description, and submit them to (presumably Bocal).

#Further Resources

If you enjoyed the Feynman lecture above, you may be excited to know that he taught an entire introductory course on computation available in book form as Feynman Lectures on Computation. There are several other books that provide a good high-level introduction: a popular one is Code by Charles Petzold, another is But How Do It Know by J Clark Scott.

For those looking for an introduction to computer architecture from a more traditional academic perspective, I recommend P&H chapters 1.3-1.5 and 2.4, as well as an 61C lecture from 55:51 onward I'll upload sometime soon.

#Lab: Writing a Virtual Machine

This lab-style class aims to consolidate your understanding of the fetch-decode-execute cycle by having you implement a simple virtual machine.

#Pre-class Work
In preparation, please review any new concepts from last class, and read the in-class exercise instructions.

#In-class Exercise
In class, you will begin to write a virtual machine for a very simple architecture that we have designed. The purpose of this exercise is to consolidate your understanding of the fetch-decode-execute model by writing a program that emulates it!

#Post-class Work
Please complete your virtual machine implementation as we will be building upon it in a later class.
