# Setting up github and bitbucket on the same computer
Github will be the main account and bitbucket the secondary.

1. Create SSH Keys

```shell
ssh-keygen -t rsa -C "github email"
ssh-keygen -t rsa -C "bitbucket email"
```
Enter passphrase when prompted.

Save keys to:

`~/.ssh/id_rsa`
`~/.ssh/id_rsa_bb`

2. Attach Keys
Login to remote repo and add ssh key:

```shell
pbcopy < ~/.ssh/id_rsa.pub
pbcopy < ~/.ssh/id_rsa_bb.pub
```

Paste into text area under ssh settings in your github or bitbucket account.
Also give the ssh key a title like Ross' Laptop.

3. Create Config file
I am using macvim, alias mvim, enter your editor here if different:

`mvim ~/.ssh/config`

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

4. Add the identity to SSH:

```shell
ssh-add ~/.ssh/id_rsa
ssh-add ~/.ssh/id_rsa_bb
```

Enter passphrase when prompted.

Check keys were added:

`ssh-add -l`

5. Check that repo recognizes keys:

```shell
ssh -T gh
ssh -T bb
```

6. Test repositories

## Github (default)
Create a repo online called testmulti

```shell
mkdir ~/testmulti
cd ~/testmulti
touch readme.md
git init
git add .
git commit -am "first commit"
git remote add origin git@gh:rosswd/testmulti.git
git push origin master
```

Add some text to readme on github.com, then:

```shell
git fetch origin master
git diff master origin/master
git merge origin/master
git push origin master
```

## Bitbucket (secondary)
Create a repo online called testbucket

```shell
mkdir ~/testbucket
cd ~/testbucket
touch readme.md
git init
```

*This being the secondary account, the username and email have to be
overwritten, using secondary account values, at the repo level:*

```shell
git config user.name "Full Name"
git config user.email "email"
```

This must be done **once** for every *bitbucket* repo, it is not needed for github
repos because the global is used in that scenario. There may be a cleaner way
to do this but right now it works ok.

```shell
git add .
git commit -am "first commit"
git remote add origin git@bb:rosswd/testbucket.git
git push origin master

Add some text to readme on bitbucket.org, then:

```shell
git fetch origin master
git diff master origin/master
git merge origin/master
git push origin master
```

