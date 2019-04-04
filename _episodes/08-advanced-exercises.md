---
title: "Advanced exercises"
teaching: 0
exercises: 0
questions:
- "Can I stretch myself gitwise?"
objectives:
- "Try out some resets"
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

