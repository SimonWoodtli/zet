# Perl Substitute Multiline: Caveats

> 🧐 perl is hard, but perl is powerful!

* basic perl replace: `perl -i -0777pe "s/searchpattern/replacepattern/s" file`

0777 = slurp mode, meaning the file that gets read will be put on a single line so the whole content is just one line.
-i   = write to file
-p   =
-e   =

> 📝 As of yet I haven't figured out how to do multi line replacements that don't involve slurp mode.

Example
```bash
#!/usr/bin/bash
#foo='hello world'
foo='<h1>Hello World</h1>'
#perl -i -0777pe "s/<!--languages-start-->\n<!--languages-end-->/<!--languages-start-->\n${foo}\n<!--languages-end-->/s" input-fileXXX
#perl -i -0777pe "s|<!--languages-start-->\n<!--languages-end-->|<!--languages-start-->\n${foo}\n<!--languages-end-->|s" input-fileXXX
perl -i -0777pe "s|<!--languages-start-->\s*<!--languages-end-->|<!--languages-start-->\n${foo}\n<!--languages-end-->|s" input-fileXXX
```
 
Output: The last perl command works indented or not and with foo being html
content too.

```bash
safd
asdf

<!--languages-start-->
<h1>Hello World</h1>
<!--languages-end-->
```

Of course keeping the html tags in your output might not always be desired.
If you don't want to keep the tags:
`perl -i -0777pe "s|<!--languages-start-->\s*<!--languages-end-->|${foo}|s" input-fileXXX`

The input-file are simplified but I threw some pretty hard stuff at it and perl
handles it like a charm. Look at my readme-scraper/readme-writer project for
a better understanding of what perl can do.

input-file1:

```bash
safd
asdf

<!--languages-start-->
<!--languages-end-->
```

input-file2

```
safd
asdf

  <!--languages-start-->
  <!--languages-end-->
```

input-file3

```
safd
asdf

  <!--languages-start-->
<!--languages-end--> 
```

## Problems and Solutions explained

Problem 1: The first perl command in the example uses / as a delimiter. If you
only replace a simple 'hello world' string this is not a  problem. But if you
want to replace something wrapped with html tags, then the closing / in `</h1>`
will get interpreted as a delimiter. Well that's no good.

Solution 1: Just like in sed you can actually set any delimiter with perl as
well: So the second perl command fixes that using `|` as a delimiter.

Problem 2: If your input file is indented, you have white space at the
beginning. If you use `"s|<!-...-->\n<!-...-->|..."` the `\n` will tell the
perl command to look for a line that starts with an html comment followed by
a linebreak `\n` followed directly by another html comment. The start html
comment can be indented, as it looks through the file starting with this, but
then coding a `\n` immediately followed with another html comment means that it
would only look for a structure like input-file3. 

Solution 2: Use the third perl command with `\s*` instead of `\n`. In regex
`\s` refers to white space and * to any white space.

Related:

* <https://github.com/SimonWoodtli/readme-scraper>

Tags:

    #text #manipulation #multistring #perl #substitute
