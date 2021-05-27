# How to collaborate on a forked repo?

1. fork the repo via website
2. create a branch you wanna do you your work on. Name it so it is clear which issue/topic you work on or by your name or convention the repo has. -> `git checkout -b your_branch_name` # you only need to do this once
3. `git push -u origin your_branch_name` # you only need to do this once
4. `git remote add upstream https://github.com/c-h-o-c-h-o/100devs.git` # you only need to do this once


to pull from a collaborated repo: `git pull upstream master`
rest is normal workflow from here on:
1. add your files and then `git add .`
2. `git commit -m "commit message"`
3. `git push`
4. make a pull request via website
