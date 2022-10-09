# Zettelkasten: Markup Guidelines

## Hints and Notes

If you have a Hint or a Note use "> Hint:" or "> Note:"

example:

```markdown
> Hint: Don’t forget to create Hints and Notes with a > at the beginning.
```

output:

> Hint: Don't forget to create Hints and Notes with a > at the beginning.

## Embedded Links

* If it's an external link to another website mentioned inside a text block.
* If it's a search for a keyword on your githubs zet repo mentioned inside a 
text block.

> Hint: Never embed internal links

If the link is embedded inside of text you can use `[Foo]`. And then put 
all the embedded links between `Related:` and `Tags:`

example:

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

## Embedded Terms

A term is a word that requires a definition. It is marked with tripple 
asterisk e.g. ***zettelkasten***. Terms can be added to a glossary or simply 
have additional information.

## Section: Related 

Links that are related to your current zettel:

* If it's an external link that you don't want to embed in your text.
* If it's an internal link that refers to another zettel inside your zet repo.

* external links: paste the url
* internal links: `[timestamp](/timestamp/) Title of Zettel`

> Hint: use the `....` command to get the link for internal zettels easily

## Section: Tags

* Create Tags always at the end of the document.
* Use camelCase to create multi word tags
* First letter is #
* Indentation matters
* Checkout the markdown example in 'Embedded Links'

Related:

* <https://luhmann.surge.sh>
* <https://raw.githubusercontent.com/rwxrob/zet/815dd6c0039559f130bfa47a5cdac4ef6223a411/20210813154054/README.md>
* <https://raw.githubusercontent.com/rwxrob/zet/a4458625e1707a62c4a7879141e7a4986bc2b3c6/20210502045853/README.md>
* <https://raw.githubusercontent.com/rwxrob/zet/25b3b4f54c05124c6f74aae2a692a739372f7e80/20210902115330/README.md>
* <https://github.com/rwxrob/zet/blob/35c0fd6fd62489eab91df16bae76c85f28a11280/20210813210814/README.md>
* <https://github.com/rwxrob/zet/tree/main/20210502004642>
* <https://github.com/rwxrob/zet/tree/main/20210813153813>
* <https://github.com/rwxrob/zet/blob/7809d84680cec0409838f6d3220af81255e94c4c/20210812154738/README.md>
* [20220205135643](/20220205135643/) Zettelkasten Notes and Hints

Tags:

    #zettelkasten #guideline #markup
