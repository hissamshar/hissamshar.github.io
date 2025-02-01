---
date: '2025-02-01T15:33:56+05:00'
title: "Setting Up Neovim: A Beginner's Guide"
---



# Setting Up Neovim: A Beginner's Guide

Neovim is a modern and extensible text editor that enhances Vim’s capabilities. If you're using Linux, setting up Neovim can be a rewarding experience, allowing you to customize it for an efficient workflow. In this guide, we'll cover installing Neovim, setting up a basic configuration, and enhancing it with essential plugins.

---

## **1. Installing Neovim**
Neovim is available in the official Arch Linux repositories. You can install it using `pacman`:

```sh
sudo pacman -S neovim
```

For the latest nightly version, install it from the AUR using an AUR helper like `yay`:

```sh
yay -S neovim-nightly-bin
```

---

## **2. Setting Up Neovim Configuration**
Neovim’s configuration is stored in `~/.config/nvim/`. Create the directory and initialize a basic configuration:

```sh
mkdir -p ~/.config/nvim
nvim ~/.config/nvim/init.lua
```

### **Minimal Configuration (`init.lua`)**
Add the following settings to your `init.lua` file:

```lua
-- Enable line numbers
vim.opt.number = true
vim.opt.relativenumber = true

-- Set tab size
vim.opt.expandtab = true
vim.opt.shiftwidth = 4
vim.opt.tabstop = 4

-- Enable mouse support
vim.opt.mouse = "a"

-- Set clipboard to system clipboard
vim.opt.clipboard = "unnamedplus"
```

Save and exit Neovim.

---

## **3. Installing a Plugin Manager**
The best plugin manager for Neovim is `lazy.nvim`. Install it by running:

```sh
git clone --depth 1 https://github.com/folke/lazy.nvim.git \
  ~/.local/share/nvim/lazy/lazy.nvim
```

Then, update your `init.lua` to load it:

```lua
local lazypath = vim.fn.stdpath("data") .. "/lazy/lazy.nvim"
if not vim.loop.fs_stat(lazypath) then
  vim.fn.system({
    "git", "clone", "--filter=blob:none",
    "https://github.com/folke/lazy.nvim.git", lazypath
  })
end
vim.opt.rtp:prepend(lazypath)
```

---

## **4. Installing Essential Plugins**
With `lazy.nvim` installed, you can add plugins in `init.lua`:

```lua
require("lazy").setup({
  "nvim-treesitter/nvim-treesitter", -- Syntax highlighting
  "nvim-telescope/telescope.nvim",   -- Fuzzy finder
  "neovim/nvim-lspconfig",           -- LSP support
  "hrsh7th/nvim-cmp",                -- Auto-completion
  "tpope/vim-surround",              -- Surround text objects
  "tpope/vim-commentary",            -- Commenting shortcuts
})
```

Save and exit Neovim, then open it and run:

```vim
:Lazy sync
```

This will install the plugins automatically.

---

## **5. Setting Up LSP (Language Server Protocol)**
LSP enables features like code completion and linting. Install LSP servers for your language:

```sh
# Python
sudo pacman -S python-lsp-server

# C++
sudo pacman -S clang

# JavaScript/TypeScript
npm install -g typescript-language-server
```

Then, enable LSP support in Neovim:

```lua
local lspconfig = require("lspconfig")

lspconfig.pyright.setup({})  -- Python
lspconfig.tsserver.setup({})  -- JavaScript/TypeScript
lspconfig.clangd.setup({})    -- C++
```

Restart Neovim and check LSP status:

```vim
:LspInfo
```

---

## **6. Enhancing Auto-Completion with `nvim-cmp`**
To enable code auto-completion, update your `init.lua`:

```lua
local cmp = require'cmp'
cmp.setup({
  mapping = {
    ['<C-Space>'] = cmp.mapping.complete(),
    ['<CR>'] = cmp.mapping.confirm({ select = true }),
  },
  sources = {
    { name = 'nvim_lsp' },
    { name = 'buffer' },
  }
})
```

---

## **7. Custom Keybindings**
To make Neovim easier to use, add custom keybindings:

```lua
vim.g.mapleader = " " -- Set space as the leader key

-- Open Telescope with leader + f
vim.api.nvim_set_keymap('n', '<leader>f', ":Telescope find_files<CR>", { noremap = true, silent = true })

-- Map jk to escape
vim.api.nvim_set_keymap('i', 'jk', '<Esc>', { noremap = true })
```

---

## **8. Final Thoughts**
Congratulations! You now have a powerful, customized Neovim setup on Arch Linux. From syntax highlighting to LSP support and fuzzy finding, your development workflow will be much smoother.

If you’d like to further improve your Neovim experience, explore more plugins and tweak your settings. Happy coding!

---

### **Further Reading**
- [Neovim Documentation](https://neovim.io/doc/)
- [Awesome Neovim Plugins](https://github.com/rockerBOO/awesome-neovim)
- [Arch Wiki: Neovim](https://wiki.archlinux.org/title/Neovim)

