# Git remove big files from your git history after you commited and pushed it

So it can happen that a big file enters your git repo by accident. Or maybe by choice. Like for me today I forgot that the github.com website can now upload images/videos into README.md by dragging them. Which in turn creates an URL like 'https://user-images.githubusercontent.com/66033447/209848299-2d3f7c8b-7ee6-4044-b7ef-61a54c78d10b.webm'. However instead I choose to put the screencast video from `cmd-zet` repo inside the repo itself like I use to do before this feature existed. Well obviously that bloats up an otherwise very small text only repo. I was just not thinking for a minute until it was too late.

Now of course I already commited and pushed the development branch. I even merged it into the main branch. 
What I didn't do is remove the file and make new commit because that is just pointless as it would still remain in your git history.

So how can we remove this file?

1. to see your big file this command really helps:

```
git rev-list --objects --all \
| git cat-file --batch-check='%(objecttype) %(objectname) %(objectsize) %(rest)' \
| awk '/^blob/ {print substr($0,6)}' \
| sort --numeric-sort --key=2 \
| cut --complement --characters=13-40 \
| numfmt --field=2 --to=iec-i --suffix=B --padding=7 --round=nearest
```

Both of these command required me to force them with the -f flag:  

2. Now to delete it I used `git filter-branch -f --tree-filter 'rm -f assets/screencast.webm' HEAD`
2. However I am pretty sure this would also work: `git filter-branch -f --index-filter 'git rm --cached --ignore-unmatch assets/screencast.webm' HEAD`

Now I did this first on the development branch and than I forced a push. After that I switched to main and did the same thing there.
However things got a bit messy after that so I decided to remove the local repo and clone it again from github.
After that it still showed me the video file if I used the `git rev-list ...` command from above. And a `git graph` on the dev branch showed very a messed up order of commits it sorted all the merges and actual commits on the branch. So I decided to manually delete the 'dev' branch from the github website. Then I removed the local repo again and cloned it one more time. After that the file was gone. I am not sure if this last part was me being inpatient and if I waited a little it would have removed the video file or what. Because I don't know what deleting this branch in this case actually did.

3. remove local repo, go to github website and delete branch where you first introduced the big file, clone the repo again to your machine

Related:

* <https://stackoverflow.com/questions/2100907/how-to-remove-delete-a-large-file-from-commit-history-in-the-git-repository>

Tags:

    #git #github #file #big #delete #history
