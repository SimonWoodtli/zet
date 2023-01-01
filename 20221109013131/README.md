# Add Tags to Git Repository

> 🧐 Tags only apply to the current branch and are directly linked to the
commit SHA1 hash. So basically tags just give a commit ID a name.

## Tag on dev/test branches

1. `git add .` and `git commit`
2. `git tag -a -m"Here goes description of Tag" v0.1`
3. `git push --follow-tags`

## Tag on main branch

As you either use pull requests and merges if it's a collaborated repo or just
merges. The main branch should not ever have directly commited changes on it
but only have merge commits from different branches on it.

The Problem: how can I merge and Tag the merge commit?

Tags:

    #git #github #tags #releases
