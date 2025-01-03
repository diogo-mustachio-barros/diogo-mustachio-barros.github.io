---
number: -6
classes: wide
title: "FreeST Website"
techs:
  - Jekyll
status: complete
repo: https://github.com/freest-lang/freest-lang.github.io
toc: true
abstract: |
  A Jekyll and GitHub Pages website that has now become FreeST's new home. It also serves as a 
  learning starting point with its tutorial and documentation for standard libraries. On the 
  academic side, it has a compendium of all publications, talks and posters produced since its 
  first inception.
---

## Motivation

FreeST's old page is just a simple information page at [FCUL's RSS page][rss] 
  (it has since been updated with more modern styles). FreeST had already grown beyond a single page
  website, for example with its separate [TryIt][try-it] website where visitors could program using
  FreeST in their browser. 

Taking advantage of the fact that we wanted to transition to GitHub instead of the faculty's 
  self-hosted GitLab, we decided to create a better and more complete website using GitHub pages.
  Since it was hosted by GitHub itself, availability is near permanent and its easier for
  any one from the project to edit and change whatever is needed.

For this new website we envisioned the following contents:
  - Download page with every FreeST version (complete with release date and release notes)
  - FreeST tutorial
  - Documentation for FreeST's standard libraries
  - Team page
  - Online FreeST editor and runner

For inspiration we looked at other websites and how they organized their content:
  - [Idris](https://www.idris-lang.org/)
  - [Granule](https://granule-project.github.io/)
  - [Iris](https://iris-project.org/)
  - [Kotlin](https://kotlinlang.org/)
  - [Rust](https://www.rust-lang.org/)

## Implementation with Jekyll

FreeST's new website wasn't something complex, most of its content would be static in fact.
  After searching for some options, Jekyll seemed to be the better one for its simplicity,
  integration with GitHub Pages and, above all, because it uses Markdown for its pages' content.

Using Markdown is great because most (if not all) programmers are used to its syntax and 
  frequently write with it in a programming context. FreeST developers could easily write 
  documentation or just add note about some new feature and they would be indirectly writing
  for the website at the same time.

Alongside Jekyll, we were just missing a theme that already had some base styles that we could
  further customize or add if needed. We ended up using [Just the Docs][just-the-docs] which
  has a lot of features for documentation - the main aspect of FreeST's website.

{% include figure image_path="/assets/images/freest-website.png" alt="freest-website" caption="FreeST website" %}{: .align-center style="width: 100%;"} 

## TryIt integration

We were able to implement an fresh TryIt editor similar to the [old one][try-it] using Ace as 
  an editor. However, there was an issue with our backend running in HTTP while the frontend
  was HTTPS. You can read all about it [in this post]({% link _posts/2023-08-24-freest-try-it.md %}).


[just-the-docs]: https://just-the-docs.com/
[rss]: https://rss.campus.ciencias.ulisboa.pt/tools/freest/
[try-it]: http://gloss.di.fc.ul.pt/tryit/FreeST
