---
layout: post
title: Development Setup on Windows 10
date: 2020-07-31 19:20 -0600
category: Tools
tags: [windows, unix, linux]
---

![Screenshot of how my Windows 10 setup looks like](https://raw.githubusercontent.com/andres-arias/andres-arias.github.io/master/assets/img/windows.PNG)
*Screenshot of how my Windows 10 setup looks like*

Before I start, let me get something straight: No, I haven't lost my mind.

With that cleared out, let me give a little background: I've been working for years on Unix systems, first on Ubuntu (and a bunch of different distros) and recently on a Mac. I never really liked the development ecosystem for Windows. That haven't changed much, my work laptop still is a MacBook Pro, however, I've been *gaming* quite more often now during quarantine, so having to change between my "work" OS (Ubuntu) and my gaming OS was starting to get a little bit tiring, so I was wondering if I could get Windows 10 set the way I like to work on my personal projects.

I found this [great blog post](https://char.gd/blog/2017/how-to-set-up-the-perfect-modern-dev-environment-on-windows) after Googling "Developer setup for Windows 10", and it really helped me get where I can work comfortably. Here's a writeup on what I did, so maybe it might be useful for someone trying to move from a Mac (or Linux) to Windows.

<!--more-->

## Install the Chocolatey package manager

I don't really use [Chocolatey](https://chocolatey.org/) much, but it's useful when constantly working on Windows, also, I used it to install my Terminal Emulator of preference.

1. First, open PowerShell with administrative privileges.
2. Check you Execution Policy with `Get-ExecutionPolicy`
3. If your Execution Policy is set to `Restricted`, run `Set-ExecutionPolicy AllSigned` (Don't forget to change it back to `Restricted` once you're finished).
4. Run the following command:
```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

That's it!

## Install the Windows Linux Subsystem (WSL)

First, I installed the Windows Linux Subsystem, so I can get all the goodness from a Unix system into my Windows setup, there are a lot of tutorials on how to install WSL, and there's no much use for me to write down the same thing, so just go ahead [and follow this one](https://char.gd/blog/2017/how-to-set-up-the-perfect-modern-dev-environment-on-windows).

## Install the Fluent Terminal

I tried a bunch of terminal emulators, from the web-based [Hyper](https://hyper.is/), to Microsoft's shiny new [Windows Terminal](https://www.microsoft.com/en-us/p/windows-terminal/9n0dx20hk701), but ended up settling with [Fluent Terminal](https://github.com/felixse/FluentTerminal) because it was super easy to configure, and it looks great. This is where Chocolatey comes in handy:
```powershell
choco install fluent-terminal
```

Then go to Fluent Terminal's Settings (`Ctrl + Shift + ,`) and edit your Profiles to set WSL bash shell as the default. I didn't really have to change anything, there should already be a WSL profile pointing to `C:\windows\system32\wsl.exe` that you can just set as Default.

## Install and configure your favorite shell

Okay! So we're inside good ol' bash running on WSL (With your Linux distro of choice, most likely Ubuntu I presume), so now you can pretty much do what you normally do with you Linux setup. I'm going to assume Ubuntu.

Most people are fine just using bash, if you're one of them, you're pretty much set at this point. However, I'm more of a [zsh](https://www.zsh.org/) guy, so the first thing I do when setting up a new Unix system, is install zsh with [Oh My Zsh](https://ohmyz.sh/):

```bash
sudo apt install zsh # Install zsh
sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" # Install Oh My Zsh using wget
chsh -s $(which zsh) # Set zsh as the default shell.
```

Finally, I edit my `~/.zshrc` file to use the `agnoster` theme (The one with the cool looking arrows that pretty much everybody uses these days):

```
ZSH_THEME="agnoster" 
```

*Just a quick tip for WSL*: You probably will be working on the everyday Windows directories (You know, `C:\Users\username\Document` and such), so be aware that the WSL `home` will be different, and your Windows filesystem will be mounted on `/mnt/c/`, so a quick `cd ~` will take you to the Ubuntu `home` and not `C:\Users\username\`.

Having to type `cd /mnt/c/Users/andres/` every time I want to go to my Windows Home directory got annoying really quick, so I created a handy alias on my `~/.zshrc` file:

```
alias home="cd /mnt/c/Users/andres/"
```

So now I can just type `home` and get there.

## Install what you need to get work done

This is pretty much up to you! Install what you need to work on your projects, you now have the entire Ubuntu repositories and PPA's on your Windows system! Ruby? You got it, NodeJS? Of course.

What I mainly need is [tmux](https://github.com/tmux/tmux) and [Neovim](https://neovim.io/) with my setup:

1. Install tmux: `sudo apt install tmux`
2. Install Neovim (I used the PPA to have a more recent version, as required by my [autocompletion engine](https://github.com/neoclide/coc.nvim)):
```bash
sudo add-apt-repository ppa:neovim-ppa/stable
sudo apt update
sudo apt install neovim
```
3. Set a `vim` alias to open `nvim` (`~/.zshrc` in this case):
```
alias vim="nvim"
```
4. Then install [Vundle](https://github.com/VundleVim/Vundle.vim) (Or whatever package manager you use for vim):
```bash
 git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```
5. Apply your dotfiles (if you have any, which I recommend you do and keep them under version control). You can find [mine here](https://github.com/andres-arias/dotfiles).

## You're done!

That's pretty much it, you can now use your Windows 10 system to do dev work, I installed Ruby (and Bundle) to maintain this site, Python 3, pip, NodeJS, npm, yarn, awscli and Angular CLI for my Personal Projects. The best part is that I can do everything I normally do with my PC: gaming, webdev, write on this blog, all without having to jump back and forth between Operating Systems.

I'm going to play around with this setup and I might write in the future on how my experiment went.

Thanks for reading!
