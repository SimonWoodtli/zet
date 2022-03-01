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
