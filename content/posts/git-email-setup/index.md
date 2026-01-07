---
title: "Setting Up Git Send-Email with Gmail"
description: "A comprehensive guide on installing and configuring git send-email with Gmail for submitting patches."
summary: "Learn how to configure git send-email with Gmail, including 2FA setup, app passwords, and sending your first patch to a mailing list."
categories: ["Version Control", "Git"]
tags: ["git", "email", "tutorial", "cli", "gmail"]
#externalUrl: ""
date: '2026-01-06T15:45:33+05:00'
draft: false
---

Using `git send-email` allows you to send patches directly from the command line, a workflow favored by many open-source projects (like the Linux kernel and SourceHut). This guide walks you through setting up `git send-email` with Gmail and sending your first patch.

## 1. Installation

First, ensure you have the necessary tools installed. On Ubuntu/Debian, run:

```bash
sudo apt install git git-email
```

## 2. Configuring Gmail

To use Gmail with `git send-email`, you need to configure authentication:

1.  **Enable Two-Factor Authentication (2FA)** on your Google account if you haven't already.
2.  **Generate an App Password**: Go to your Google Account > Security > App passwords. Create a new one for "Mail" or "Custom" (name it "git").
3.  **Configure Git**: Run the following command (substituting `your password` with the App Password you just generated):

```bash
git config --global sendemail.smtpPass 'your_app_password_here'
```

Next, add your SMTP server details to your global `.gitconfig`:

```bash
git config --global sendemail.smtpserver smtp.gmail.com
git config --global sendemail.smtpuser you@gmail.com
git config --global sendemail.smtpencryption ssl
git config --global sendemail.smtpserverport 465
```

*Make sure to replace `you@gmail.com` with your actual email address.*

If you haven't set your global user details yet, do so now:

```bash
git config --global user.email "you@gmail.com"
git config --global user.name "Your Name"
```

> **Note**: It is possible to use OAuth 2.0 instead of App Passwords, but it requires more complex setup with `git-credential-oauth`.

## 3. sending Your First Patch

Let's test the configuration by sending a patch to a test repository.

### Clone and Modify

Clone the test drive repository:

```bash
git clone https://git.sr.ht/~sircmpwn/email-test-drive
cd email-test-drive
```

Create a change to commit:

```bash
echo "I'm about to try git send-email" > your-name
# Replace 'your-name' with your actual identifier or name file
```

Commit the change:

```bash
git add your-name
git commit -m "Demonstrate that I can use git send-email"
```

### Send the Email

The README for this test repo asks us to send patches to `~sircmpwn/email-test-drive@lists.sr.ht`.

```bash
git send-email --to="~sircmpwn/email-test-drive@lists.sr.ht" HEAD^
```

Follow the prompts (you can ignore Message-ID for now). If successful, your email should appear in the [mailing list archives](https://lists.sr.ht/~sircmpwn/email-test-drive).

## 4. Handling Feedback (The Workflow)

Wait for feedback. In this test flow, a bot will likely reply to your email.
If you need to make changes based on feedback:

1.  **Edit the file**: Make the requested changes.
2.  **Amend the commit**:
    ```bash
    git commit -a --amend
    ```
3.  **Send v2**:
    ```bash
    # Set default To address to save time
    git config sendemail.to "~sircmpwn/email-test-drive@lists.sr.ht"

    # Send version 2 with annotation
    git send-email --annotate -v2 HEAD^
    ```

Using `--annotate` opens your editor, allowing you to add a "cover letter" or comments under the `---` line, like:

```text
Subject: [PATCH v2] Demonstrate that I can use git send-email

---
This fixes the issues raised from the first patch.

your-name | 1 +
1 file changed, 1 insertion(+)
```

## 5. Advanced Tips

### Sending Multiple Patches

To send a series of commits (e.g., the last 3):

```bash
git send-email HEAD~3
```

### Cover Letters

For complex series, add a cover letter to explain the context:

```bash
git send-email --cover-letter origin/master
```

This generates a template (`[PATCH 0/N]`) where you can describe the goal of the series.

### Sign-off

Many projects (like Linux) require a "Signed-off-by" line (DCO).

```bash
git send-email --signoff
# Or configure it globally
git config format.signOff yes
```

### Always Annotate

To review every email before sending:

```bash
git config --global sendemail.annotate yes
```

Happy Hacking!
