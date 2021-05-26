# bboost20: pandoc

Pandoc Markdown, World's Best for Knowledge Source

##  Thursday, June 11, 2020, 9:53:00AM

### Pandoc Markdown

***HINTS***
* Markdown is designed with HTML generation in mind. However Pandoc Markdown isn't. It is designed to for multiple output formats.
* 1 paragraph always on one line. Don't use other techniques that allow you to do other stuff. It is a no go.
* only use 4 dashes to get a horizontal rule. This way you can always find your rules in any document if you search for them. 3 dashes indicate the beginning of a yaml front matter/section.
* three things rob loves about pandoc markdown: attributes, divs and tables (+bracketed spans)

#### header

* you can use hyperlinks in your headers, but rob suggests not to do. Because it will be less problematic to copy/paste into a blog.  *emphasis* is ok or if you want to link the whole header that is fine too.
* header_attributes are a cool extension. With curly brackets  after a header title like # Header1 {#foo}. Creates an ID-Tag/Equal if it gets rendered in HTML. Can also create .class to make classes. Use them!
* header_attributes are also super important. Because if you refer to a title and use [Titlename](#foo) to refer to it. You can always change the title but your link will not break!
* they are by default numbered
* ToC, table of content can be automatically created according to your titles at top of page. (works on any rendered format!)
* don't use reference links. Keep your links together. It is not the same as references! Do inline links instead.

#### block quotes

* use > to quote blocks. e.g. `> This is a block quote`
* indented code blocks require four spaces (or one tab) to make them be verbatim text. DON'T use tabs for indented blocks or better yet not at all in any Markdown file. same goes for python don't use tabs there either, although it is not as strict there. in GO it is totally fine. Because everyone has or can have a different defintion of how many spaces a tab is. default is 4 but that can be changed by anyone. And since this is a document you want to make sure everyone gets to read it the way you want it to look.
* instead of indented code blocks rob suggest to use \``` .... \``` this is called a code fence. Because it is better visible. This idea only applies to if you write code. If you write poetry or just want to make an indented block in your text it is fine.
* code fence can have syntax highlighting if you write right next to the first three backticks the language your code block is written in 3xbacktickjson your json data3xbackticks.

#### definition lists

* : Defintion 1 to that
* rob does not refer to things this way. He suggests to put your definitions/things to refer to in a seperate file and then link to it. This allows you to use this anywhere you want. Not just in the current document and it does not have to be in the same document. This is only a good way if you want to work on one huge document but for coding and blogging it is not. A massive file can also still be created with automation the way rob is doing it.


#### table captions

* useful to name your tables

#### simple table

see pandoc manual for examples
* just use simple tables to create your tables. All the rest is nonsense.
Table: Tablecaption

#### native divs

* a way to put a logical, stylistic grouping of your stuff
* only supported by pandoc

#### fenced divs

* it identifies a div. and gets rendered as a div when you it in HTML. So you can apply styles to a section.
:::: blabla


### Create a blog

Watch Video again 01:20:00, super useful


----
### General Notes
* at 01:20:00 rob explains how to use pandoc markdown to create his website, super useful!
#### Common Practice
1. bb
#### Tips
1. to learn coding: look at someone elses code that does something you wanna do too. Then remake it in your own way or exactly as it is. Or get to understand what it does and set it aside as the "solution" and then make your own and double check with your solution.
