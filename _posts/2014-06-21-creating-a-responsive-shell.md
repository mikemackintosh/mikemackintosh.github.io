---
layout: post
permalink: /creating-responsive-shell
title: "Create a Responsive Shell Prompt"
category: "How To"
tags: bash linux-system-administration mac-osx cli fish responsive shell terminal tput tty zsh
redirect_from:
 - /creating-a-responsive-shell/
 - /creating-a-responsive-shell/trackback/
 - /creating-a-responsive-shell/feed/ 
---
## Introduction

One of the biggest web trends over the past 3 years has been [_responsive designs_](http://www.smashingmagazine.com/responsive-web-design-guidelines-tutorials/). This is where the width of your device dictates the layout of your website. This allows you to support 1 design, but let it dynamically adapt from mobile device to tablets to full blown desktop browsers.

Well, what about terminal shells? I spend the majority of my day in a shell, and using a tool like [Spectacle](http://spectacleapp.com/), the width of my terminal changes every few minutes. I thought to myself how I could take the idea of responsive web and deploy it to my terminal.

For the interested, you can check out my [`.dotfiles`](https://github.com/mikemackintosh/dotfiles).

## Responsive Shell, Oh My!

We're going to do this quickly and efficiently using `tput`. This would still be possible without `tput`, which became public domain back in 1986, but you would not get cross-environment support. `tput` allows you to query and change certain terminal capabilities. If your environment doesn't support it, you are seriously in need of a new box.

The command we are specifically interested in at the moment is `tput cols`. This command returns the width of the current terminal window. Using the response, we can formulate our `PS1` based on different widths.

## Example

Below, we take the ideas from above and put some bash around it:

    # Get columns
    cols=$(tput cols)
    
    
    # Resize for smaller prompts
    if [["$cols" -le 60]]; then
        PS1="\u@\h in \W # "
    # Full width  
    else
        # Output the Prompt
        PS1="[\t] \u@\h: \w # "
    fi

## Making It Responsive

Just detecting the width does not mean it's responsive. What responsive really means is, when you change the window size, it adjusts automatically. By just using the code above, you would have to reload bash or source your `.bashrc`/`.bash_profile` files to get the change. But this would be annoying, so how do we fix this?

We can use the `PROMPT_COMMAND` environment variable to assign a function to the prompt. How would one do this you may be asking?

    # .bashrc
    export PROMPT_COMMAND=prompt
    
    
    # Create the prompt
    prompt() {
    
    
        # Get columns
        cols=$(tput cols)
    
    
        # Resize for smaller prompts
        if [["$cols" -le 60]]; then
            PS1="\u@\h in \W # "
        # Full width  
        else
            # Output the Prompt
            PS1="[\t] \u@\h: \w # "
        fi
    }

What this does is execute the `prompt()` function every time you enter

## Conclusion

So one of the biggest gotcha's with this method is that, SCP and certain SSH methods don't use interactive shells. This means the `tty` environment will not be set how you expect. As a result, `tput` will error out.

To get around this, you can do a check to make sure a `tty` exists, by using:

    # Check if this is a TTY or not
    IS_TTY=$(tty -s)
    if [$? -eq 0]; then
        # Code goes here
    fi

Enjoy.

