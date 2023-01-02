# Create and manage Git releases on GitHub.com with gh cli

> 🧐 Before you can publish a release you need to tag a commit message. See
'Add Tags to Git Repo'

* create: `gh release create`
* create draft: `gh release create --draft`
* publish draft: `gh release edit <tagName> --draft=false`
* remove release: `gh release edit <tagName> --draft=true` (better than delete)
* edit: `gh release edit --flag <tagName>`
* list: `gh release list`

You can only ever have one release connected to a tag. If you choose the same
tag for another release the old one gets over written.

Related:

* [20230101162943](/20230101162943/) Create signed commits on Git Repos
* [20221109013131](/20221109013131/) Add Tags to Git Repo: Create locally and push to remote
* <https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository>
* <https://cli.github.com/manual/gh_release>
