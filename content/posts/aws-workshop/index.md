---
title: "Mastering Git and GitHub CLI: A Practical Beginner's Workshop Guide"
description: ""
summary: ""
categories: ["Linux", "Tutorial"]
tags: ["linux", "git"]
#externalUrl: ""
date: 2026-02-10
draft: false
---

# Mastering Git and GitHub CLI: A Practical Beginner's Workshop Guide

In modern software development, writing code is only half the job.
Managing it properly is what separates beginners from professionals.

Whether you're: - A university student working on assignments\
- Collaborating on group projects\
- Building a final year project\
- Preparing for internships\
- Contributing to open source

You must understand Git and GitHub.

This workshop-style guide walks you through: - Git fundamentals\
- GitHub integration\
- Branching and collaboration\
- GitHub CLI (gh) for professional workflows

By the end, you'll understand how real developers work.

------------------------------------------------------------------------

## What is Git?

Git is a distributed version control system.

In simple words:

Git tracks changes in your files so you can go back to any previous
version at any time.

Think of Git as: - A time machine for your code\
- A history tracker\
- A collaboration tool\
- A safety net

Without Git, projects quickly become messy with multiple versions of the
same file.

------------------------------------------------------------------------

## Why Version Control Matters

Imagine: - You accidentally delete working code\
- Your teammate overwrites your changes\
- You introduce a bug and don't know where

With Git, you can: - Revert to earlier commits\
- Track who made changes\
- Compare file versions\
- Safely collaborate

Version control is expected in professional environments.

------------------------------------------------------------------------

## Git vs GitHub

  Git                  GitHub
  -------------------- ---------------------------------------
  A tool               A platform
  Works locally        Cloud-based
  Tracks changes       Hosts repositories
  Command-line based   Web interface and collaboration tools

Git manages versions.\
GitHub stores and shares them.

------------------------------------------------------------------------

## Installing Git

Download from:

https://git-scm.com/

Verify installation:

``` bash
git --version
```

------------------------------------------------------------------------

## Understanding Git Workflow

Git has three main areas:

Working Directory → Staging Area → Repository

### Initialize a Repository

``` bash
git init
```

### Add Files to Staging

``` bash
git add filename.txt
```

Or:

``` bash
git add .
```

### Commit Changes

``` bash
git commit -m "Initial commit"
```

A commit is a snapshot of your project at a specific point in time.

### Check Status

``` bash
git status
```

### View Commit History

``` bash
git log --oneline
```

------------------------------------------------------------------------

## Branching in Git

Branches allow you to: - Work on features separately\
- Fix bugs safely\
- Experiment without affecting main code

### Create a Branch

``` bash
git branch feature-login
```

### Switch to Branch

``` bash
git switch feature-login
```

### Merge Branch

``` bash
git switch main
git merge feature-login
```

------------------------------------------------------------------------

## Handling Merge Conflicts

If two people modify the same line, Git marks the conflict:

    <<<<<<< HEAD
    Your version
    =======
    Other version
    >>>>>>> branch-name

Fix manually, then:

``` bash
git add .
git commit -m "Resolve merge conflict"
```

------------------------------------------------------------------------

## Connecting to GitHub

### Add Remote

``` bash
git remote add origin https://github.com/username/repo.git
```

### Push Code

``` bash
git push -u origin main
```

------------------------------------------------------------------------

## Introducing GitHub CLI (gh)

GitHub CLI allows you to manage GitHub directly from your terminal.

Install from:

https://cli.github.com/

Verify:

``` bash
gh --version
```

Authenticate:

``` bash
gh auth login
```

------------------------------------------------------------------------

## Create Repository from Terminal

``` bash
gh repo create my-project --public --source=. --push
```

This creates the repository, connects the remote, and pushes code in one
step.

------------------------------------------------------------------------

## Professional Workflow Example

``` bash
git checkout -b feature-login
git add .
git commit -m "Add login feature"
git push -u origin feature-login
gh pr create --fill
```

This reflects real-world development workflow.

------------------------------------------------------------------------

## Managing Issues with GH

Create issue:

``` bash
gh issue create
```

List issues:

``` bash
gh issue list
```

View issue:

``` bash
gh issue view 1
```

------------------------------------------------------------------------

## Best Practices

-   Commit small logical changes\
-   Write meaningful commit messages\
-   Use branches for features\
-   Pull before pushing\
-   Review code before merging

Avoid: - Pushing broken code\
- Committing unnecessary files\
- Working directly on main

------------------------------------------------------------------------

