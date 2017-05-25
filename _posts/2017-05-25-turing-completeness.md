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

Note that just because a Turing machine can compute anything, doesn't mean it can compute anything *efficiently*. Even simple algorithms you can write in a few lines in C takes is [quite](http://courses.cs.vt.edu/~cs1104/TM/TM.samples.html) [more](http://aturingmachine.com/examples.php) [complex](http://cnl.salk.edu/~oernst/projects/turing.html) if expressed purely with a Turing machine. That "can compute anything" comes with a caveat: that it might be very hard to write the program, or that it might take a ridiculously long time and/or ridiculously large memory as to be utterly useless in many cases.

Turing machines soon became very popular, and eventually a standard because they both provided a powerful mechanism to calculate anything and were easy to understand.

The initial version of the Turing machine had just a long single tape. Later on, people came up with the concept of "multiple" tape Turing machines that used two to five tapes. Each head can move independently of the other heads. Since every multi-tape Turing machine has an equivalent single-tape Turing machine, multi-tape Turing machines were not any more powerful than single-tape ones, but they helped to simplify programs.

<div class="embed-responsive embed-responsive-16by9">
  <iframe width="640" height="360" src="https://www.youtube-nocookie.com/embed/psUCIK2k0FY?controls=0&amp;" frameborder="0" allowfullscreen></iframe>
</div>

# Turing Completeness




----------
References, in no particular order:
* [Wikipedia, obviously](https://en.wikipedia.org/wiki/Turing_completeness)
* [freeCodeCamp's much better attempt at explaining this](https://medium.freecodecamp.com/javascript-is-turing-complete-explained-41a34287d263)
* [University of Cambridge's explanation on Turing machines](https://www.cl.cam.ac.uk/projects/raspberrypi/tutorials/turing-machine/one.html)
* [ScienceBlogs' explanation on Turing machines](http://scienceblogs.com/goodmath/2007/02/03/basics-the-turing-machine-with-1/)
* [Redditors attempting to explain Turing completeness to a five-year old](https://www.reddit.com/r/explainlikeimfive/comments/1nbcl5/turing_complete/)

[example-tape]: /assets/images/2017-05-25-turing-completeness/example_turing_tape.png
