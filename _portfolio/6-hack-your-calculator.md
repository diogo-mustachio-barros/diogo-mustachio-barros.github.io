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

{% include figure image_path="/assets/images/ti-84-plus.jpg" alt="ti-84-plus" caption="Home page" %}{: .align-center style="width: 70%;"}

Back on topic. Why not try to set these visiting students on the same path as me? So the plan
  is clear now, create a tutorial on how to program some simple games in a TI-84
  (they're overall more interesting than solving the quadratic formula). For simplicity, the
  best was to create a website for easy access across lab computers and for future reference
  if any student wanted to explore more after the activity.

# Programming with a TI-84
- Writing code
  - Calculator
  - Text editor
  - IDE
    - small text/phrases on why IDEs are important 
- TI-BASIC language
- Compiling

## TokenIDE
- VSCode is a popular IDE, however, TokenIDE was designed for our purposes and is easier 
  out-of-the-box

## TI-BASIC
- similar to BASIC
- small amount of instructions

# Building a simple website with Jekyll and GitHub Pages
- Simple page with:
  - a small tutorial on how to start
  - a page for each example with:
    - short description
    - overall game logic
    - simplified code explanation
    - full code and files
    - references to 
- Jekyll + GitHub Pages
  - Theme: [Lanyon](https://lanyon.getpoole.com/)
  - adapted with FCUL's colors

[![styled-image](/assets/images/hack-your-calculator-home.png " "){: .align-center}](/assets/images/hack-your-calculator-home.png "Home page")