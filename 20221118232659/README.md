# Manage and Open Browser Bookmarks from your Terminal

> 🧐 Dependencies: cat, grep, fzf, cut, tr, xclip and [my open script]

1. install all dependencies
2. export your current bookmarks from your browsers into .json or .html
3. cat the exported file and use grep to put only urls into a new file: `cat bookmarks_11_18_22.html | \grep -Eo 'http[^ ]*' | sed 's/.$//' > bookmarksWork`
4. add this `bmw` function to your bashrc:

```bash
bmw() {
  grep -v '^#' "$HOME/Private/bookmarks/bookmarksWork" | fzf --no-preview | cut -d' ' -f1 | tr -d '\n' | xclip -sel clipboard && open $(xclip -o -sel clip)
}
```

What does this function do? It will let you fuzzy find all of your links. Then it saves your selected link  to your clipboard. At the end it will open your default web browser as well as lynx with your selected link.

5. export/add the \$BROWSER env. variable to your bashrc too: `export BROWSER=/usr/bin/brave-browser`  \#adjust path with your default browser
6. furthermore I recommend adding tags at the end of each link so you can also search for links with those tags like so:

```markdown
https://searx.github.io/searx/admin/installation.html #project #idea #search #searchEngine
https://news.ycombinator.com/ #work #news #trends #tech
... and all your other links
```

[my open script]: <https://raw.githubusercontent.com/SimonWoodtli/dotfiles/main/scripts/open>

Tags:

    #linux #terminal #browser #cli #web #bookmark #fzf

