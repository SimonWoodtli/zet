# When to use grep, sed, awk or perl

### grep

* Use grep to search through a file. 

### sed

* Use sed to edit a file. 
* Use sed over awk when you mainly want to deal with lines (such as delete or add lines of text). 
* Use sed to if the replacement string is single line.
* Don't use sed if the replacement string is multiline - it's possible but other tools are easier (problem being you have to escape each line)

### awk

I am tempted not to learn awk and only know the very basics of it. I'd rather spend time to learn perl.

* Use awk over grep and sed when the file you want to process has some kind of structure (such as columns). 

### perl

Can do all of the above and more. The text editing language period.

Related:

* <https://unix.stackexchange.com/questions/303044/when-to-use-grep-less-awk-sed>
* <http://www.wellho.net/mouth/3902_Shell-Grep-Sed-Awk-Perl-Python-which-to-use-when-.html>

Tags:

    #linux #cli #terminal #shell #commandLine #unix #tools #textProcessing #textEditing #textManipulation #findReplace
