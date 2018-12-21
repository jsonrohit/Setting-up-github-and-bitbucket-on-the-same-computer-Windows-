# Setting up github and bitbucket on the same computer (Windows)

# Guide for Windows
[mix3d](https://gist.github.com/mix3d) asked for some help using this guide with windows so here we go. This was tested with Windows 10. Run all commands in **Git Bash** once it's installed.

Github will be the main account and bitbucket the secondary.

## Git for Windows
+ Download and install [Git for Windows](https://gitforwindows.org/)
    + In the installer, select everything but decide if you want a desktop icon (2nd step)
    + Select your editor, I use Visual Studio Code
    + Choose *Git from the command line...* on the PATH environment step
    + Choose *Use the OpenSSL library*
    + Choose *Checkout Windows style, commit Unix-style line endings* 
    + Choose *Use minTTY (the default terminal editor of MSYS2)*
    + Deselect *Enable symbolic links* if checked
    + Click Install and then Finish

## Configure Git
+ `git config --global user.name "Your Name"`
+ `git config --global user.email "username@email.com"`

Confirm changes: `git config --global -l`

## Create SSH Keys
+ Right click on desktop and choose *Git Bash here*
+ Enter `cd` to get to home directory (*c:/Users/[username]*)

    `ssh-keygen -t rsa -b 4096 -C "your github email"`

Enter passphrase when prompted

Save keys to: `~/.ssh/id_rsa`  

Repeat for bitbucket: `ssh-keygen -t rsa -b 4096 -C "bitbucket email"`

Save bitbucket key to `~/.ssh/id_rsa_bb`  

## Auto run ssh-agent
+ Make the ssh agent auto run:
    + Check for a *.profile* or *.bashrc* file in your home directory (*c:/Users/[username]*)
    + Open the file in your editor if found or create a new file in your editor if not
    + Place the following code in your file: [Auto-launching ssh-agent...](https://help.github.com/articles/working-with-ssh-key-passphrases/#auto-launching-ssh-agent-on-git-for-windows)
    + Save the file (I used .profile)
    + Quit Git Bash

## Attach Keys  
Login to remote git provider and add ssh key:

```shell
clip < ~/.ssh/id_rsa.pub
clip < ~/.ssh/id_rsa_bb.pub
```

Obviously run each `clip` command **individually.**

Paste into text area, under ssh settings, in your github or bitbucket account.
Also give the ssh key a title like "your name" Laptop.  

## Create Config file  
I am using vim, enter your editor here if different:

`vim ~/.ssh/config`

Create your git aliases like so:

```vim
# Github (default)
  Host gh
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa

# Bitbucket (secondary)
  Host bb
  HostName bitbucket.org
  User git
  IdentityFile ~/.ssh/id_rsa_bb
```  

## Add the identities to SSH:  

```shell
ssh-add ~/.ssh/id_rsa
ssh-add ~/.ssh/id_rsa_bb
```

Enter passphrase if prompted.

Check keys were added:

`ssh-add -l`  

## Check that remote git provider recognizes keys:  

```shell
ssh -T gh
ssh -T bb
```  

bitcoin:bc1q95kxs25a6d7fp8prqdmgm6sstjxav8q959t30u
