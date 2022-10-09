# Zettelkasten: Markdown Guideline

## 1. Hints and Notes

### When

If you have a Hint or a Note within a section. That relates to the given 
section but is "additional" info.

### How

* `> Hint:` 
* `> Note:`

### Example

```markdown
> Hint: Don’t forget to create Hints and Notes with a > at the beginning.
```

output:

> Hint: Don't forget to create Hints and Notes with a > at the beginning.

## 2. Embedded Links

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

## 4. Footer: Related 

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

# 5. Footer: Tags

### When

* Always add tags to your zettels as they make it easier to find them when your knowledge base grows.
* Also checkout the zettel about tags.

### How

* Create Tags always at the end of the document.
* Use camelCase to create multi word tags
* First letter is #
* Indentation matters

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

    #zettelkasten #guideLine #markdown #knowledgeBase #zettel #zet
