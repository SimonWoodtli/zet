# Add Tags to Git Repo: Create locally and push to remote

> 🧐 Tags only applky to the current branch and are directly linked to the
commit SHA1 hash. So basically tags just give a commit ID a name.

## When to use annotated tags vs lightweight tags? 

* Annotated tags are those tags meant to be published for other developers,
  most probably new versions (which should also be signed). Not only to see who
  tagged and when it was tagged, but also why (usually a changelog).
* Lightweight are more appropriate for private use, that means tagging special
  commits to be able to find them again. May it be to review them, check them
  out to test something or whatever.

So to tag you have these options:

* signed tag (public,prefer): `git tag -s -m"Here goes description of Tag" v0.1`
* tag with msg, annotated (public): `git tag -a -m"Here goes description of Tag" v0.1`
* lightweight tag (private): `git tag v0.1`

> 🧐 A signed tag also requires some setup like gpg key and .gitconfig settings, see signed commit Zettel

## Tag on dev/test branches

> ⚠️  git tags are unique. So if you create a git tag on a dev/test branch the
same name you gave cannot be used on another branch again.

1. `git add .` and `git commit`
1. `git tag ...` with one of the commands from above 
1. if the branch already exists on remote: `git push --follow-tags`
1. if the branch needs to be created on remote:
`git push --set-upstream origin yourBranchName --follow-tags`

## Tag on main branch

1. `git checkout main`
1. `git tag ...` with one of the commands from above
1. `git merge dev`
1. `git push --follow-tags`

TODO: With pull request?

Related:

* [20230101162943](/20230101162943/) Create signed commits on Git Repos
* [20230101142814](/20230101142814/) gh cli: Create and manage git releases on github.com
* manage releases
* create signed commits
* <https://docs.github.com/en/authentication/managing-commit-signature-verification/signing-tags>

Tags:

    #git #github #tags #releases
