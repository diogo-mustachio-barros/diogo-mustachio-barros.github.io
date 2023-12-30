---
# hidden: true
number: -4
classes: wide
title: "Hack your calculator @ Ciências"
techs:
  - TI-BASIC
  - Jekyll
status: complete
toc: true
repo: https://github.com/hack-your-calculator/hack-your-calculator.github.io
---

As of late march 2023, still at FCUL as student and investigator, I was asked if I had any activity
  ideas for FCUL's Open Day. What is Open Day? It's a day where the faculty has activities and 
  presentations for interested high-school students. It serves as a way to grow interest in what
  we do and to, in some way, *recruit* future students.

The question I put up in my head is "*What would be more interesting for high-schoolers?*". My 
  target theme was plain Programming. How do I make programming interesting for non-programmers?
  The best way, in my view, is to make something *practical* and close to them. Something sparked
  in my head: their graphical calculator!

Before I wanted to be a programmer, I was in love with physics and was ready to pursue it in 
  university. However, work opportunities were not the brightest in Portugal, and at the time a 
  friend of mine challenged me to try programming. At first it was interesting, I called it my 
  "*Lego sandbox*", but then I discovered that our calculators could be programmed by us.

TI-BASIC was my first true love in programming. I would spend hours programming tools to use in
  class and games to pass time. The ability to create something *real* with a direct effect
  was exhilarating. Because of this, I went on to learn a bit of Java and then decide to study
  Informatics Engineering in university.

{% include figure image_path="/assets/images/ti-84-plus.jpg" alt="ti-84-plus" caption="Texas Instruments TI-84 Plus" %}{: .align-center style="width: 70%;"}

Back on topic. Why not try to set these visiting students on the same path as me? So the plan
  is clear now, create a tutorial on how to program some simple games in a TI-84
  (they're overall more interesting than solving the quadratic formula). For simplicity, the
  best was to create a website for easy access across lab computers and for future reference
  if any student wanted to explore more after the activity.

# Programming with a TI-84
For most, the first interaction with programming a TI calculator is through the calculator itself.
It has a basic editor and several menus of functions and operators to program with. But, as one 
  would expect, a calculator with a handful of characters in width and height is not the most 
  practical to use. A natural upgrade is to use a text editor on a pc as it avoids the awkwardness
  of a calculator's menus and 'keyboard' (if we can even call it that). However, the lack of 
  tools (such as the function and operator menus) when writing programs ends up delaying the 
  development cycle because it requires more testing iterations to detect and fix bugs.

Fear not, for we TI developers are saved from such problems! At least most of them. There are a 
  myriad of IDEs for every case and the TI calculators is no exception. Tokens IDE is perfect for
  the use case and it speeds development significantly. It helps with the TI-BASIC syntax with
  autocomplete and overall checking for syntax errors as we write programs. Most importantly it
  compiles directly to a *calculator-ready* program.

TI-BASIC is (at least for me) a very good language to start. It is very basic (as it name says)
  while still having all core elements of mainstream programming languages. But we'll explore it
  a bit more later on.

Similarly to how I personally did it many years ago, the entire idea was guided by 
  [this amazing tutorial](ti-basic-developer), to which I owe my software engineering path to.

## Tokens IDE
Dear seasoned programmers: yes VSCode is also useful, but Tokens IDE is ready out-of-the-box.
  Although Visual Studio Code can be setup for TI-BASIC programming, it takes some time and 
  attention that most high-schoolers don't have (yet).

(Tokens IDE)[tokens-ide] is by far the most simple to both setup and use. It combines editing 
  with error highlighting and compilation.

{% include figure image_path="/assets/images/tokens-ide.gif" alt="tokens-ide" caption="Tokens IDE (source: https://www.ticalc.org" %}{: .align-center style="width: 100%;"}

The best feature of Tokens IDE is by far the ability to compile directly to a file that can be
  loaded to 

## TI-BASIC
- similar to BASIC
- small amount of instructions

## TI Connect

TI Connect is Texas Instruments' tool to link with a calculator. It's main features are screen 
  capture and file/program management. We use this to load our freshly compiled programs into
  the calculator to then test/use. There are other tools that do this but we always prefer
  the official one whenever possible.

{% include figure image_path="/assets/images/ti-connect.png" alt="ti-connect" caption="TI Connect (source: https://education.ti.com)" %}{: .align-center style="width: 80%;"}

## Wabbitemu Emulator
At some point, the development cycle becomes tiresome. Having to constantly transfer programs to a 
  calculator to test them takes that immediate gratification of quick development, makes it feels
  that the goal is still a hundred fixes, compiles, transfers and tests away.

A quick and easy solution for this inconvenience is to bring your calculator into the computer 
  itself. No need to transfer via TI-Connect, it is now a simple drag-and-drop to load a program 
  into a virtual calculator. 

[Wabbitemu](wabbitemu) is one such emulator. It's a calculator on a window where your mouse pointer
  is your finger (but you can also use keybindings for shortcuts). The only thing to note is: you
  are required to load your own ROM image. In other words, you must own a calculator, extract its
  ROM and load it to use Wabbitemu. Acquiring a ROM through other sources is illegal, so be 
  careful with that.

{% include figure image_path="/assets/images/wabbitemu.jpg" alt="wabbitemu" caption="Wabbitemu emulator (source: https://www.majorgeeks.com)" %}{: .align-center style="width: 40%;"}

For those who wish to explore more, you can also use it to debug programs at deeper levels (cpu 
  registries, stack viewer, breakpoints). 

# Building a small website with Jekyll and GitHub Pages
At this point, I already have everything needed to give a cool and interesting experience for
  students (I hope), but I still need a medium. Well, powerpoint presentation is out of question
  -- its a complete hell for coding -- and it would be a complete waste to give a paper tutorial.
  If we already have students using computers to code, we'll give them a digital way of following
  the tutorial. Furthermore, instead of a PDF file we opted to build a simple website for students
  to visit even after they leave Open Day.

As a *lazy* programmer, any complex combo of web frameworks are completely out of question. This
  website is supposed to be a static display of information. Simpler than Jekyll and GitHub Pages
  is impossible! I quickly chose [Lanyon](lanyon) as a theme and adapted with that gorgeous
  FCUL blue. Now, the website could be built through only the use of markdowns files and everything
  else is handled by GitHub Pages, more importantly the deployment.

This is not supposed to be a complex tutorial with 20 pages of information. There's a home page 
  with: information of what are the objectives, what tools we'll use and what are the basics to 
  use them. Finally there's a page for each tutorial/program students can explore during the
  activity with these main points:
  - short description
  - overall game logic
  - simplified code explanation
  - download links for full code and executable files
  - references to helpful information

There are 3 programs in total: Defuse the Bomb, Guessing Game and Slots. Each program has its 
  difficulty, so students are advised to follow a given order. 
  
[![styled-image](/assets/images/hack-your-calculator-home.png " "){: .align-center}](/assets/images/hack-your-calculator-home.png "Home page")

Check it out yourself [here](hack-your-calculator)!



[ti-basic-developer]: http://tibasicdev.wikidot.com/
[tokens-ide]: https://www.ticalc.org/archives/files/fileinfo/433/43315.html
[wabbitemu]: http://wabbitemu.org/
[lanyon]: https://lanyon.getpoole.com/
[hack-your-calculator]: https://hack-your-calculator.github.io/pages/guess.html