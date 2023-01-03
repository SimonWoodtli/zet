# Zettelkasten: Markdown Guideline

## General Rules

Generally speaking use the CommonMark guideline but extent it with these rules.

* Title max. 50 characters wide (so git commit message works too)
* Wrap everything but references block to 79 columns
* Substantial code, raw, and images should be in separate files
* All reference links must be explicit and visible
* Begin block quotes with a semantic emoji (> 🧐)
* Try not too use too many embedded links but prefer to put them in 'Related'
  section at the end
* Only use screenshots when necessary, if you can describe something with words
  you should. Otherwise it will bloat your zet repo over time.
* No HTML allowed in any way
* No spaces between opening fence token and keyword
* Use three backticks for fenced content
* Only use three tildes if fenced content contains three backticks
* Only use four tildes if fenced content contains three tildes
* Use two or more trailing spaces for hard breaks
* Avoid horizontal rule like these: `----`
* Only `*`,`**`,`***` for italic, bold, bold italic
* Only `1.` for ordered lists
* Only `* ` for unordered lists
* Table block must begin with `|`
* If a block begins with `|` it *must* be a table.

## 1. Quote Blocks: Hints, Notes, Important etc.

### When

If you have some 'additional' info in a section/paragraph. Which relates to the
section/title/paragraph and can be described briefly.

### How

* Quoted blocks start with a angle bracket at the beginning
* Idea: `> 💡` 
* Note: `> 📝`
* Hint/Advice: `> 🧐`
* Thought/Thinking: `> 🤔`
* Important/Warning: `> ⚠️ `
* Mistake: `> 🤦`
* Search/Research: `> 🔎`
* Book Related, Quotes/Info: `> 📚`

### Example

```markdown
> 🧐 Don’t forget to create Hints and Notes with a > at the beginning.
```

output:

> Hint: Don't forget to create Hints and Notes with a > at the beginning.

## 2. Embedded Links - Reference Links

### When?

* If it's an external link to another website mentioned inside a text block.
* If it's a search for a keyword on your githubs zet repo mentioned inside a 
text block.

> Hint: Never embed internal links

### How

If the link is embedded inside of text you can use `[Foo]`. And then put 
all the embedded links between in the Footer `Related:` and `Tags:`

* To get the github search of a keyword within vim: `!!zet query [searchterm]`

### Example

```markdown
# Title: Mini Example

This is an example of an embedded Link (reference) that is used to refer to a website or a search on your zets github repo.

* external link: If you want to know more about [basic Pandoc Markdown]
* github search: If you want to see all my [Zettelkasten] related zettels.

Related:

* <https://luhmann.surge.sh>
* [20220205135643](/20220205135643/) Zettelkasten Notes and Hints

[basic Pandoc Markdown]: <https://rwx.gg/lang/md/>
[Zettelkasten]: <https://github.com/SimonWoodtli/zet/search?q=zettelkasten>

Tags:

    #exampleTag1 #foo
```

## 3. Embedded Terms

### When

A term is a word that requires a definition. It is marked with tripple 
asterisk e.g. ***zettelkasten***. Terms can be added to a glossary or simply 
have additional information.

### How

* `***term***`

### Example

```markdown
And ***Zettelkasten*** can truly empower the way you organize knowledge.
```

output:

And ***Zettelkasten*** can truly empower the way you organize knowledge.

## 4. Tables

### When

I try not to use tables too often but sometimes they are great. Especially if
you can no longer easily describe content with a list.

### How

> 📝 Use the table format that is supported by GitHub and (if and when added)
eventually CommonMark. 

* Only use `|` for table blocks

### Example

```markdown
|Name|Description|
-|-
Foo|The foo of it all
Bar|The bar as well
```

## 5. Footer: Related - Reference Links

### When

Links that are related to your current zettel:

* If it's an external link that you don't want to embed in your text.
* If it's an internal link that refers to another zettel inside your zet repo.

### How

* internal link of a zettel within vim: `!!zet link [searchterm]`
* external links: `* <https://duckduckgo.com>`

### Examples

```markdown
* [20220224055145](/20220224055145/) Zettelkasten `zet` commands: cheatsheet
* <https://duckduckgo.com>
```

output:

* [20220224055145](/20220224055145/) Zettelkasten `zet` commands: cheatsheet
* <https://duckduckgo.com>

# 6. Footer: Tags

### When

* Always add tags to your zettels as they make it easier to find them when your
  knowledge base grows.
* Also checkout the zettel about tags.

### How

* Use camelCase to create multi word tags, no whitespace
* Use hastag
* End of document at the very last line
* Indented with 4 spaces
* Tagline max. 79 characters 

> 📝 Don't go overboard with too many tags keep it short and simple. Limiting
the tag length has the benefit of choosing tags more carefully and avoids
redundant tags or duplicants.

### Example

* Checkout the markdown example in '2. Embedded Links'

Related:

* <https://luhmann.surge.sh>
* <https://rwx.gg/lang/md/>
* <https://raw.githubusercontent.com/rwxrob/zet/815dd6c0039559f130bfa47a5cdac4ef6223a411/20210813154054/README.md>
* <https://raw.githubusercontent.com/rwxrob/zet/a4458625e1707a62c4a7879141e7a4986bc2b3c6/20210502045853/README.md>
* <https://raw.githubusercontent.com/rwxrob/zet/25b3b4f54c05124c6f74aae2a692a739372f7e80/20210902115330/README.md>
* <https://github.com/rwxrob/zet/blob/35c0fd6fd62489eab91df16bae76c85f28a11280/20210813210814/README.md>
* <https://github.com/rwxrob/zet/tree/main/20210502004642>
* <https://github.com/rwxrob/zet/tree/main/20210813153813>
* <https://github.com/rwxrob/zet/blob/7809d84680cec0409838f6d3220af81255e94c4c/20210812154738/README.md>
* [20220205135643](/20220205135643/) Zettelkasten Notes and Hints
* [20220404224249](/20220404224249/) Zettelkasten Tags: 3 Rules to create a knowledge repository

Tags:

    #zettelkasten #guideLine #markdown #knowledgeBase #notes
