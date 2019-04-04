---
title: "Undoing things"
teaching: 10
exercises: 10
questions:
- "How can I undo things from git history?"
objectives:
- "Learn to undo changes safely"
- "See when undone changes are permanently deleted and when they can be retrieved"
keypoints:
- "`git revert` is a safe method to undo changes by adding a **new commit**"
- "`git amend` can add to the previous commit, but changes history"
- "`git checkout [filename]` can permanently remove uncommitted changes"
---

## Undoing things

- Commits that are part of any branch will not get lost.
- Files which were added and later removed can always be recovered.
- In git we can modify, reorder, squash, and remove commits and these actions can also be undone.
- Some commands can permanently delete **uncommitted** changes. In doubt always commit first.
- Some commands **modify history**. This is OK for local commits but may not be OK for commits 
shared with others. So if the commits have been pushed, **don't rewrite history**.


## Reverting commits

- Let's add an onion to the `ingredients.txt` file.

~~~
$ echo "* 1 onion" >> ingredients.txt
$ git add ingredients.txt
$ git commit -m "Add an onion to the recipe"
~~~
{: .bash}

It turns out the onion **did not** improve the guacamole. Let's undo it. 

First, let's have a look at the history.

~~~
$ git log --oneline

295b424 Add an onion to the recipe
53f42b7 Added salt to ingredients
1fea6fd Added instructions file
1805665 added txt file extension to ingredients
db9b3a9 Initial commit - added ingredients
~~~
{: .bash}

A safe way to undo the commit is to revert the commit with `git revert`:

~~~
$ git revert 445309b
# or
$ git revert HEAD
~~~
{: .bash}

This creates a **new commit** that does the opposite of the reverted commit.
The old commit remains in the history:

~~~
$ git log --oneline

40a87b4 Revert "Add an onion to the recipe"
295b424 Add an onion to the recipe
53f42b7 Added salt to ingredients
1fea6fd Added instructions file
1805665 added txt file extension to ingredients
db9b3a9 Initial commit - added ingredients
~~~
{: .bash}

> ## Challenge 3
>
> - Create a commit.
> - Revert the commit with `git revert`.
> - Inspect the history with `git log --oneline`.
> - Now try `git show` on both the reverted and the newly created commit.
{: .challenge}

## Adding to the previous commit

Sometimes we commit but realize we forgot something.
We can amend to the last commit:

~~~
$ git commit --amend
~~~
{: .bash}

This can also be used to modify the last commit message.

> ## IMPORTANT
> Note that this **will change the commit hash**. This command **modifies the history**.
> This means that we **NEVER** use this command on commits that we have shared with others.
{: .callout}

