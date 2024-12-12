---
title: "Linux Shell"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 1. </b> "
---

# Introduction to the Linux Shell

- This is a way to interact with the linux system, the shell provide more power than the graphical interface can. 
- When first load the shell, the shell usually point to `home directory`, by default each user in linux system have their own username which match exactly with the their home directory located at the path `~`. For example user `alen` has his home directory `/home/michael`, other user can't access your files

![Home directory](images/_index.png)

# Command in Linux
## Internal or built-in commands
- These commands are bundle by the shell itself.
- Common internal command including: 
  - `cd` (Change directory)
  - `export` to expose environment variables
  - `mkdir` (make directory)

## External commands
- These are binary program or scripts, which are usually located in distinct files in the system

- To determine whether command is internal or external , you can use the `type` command. For example: type echo

# Basic Linux commands
- Show current working directory run `pwd`
- Show conntent in the directory run `ls`
- To make a new directory run `mkdir` this command can create multiple directory by passing separated directory names.
- To create a directory with specify path you can run the command `mkdir -p /home/dungle/test-os`
- There is a command `popd` helps us to navigate to the recent visited directory, it works like as a stack mechanism.
