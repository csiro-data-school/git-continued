---
title: "Branching and merging"
teaching: 20
exercises: 15
questions:
- "How can I or my team work on multiple features in parallel?"
- "How to combine the changes of parallel tracks of work?"
- "How can I permanently reference a point in history, like a software version?"
objectives:
- "Be able to create and merge branches."
- "Know the difference between a branch and a tag."
keypoints:
- "A branch is a division unit of work, to be merged with other units of work."
- "A tag is a pointer to a moment in the history of a project."
---

## Motivation for branches

In the previous section we tracked a guacamole recipe with git.

Up until now our repository had only one branch with one commit coming
after the other:

![Linear]({{ page.root }}/fig/git-branch-1.svg "Linear git repository")

- Commits are depicted as little boxes with abbreviated hashes.
- The sequence of commits forms a **branch**.
- Here the branch is called "master".
- "HEAD" is the current position. Notice that HEAD is pointing to the "master" label, not the 
actual commit.

Software development, and analytical code, is often non-linear:

- We typically need at least one version of the code to "work" (to compile, to give expected results, ...).
- At the same time we work on new features, or alternative approaches, often several features concurrently.
- Often they are unfinished.
- We need different people to be able to work on different approaches in parallel.
- We need to be able to separate different lines of work really well.

The strength of version control is that it permits the researcher to **isolate
different tracks of work**. Researchers can work on different things and merge
the changes they made to the source code files afterwards to create a composite
version that contains both the changes:

![Git collaborative]({{ page.root }}/fig/git-collaborative.svg)

- We see branching points and merging points.
- Main line development is often called `master`.
- Other than this convention there is nothing special about `master`, it is just a branch.
- Commits form a directed acyclic graph (we have left out the arrows to avoid confusion about the time arrow).

A group of commits that create a single narrative are called a **branch**.
There are different branching strategies, but it is useful to think that a branch
tells the story of a feature, e.g. "fast sequence extraction" or "Python interface" or "fixing bug in
matrix inversion algorithm".

---

## What is a commit?

Before we exercise branching, a quick recap of what we got so far.

We have three commits (we use the first two characters of the commits) and only
one development line (branch) and this branch is called "master":

![]({{ page.root }}/fig/git-branch-1.svg)


~~~
$ git log --oneline --decorate

40a87b4 (HEAD -> master) Revert "Add an onion to the recipe"
295b424 Add an onion to the recipe
53f42b7 (origin/master, origin/HEAD) Added salt to ingredients
1fea6fd Added instructions file
1805665 added txt file extension to ingredients
db9b3a9 Initial commit - added ingredients
~~~
{: .bash}

- Commits are states characterized by a 40-character hash (checksum).
- `git log --oneline` prints abbreviations of these checksums.
- Branches are labels that point to a commit (pointers). In this case "master".
- Branch `master` points to commit `f13051c`.
- `HEAD` is another pointer, it points to where we are right now (currently `master`) 


> ## Challenge
>
> Go to [http://git-school.github.io/visualizing-git/](http://git-school.github.io/visualizing-git/) and create several commits.
> Note the way the master and head pointers move with each commit.
> - What happens when you enter a 'detached HEAD state'?
>
>> ## Solution
>>
>> You can enter a detached HEAD state by checking out a previous commit
>> `git checkout HEAD~1`
>> The HEAD pointer no longer points to a branch pointer, but directly to a commit.
>> This is what a 'detached HEAD' means - HEAD is detached from a branch.
> {: .solution}
{: .challenge}


### Which branch are we on?

To see where we are (where `HEAD` points to) use `git branch`:

~~~
$ git branch

* master
~~~
{: .shell}

- This command shows where we are, it does not create a branch.
- There is only one brach, `master`, and we are on `master` (`*` represents the `HEAD`).

In the following we will learn how to create branches, how to switch between them, 
how to merge branches, and how to remove them afterwards.


## Creating and working with branches

In the [vizualising git environment](http://git-school.github.io/visualizing-git/#free), let's create a branch called `experiment`.

~~~
git branch experiment
~~~
{: .bash}

Notice that there is a new pointer (at the same commit) with the `experiment` label.

We can move `HEAD` to that pointer by using checkout:

~~~
git checkout experiment
~~~
{: .bash}

Notice that `HEAD` is now pointing to `experiment` instead of `master`.

Let's make a commit on `experiment`.

~~~
git commit -m "new commit on experiment branch"
~~~
{: .bash}

The two branches, `master` and `experiment` have started to diverge.

> ## Challenge
> Make some commits on the master branch, and notice the way the graph is growing.
>
>> ## Solution
>>
>> ~~~
>> git checkout master
>> git commit -m "new commit on master"
>> ~~~
>> {: .bash}
> {: .solution}
{: .challenge}

> ## Challenge
> 
> Back in our `git-guacamole` repository, create and switch to a branch called `experiment`
> pointing to the present commit. 
> 
> - Add garlic as a new ingredient **at the top** of the ingredients list, and commit the change.
> - Now make a new commit where you reduce the amount of garlic.
> - Use `git branch` and `git log` to view the branches and commits.
>
>> ## Solution
>> ~~~
>> $ git branch experiment
>> $ git checkout experiment
>> $ echo "* 1 clove crushed garlic" >> ingredients.txt
>> $ git add ingredients.txt
>> $ git commit -m "I wonder if garlic will work"
>> $ vim ingredients.txt
>> $ git add ingredients.txt
>> $ git commit -m "A bit less garlic"
>> $ git branch
>> * experiment
>> master
>> $ git log --oneline --decorate
>> 47d0a9d (HEAD -> experiment) A bit less garlic
>> e1e6f7d I wonder if garlic will work
>> 40a87b4 (master) Revert "Add an onion to the recipe"
>> 295b424 Add an onion to the recipe
>> 53f42b7 (origin/master, origin/HEAD) Added salt to ingredients
>> 1fea6fd Added instructions file
>> 1805665 added txt file extension to ingredients
>> db9b3a9 Initial commit - added ingredients
>> ~~~
>> {: .bash}
> {: .solution}
{: .challenge}


> ## Extra options to `git log`
> You can get a graph view from git log by adding `--graph`. 
>
> Try:
>
> `git log --all --graph --oneline --decorate`
{: .callout}

> ## Challenge
> 
> - Change to the branch `master`.
> - Create another branch called `less-salt` where you reduce the amount of salt.
> - Commit your changes to the `less-salt` branch.
> - View the branches and commits.
>
>> ## Solution
>>
>> ~~~
>> git checkout master
>> git branch less-salt
>> git checkout less-salt
>> vim ingredients
>> git add ingredients.txt
>> git commit -m "reduce the salt"
>> git branch
>>   experiment
>> * less-salt
>>   master
>> git log --all --graph --oneline --decorate
>> * 3db05f9 (HEAD -> less-salt) reduce the salt
>> | * 47d0a9d (experiment) A bit less garlic
>> | * e1e6f7d I wonder if garlic will work
>> |/
>> * 40a87b4 (master) Revert "Add an onion to the recipe"
>> * 295b424 Add an onion to the recipe
>> * 53f42b7 (origin/master, origin/HEAD) Added salt to ingredients
>> * 1fea6fd Added instructions file
>> * 1805665 added txt file extension to ingredients
>> * db9b3a9 Initial commit - added ingredients
>> ~~~
>> {: .bash}
> {: .solution}
{: .challenge}


Here is a graphical representation of what we have created:

![]({{ page.root }}/fig/git-branch-2.svg)

- Now switch to `master`.
- Add and commit the following `README.md` to `master`:

~~~
# Guacamole recipe

Used in teaching git.
~~~
{: .markdown}

Now you should have this situation:

~~~
$ git log --all --graph --oneline --decorate

* 29e2be2 (HEAD -> master) added readme
| * 3db05f9 (less-salt) reduce the salt
|/
| * 47d0a9d (experiment) A bit less garlic
| * e1e6f7d I wonder if garlic will work
|/
* 40a87b4 Revert "Add an onion to the recipe"
* 295b424 Add an onion to the recipe
* 53f42b7 (origin/master, origin/HEAD) Added salt to ingredients
* 1fea6fd Added instructions file
* 1805665 added txt file extension to ingredients
* db9b3a9 Initial commit - added ingredients
~~~
{: .bash}

![]({{ page.root }}/fig/git-branch-3.svg)

And for comparison this is how it looks [on GitHub](https://github.com/bast/recipe/network).


## Merging branches

**If you got stuck in the branching exercises**:

- **Skip this unless you got stuck**.
~~~
cd ..
git clone https://github.com/afdataschool/git-guacomole-branched
cd recipe-branching
git log --all --graph --oneline --decorate
~~~
{: .bash}

It turned out that the experiment with garlic was a good idea.
Our goal now is to merge `experiment` into `master`.

First we make sure we are on the branch we wish to merge **into**:

~~~~
$ git branch

  experiment
  less-salt
* master
~~~
{: .bash}

Then we merge `experiment` into `master`:

~~~
git merge experiment
~~~

![]({{ page.root }}/fig/git-merge-1.svg)

We can verify the result in the terminal:

~~~
git log --all --graph --oneline --decorate

*   393dfaf (HEAD -> master) Merge branch 'experiment'
|\
| * 47d0a9d (experiment) A bit less garlic
| * e1e6f7d I wonder if garlic will work
* | 29e2be2 (origin/master) added readme
|/
| * 3db05f9 (less-salt) reduce the salt
|/
* 40a87b4 Revert "Add an onion to the recipe"
* 295b424 Add an onion to the recipe
* 53f42b7 Added salt to ingredients
* 1fea6fd Added instructions file
* 1805665 added txt file extension to ingredients
* db9b3a9 Initial commit - added ingredients
```

What happens internally when you merge two branches is that git creates a new
commit, attempts to incorporate changes from both branches and records the
state of all files in the new commit. While a regular commit has one parent, a
merge commit has two (or more) parents.

To view the branches that are merged into the current branch we can use the command:

```shell
$ git branch --merged

  experiment
* master
```

We are also happy with the work on the `less-salt` branch. Let us merge that
one, into `master` as well:

~~~
$ git branch  # make sure you are on master
$ git merge less-salt
~~~
{: .bash}

![]({{ page.root }}/fig/git-merge-2.svg)

We can verify the result in the terminal:

```shell
$ git log --all --graph --oneline --decorate

*   95a5f4c (HEAD -> master) Merge branch 'less-salt'
|\
| * 3db05f9 (less-salt) reduce the salt
* |   393dfaf Merge branch 'experiment'
|\ \
| * | 47d0a9d (experiment) A bit less garlic
| * | e1e6f7d I wonder if garlic will work
| |/
* | 29e2be2 (origin/master) added readme
|/
* 40a87b4 Revert "Add an onion to the recipe"
* 295b424 Add an onion to the recipe
* 53f42b7 Added salt to ingredients
* 1fea6fd Added instructions file
* 1805665 added txt file extension to ingredients
* db9b3a9 Initial commit - added ingredients
```

Observe how git nicely merged the changed amount of salt and the new ingredient **in the same file
without us merging it manually**:

~~~
$ cat ingredients.txt

* 1/2 clove garlic
* 2 avocados
* 1 lime
* 1 tsp salt
~~~
{: .bash}

If the same file is changed in both branches, git attempts to incorporate both
changes into the merged file. If the changes overlap then the user has to
manually *settle merge conflicts* (we will do that later).


## Deleting branches safely

Both feature branches are merged:

~~~
$ git branch --merged

  experiment
  less-salt
* master
~~~
{: .bash}

This means we can delete the branches:

~~~
$ git branch -d experiment less-salt

Deleted branch experiment (was 47d0a9d).
Deleted branch less-salt (was 3db05f9).
~~~
{: .bash}

> ## Discussion
> Think about what you expect the graph to look like now. What has been removed?
>
>> ## Solution
>> This is the result:
>>
>> ![]({{ page.root }}/fig/git-deleted-branches.svg)
>>
>> Notice all of the history is still there, just the lables (pointers) have been removed.
> {: .solution}
{: .discussion}

Compare in the terminal:

~~~
$ git log --all --graph --oneline --decorate

*   95a5f4c (HEAD -> master) Merge branch 'less-salt'
|\
| * 3db05f9 reduce the salt
* |   393dfaf Merge branch 'experiment'
|\ \
| * | 47d0a9d A bit less garlic
| * | e1e6f7d I wonder if garlic will work
| |/
* | 29e2be2 (origin/master) added readme
|/
* 40a87b4 Revert "Add an onion to the recipe"
* 295b424 Add an onion to the recipe
* 53f42b7 Added salt to ingredients
* 1fea6fd Added instructions file
* 1805665 added txt file extension to ingredients
* db9b3a9 Initial commit - added ingredients
~~~
{: .bash}

git will not let you delete a branch which has not been reintegrated unless you
insist using `git branch -D`. Even then your commits will not be lost but you
may have a hard time finding them as there is no branch label pointing to them.



### Exercise: encounter a fast-forward merge

- Create a new branch from `master` and switch to it.
- Create a couple of commits on the new branch (for instance edit `README.md`):

![]({{ page.root }}/fig/git-pre-ff.svg)

- Now switch to `master`.
- Merge the new branch to `master`.
- Examine the result with `git graph`.
- Have you expected the result? Discuss what you see.

---

### Optional advanced exercises

These are advanced exercises. Absolutely no problem to postpone them to
few months later.

They make use of the following commands:

```shell
$ git reset --hard <branch/hash>  # rewind current branch to <branch/hash>
                                  # and throw away all later code changes
$ git reset --soft <branch/hash>  # rewind current branch to <branch/hash>
                                  # but keep all later code changes and stage them
$ git rebase <branch/hash>        # cut current branch off and transplant it on top of <branch/hash>
$ git reflog                      # show me a log of past hashes I have visited
$ git checkout -b <branch/hash>   # create a branch pointing to <branch/hash>
```

- Make a few commits to `master`, then realize you committed to the wrong branch,
  branch off and rewind the `master` branch back.
- Delete a branch that is merged, then recreate it.
- Rebase a branch.
- Squash commits that are "at the end".
- Squash a couple of commits except the last one.
- Delete an unmerged branch, then try to recreate it.

---

## Summary

Let us pause for a moment and recapitulate what we have just learned:

```shell
$ git branch               # see where we are
$ git branch <name>        # create branch <name>
$ git checkout <name>      # switch to branch <name>
$ git merge <name>         # merge branch <name> (to current branch)
$ git branch -d <name>     # delete branch <name>
$ git branch -D <name>     # delete unmerged branch
```

Since the following command combo is so frequent:

```shell
$ git branch <name>        # create branch <name>
$ git checkout <name>      # switch to branch <name>
```

There is a shortcut for it:

```shell
$ git checkout -b <name>   # create branch <name> and switch to it
```

### Typical workflows

With this there are two typical workflows:

```shell
$ git checkout -b new-feature  # create branch, switch to it
$ git commit                   # work, work, work, ...
                               # test
                               # feature is ready
$ git checkout master          # switch to master
$ git merge new-feature        # merge work to master
$ git branch -d new-feature    # remove branch
```

Sometimes you have a wild idea which does not work.
Or you want some throw-away branch for debugging:

```shell
$ git checkout -b wild-idea
                               # work, work, work, ...
                               # realize it was a bad idea
$ git checkout master
$ git branch -D wild-idea      # it is gone, off to a new idea
                               # -D because we never merged back
```

No problem: we worked on a branch, branch is deleted, `master` is clean.

---

## Tags

- A tag is a pointer to a commit but in contrast to a branch it does not move.
- We use tags to record particular states or milestones of a project at a given
  point in time, like for instance versions (have a look at [semantic versioning](http://semver.org),
  v1.0.3 is easier to understand and remember than 64441c1934def7d91ff0b66af0795749d5f1954a).
- There are two basic types of tags: annotated and lightweight.
- **Use annotated tags** since they contain the author and can be cryptographically signed using
  GPG, timestamped, and a message attached.

Let's add an annotated tag to our current state of the guacamole recipe:

```shell
$ git tag -a nobel-2017 -m "recipe I made for the 2017 Nobel banquet"
```

As you may have found out already, `git show` is a very versatile command. Try this:

```shell
$ git show nobel-2017
```

For more information about tags see for example
[the Pro Git book](https://git-scm.com/book/en/v2/Git-Basics-Tagging) chapter on the
subject.

---

## Questions

- What is a detached `HEAD`?
- What are orphaned commits?
- What will happen to orphaned commits?
- How can you recover orphaned commits?
