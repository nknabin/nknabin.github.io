---
layout: post
title: "Set up git and GitHub"
date: 2022-07-23 11:10:12 +0545
categories: git
---

If this is your first time trying to use git or github, there are a few things that you need to do as follows:

## Github

Get a GitHub account. It's as simple as just signing up.

## Git

On Windows machines, download and install git. On Linux machines, you can install it using CLI tools such as apt for Debian-based distros with the command: `sudo apt install git`.

If you don't want to host your project on github, you can use git without github as well to track your projects locally (on your personal device).

## Set up your user name and email for git

Open up a terminal and type the following commands: (On windows, open git-bash after installing git or cmd or Powershell if git is available on your path).

```
$ git config --global user.name "Your name here"
$ git config --global user.email "your_email@example.com"
```

Do this if you want colored output in terminal (this is helpful in some cases but this is optional).

```
$ git config --global color.ui true
```

## Using ssh with github (eliminate the need for passwords on every push)

Every time you push your changes to a repository in github, you will need to provide the username and the password of your github. This will be a hassle in everyday workflow and in working with teams. Github provides an alternative of using ssh keys. Follow the steps below to setup ssh on your machine:

### Set up ssh

Check if these files are present: `~/.ssh/id_rsa` and `~/.ssh/id_rsa.pub` (~ represents your home directory: /home/<username>. The first one is your private key and the second one is your public key. **You should never share your private key with anyone.** If you have these files, skip the step below.

If the above files are not present, generate them using the following commands:

```
$ ssh-keygen -t rsa -C "your_email@example.com"
```

When you generate the keys, it asks you for a password that will be used to encrypt the keys. If you enter a password here, you will need to enter this password while using git push. You can leave this password empty. Don't leave it empty if you need your keys encrypted at all cost.

Copy the public key by opening it on a text editor or using a terminal as follows (this command requires `xclip`):

```
$ xclip -se c ~/.ssh/id_rsa.pub
```

Paste your ssh public key into your github. 

1. Go to Account Settings. 
2. Click on SSH Keys on the left. 
3. Click on Add SSH Key. 
4. Add a label (eg. work machine) and paste the public key on the text box. 

Test your ssh set up using the following command:

```
$ ssh -T git@github.com

Hi username! You've successfully authenticated, but Github does
not provide shell access.
```

If you get the above output, your ssh configuration is correct.

To use ssh, use the GitHub SSH link available in in Clone button in your repository on GitHub. The ssh link follows the structure git@github.com:<username>/<repository>. If you use a repository as https://github.com/<username>/<repository>, it won't use ssh and ask for username and password for HTTPs authentication. Go through the next tutorial to learn more about this!.

Next: [First git repository]({% post_url 2022-07-23-git-start-a-new-repository %})

Also see: [Common usage of git]({% post_url 2022-07-23-set-up-git-and-github %})

