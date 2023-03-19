---
title: "Making pyenv and neovim play nice (and a few gotchas found along the way)"
date: 2018-04-16T19:37:10+02:00
description: ""
keywords: [python, environment]
draft: false
tags: [python, environment]
math: false
toc: false
---

For my development workflow and IDE, I've recently decided to move from [Vim](https://www.vim.org/), to [NeoVim](https://neovim.io/), which is essentially Vim on steroids.

No, but seriously, NeoVim has a [few](https://neovim.io/charter/) features that are really interesting, but the real reason for moving is that, as a Python dev, I need to be able to look at source code and definitions very often, while hacking on my code.
The wonderful [jedi-vim](https://github.com/davidhalter/jedi-vim) plugin normally allows me to do just that, when it comes to my local project. But because I am using virtual environments extensively in my work, such as with [virtualenv](https://virtualenv.pypa.io/en/stable/) and/or [pyenv](https://github.com/pyenv/pyenv), I have a problem on my hands:

**Jedi-vim is painfully unaware of virtual environments**.

So if I for example,

setup a virtual environments in my project dir as such:
```
$ virtualenv -m python 3 venv
```

Activate the environment:
```
$ source venv/bin/activate
```

and then install Django:
```
$ pip install django
```

Initialise a django app with:
```
$ django-admin startproject mysite
```

Open the urls file:
```
$ vim mysite/urls.py
```

Placing the cursor on an object we are importing,
such as url, and running the jedi-vim goto definition shortcut (mine is `,d`):
```
from django.conf.urls import url
from django.contrib import admin
```

Returns the following error:
```
jedi-vim: Couldn't find any definitions for this.
```

Which sucks.

Spent a good amount of time googling, asking around, playing with different plugins, but nope, no joy.
The internet was suggesting three things:
1. Suck it up, cause similar [issues](https://github.com/davidhalter/jedi-vim/issues/744) were not tackled yet or at all..
2. Tackle the issue myself, for which I feel I am not well versed enough (yet).
3. Move to NeoVim plus Pyenv and jedi-vim, where things magically work.

So here I am with my brand new NeoVim setup.
Of course I had to go through a few gotchas to get this to work, which I will share here for all those out there hacking around with this.

## Installing Pyenv

The first thing I did was to ditch virtualenv and use Pyenv instead.
I used homebrew to do that.

```
$ brew update
$ brew install pyenv
$ brew install pyenv-virtualenv
```

We still need to complete the install by adding a few environment variables to our shell profiles.
So depending on our preferred shell, we use ~/.bash_profile , or ~/.zshrc or whatever else:
```
$ echo 'export PYENV_ROOT="$HOME/.pyenv"' >> <profile_file>
$ echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> <profile_file>
$ echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> <profile_file>
```

(more info on [PyEnv Github](https://github.com/pyenv/pyenv))

We can then initialize some environments and play around:
```
$ pyenv install 3.6.5
$ pyenv install 2.7.14
$ pyenv virtualenv 2.7.14 my_p2_project
$ pyenv virtualenv 3.6.5 my_p3_project
$ pyenv activate my_p3_project
$ pyenv versions
$ pyenv deactivate
```
## Installing NeoVim and jedi-vim

For NeoVim, I used, you guess it, Homebrew:
```
$ brew install neovim
```

At which point, we can run it with:
```
$ nvim
```

I then proceeded to install a bunch of plugins for my Python stuff.
I took a great deal of inspiration from [Fisa's NeoVim configuration](https://raw.githubusercontent.com/fisadev/fisa-nvim-config/master/init.vim) for that, but for the sake of this guide, we can simply install the jedi-vim plugin by adding the following to our ~/.config/nvim/init.vim :
```
call plug#begin('~/.config/nvim/plugged')
Plug 'davidhalter/jedi-vim
call plug#end()
```

We can then reopen `nvim` and run a `:PlugInstall`

## Now for our gotcha part

For being able to use Python 2/3 plugins,we will need the neovim Python module, with:
```
pip install neovim
```

But this would mean that we'd have to install the neovim plugin in all the virtual environments we create.
To get around this, we need to create 1-2 virtual environments just for neovim, and add a line in our `init.vim` that will tell nvim to look into those envs for its plugin. We do the following:

```
pyenv virtualenv 2.7.14 neovim2
pyenv virtualenv 3.6.5 neovim3

pyenv activate neovim2
pip install neovim
pyenv which python  # Note the path

pyenv activate neovim3
pip install neovim
pyenv which python  # Note the path
```

We will need to add also the following to our ~/.config/nvim/init.vim:
```
let g:python_host_prog = '/path/to/neovim2/bin/python'
let g:python3_host_prog = '/path/to/neovim3/bin/python'
```

Run a health check in nvim to make sure all is ok:
```
:CheckHealth
```
Once this is done, we are all set with a reachable pyenv environment in our shiny new neovim setup!

