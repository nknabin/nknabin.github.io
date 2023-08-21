---
layout: post
title: "Start a new git repository"
date: 2022-07-23 13:00:22 +0545
categories: git
---

When you start a new project, always do one of the following:
- Always start off a project with a git repository.
- If you want your project to be hosted on GitHub, create a new repository and clone it.

You can also use git locally (without hosting your project). Type git init in the project root (the base folder for the project). It initializes the directory as a git project. It creates a hidden folder called .git. This folder contains all the information about this repository to enforce version control on it. Every collaborator has their own .git folder and this folder is not tracked for changes by git.

.git folder contains information like configuration for the current repository, history of the commits, addresses of remote repositories, branch information, commit logs and more. It contains these information in files (for examples: HEAD contains current branch, config contains configuration options for this repository) and folders (for examples: objects contain all the files, directories and commits in the repository).

## Before we start

Here are some technical terms:

- repository<br>
a data structure that stores information about commit objects and their references for a project (think of it as the .git folder for each project)

- git project<br>
any project that contains a git repository

- collaborator<br>
someone who is on the core development team for a project and has commit access to the main repository of the project

- contributor<br>
someone outside of the core development team for the project who wants to add their changes to the project

- gitignore<br>
list of files/directories to be ignored (not tracked for any changes) by the git

- LICENSE<br>
It defines the conditions for the reuse, distribution and modification of a project.

- README<br>
It is usually a Markdown file (or plain text file) that contains a description of the project. README is an excellent way of providing visitors with a good description of the project and necessary guidelines for its usage (version of tools used, installation guide and more).

## Create your first GitHub repository

Hosting repositories on GitHub provides many advantages (it serves as back up and is great for collaboration).

Follow the steps below to create a new GitHub repository:

1. Go to github and log in to you account.
2. Go to github.com/new or Click the New repository button on the top-left side. Optionally, If you require, you can also initialize this repository with a README file and add a gitignore file based on your programming language and a license.
3. Click the Create repository button.

Your repository will be created and you can view it under the list of your repositories on the left side of your home screen. Follow the steps below to clone it and work on it.

## Clone a github repository

After the repository has been created, it can be cloned in your machine. Cloning a repo means creating a copy/clone of the repository. You clone the repository on your machine, work on it and push the new changes back to github.

```
$ git clone <remote-url>
```

Replace remote-url with the url of your repository. Find this by clicking on Clone or download on your repository page on github. The difference bewteen HTTPS and SSH in cloning is explained below.

### HTTPS vs SSH

If you use HTTPS to clone a git repository, you have to provide your github username and password for every push you make to the repository. However, if you use SSH, git push uses your SSH keys for authentication. This saves time and is suitable for a team project (setting up the collaborators to use SSH is secure and efficient than providing them all with a password).

HTTPS url: https://github.com/\<username\>/\<repository\>.git

```
$ git clone https://github.com/<username>/<repository>.git
```

SSH url: git@github.com:\<username\>/\<repository\>.git

```
$ git clone git@github.com:<username>/<repository>.git
```



## Use git locally

You can use git locally as well (without hosting your project on GitHub). This can be extremely useful to manage your local projects. See below.

- Create a new repository from scratch
  1. Create a new directory that will contain all the project files. This is also called the project root or the base directory.
  2. Navigate into the project root in a terminal. (Use `cd` inside a terminal).
  3. Enter the command: `git init`.

  This initializes the current directory as a git repository. After you add source code, add the files to stage and commit the files.

- Create a new repository from an existing project
  1. Navigate into the project root (the directory that contains all the project files).
  2. Type git init.
  3. Also, create a .gitignore file to tell git not to track certain files (like binary files and log files in the project).
  4. Commit these files.

- Connect a local repository to GitHub<br>
  To push a local repository to GitHub, define a remote-url for the local repository to push its changes to. You can do this as follows:

  ```
  # If you are using HTTPs
  $ git remote add origin https://github.com/<username>/<repository>.git

  # If you are using SSH
  $ git remote add origin git@github.com:<username>/<repository>.git
  ```

  The above commands adds a new remote connection named origin that points to the given url. You can use `git remote get-url origin` and git remote remove origin` to get the URL origin points to and to remove that connection respectively. Note that origin is just a placeholder for the URL. You can use any placeholders you want.

  Note that if the remote repository has any changes that you don't have in your local repository, you need to do git pull before pushing your changes.
  ```
  $ git pull origin main
  ```

## Add, Commit, and Push
This section contains brief guide to add, commit, and push your changes. See [here]({% post_url 2022-07-25-git-basic-usage %}) for more details.

### Add your changes
Now, it is time to add your changes to your repository. For now, create a file called "README.md" with a brief description about your repository.

```
$ echo "This is a test repo!" > README.md
```

To see the current status of the repository, use `git status` command.

### Commit your changes.
When you are ready to commit your changes, enter the following commands.

```
$ git add .
$ git commit -m "Initial commit"
```

### Push to GitHub.
If you cloned a GitHub repository, push your commits to the GitHub repository with the following commands:

```
$ git push origin main
```

Next: [Common usage of git]({% post_url 2022-07-25-git-basic-usage %})
