# Setup local Git Repo with remote backup

## Setup Repo

1. On local dir:

```bash
git init
git add .
git commit -m "initial commit"
```

2. On backup dir:

```bash
git init --bare projectFoo.git
```

3. On local dir:

```bash
git remote add origin ~/mnt/of/backupdir/projectFoo.git
git push -u origin main
```

Use `$ git push --mirror origin` to push any new commits to the backup repo.

## Recover/Clone Repo from backup

Simply clone it: `git clone ~/mnt/of/backupdir/projectFoo.git`
Then check to see if remote backup is still up `git remote -v` if not: `git remote add origin ~/mnt/of/backupdir/projectFoo.git`

also make sure the .git/hooks/pre-commit is deleted -> to allow commits on main branch

## Setup multiple remotes

You can also setup a repo which pushes to multiple remotes either with just one push like `git push`. Or If you want to you could do the same with `git push origin` and another push with `git push remote2`.

* To have two remotes that need to be pushed seperatly is easy just `git remote add remote2 /place/where`.

* Two remotes that push at the same time:
1. on the remote place: `git init --bare foo.git`
2. on the local place: `git remote add origin /place/where/foo.git`
3. on the local place: `git remote set-url --add --push origin /place/where/foo.git`
4. on the local place: `git remote set-url --add --push origin /another/place/where/foo.git`
5. initialize with first push `git push origin` later only `git push` needed
