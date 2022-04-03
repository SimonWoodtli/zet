# Linux Regular expressions 

```sh
#!/bin/sh
# see also `man grep`, `man 7 regex`, and `man 7 glob`

## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
## REGEX
## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Regex
# ~~~
# Regex is inspired by regular expressions in automata theory
# There are three regex variations implemented in unix systems
#   Basic Regular Expressions (BRE)
#   Extended Regular Expressions (ERE)
#   Perl Compatible Regular Expressions (PCRE)
# Regex can be converted to finite automata to run in linear time (except PCRE)
# Regex is different than glob
#   Glob is simpler whereas regex is richer
#   Glob is for filenames whereas regex is for arbitrary text
#   Glob is used in shell whereas regex is used in text processing tools

# all words containing 'x'
grep 'x' /usr/share/dict/words
# Acrux
# Acrux's
# Ajax
# ...
# xylophonist
# xylophonist's
# xylophonists

# all words containing 'xy'
grep 'xy' /usr/share/dict/words
# Oxycontin
# Oxycontin's
# Roxy
# ...
# xylophonist
# xylophonist's
# xylophonists

# all words containing 'xy' or 'yz' (BRE)
grep 'xy\|yz' /usr/share/dict/words
# Byzantine
# Byzantine's
# Byzantines
# ...
# xylophonist
# xylophonist's
# xylophonists

# all words containing 'xy' or 'yz' (ERE)
grep -E 'xy|yz' /usr/share/dict/words
# Byzantine
# Byzantine's
# Byzantines
# ...
# xylophonist
# xylophonist's
# xylophonists

# all words containing 'xya' or 'yza'
grep -E '(xy|yz)a' /usr/share/dict/words
# Byzantine
# Byzantine's
# Byzantines
# Byzantium
# Byzantium's
# oxyacetylene
# oxyacetylene's

# all words with a vowel followed by 'xy'
grep -E '[aeiou]xy' /usr/share/dict/words
# Roxy
# Roxy's
# apoplexy
# ...
# sexy
# taxying
# waxy

# all words with a vowel followed by 'xy', any character, and then 's'
grep -E '[aeiou]xy.s' /usr/share/dict/words
# Roxy's
# apoplexy's
# epoxy's
# galaxy's
# heterodoxy's
# orthodoxy's
# pixy's
# proxy's

# ... any character or none ...
grep -E '[aeiou]xy.?s' /usr/share/dict/words
# Roxy's
# apoplexy's
# epoxy's
# galaxy's
# heterodoxy's
# orthodoxy's
# paroxysm
# paroxysm's
# paroxysms
# pixy's
# proxy's

# ... any number of characters ...
grep -E '[aeiou]xy.*s' /usr/share/dict/words
# Roxy's
# apoplexy's
# epoxy's
# galaxy's
# heterodoxy's
# orthodoxy's
# oxyacetylene's
# oxygen's
# oxygenates
# oxygenation's
# oxymoron's
# oxymorons
# paroxysm
# paroxysm's
# paroxysms
# pixy's
# proxy's

# ... at least one character ...
grep -E '[aeiou]xy.+s' /usr/share/dict/words
# Roxy's
# apoplexy's
# epoxy's
# galaxy's
# heterodoxy's
# orthodoxy's
# oxyacetylene's
# oxygen's
# oxygenates
# oxygenation's
# oxymoron's
# oxymorons
# paroxysm's
# paroxysms
# pixy's
# proxy's

# ... two characters ...
grep -E '[aeiou]xy.{2}s' /usr/share/dict/words
# paroxysms

# ... two or more characters ...
grep -E '[aeiou]xy.{2,}s' /usr/share/dict/words
# oxyacetylene's
# oxygen's
# oxygenates
# oxygenation's
# oxymoron's
# oxymorons
# paroxysm's
# paroxysms

# ... at most three characters ...
grep -E '[aeiou]xy.{,3}s' /usr/share/dict/words
# Roxy's
# apoplexy's
# epoxy's
# galaxy's
# heterodoxy's
# orthodoxy's
# paroxysm
# paroxysm's
# paroxysms
# pixy's
# proxy's

# ... a number of characters between one and three ...
grep -E '[aeiou]xy.{1,3}s' /usr/share/dict/words
# Roxy's
# apoplexy's
# epoxy's
# galaxy's
# heterodoxy's
# orthodoxy's
# paroxysm's
# paroxysms
# pixy's
# proxy's

# ... any number of characters followed by dot, comma, or apostrophe ...
grep -E "[aeiou]xy.*[.,']s" /usr/share/dict/words
# Roxy's
# apoplexy's
# epoxy's
# galaxy's
# heterodoxy's
# orthodoxy's
# oxyacetylene's
# oxygen's
# oxygenation's
# oxymoron's
# paroxysm's
# pixy's
# proxy's

# ... followed by a punctutation character ...
grep -E '[aeiou]xy.*[[:punct:]]s' /usr/share/dict/words
# Roxy's
# apoplexy's
# epoxy's
# galaxy's
# heterodoxy's
# orthodoxy's
# oxyacetylene's
# oxygen's
# oxygenation's
# oxymoron's
# paroxysm's
# pixy's
# proxy's

# ... followed by a non-punctutation character ...
grep -E '[aeiou]xy.*[^[:punct:]]s' /usr/share/dict/words
# oxygenates
# oxymorons
# paroxysms

# ... followed by a character in range from 'a' to 'n' ...
grep -E '[aeiou]xy.*[abcdefghijklmn]s' /usr/share/dict/words
# oxygenates
# oxymorons
# paroxysms

# ... followed by a character in range from 'a' to 'n' ...
grep -E '[aeiou]xy.*[a-n]s' /usr/share/dict/words
# oxygenates
# oxymorons
# paroxysms

# ... followed by a character NOT in range from 'a' to 'n' ...
grep -E '[aeiou]xy.*[^a-n]s' /usr/share/dict/words
# Roxy's
# apoplexy's
# epoxy's
# galaxy's
# heterodoxy's
# orthodoxy's
# oxyacetylene's
# oxygen's
# oxygenation's
# oxymoron's
# paroxysm's
# pixy's
# proxy's

# all words starting with a vowel followed by 'xy'
grep -E '^[aeiou]xy' /usr/share/dict/words
# oxyacetylene
# oxyacetylene's
# oxygen
# oxygen's
# oxygenate
# oxygenated
# oxygenates
# oxygenating
# oxygenation
# oxygenation's
# oxymora
# oxymoron
# oxymoron's
# oxymorons

# all words with a vowel followed by 'xy' at the end of the word
grep -E '[aeiou]xy$' /usr/share/dict/words
# Roxy
# apoplexy
# epoxy
# foxy
# galaxy
# heterodoxy
# orthodoxy
# pixy
# proxy
# sexy
# waxy

# all words starting with a vowel and ending with 'xy'
grep -E '^[aeiou].*xy$' /usr/share/dict/words
# apoplexy
# epoxy
# orthodoxy

## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
## SPECIAL ESCAPES
## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# 'o' followed by a space and then 'w' (POSIX)
printf 'hello world' | grep -E 'o[[:space:]]w'
# hello world

# 'o' followed by a space and then 'w' (GNU extension -- non-POSIX)
printf 'hello world' | grep -E 'o\sw'
# hello world

# 'e' followed by a number of non-space and then 'o' (POSIX)
printf 'hello world' | grep -E 'e[^[:space:]]*o'
# hello world

# 'e' followed by a number of non-space and then 'o' (GNU extension -- non-POSIX)
printf 'hello world' | grep -E 'e\S*o'
# hello world

# exact word 'two' (GNU extension -- non-POSIX)
printf 'one two three' | grep -E '\<two\>'
# one two three

# word starting with 'wo' (GNU extension -- non-POSIX)
printf 'one two three' | grep -E '\<wo'
# (does not match)

# word starting with 'wo' (GNU extension -- non-POSIX)
printf 'one two three' | grep -E '\bwo'
# (does not match)

# pattern 'wo'
printf 'one two three' | grep -E 'wo'
# one two three

# word ending with 'tw' (GNU extension -- non-POSIX)
printf 'one two three' | grep -E 'tw\>'
# (does not match)

# word ending with 'tw' (GNU extension -- non-POSIX)
printf 'one two three' | grep -E 'tw\b'
# (does not match)

# pattern 'tw'
printf 'one two three' | grep -E 'tw'
# one two three

# c function with empty arguments (POSIX)
printf 'C_Function_4_Demonstration()' | grep -E '\s*[_[:alnum:]]*\(\s*\)'
# C_Function_4_Demonstration()

# c function with empty arguments (GNU extension -- non-POSIX)
printf 'C_Function_4_Demonstration()' | grep -E '\s*\w*\(\s*\)'
# C_Function_4_Demonstration()

## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
## BACK REFERENCES
## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# a pattern followed by any number of characters and then the same pattern
grep -E '([a-f][aeiou][a-f]).*\1' /usr/share/dict/words
# Deadhead
# Deadhead's
# deficiencies
# inefficiencies

# NOTE: Back references do not necessarily run in linear time

# Extra
# ~~~
# Following example demonstrates a catastrophic backtracking case.
#
# grep (1) implements basic regular expressions using a Thompson NFA engine
#
# perl (1) implements the more powerful Perl Compatible Regular Expressions
# (PCRE) using backtracking
#
# https://swtch.com/~rsc/regexp/regexp1.html
#
# run the following examples in grep and perl and compare their performances
echo 'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa' | grep -E            'a?a?a?a?a?a?a?a?a?a?a?a?a?a?a?a?a?a?a?a?a?a?a?a?a?a?a?a?a?a?aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa'
echo 'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa' | perl -ne 'print if /a?a?a?a?a?a?a?a?a?a?a?a?a?a?a?a?a?a?a?a?a?a?a?a?a?a?a?a?a?a?aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa/'
```

Related:

* <https://gokcehan.github.io/>

Tags:

    #linux #regularExpressions #commands #cheatsheet
