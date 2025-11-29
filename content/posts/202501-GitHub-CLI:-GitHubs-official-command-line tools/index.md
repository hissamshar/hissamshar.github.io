---
date: '2025-01-23T14:49:19+05:00'
title: "GitHub CLI: GitHub's Official Command Line Tools"
---

## What is it?

**gh** is GitHub's official command-line tool designed to extend Git's functionality with GitHub-specific features.

## Purpose:

Simplifies interaction with GitHub's ecosystem directly from the terminal. Allows you to manage repositories and use GitHub features like issues, pull requests, and workflows.

## Key Features:

GitHub-specific tasks:
- Authentication: Easier login (gh auth login) without dealing with tokens manually.
- Repository Management: Create, fork, or clone repositories.
- Issues & Pull Requests: Manage issues, PRs, and comments directly.
- Actions: Manage and view GitHub Actions workflows.
- Works alongside Git for basic version control tasks.

## Use Case:

Best for developers heavily using GitHub and its features (for example: pull requests, issues, and actions).

## How it works
- Install gh:

        > yay -S github-cli

- Verify the installation:

        > gh --version

- Login with GitHub CLI (gh)

        > gh auth login

- Follow the interactive prompts to log in:

    - Choose HTTPS or SSH for connection.
    - Log in via a browser using a one-time code or SSH keys.

- Verify authentication:

        > gh auth status

What's best about it that you can install and use both Git and gh (GitHub CLI) seamlessly. Here's how to set them up:

- Install Git
        
        >sudo pacman -S git

- Check the installation:
        
        > git --version

## Using Git and gh Together

**You can now: Use Git for version control:**

    > git clone https://github.com/username/repo.git
    > git add .
    > git commit -m "message"
    > git push




