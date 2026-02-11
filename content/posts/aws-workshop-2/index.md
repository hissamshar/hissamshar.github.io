---
title: "Setting Up Linux on WSL and Learning Basic Terminal Commands"
description: ""
summary: ""
categories: ["Linux", "Tutorial"]
tags: ["linux", "git"]
#externalUrl: ""
date: 2026-02-10
draft: false
---
# Setting Up Linux on WSL (Windows Subsystem for Linux)

If you're using Windows but want a real Linux environment for
development, learning, or experimenting, **WSL (Windows Subsystem for
Linux)** is one of the easiest ways to get started.

It allows you to run a Linux distribution directly inside Windows --- no
reboot required.

------------------------------------------------------------------------

## What is WSL?

WSL lets you run a full Linux user-space environment inside Windows. You
can:

-   Use the Linux terminal
-   Install packages
-   Compile programs
-   Run tools like `git`, `gcc`, `python`, `node`
-   Even run some GUI apps (WSLg)

------------------------------------------------------------------------

# Installing Linux on WSL

## Step 1: Install WSL

Open **PowerShell as Administrator** and run:

``` powershell
wsl --install
```

This will: - Enable required Windows features - Download the Linux
kernel - Install Ubuntu by default - Ask you to restart your system

------------------------------------------------------------------------

## Step 2: Set Up Your Linux User

After rebooting:

1.  Ubuntu will launch automatically.
2.  Create a username.
3.  Set a password.

Now you are inside Linux.

------------------------------------------------------------------------

## Step 3: Update Your System

Inside the Linux terminal, run:

``` bash
sudo apt update
sudo apt upgrade -y
```

This ensures everything is up to date.

------------------------------------------------------------------------

# Alternative Ways to Install Linux

WSL is convenient, but there are other options.

------------------------------------------------------------------------

## Virtual Machine (VM)

You can install Linux using:

-   VirtualBox
-   VMware

### Pros:

-   Full Linux system
-   Safe testing environment
-   Easy to remove

### Cons:

-   Slower than WSL
-   Uses more RAM and disk space

------------------------------------------------------------------------

## Dual Boot

Install Linux alongside Windows on your computer.

### Pros:

-   Full performance
-   Direct hardware access

### Cons:

-   Requires disk partitioning
-   Must reboot to switch OS
-   Risky if done incorrectly

------------------------------------------------------------------------

## Which Option Should You Choose?

  Use Case                    Recommended Option
  --------------------------- --------------------
  Beginner learning Linux     WSL
  Testing OS-level features   VM
  Daily Linux user            Dual Boot
  Development on Windows      WSL

------------------------------------------------------------------------

# Basic Linux Terminal Commands

Once you're inside Linux, the terminal becomes your best friend.

Here are essential commands every beginner should know.

------------------------------------------------------------------------

## File and Directory Commands

### pwd

Print working directory.

``` bash
pwd
```

### ls

List files and directories.

``` bash
ls
ls -l
ls -a
```

### cd

Change directory.

``` bash
cd foldername
cd ..
cd ~
```

### mkdir

Create directory.

``` bash
mkdir myfolder
```

### rm

Remove files or folders.

``` bash
rm file.txt
rm -r foldername
```

Be careful. Deleting is permanent.

------------------------------------------------------------------------

# File Operations

### touch

Create empty file.

``` bash
touch file.txt
```

### cat

Display file content.

``` bash
cat file.txt
```

### nano

Edit file in terminal.

``` bash
nano file.txt
```

------------------------------------------------------------------------

# Package Management (Ubuntu/Debian)

### Install a package

``` bash
sudo apt install package_name
```

Example:

``` bash
sudo apt install git
```

### Remove a package

``` bash
sudo apt remove package_name
```

------------------------------------------------------------------------

# System Information Commands

### whoami

``` bash
whoami
```

### uname -a

``` bash
uname -a
```

### top

``` bash
top
```

Press `q` to quit.

------------------------------------------------------------------------

# Understanding sudo

`sud o` means:

Super User DO

It gives administrative privileges.

Example:

``` bash
sudo apt update
```

------------------------------------------------------------------------

# Final Thoughts

WSL makes Linux accessible without leaving Windows. It's perfect for
developers, students, and beginners exploring Linux.

Use the terminal daily. Practice commands. Break things. Fix them.
Learn.

Happy hacking.
