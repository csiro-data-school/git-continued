---
title: "Merging"
teaching: 30
exercises: 15
questions:
- "How to combine the changes of parallel tracks of work?"
objectives:
- "Be able to merge branches."
keypoints:
- "Branches can be brought back together while maintaining their independent history"
---

## Merging branches

**If you got stuck in the branching exercises**:

- **Skip this unless you got stuck**.
~~~
cd ..
git clone https://github.com/afdataschool/git-guacamole-branched
cd recipe-branching
git log --all --graph --oneline 
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
git log --all --graph --oneline 

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
$ git log --all --graph --oneline 

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
$ git log --all --graph --oneline 

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


