---
layout: post
title:  "Learning The Command Line"
date:   2015-03-26 20:15:28
categories: programming commandline command-line software unix linux shell bash zsh
---

Have you ever wanted to level up your command-line skills, but didn't know where to start? Ever wanted to learn about
all the command-line utilities available to you, but didn't know where to look? I too had these problems, and I'm going
to show you how I went about solving them.<!--more-->

## Unix Commands

First off, I wanted to get a list of all the commands available to me so that I could drill down on specific ones and
learn more about them. From what I've learned, the executables for available commands live in `/usr/bin` and in `/bin`.
You can `cd` into these directories to see available commands and learn more about them.

*Example*:

    $ cd /bin
    $ ls

    [          cat        cp         date       df         echo       expr       kill       launchctl  ln         mkdir      pax        pwd        rm         sh         stty       tcsh       unlink     zsh
    bash       chmod      csh        dd         domainname ed         hostname   ksh        link       ls         mv         ps         rcp        rmdir      sleep      sync       test       wait4pat

One of the commands shown above is `ls`, which lists the directory contents of the directory you're in. To learn more
about this command, simply execute:

    $ man ls


The executalbe for `man` is located in `/usr/bin`; it stands for "manual" and gives you documentation for a specific
command. Use `man` anytime you want to learn more about a specific command. `cd` into `/usr/bin` to see more commands
available to you.

## Built-in Shell Commands

If you looked around for `cd` and wondering where it's located, you won't find it in any of the directories described
above. This is because `cd` is a built-in shell command. There are many other commands built into the shell you're
using, and whether you're using `bash` or `zsh`, you can view all the avialable commands and learn more about them by
executing:

    ### Bash ###
    $ man bash

    ### Zsh ###
    $ man zsh

## Fin

Though you could simply google more information about specific commands, it's important to use the command-line as much
as possible if you want to level up!