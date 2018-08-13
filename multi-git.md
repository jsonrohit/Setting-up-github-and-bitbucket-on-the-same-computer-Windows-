# Setting up github and bitbucket on the same computer (Mac OS)
Github will be the main account and bitbucket the secondary.

## Intall Git
Use [Homebrew](https://brew.sh/) to install Git.

## Configure Git
+ `git config --global user.name "Your Name"`
+ `git config --global user.email "username@email.com"`

Confirm changes: `git config --global -l`

## Create SSH Keys  

`ssh-keygen -t rsa -C "github email"`

Enter passphrase when prompted. If you see an option to save the passphrase in
your keychain, **do it** for an easier life.

Save keys to:  

`~/.ssh/id_rsa`  

Repeat for bitbucket:

`ssh-keygen -t rsa -C "bitbucket email"`

Save bitbucket key to `~/.ssh/id_rsa_bb`  

## Attach Keys  
Login to remote repo and add ssh key:

```shell
pbcopy < ~/.ssh/id_rsa.pub
pbcopy < ~/.ssh/id_rsa_bb.pub
```

Obviously run each `pbcopy` command **individually.**

Paste into text area, under ssh settings, in your github or bitbucket account.
Also give the ssh key a title like "your name" Laptop.  

## Create Config file  
I am using vim, enter your editor here if different:

`vim ~/.ssh/config`

Create your git aliases like so:

```vim
#Github (default)
  Host gh
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa

#Bitbucket (secondary)
  Host bb
  HostName bitbucket.org
  User git
  IdentityFile ~/.ssh/id_rsa_bb
```  

**Note: On Mac OS Sierra onwards you have to add this to the top of the config file:**

```vim
Host *
  UseKeychain yes
  AddKeysToAgent yes
```

## Add the identities to SSH:  

```shell
ssh-add ~/.ssh/id_rsa
ssh-add ~/.ssh/id_rsa_bb
```

Enter passphrase if prompted.

Check keys were added:

`ssh-add -l`  

## Check that repo recognizes keys:  

```shell
ssh -T gh
ssh -T bb
```  

bitcoin:bc1q95kxs25a6d7fp8prqdmgm6sstjxav8q959t30u