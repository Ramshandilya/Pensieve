# Git 

[Git Cheatsheet](https://gitsheet.wtf/)

## Amend Author Of Previous Commit
The author of the previous commit can be amended with the following command
```
 $ git commit --amend --author "Don Draper <ddraper@sterlingcooper.com>" 
 ```

## Accessing A Lost Commit
If you have lost track of a recent commit (perhaps you did a reset), you can generally still get it back. Run `git reflog` and look through the output to see if you can find that commit. Note the sha value associated with that commit. Let's say it is `39e85b2`. You can peruse the details of that commit with `git show 39e85b2`.

From there, the utility belt that is git is at your disposal. For
example, you can `cherry-pick` the commit or do a `rebase`.

## Caching Credentials

When public key authentication isn't an option, you may find yourself typing your password over and over when pushing to and pulling from a remote git repository. This can get tedious. You can get around it by configuring git to cache your credentials. Add the following lines to the `.git/config` file
of the particular project.

```
[credential]
    helper = cache --timeout=300
```

This will tell git to cache your credentials for 5 minutes. Use a much larger number of seconds (e.g. 604800) to cache for longer.

Alternatively, you can execute the command from the command line like so:

```bash
$ git config credential.helper 'cache --timeout=300'
```

[source](http://git-scm.com/docs/git-credential-cache)

## Renaming A Branch

The `-m` flag can be used with `git branch` to move/rename an existing branch. If you are already on the branch that you want to rename, all you need to do is provide the new branch name.

```bash
$ git branch -m <new-branch-name>
```

If you want to rename a branch other than the one you are currently on, you must specify both the existing (old) branch name and the new branch name.

```bash
$ git branch -m <old-branch-name> <new-branch-name>
```

## Resetting A Reset

Sometimes we run commands like `git reset --hard HEAD~` when we shouldn't have. We wish we could undo what we've done, but the commit we've *reset* is gone forever. Or is it?

When bad things happen, `git-reflog` can often lend a hand. Using
`git-reflog`, we can find our way back to were we've been; to better times.

```bash
$ git reflog
00f77eb HEAD@{0}: reset: moving to HEAD~
9b2fb39 HEAD@{1}: commit: Add this set of important changes
...
```

We can see that `HEAD@{1}` references a time and place before we destroyed our last commit. Let's fix things by resetting to that.

```bash
$ git reset HEAD@{1}
```

Our lost commit is found.

Unfortunately, we cannot undo all the bad in the world. Any changes to tracked files will be irreparably lost.

[source](http://stackoverflow.com/questions/2510276/undoing-git-reset)

## What Changed?

If you want to know what has changed at each commit in your Git history, then just ask `git whatchanged`.

```bash
$ git whatchanged

commit ddc929c03f5d629af6e725b690f1a4d2804bc2e5
Author: jbranchaud <jbranchaud@gmail.com>
Date:   Sun Feb 12 14:04:12 2017 -0600

    Add the source to the latest til

:100644 100644 f6e7638... 2b192e1... M  elixir/compute-md5-digest-of-a-string.md

commit 65ecb9f01876bb1a7c2530c0df888f45f5a11cbb
Author: jbranchaud <jbranchaud@gmail.com>
Date:   Sat Feb 11 18:34:25 2017 -0600

    Add Compute md5 Digest Of A String as an Elixir til

:100644 100644 5af3ca2... 7e4794f... M  README.md
:000000 100644 0000000... f6e7638... A  elixir/compute-md5-digest-of-a-string.md

...
```

This is an old command that is mostly equivalent to `git-log`. In fact, the man page for `git-whatchanged` says:

> New users are encouraged to use git-log(1) instead.

The difference is that `git-whatchanged` shows you the changed files in their raw format which can be useful if you know what you are looking for.

See `man git-whatchanged` for more details.