---
layout: post
title:  Server Setup Guide 
date:   2024-8-17
permalink: /server-setup-guide/
author: Christopher J. MacLellan
comments: false
---

Here is the way that I typically prefer to install python, both on my local
machine (a mac) and on the servers that I work with. In general, I use pyenv
to manage my python versions and I compile with flags that increase
performance.

* TOC
{:toc}

# Get Python Installed

## Install pyenv

First, whenever I get to a new machine where I need python, I install pyenv.
This is much preferrable to using system python, which often has permissions
issues with installing new packages. When you install python with pyenv, then
it is installed in your home directory and you have permissions to install
whatever packages you please. Additionally, I prefer pyenv to conda because the
later likes to install all kinds of other things on your system and is (in my
opinion) bloated. Still, if you are trying to get a specific build chain working
there are sometimes advantages to using conda to get up and running.

I install pyenv with the [https://github.com/pyenv/pyenv-installer](pyenv-installer).

This can be done simply with the following command: `curl https://pyenv.run | bash`

Once this is done I update my appropriate `.bashrc` (or `.zshrc` on my mac)
with the following:
```
export PYENV_ROOT="$HOME/.pyenv"
command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```

## Install Desired Version of Python 

Usually whenever I need python I go to the 
[https://www.python.org](python website) and look up what the current latest
version is (e.g., at the time of writing the latest is 3.12.5). 

Next, I install this version using pyenv:
```
PYTHON_CFLAGS='-march=native -mtune=native' \
CONFIGURE_OPTS='--enable-optimizations --with-lto' \
pyenv install -v 3.12.5
```

Note that the configuration flags yield a version of python that runs faster
because it profiles and tunes python for the specific machine I'm using. 

After you've installed python, you can see it is installed using:
`pyenv versions`.

You can set the global python version by using `pyenv global 3.12.5` or you
can set the local version (for a specific folder and all subfolders) using
`pyenv local 3.12.5`.

# Set up Nostalgia, our fileserver
One you SSH into the machine (e.g., `ssh honor.cc.gatech.edu`), you can set up
the fileserver mount. 

To get the machine to mount the file server, navigate to `/nfs/nostalgia/`. By
going to this location the server will automatically initiate the remote
fileserver mount. Once you are here, create a folder with your username (e.g.,
I created `/nfs/nostalgia/cmaclellan3`). 

You should put all the files you work with here, so you do not fill up the hard
dive space available in your home directory. To make things easier, I creat a
system link in my home directory that points to this folder: `ln -s
/nfs/nostalgia/cmaclellan3/ ~/nostalgia`. This means that there is a
folder in my home directory called nostalgia that links to this location.

One thing to note is that the server's local hard disk is an NVMe drive, which
is very fast. As a result, sometimes it makes sense to keep things locally
on the machine that you need to be able to read and write quickly to.
