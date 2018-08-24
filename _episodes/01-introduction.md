---
title: "Introduction & Refresher"
teaching: 10
exercises: 10
questions: 
- "How do I add changes to a git repository?"
- "How do I work with a remote git repository?"
objectives:
- "Remind ourselves how git works"
keypoints:
- "git has a two stage process: `add` and `commit`"
- "We can have local and remote repositories, and synchronise changes between them"
---

## Refresher

- git is a version control system which records snapshots of a project.
- We can navigate back and forwards in the history of the changes made to a repository. 
- To record a snapshot in the repository, we need to *add* the changes we want to record to the staging area with `git add [filename]`, and then *commit* the changes with `git commit -m "useful message"`.

> ## Challenge 1
> 
> Create a git repository named `git-refresher`. Make sure it is not nested inside another repository.
> Make a text file called `books.txt`. In three separate commits, add the titles of your favourite
> three books on separate rows.
> 
> Navigate back to the first commit, and check that `books.txt` only has a single title.
> 
>> ## Solution
>> 
>> ~~~
>> $ mkdir git-refresher
>> $ cd git-refresher
>> $ git init
>> $ echo "True Grit" > books.txt
>> $ git add books.txt
>> $ git commit -m "Initial commit - added first book title"
>> $ echo "The Princess Bride" >> books.txt
>> $ git add books.txt
>> $ git commit -m "added second book"
>> $ echo "The Mask of Dimitrios" >> books.txt
>> $ git add books.txt
>> $ git commit -m "added third book"
>> 
>> #To navigate back two commits
>> $ git checkout HEAD~2
>> $ cat books.txt
>> ~~~
>> {: .bash}
> {: .solution}
{: .challenge}

> ## Reminder
> We can always check on the state of the git repository with `git status`. This will show which
> files are untracked, which have changed, and whether they are staged or not. 
{: .callout}

## Working with remotes

- We can have a *local* copy of a repository (on the disk of the computer we're using), and/or a
*remote* copy of a repository on a git hosting service like github or bitbucket.
- To make a local copy from a remote repository, use `git clone [repo-address]`
- To bring changes from a remote to the local repository, use `git pull`
- To send local changes to the remote, use `git push`

> ## Challenge 2 
> Clone a local copy of the [git-guacamole repo](https://github.com/afdataschool/git-gaucamole). 
> Make sure you think about where you should make your local copy.
>
> - How many commits have been made in the repository?
> - Who made them?
> - Was `instructions.txt` or `ingredients.txt` created first?
>
>> ## Solution
>> 
>> Make sure you are not inside a git repository when you do `git clone`
>> ~~~
>> $ git clone https://github.com/afdataschool/git-guacamole
>> $ cd git-guacamole
>> $ git log
>>
>> # One possible solution:
>> $ git log --oneline | wc -l
>> ~~~
>> {: .bash}
> {: .solution}
{: .challenge}

> ## Resources
> 
> - A great visual way to play with git concepts: [http://git-school.github.io/visualizing-git/](http://git-school.github.io/visualizing-git/)
> - A git cheat sheet [https://services.github.com/on-demand/downloads/github-git-cheat-sheet.pdf](https://services.github.com/on-demand/downloads/github-git-cheat-sheet.pdf)
> - A game based intro to git: [https://learngitbranching.js.org/](https://learngitbranching.js.org/)
{: .callout}


{%include links.md %}
