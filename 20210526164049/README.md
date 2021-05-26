# Setup local repo with backup

1. On local dir:

```bash
git init
git add .
git commit -m "initial commit"
```

1. On backup dir:

```bash
git init --bare projectFoo.git
```

1. On local dir:

```bash
git remote add origin ~/mnt/of/backupdir/projectFoo.git
git push -u origin main
```

Use `$ git push --mirror origin` to push any new commits to the backup repo.

## Recover local repo from backup

Simply clone it: `git clone ~/mnt/of/backupdir/projectFoo.git`
Then check to see if remote backup is still up `git remote -v` if not: `git remote add origin ~/mnt/of/backupdir/projectFoo.git`

also make sure the .git/hooks/pre-commit is deleted -> to allow commits on main branch
