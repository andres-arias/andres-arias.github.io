---
layout: post
title: PowerShell for the (Unix) masses
date: 2021-04-14 17:45 -0600
category: Tools
tags: [windows, powershell, unix]
---

![Screenshot of how my PowerShell setup looks like](https://raw.githubusercontent.com/andres-arias/andres-arias.github.io/master/assets/img/powershell.png)
*Screenshot of how my PowerShell setup looks like*

Last time [I wrote about my development setup on Windows 10](https://andres.world/tools/2020/08/01/development-setup-on-windows-10/), I suggested using the Windows Subsystem for Linux (WSL), and I still think this is the best (or most confortable) approach for most Unix people. However, you might find yourself in a situation where you can't really use WSL, such as needing to compile Windows native binaries or your company-issued laptop just won't allow it. 

I actually am in one of those situations, so at first I tried a [Cygwin](https://www.cygwin.com/) setup, and it did work, but not painlessly (it took me some work to get to a place where I was confortable) and it was slow and buggy at times.

After much pain and unwillingness to dedicate more time to Cygwin, I said "screw it" and went the Windows-way (not without some initial resistance of course) and found out that you can get a pretty "Riced Unix" feel with PowerShell easily, and that PowerShell is actually good. So here's a writeup on how I got it working the way I like.

<!--more-->

## 1. Install Git

You know you need it. You can get [Git for Windows here](https://git-scm.com/download/win).

## 2. Install Windows Terminal

The default PowerShell Terminal is quite limited, so you will want something else. The new [Windows Terminal by Microsoft](https://github.com/microsoft/terminal) is actually quite nice, and also works great with Cygwin and WSL. You just need to go to the [Windows Store](https://aka.ms/terminal-preview) and install it.

## 3. Get a decent package manager

We are all used to have a decent CLI-based package manager, such as Homebrew or APT. There's plenty of those on Windows, and you'll need one to feel like home. I recommend [Chocolatey](https://chocolatey.org/), but [Scoop](https://scoop.sh/) is also a good option.

1. First, open PowerShell with administrative privileges.
2. Check you Execution Policy with `Get-ExecutionPolicy`
3. If your Execution Policy is set to `Restricted`, run `Set-ExecutionPolicy AllSigned` (Don't forget to change it back to `Restricted` once you're finished).
4. Run the following command:
```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

Now you can install any package you want using:
```powershell
choco install package
```

## 4. Install a color scheme you like

The default color scheme for Windows Terminal is a plain black-white theme similar to the one found in the Command Prompt (cmd.exe). You can easily add color schemes to Windows Terminal by editing the Settings JSON file. You can open the file with `Ctrl+,` and then add the color scheme you like. I really like this [dark version of Monokai](https://github.com/NickSeagull/windows-terminal-monokai-night):

```json
"schemes": 
    [
        {
            "background": "#1F1F1F",
            "black": "#1F1F1F",
            "blue": "#6699DF",
            "brightBlack": "#75715E",
            "brightBlue": "#66D9EF",
            "brightCyan": "#E69F66",
            "brightGreen": "#A6E22E",
            "brightPurple": "#AE81FF",
            "brightRed": "#F92672",
            "brightWhite": "#F8F8F2",
            "brightYellow": "#E6DB74",
            "cursorColor": "#FFFFFF",
            "cyan": "#E69F66",
            "foreground": "#F8F8F8",
            "green": "#A6E22E",
            "name": "Monokai Night",
            "purple": "#AE81FF",
            "red": "#F92672",
            "selectionBackground": "#FFFFFF",
            "white": "#F8F8F2",
            "yellow": "#E6DB74"
        }
    ]
```
Then you can select the scheme you like for your PowerShell profile.

## 5. Install oh-my-posh

I'm a huge fan of [Oh my Zsh for zsh](https://github.com/ohmyzsh/ohmyzsh), in fact, is the first thing I install when I configure a new Linux or Mac system. You can get a similiar look using the PowerShell version: [Oh My Posh](https://ohmyposh.dev/):

First, install the module:
```powershell
Install-Module oh-my-posh -Scope CurrentUser
```

Then download all the themes to your machine:
```powershell
Get-PoshThemes
```

Finally, configure your `$PROFILE` (think of this file as the PowerShell version of your .bashrc/.zshrc) to automatically start oh-my-posh on every session:
```powershell
code $PROFILE
```

```powershell
Import-Module oh-my-posh
Set-PoshPrompt -Theme agnoster # Select your theme here
```

Restart PowerShell

## 6. Install a NerdFont

Yeah, your oh-my-posh prompt probably looks weird, that's because you need a font with glyph support. For this, install a [NerdFont](https://www.nerdfonts.com/) version of your preferred font and set it as your PowerShell profile font on Windows Terminal.

I like [Fira Mono](https://www.nerdfonts.com/font-downloads).

## 7. Install fzf

I can't live without fzf, it's the next thing I install after oh-my-zsh. You can also get it on PowerShell, but you'll need a wrapper to get it working with the usual keybindings (Ctrl+R and Ctrl+T).

First, install fzf using your package manager:
```powershell
choco install fzf
```

Then, install the [PSFzf](https://github.com/kelleyma49/PSFzf) wrapper:
```powershell
Install-Module PSFzf -Scope CurrentUser
```

Finally, add the following configuration to your `$PROFILE` to override the keybindings:
```powershell
Remove-PSReadlineKeyHandler 'Ctrl+r' 
Remove-PSReadlineKeyHandler 'Ctrl+t' 
Import-Module PSFzf
```

And that's it, you should have a pretty decent PowerShell experience from now on. The screenshot at the beginning of this post shows how mine looks.

From here, you can use your package manager to install the things you need to get your work done: Python, Ruby, Vim, you name it. You can also use the binaries you normally install the Windows-way, y'know, the ones you install by running an Installer downloaded from the Internet (Security concerns aside).

Now all that's left for me is to learn some PowerShell scripting.

Thanks for reading and until next time :)