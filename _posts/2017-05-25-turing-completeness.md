---
title: "Turing Completeness"
author: felixlimanta
image: /assets/images/2017-05-25-turing-completeness/turing-machine-imitation-game.jpg
categories:
  - Computing
tags:
  - assignment
  - computing
last_modified_at: 2017-05-25T11:55:49+07:00
excerpt: A (hopefully) simple explanation on Turing completeness
---

Wikipedia's first paragraph on Turing completeness is this.

> In [computability theory](https://en.wikipedia.org/wiki/Computability_theory "Computability theory"), a system of data-manipulation rules (such as a computer's [instruction set](https://en.wikipedia.org/wiki/Instruction_set "Instruction set"), a [programming language](https://en.wikipedia.org/wiki/Programming_language "Programming language"), or a [cellular automaton](https://en.wikipedia.org/wiki/Cellular_automaton "Cellular automaton")) is said to be **Turing complete** or **computationally universal** if it can be used to simulate any single-taped [Turing machine](https://en.wikipedia.org/wiki/Turing_machine "Turing machine"). The concept is named after English mathematician [Alan Turing](https://en.wikipedia.org/wiki/Alan_Turing "Alan Turing"). A classic example is [lambda calculus](https://en.wikipedia.org/wiki/Lambda_calculus "Lambda calculus").

You know something is a jargon when people use jargon to explain said jargon. This is my attempt to break down these concepts in (relatively) simpler terms.

# Turing Machines
In 1936, mathemathician Alan Turing came up with a very simple theoretical computing machine. Despite its simplicity, this machine is capable of *any* computation, no matter how complicated.

![Representation of a Turing machine][example-tape]

A Turing machine is a machine that runs on top of an infinitely-long tape, which acts like the memory of the computers you know or other forms of data storage. The tape is made up of a long series of little cells, which are uually blank at the start and can be written with symbols.

At any one time, the machine has a head positioned over one of the cells on the tape. With this head, the machine is capable of performing three basic operations:
1.  Read the symbol on the cell under the head.
2.  Edit the symbol by replacing it with a new symbol or erasing it.
3.  Move the tape left or right by one cell to read/write the symbol on a neighboring square.

<div class="embed-responsive embed-responsive-16by9">
  <iframe width="640" height="360" src="https://www.youtube-nocookie.com/embed/gJQTFhkhwPA?controls=0&amp;" frameborder="0" allowfullscreen></iframe>
</div>

Note that just because a Turing machine can compute anything, doesn't mean it can compute anything *efficiently*. Even simple algorithms you can now write in a few lines in C takes is [quite](http://courses.cs.vt.edu/~cs1104/TM/TM.samples.html) [more](http://aturingmachine.com/examples.php) [complex](http://cnl.salk.edu/~oernst/projects/turing.html) if expressed purely with a Turing machine. That "can compute anything" comes with a caveat: that it might be very hard to write the program, or that it might take a ridiculously long time and/or ridiculously large memory as to be utterly useless in many cases.

Despite those limitations, Turing machines are the very height of technology back in the day. Turing machines soon became very popular, and eventually a standard because they both provided a powerful mechanism to calculate anything and were easy to understand.

The initial version of the Turing machine had just a long single tape. Later on, people came up with the concept of "multiple" tape Turing machines to simplify calculations. Multi-tape Turing machines use two to five tapes with an independently-moving head for each. Since every multi-tape Turing machine has an equivalent single-tape Turing machine, multi-tape Turing machines were not any more powerful than single-tape ones, but they helped to simplify programs.

<div class="embed-responsive embed-responsive-16by9">
  <iframe width="640" height="360" src="https://www.youtube-nocookie.com/embed/psUCIK2k0FY?controls=0&amp;" frameborder="0" allowfullscreen></iframe>
</div>
<br />

# Turing Completeness

If a physical machine or a virtual machine can take any program and run it just like a Turing machine, then that machine is called "Turing complete". Turing completeness is kind of a certification---if something can solve any (solvable) problem, that something is Turing complete.

![A scientific calculator, despite its power, is not Turing complete][scientific-calc]
*A Turing incomplete system*

A good example of a Turing incomplete machine is a calculator. A calculator, whether a cheap calculator your local store uses or that shiny $100 graphing calculator you inherited from your forefathers can only perform a pre-defined set of calculations.

However, the phone or PC or whatever you're reading this article on is a Turing complete machine because it can do any calculation that a Turing machine can do if given enough time and memory, even though "enough" can mean anywhere from less than a blink of an eye to more than the time it takes to gather enough money from work, buy a new computer, set it up, and run the algorithm.

## Turing Completeness and Programming Languages
A Turing machine is just a concept---anything (physical or virtual) that can take any program and run it is a Turing machine. And if that thing can run every program that a Turing machine can run, it's Turing complete.

A programming language is Turing complete if it can simulate a single-taped Turing machine. In general, for a programming language to be Turing complete, it needs:
1. A form of conditional repetition or conditional jump (e.g. `while`, `if + goto`)
2. A way to read and write some form of storage (e.g. variables, tape)

This means that most programming languages you can think of are Turing complete. C, C++, Java, Python, Haskell, Prolog, etc. are Turing complete.

Besides programming languages, there are other systems that if used appropriately can be used to store and compute information. The card swapping and storing mechanic of [Magic: the Gathering](https://boingboing.net/2012/09/12/magic-the-gathering.html) is another system that would allow you to compute anything given enough cards, enough time, and enough transactions. Other unintentionally Turing-complete systems include, but not limited to [Minecraft](https://gaming.stackexchange.com/questions/20219/is-minecraft-turing-complete), [Minesweeper](http://web.mat.bham.ac.uk/R.W.Kaye/minesw/infmsw.pdf), [Pokémon](http://www.curtisbright.com/bln/2013/03/01/pokemon-yellow-is-turing-complete/), [PowerPoint](https://www.reddit.com/r/compsci/comments/62x9g9/powerpoint_is_turing_complete/),  [`mov` (and only `mov`) instruction in x86 Assembly](https://www.cl.cam.ac.uk/~sd601/papers/mov.pdf), [the rules used by the airline industry for determining flight availability](http://www.ai.mit.edu/courses/6.034f/psets/ps1/airtravel.pdf), [a series of buckets and stones](http://www.reddit.com/r/explainlikeimfive/comments/1nbcl5/turing_complete/cch68ft), and [the swarming behaviour of soldier crabs](https://www.technologyreview.com/s/427494/computer-scientists-build-computer-using-swarms-of-crabs/).

![A Turing complete system][soldier-crab]
*A Turing complete system*

## Turing Incomplete Languages
If a programming language is mainstream, it's Turing complete. There are, however, several Turing incomplete *domain-specific* languages. ANSI SQL, regular expressions, data languages (JSON, etc.), and markup languages (HTML, CSS, etc.) are a few Turing incomplete languages you might know.

Like every other piece of software there is, programming languages are about trade-offs. There's really no benefit for Turing incomplete programming languages that target several application domains, since the flexibility of Turing completeness is a lot more important than its complexity. On the other hand, if you're not building the "one language to rule them all", you're free to implement only features that make sense to the very specific purpose of the language.

## Turing Tarpits and Esoteric Languages
Just because a language is Turing complete doesn't mean you want to use it. Turing tarpits are Turing-complete languages that can compute anything, but writing a program to do it is ungodly difficult. Examples of Turing tarpits and general esoteric languages with `Hello world` examples include:

* Brainfuck
  ```brainfuck
  ++++++++[>++++[>++>+++>+++>+<<<<-]>+>+>->>+[<]<-]>>.>---.+++++++..+++.>>.<-.<.+++.------.--------.>>+.>++.
  ```

* FRACTRAN
  ```fractran
  5^205469705004861725972941750691144/2
  ```
* Grass
  ```grass
  wvwwWWwWWWwvWwwwwWWwWWWwWWWWwWWWWWwWWWWWWwWWWWWWWwWwwwwwwwwwwwwWWWWwWWWWWWWwWWWWWWWWWWWWWWwWWWWWWWWWWWwwWWWWWWWWWWwwWWWWWWWWWWWWwWWWWWWWWWWwwWWWWWWWWWWwwwwwwWWWWWWWWWWWWWWWwWWWWWWWWWWWWWWWWWWWWWwWWWWWWWWWWWWWWWWWWwwWWWWWWWWWWWWWWWWWwwWWWWWWWWWWWWWWWWWwwwwwWWWWWWWWWWWWWWWWWWWWwwWWWWWWWWWWWWWWWWWWWWWWwWWWWWWWWWWWWWWWWWWWWWWWWWwwwwwwwwwwwwwwwwwwwwwwwwwwWwwwwwwwwwwWWwwwwwwwWWWwwwwwwwWWWWwWWWWWwwwwwwwwWWWWWWwwwwwwwwwwwwwwwwWWWWWWWwwwwwwwwwwwwwwwwwwwwWWWWWWWWwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwwWWWWWWWWWwwwwWWWWWWWWWWwwwwwwwwwwwWWWWWWWWWWWwwwwwwwWWWWWWWWWWWWwwwwwwwwwwwwwwwwwwWWWWWWWWWWWWWwwwwwwwwwwwwwwwwwwwwwwwww
    ```
* Piet (yes, this is code)
  
  ![Hello World in Piet][piet-hello-world]
* Malbolge
  ```malbolge
  (=<`#9]~6ZY32Vx/4Rs+0No-&Jk)"Fh}|Bcy?`=*z]Kw%oG4UUS0/@-ejc(:'8dc
  ```

In conclusion, just because something can calculate anything doesn't mean you should ever actually try to program with it. Just because you can make crabs solve the Travelling Salesman Problem doesn't mean you should.

# TL;DR
If something can run any algorithm, it's Turing complete. Yes, even crabs.

----------
References, in no particular order:
* [Wikipedia, obviously](https://en.wikipedia.org/wiki/Turing_completeness)
* [freeCodeCamp's much better attempt at explaining this](https://medium.freecodecamp.com/javascript-is-turing-complete-explained-41a34287d263)
* [University of Cambridge's](https://www.cl.cam.ac.uk/projects/raspberrypi/tutorials/turing-machine/one.html) and [ScienceBlogs'](http://scienceblogs.com/goodmath/2007/02/03/basics-the-turing-machine-with-1/) explanation on Turing machines
* [Redditors attempting to explain Turing completeness to a five-year old](https://www.reddit.com/r/explainlikeimfive/comments/1nbcl5/turing_complete/)
* [This StackExchange thread](https://softwareengineering.stackexchange.com/questions/132385/what-makes-a-language-turing-complete) on minimum Turing completeness
* [This Reddit thread](https://www.reddit.com/r/compsci/comments/1tvh6a/accidentally_turingcomplete_andreas_zwinkau/) and [this article](http://beza1e1.tuxen.de/articles/accidentally_turing_complete.html) on accidental Turing completeness
* [Esolang Wiki](https://esolangs.org/wiki/Turing_tarpit) for all your esoteric language needs

[example-tape]: /assets/images/2017-05-25-turing-completeness/example_turing_tape.png
[scientific-calc]: /assets/images/2017-05-25-turing-completeness/calculator-scientific.jpg
[soldier-crab]: /assets/images/2017-05-25-turing-completeness/soldier-crab.jpg
[piet-hello-world]: /assets/images/2017-05-25-turing-completeness/piet-hello-world.jpg
