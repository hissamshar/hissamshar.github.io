---
date: '2025-02-28T23:14:21+05:00'
title: 'How to up custom domain for GitHub Pages'
---
# Introduction 

So, you’ve built a sleek website with Hugo and deployed it to GitHub Pages. Now, you want to give it a professional touch with a custom domain like `yourdomain.tech` instead of the default `username.github.io` URL. This guide walks you through the process step-by-step.

## Prerequisites
1. A Hugo website hosted on GitHub Pages (public repository).
2. A custom domain (e.g., `yourdomain.tech`) purchased from a registrar like Namecheap, Google Domains, etc.
3. Basic familiarity with DNS settings and GitHub repository configurations.

---

## Step 1: Configure Your GitHub Repository
First, ensure your GitHub Pages site is set up correctly:
- Your repository should be named `<username>.github.io` (for user/organization sites) or `<repo-name>` (for project sites).
- The `gh-pages` branch (or the `/docs` folder) should contain your Hugo-generated static files.

---

## Step 2: Configure DNS Settings for Your Domain
### Option 1: Use an Apex Domain (e.g., `yourdomain.tech`)
If you want your site to live at the root domain (e.g., `yourdomain.tech`), configure **A records** in your DNS settings:
1. Go to your domain registrar’s DNS management page.
2. Create **four A records** pointing to GitHub’s IP addresses:
   ```plaintext
   Host: @
   Type: A
   Value: 185.199.108.153
   TTL: Automatic
