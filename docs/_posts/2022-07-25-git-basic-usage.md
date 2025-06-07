---
layout: post
title: "Git: Basic usage"
date: 2022-07-25 09:52:12 +0545
categories: git
---

The daily usage of git revolves around only a few commands: status, add, commit and push.

You can work with git and github using a GUI like Github Desktop, which is pretty straightforward to use. Using the command line is easier, powerful and necessary in the long run since not all machines will have a GUI.

## Before we start

Make sure you have a git project (repository). Or see [here]({% post_url 2022-07-23-git-start-a-new-repository %}) to make one.

Here are some technical terms:

- Commit<br>
It is like a checkpoint for your project. Before you commit you changes, you need to add them to a stage. This process is called staging. Changes in the staging phase (called a stage) can be edited (replaced and removed). Then, you finally commit all the changes in the stage. This process (commit) saves these changes on the git repository.

- Push<br>
Push actually sends your local commits to the remote host.

## Git Status

`git status` gives you a list of files that have been changed, plus new files that haven't been formally added.

If your project is empty, create a new file. Give it a name and add any content to it. Then, type the following in a terminal: (Make sure you are in the project root directory.)

```
$ git status
```

Look for two main different things in the output:
- Changes not staged for commit (Red color)<br>
Modified files that are not staged yet
- Untracked files (Red color)<br>
New files that are not tracked by get yet.
- Changes to be committed (Green color)<br>
Changes that have been staged and are ready to be committed

## Add, commit and commit message

After you have made change to a file, use git add to add them to stage for committing.

```
$ git add filename
```

This adds filename to stage and it is then ready for commit. You can use `git add .` command to add all files in current path to stage.

Then use `git commit` to add the changes in the stage to the git repository.

```
$ git commit -m "write commit message here"
```

A commit message summarizes the changes in that commit. It should be short (not more than 60 characters) and descriptive so that it's easy to read in a list, very much like an email subject. It should describe exactly what has been done so that the history of the repository becomes more readable.

The commit should answer '**What does this commit do?** and not '**What did I do in this commit?**.

Here are some examples are:
- Fix navbar dropdown bug
- Add dropdown feature in primary menu

For a commit message with multiple lines line, use the commit option without the m flag as follows:

```
$ git commit
```

The above command opens up a text editor and lets you add your message. Write an initial line that describes the changes (like an email subject) and finally add other lines as required (like an email body). After you write your message, save it and exit to return to the command prompt. To abandon git commit, simply exit the editor without adding text.

### Push to GitHub

Simply committing changes locally does not add your changes to github. New commits need to be pushed to the github. To push a commit to github, use the following command:

```
$ git push <name> <branch>
```

Here is the actual command:

```
$ git push origin main
```

Here, origin is the name of the remote URL and contains the URL of your git repository. You can see it by typing the command `git remote get-url origin`. Also, main is the name of the branch on which the changes will be pushed to. So, the command tells git to push the changes to main branch of the git repository on the URL specified by origin.

If the remote repository has changes that you don't in your local repository, you need to do git pull first. It is always a good idea to perform git pull before making any commits and pushing them.

Don't push your every small change to GitHub. Do this after making a series of definitive changes that completes a major part of your work. If you have not pushed it yet, you can go back. But pushing a commit to github makes it a part of the project's history and is difficult to remove later.

Next: [Good commit practices and .gitignore]({% post_url 2022-07-25-git-best-practices-and-gitignore %})
