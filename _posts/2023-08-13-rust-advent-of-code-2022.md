---
# hidden: true
layout: single
title:  "Advent of Code 2022: Learning Rust"
date:   2023-08-13 22:30:00 +0100
categories: rust advent-of-code
techs: 
  - Rust
---

My first ever (*proper*) language was Java, learned during my first BSc year. C, Python and Haskell
  would also make appearances during the course. A brief sum of my opinions:
- Java - it's like home, might not be the best but it's good for everything
- C - powerful language, love the low-level stuff, hate pointers and memory allocation
- Python - good for quick implementations, and *not* my cup of tea
- Haskell - love functional programming and the powerful type system

Moving to my MSc I delved into the session types niche. It was mostly Haskell and 
  [FreeST][freest]{: target="_blank" rel="noopener noreferrer"} (if we ignore all theoretical
  stuff). [FreeST][freest]{: target="_blank" rel="noopener noreferrer"} was part of my thesis, and
  was a part of my academic life since I started at LASIGE. A part of it that was fascinating
  (since I fully understood it) was the concept of linearity, where some variables **must** be
  used (or consumed) exactly one time. Linearity is essential when it comes to ensuring 
  communication goes according a pre-defined protocol, but also for resources that must be closed 
  such as files.

Working with linearity and session types made me remember a *small* language called **Rust**.
  I used Rust a single time when trying to emulate FreeST's channels, but I've already hear of
  it's C-like performance and great features but mostly because of it's **borrowing system** which
  eliminates most of C's memory headaches. For me it seemed to be the best of both worlds.

Now that I'm on vacation, it seems to be the perfect time to start learning this new language.
  Usually, it would be best if a book (perhaps the 
  [official online book][rust-book]{: target="_blank" rel="noopener noreferrer"} ) or a tutorial.
  However, my arrogance tells me to *just dive in* and start doing interesting things. Instead
  of thinking and designing a project completely blind of Rust's features, what better way to
  learn and do something fun than completing 
  [Advent of Code 2022][advent-of-code]{: target="_blank" rel="noopener noreferrer"}?

There are a total of 25 days an 2 parts per day, a total of 50 code *challenges*. I'll be doing
  them all in Rust as best as I can trying to improve each day. You can see my progress 
  in my [portfolio]({% link _portfolio/9-rust-advent-of-code-2022.md %}){: target="_blank" rel="noopener noreferrer"}
  where I'll be documenting each day, i.e. problem, solving said problem and its coding in Rust.
  Any major *breakthrough* or collection of smaller ones might deserve a proper post in the future,
  but until then I'll be *crabbing* around.

{% include figure image_path="/assets/images/rustacean-flat-happy.png" alt="ferris" caption="Ferris the crab, the unofficial mascot for Rust. Credits: rustacean.net" %}{: .align-center style="width: 70%;"}

[freest]: https://freest-lang.github.io/
[rust-book]: https://doc.rust-lang.org/stable/book/title-page.html
[advent-of-code]: https://adventofcode.com/2022