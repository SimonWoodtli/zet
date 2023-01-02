# Create signed commits on Git Repos

## Setup

1. create key: `gpg --full-generate-key`
1. get key id: `gpg --list-keys`
1. add key to gitconfig: `git config --global user.signingkey <key-id>`
1. export key and add it on github.com website: `gpg --armor --export <key-id>`

Pick either one of these methods:

1. use `git commit -S` if you just want your current commit signed
1. use `git config --global commit.gpgsign true` to enable signed commits for
   every repo, use `git commit -S` to disable it for the current commit
1. use `git config --local commit.gpgsign true` to enable signed commits for
   current repo

Related:

* [20221109013131](/20221109013131/) Add Tags to Git Repo: Create locally and push to remote
* [20230101142814](/20230101142814/) Create and manage Git releases on GitHub.com with gh cli
* <https://docs.github.com/en/authentication/managing-commit-signature-verification/signing-commits>

Tags:

    #github #git #signed #gpg
