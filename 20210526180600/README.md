# bboost20: markdown

## RWX.GG: Basic Markdown Boost

### Friday, June 12, 2020, 9:15:43AM

### Markdown

* paragraph: one line followed by a blank line
* one star for \*italics*
* two stars for \*\*bold\*\*
* three stars for bold&italic
* never use underscore in markdown
* backticks for monospace/code
* three backticks for codeblocks
* header with # for h1 and ##h2
* only one lvl 1 header per document
* 3 types of hyperlinks: words, URLs, images
* hyperlink word: [name of hyperlinked word](URL, OR id/attribute)
* autolinked URL: <https://rwx.gg> will show you an URL
* add image: ![name of image](place of img) (the ! makes it embedded, it includes picture)
* linked image: [![name of image](place of img)](URL, or id/attribute)
* if you want to add a specific img size you can do that with pandoc light/pandoc fullmarkdown by adding an attribute
* lists: just use 1. item1 1. item2 ... (if u wanna count: use !!wc)
* horizontal rule: 4 dashes
* hard returns/line breaks: ?

here is *a list*

### Markdown editors, tools





### General Notes

* alt text is the text that appears when you hover over it on a website. and then gives you an explanation about what that is.
* if you want to write about markdown code in markdown use three or for tildes tildetildetildemarkdown.
* tables are not supported by basic markdown you need pandoc for that

#### Common Practice

1. don't put HTML inside of a markdown/knowledge file. You can add this by creating a sepearate file and linking it into your knowledge/md file. Because otherwise the two become married and its hard to seperate them and use your knowledge somewhere else.
1. you can add HTML but only if it is an extra in your MD like a widget. But your knowledge is not dependent on it.
1. if you want syntax coloring provide a way to turn it off. For all the people who are colorblind or want to print out your knowledge
1. use three colons for pandoc light and three backticks for fence code and three tildes to mark markdown. Be consistent so it is easy to search content in your files and manipulate it with a parser.
1. don't use reference links, it is better to always write the link for your img, url together in case you wanna copy it to another file

#### Tips

1. The bes thing about markdown: it is not tied to any rendering format. You can always use your knowledge with another rendering be in HTML now, who knows what the language is in 10years for web or apps or what not.
1.
