---
# hidden: true
layout: single
title:  "A better environment for Git"
date:   2024-02-24 14:00:00 +0100
categories: terminal ubuntu git
techs: 
  - Git
  - Bash
abstract: |
  Customizing the terminal experience in Ubuntu 22.04 with a different PS1 variable and adding
  some Git aliases to ease the developer life. 
---

# Customizing the terminal with Git information 

While using a terminal to run code and commit some changes to Git I remembered something: a friend
  of mine, Windows user, once showed me his terminal and it was full of custom information. His 
  PowerShell had time, battery, Git project name and brach. Wow! To be honest, it might be too much
  for me personaly, but the Git information is very handy to avoid constant `git status` commands.

I set out to see how one could customize it in Ubuntu. `PS1` is what came from this search (and no
  not the console, the variable). `PS1` is a system variable that defines what is shown at each 
  command prompt. It's defined in the `~/.bashrc` file and it usually looks like:

```bash
if [ "$color_prompt" = yes ]; then
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
fi
```

The `then` branch is for a colored terminal and the `else` is for a colorless terminal. This might 
  seem scary at first (it was for me at least) so let's deconstruct this. 
  
These codes `\[\033[01;32m\]...\[\033[00m\]` are used to add color to the terminal. Try executing
  `echo -e "\[\033[01;32m\]Hello!\[\033[00m\]"` and you will see a green `Hello!`! If you want to
  check out more information you can go to [this page][terminal-color-codes].
  
For those that recognise such color codes, they might seem a bit *off*, and it is! But not in the 
  way you think. It's not a special character related with color, it's in fact a non-printing 
  character indicator.

Without color codes in the way, we can see that `\u` is the user, `\h` is the machine's name, 
  and `\w` is the workspace directory. Looks easy to customize! Right? Why do I hear boss music...

This definition of `PS1` is static, it's a pattern that is evaluated when the command prompt 
  appears. Dynamic content can still be added through function calls, for example `...$(my_fun)$ `.
  Dynamic color on the other hand... I'm not experienced on bash and might be saying some barbaric
  things but, it's a complete mess.

Let's look at concreet examples. While in a Git project we want to add the current branch name with
  a green background. First we define a function `get_current_git_branch` that does a `git status`
  and extracts the branch name and then we just add it to the `PS1` variable definition.

```bash
PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\[\033[01;32m\]$(get_current_git_branch)\[\033[00m\]\$ '
```

It isn't pretty right? We just added `\[\033[01;32m\]$(get_current_git_branch)\[\033[00m\]` but 
  we lose overall clarity the more we add. We could try and separate this into parts to be clearer
  but we're skiping this for now.

Next up, we want to add the state of the current branch: a green checkmark for up to date, a yellow
  up arrow for ahead, and red down arrow for behind. Once again, we create a function to do this 
  but we hit our first road block. How to return a string from a function? We can `echo` the return
  string and it will be used in place of the function call.

However, color codes mess everything. At least for me, getting the color codes (and mainly the 
  special `\[\]` characters) was not working. How do we work around this? Well, avoid using the
  function? But if we don't use a function it won't be dynamic. This is the biggest problem I ran
  into.

After several hours of searching I found out the static nature of this `PS1` definition (which I
  didn't knew at the time) and that you can define it to be evaluated instead of static. So now
  it is set to call function `set_bash_prompt` to get its definition.

```bash
get_current_git_branch() {
   git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/[\1]/'
}

set_bash_prompt() {
    user_info="\[\033[01;32m\]\u@\h\[\033[00m\]"
    workdir="\[\033[01;34m\]\w\[\033[00m\]"
    git_branch="\[\033[01;42m\]$(get_current_git_branch)\[\033[00m\]"

    PS1="${user_info}:${workdir} ${git_branch}$ "
}

if [ "$color_prompt" = yes ]; then
    PROMPT_COMMAND=set_bash_prompt
else
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
fi
```

As we can see, this is much better for customizing our terminal. It's much more simple and makes it
  easier to move things around or add new ones. Now back to our branch status feature! The 
  problem we had before doesn't appear anymore, we can evaluate and use strings comfortably without
  worrying about special characters or unicode values.

```bash
parse_git_status() {
    # branch status
    local status=$(git status 2> /dev/null | sed -n "s/Your branch is \(\S*\).*$/\1/p")
    
    # up( to date)
    if [ "$status" = 'up' ]; then
        echo "\[\033[01;42m\]$(echo -e '\U2713 ')\[\033[00m\]"
        return
    else
        local commits=$(git status 2> /dev/null | sed -n "s/Your branch is.*by \(\S*\) commit.*$/\1/p")
        if [ "$status" = "ahead" ]; then
            echo "\[\033[01;43m\]$(echo -e '\U2191 ')${commits}\[\033[00m\]"
            return
        elif [ "$status" = 'behind' ]; then
            echo "\[\033[01;41m\]$(echo -e '\U2193 ')${commits}\[\033[00m\]"
            return
        fi
    fi
}
```

The result is something like this:
![terminal-git-status](/assets/images/terminal-git-status.png)


# Useful Git aliases
This was a journey and a half, but another idea popped in the meantime. I recentely watched
  [So You Think You Know Git][so-you-think-you-know-git], a talk by Scott Chacon at FOSDEM
  2024, that went into some cool aspects of Git. Maybe my favourite was Git aliases. I know
  it's probably the simplest one of the lot, but it amazed me how dumb I have been for not
  knowing it and not using it!

Personally, `git status` has always been a hassle. Not because it is complex, but because I always
  fumble the letters and it comes out as `git satuts` or `git stttaus` -- and no, I don't have a
  stutter nor dyslexia. So why not create an alias (a shortcut) that is a lot simpler?

Just go to your `~/.gitconfig` (this one is global) file and add:

```
[alias]
	s = status
```

Now I only write `git s`! So good, so nice. You also save 50% time writing the command (for those
  obsessed whith those things).

Another useful alias is to unstage files. With Git it's easy to `add` and `commit`, but for some
  reason, if I want to unstage some file I have to write `git restore --staged <file>`. Solution:
  an alias!

```
[alias]
  ...
  unstage = restore --staged
```

Isn't `git unstage <file>` much better? Absolutely gorgeous!

Finally, and perhaps the most specific one, I wanted to merge branches but add a commit message.
  This is normally done with `git merge <branch> -m <message> --no-ff`. A quick explanation:
  a normal merge with a specified message and a no fast forward flag (`--no-ff`) to guarantee
  a commit is made (with the given message). Previous recipes for an alias don't work here because
  there are positional arguments.

But Git is a buffed out tool and has a solution for this! We can define an alias as a function and
  write a small 'script'.

```
[alias]
  ...
  mergem = "!f() { git merge $1 -m \"$2\" --no-ff; }; f"
```

To merge with a message (`mergem`) I run `git mergem <branch> "<message>"` and its much simpler!

# End of the day
If you came this far reading, spend some time customizing your tools to your personal likings. Your
  coding space will look more like **your coding space**. It will feel more comfortable and easier
  to work with! Bonus points if you work on it by yourself. 

[terminal-color-codes]:https://manpages.ubuntu.com/manpages/jammy/en/man5/terminal-colors.d.5.html
[so-you-think-you-know-git]:https://www.youtube.com/watch?v=aolI_Rz0ZqY