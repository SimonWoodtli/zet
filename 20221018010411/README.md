# When to use grep, sed, awk or perl

### grep

* Use grep to search through a file. 

### sed

* Use sed to edit a file.
* Use sed over awk when you mainly want to deal with lines (such as delete or add lines of text or single line substitution)
* Don't use sed if the replacement string for your substitution contains multiple lines (new lines have to be escaped and it's not very sleek)

#### ed and printf

* Use for multi line substitution 

Simply write out your data into a buffer file and then use a `printf` piped into `ed` to edit the file of choice.

This looks for the foo-start html comment and when it finds it it prints the data in your buffer-file into the file stored in the \$file variable. This also allows for multiline inserts and even characters that might interfere with sed such as newlines will work.
* command: `printf "%s\n" "/<!--foo-start-->/r buffer-file" w  | ed -s $file`

Of course it would be more elegant to use perl and get this probably done with a one-liner without a buffer file. However I still have to learn more about perl.

### awk

I am tempted not to learn awk and only know the very basics of it. I'd rather spend time to learn perl.

* Use awk over grep and sed when the file you want to process has some kind of structure (such as columns). 

### perl

Can do all of the above and more. The text editing language period.

Related:

* <https://unix.stackexchange.com/questions/303044/when-to-use-grep-less-awk-sed>
* <http://www.wellho.net/mouth/3902_Shell-Grep-Sed-Awk-Perl-Python-which-to-use-when-.html>
* <https://stackoverflow.com/questions/67929263/inserting-contents-of-one-text-file-into-another-in-bash>

Tags:

    #linux #cli #terminal #shell #commandLine #unix #tools #textProcessing #textEditing #textManipulation #findReplace
