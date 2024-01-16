---
# hidden: true
layout: single
title:  "Creating an online IDE for FreeST"
date:   2023-08-24 22:30:00 +0100
categories: jekyll try-it
techs: 
  - Jekyll
  - JavaScript
  - HTML
  - CSS
---

FreeST was previously available in two places: [RSS][rss] and [TryIt][old-tryit]. RSS was general 
    information and downloads, while the TryIt page was a way of trying FreeST online without the 
    need to install any tools (such as Stack or Haskell).

Recently, we decided to 'move out' to [GitHub][freest-lang-github], and thus, create a 
    [new page][freest-lang] with new and improved features. It was implemented using Jekyll and 
    served through GitHub Pages. The RSS page is now 'outdated', but the same cannot be said for 
    the [TryIt][old-tryit] page. We work around this by adding a link to it in the new website, 
    but it is desirable to have them together so that visitors do not need to toggle between 
    websites.

The general idea is to create a TryIt clone with syntax highlighting and the ability to execute
    code into FreeST's new website.

# Running code
TryIt has a very simple interface for running code: a text area with syntax highlighting, a button
    to run, and a (non-editable) text area for execution output.

Executing code is done by sending code to the TryIt server via HTTP POST request and then receiving
    its output. We can quickly recreate this via `curl`

        curl -H "Content-Type: application/json" \
             -d '{"code":"main : ()\nmain = putStrLn \"Hello, World!\""}' \
             -XPOST http://194.117.20.245:5011/run.json

The previous command yields `{"result": "Hello, World!\n()\n"}`, the pure result of running the 
    compiler. If any error would be thrown, it should appear here as-is in the 
    `result`.

## Emulating code execution using JavaScript
JavaScript has a `fetch` command to execute POST requests, and if we pair it with an HTML element
    to hold code, we can easily recreate what the TryIt page does with a few lines of code:

```js
async function executeCode() {
    const code = document.getElementById("code").value;

    const options = {
        method: 'POST',
        headers: {
        'Content-Type': 'application/json'
        },
        body: JSON.stringify({code: code}),
    };

    const response = await fetch("http://194.117.20.245:5011/run.json", options);
    
    return await response.json();
}
```

# Code editor
One important aspect of Tryit is to behave in a similar fashion to a rudimentary IDE (without any
    fancy features like auto-complete and error highlighting). How can we replicate this behaviour?

## Vanilla HTML, CSS and JavaScript 
No way I'm even going to elaborate on this. It would be hellish and unwise to try and create an
    entire framework just for this case. We'll follow common sense and search for pre-existing
    solutions that we can use in the context of Jekyll and GitHub Pages.

## CodeMirror
TryIt uses [CodeMirror][code-mirror] to implement a code editing field. We want to 
    replicate this field in the new website but fully configured for FreeST. CodeMirror uses `npm`
    (or other dependency managers if you choose so) to install it, however, Jekyll uses `Bundle` 
    and not `npm`. Attempts have been made to merge both under a same project but it becomes too 
    cumbersome when in combination with GitHub Pages (check [here][code-mirror-github-pages] for 
    more info).

Fortunately, there is a git project that provides CodeMirror with vanilla JavaScript 
    [wc-codemirror][wc-codemirror]. With a few lines of code, we can embbed a JavaScript editor 
    into any page:

```html
<wc-codemirror mode="javascript"></wc-codemirror>

<script type="module" src="https://cdn.jsdelivr.net/gh/vanillawc/wc-codemirror@1/index.js"></script>
<script type="module" src="https://cdn.jsdelivr.net/gh/vanillawc/wc-codemirror@1/mode/javascript/javascript.js"></script>
```

There is catch however: custom syntax highlighting is only available to the original CodeMirror, 
    not the vanilla JavaScript one. Which means that we either abandon syntax hightlighting 
    alltogether or settle for Haskell's one. Neither of these options seem desirable.

## Ace
[Ace][ace] is (similarly to CodeMirror) a web component for a code editor. Unlike
    CodeMirror, it does not require any other frameworks, so we can embbed it directly into 
    HTML. The following code is a small editor for JavaScript:

```html
<style type="text/css" media="screen">
    #editor { 
        height: 20em;
        font-size: inherit;
    }
</style>

<div id="editor">
function foo(items) {
    var x = "All this is syntax highlighted";
    return x;
}
</div>
    
<script src="https://cdnjs.cloudflare.com/ajax/libs/ace/1.24.1/ace.js" type="text/javascript" charset="utf-8"></script>
<script>
    var editor = ace.edit("editor");
    editor.setTheme("ace/theme/monokai");
    editor.session.setMode("ace/mode/javascript");
</script>
```

Ace also has the option of creating your own themes, syntax highlighting and IDE-like features. The
    good news is it relies just on JavaScript to implement. It uses a structure very similar to 
    what was already done for FreeST's syntax highlighting extension in VSCode (you can check it 
    in my [portfolio page]({% link _portfolio/freest-language.md %}){: target="_blank" rel="noopener noreferrer"})

# Building the page

## Editor

- **ace**
- integration
- syntax highlighting
- placeholder code

## Execute

- run button
- used one from minimal mistakes' options
- onclick function for running code
    - getting code from editor
    - POST request to tryit
    - output to terminal

## Output terminal
- copy & paste from old tryit

# Good news, bad news
- https site cannot (safely) make requests to http server


[rss]: http://rss.di.fc.ul.pt/tools/freest/
[old-tryit]: http://gloss.di.fc.ul.pt/tryit/FreeST 
[freest-lang-github]: https://github.com/freest-lang
[freest-lang]: https://freest-lang.github.io/
[code-mirror]: https://codemirror.net/
[code-mirror-github-pages]: https://talk.jekyllrb.com/t/jekyll-bundler-and-npm/5203/2
[wc-codemirror]: https://github.com/vanillawc/wc-codemirror
[ace]: https://ace.c9.io/