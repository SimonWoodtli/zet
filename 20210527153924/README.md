# How can you create a repo on  Gitlab from the CLI?

1. IF you don't have a group yet you know you want to put it in, go on the website and create a new group
1. create your local repo:
    1. put your files in it
    1. `git init`
    1. `git add .`
    1. `git commit -m "initial commit"`
1. Now you can push this local repo on to Gitlab and create a project there automatically via SSH or HTTP
    1. git push SSH: `git push --set-upstream git@gitlab.example.com:namespace/nonexistent-project.git master`
    1. or git push HTTP: `git push --set-upstream https://gitlab.example.com/namespace/nonexistent-project.git master`
    1. and finally: `git remote add origin https://gitlab.example.com/namespace/nonexistent-project.git`
namespace= name of your group
