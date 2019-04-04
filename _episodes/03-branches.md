---
title: "Branching and tags"
teaching: 40
exercises: 15
questions:
- "How can I or my team work on multiple features in parallel?"
- "How can I permanently reference a point in history, like a software version?"
objectives:
- "Be able to create and navigate between branches."
- "Know the difference between a branch and a tag."
keypoints:
- "A branch is a division unit of work, to be merged with other units of work."
- "A tag is a pointer to a moment in the history of a project."
---

## Navigating a git repository more easily

So far we have gone back in our git history by doing checking out a particular commit (eg `git checkout e03af1`). 

After doing that, you will see a message from git that starts with the ominous warning: `You are in 'detached HEAD' state.`

What exactly does that mean? It has to do with something git uses to make navigating repositories much more straightforward. Git uses pointers, which are labels for particular commits in the history. You can think of them as sticky notes, because they can move around.

There is one pointer that *every* git repository has, called `HEAD`. It simply means that commit that is currently checked out (and therefore reflect the state of the files on your local disk).

If you have a look in `git log` you will see the label `HEAD` at the place in history where you are.

We can use these pointers to make life much easier for ourselves. Instead of referring to a particular commit, we can refer to a commit in relation to a label. For example, `HEAD~1` refers to the commit immediately prior to wherever `HEAD` is pointing. `HEAD~2` refers to the commit two commits previous to `HEAD`.

So if we want to checkout a commit that is two previous to where we are, we can run:

`git checkout HEAD~2`

In addition to `HEAD`, git also uses other pointers called branches (we'll look at these in more detail below). By default, every git repository starts with one branch: `master`. 

While using branches enables lots of capability, in themselves they are just labels that point to a specific commit. 

While `HEAD` can move backward and forward around the repository, the `master` label always points to the newest commit made (while the `master` branch is checked out).

### So what is this detached HEAD all about?

A detached `HEAD` means that the `HEAD` label is pointing directly to a commit, rather than a branch label. That means that git won't let you make any commits from where you are, because commits always have to happen on a branch. So to "re-attach" your `HEAD`, it needs to be pointing at a branch label, not a commit.

> ## Challenge
>
> Enter a detached HEAD state in your git repository. See if you can exit the detached state.
>
> ### Hint
> Remember that to be "attached", `HEAD` needs to be pointing at a branch label
>
>> ## Solution
>> We can enter a detached `HEAD` state by
>> ~~~
>> $ git checkout HEAD~1
>> ~~~
>>
>> And re-attach it by checking out the master branch (so that the `HEAD` label is pointing at `master` not a commit)
>> ~~~
>> $ git checkout master
>> ~~~
>> 
> {: .solution}
{: .challenge}


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
- Other than this convention there is nothing special about `master`, it is just a branch (so just a label that points to a commit).
- Commits form a directed acyclic graph (we have left out the arrows to avoid confusion about the time arrow).

A group of commits that create a single narrative are often referred to as a branch, but from git's perspective the branch is really just the label that points to the commit.
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
$ git log --oneline 

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
- Branch `master` points to commit `40a87b4`.
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

We can see which branch we're on (where `HEAD` is poinging) with the output from `git status`.

To see all existing branches, as well as which one we're on use `git branch`:

~~~
$ git branch

* master
~~~
{: .shell}

- This command shows where we are, and which other branches exist, it does not create a branch.
- There is only one branch, `master`, and we are on `master` (`*` represents the `HEAD`).

In the following section we will learn how to create branches, how to switch between them, 
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
>> $ vim ingredients.txt #OR YOUR FAVOURITE EDITOR
>> $ git add ingredients.txt
>> $ git commit -m "A bit less garlic"
>> $ git branch
>> * experiment
>> master
>> $ git log --oneline 
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
> `git log --all --graph --oneline `
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
>> git log --all --graph --oneline 
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
$ git log --all --graph --oneline 

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

And for comparison this is how it looks [on GitHub](https://github.com/csiro-data-school/git-guacamole-branched/network).

## Some branch tips

### Branch naming

- Name your local branches such that you will recognize them 3 months later.
- "test2", "foo", "debug", "mybranch" are not good branch names.
- Give descriptive names to remote branches.
- For topic branches we recommend to name them "author/topic" (example `sarah/new-integrator`).
- Then everybody knows who is to be contacted about this branch (e.g. stale branches).
- Also you can easily find "your" branches:

~~~
$ git branch -r | grep sarah
~~~
{: .bash}

- Name bugfix branches after the issue/ticket (e.g. `issue-137`).
- For release branches we recommend e.g. `release/2.x` or `stable-2.x`.


### Always have only one main development line

- Document where it is (there can be many forks, do not leave doubt about where the main line is).
- Organize branches according to features, not according to groups of people.
- Good: branches `feature-a`, `feature-b`, `feature-c`.
- Bad: branches `stockholm`, `san-francisco`, `helsinki`.
- Reason: `stockholm`, `san-francisco`, and `helsinki` will either diverge
  (three main development lines) or somebody will spend a heroic effort to keep
  them synchronized.


### Every commit on the main development line should build/compile

- Sometimes you want to find a commit in the past that broke some functionality.
- There is no reason to commit broken or unfinished code to the main development line: for this we have branches.

## Tags

- A tag is a pointer to a commit but in contrast to a branch it does not move. In the sticky note analogy it's like a sticky note that's been stapled in place.
- We use tags to record particular states or milestones of a project at a given
  point in time, like for instance versions (have a look at [semantic versioning](http://semver.org),
  v1.0.3 is easier to understand and remember than 64441c1934def7d91ff0b66af0795749d5f1954a).
- There are two basic types of tags: annotated and lightweight.
- **Use annotated tags** (with the `-a` flag) since they contain the author and can be cryptographically signed using
  GPG, timestamped, and a message attached.

Let's add an annotated tag to our current state of the guacamole recipe:

```shell
$ git tag -a nobel-2017 -m "recipe I made for the 2017 Nobel banquet"
```

As you may have found out already, `git show` is a very versatile command. Try this:

```shell
$ git show nobel-2017
```

Have a look at the log to see how your tag looks.

For more information about tags see for example
[the Pro Git book](https://git-scm.com/book/en/v2/Git-Basics-Tagging) chapter on the
subject.

---

## Summary

Let us pause for a moment and recapitulate what we have just learned:

```shell
$ git branch               # see where we are
$ git branch <name>        # create branch <name>
$ git checkout <name>      # switch to branch <name>
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


