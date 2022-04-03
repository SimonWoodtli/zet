# Linux text processing tools

```sh
#!/bin/sh

## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
## TEXT PROCESSING
## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

## tr (1) - translate or delete characters

# delete 'l' characters
echo 'hello world' | tr -d l
# heo word

# convert 'l' to 'x'
echo 'hello world' | tr l x
# hexxo worxd

# lowercase everything
echo 'Hello World' | tr A-Z a-z
# hello world

# https://en.wikipedia.org/wiki/Caesar_cipher
echo 'hello world' | tr a-z x-za-x
# ebiil tloia

# https://en.wikipedia.org/wiki/ROT13
echo 'hello world' | tr a-z n-za-m
# uryyb jbeyq

# apply rot13 twice to get the original text
echo 'hello world' | tr a-z n-za-m | tr a-z n-za-m
# hello world

# calculate complement DNA sequence
echo 'CCTGCAACTTAGGCAGG' | tr ATGC TACG
# GGACGTTGAATCCGTCC

## cut (1) - remove sections from each line of files

# display first fields separated by ':' in '/etc/passwd'
head /etc/passwd | cut -d: -f1
# root
# daemon
# bin
# sys
# sync
# games
# man
# lp
# mail
# news

# display first and seventh fields separated by ':' in '/etc/passwd'
head /etc/passwd | cut -d: -f1,7
# root:/bin/bash
# daemon:/usr/sbin/nologin
# bin:/usr/sbin/nologin
# sys:/usr/sbin/nologin
# sync:/bin/sync
# games:/usr/sbin/nologin
# man:/usr/sbin/nologin
# lp:/usr/sbin/nologin
# mail:/usr/sbin/nologin
# news:/usr/sbin/nologin

## column (1) - columnate lists

# display a header and list of files
printf "PERM LINKS OWNER GROUP SIZE MONTH DAY "; printf "HH:MM/YEAR NAME\n"; ls -l | sed 1d
# PERM LINKS OWNER GROUP SIZE MONTH DAY HH:MM/YEAR NAME
# drwxr-x--- 2 gokcehan gokcehan 4096 Eki 26 22:02 awk
# -rw-r--r-- 1 gokcehan gokcehan 8120 Eki 27 19:33 regex.sh
# -rw-r--r-- 1 gokcehan gokcehan 2043 Eki 26 22:01 sed.sh
# -rw-r--r-- 1 gokcehan gokcehan 7873 Eki 27 20:01 text.sh

# display a header and list of files in tabular form
(printf "PERM LINKS OWNER GROUP SIZE MONTH DAY "; printf "HH:MM/YEAR NAME\n"; ls -l | sed 1d) | column -t
# PERM        LINKS  OWNER     GROUP     SIZE  MONTH  DAY  HH:MM/YEAR  NAME
# drwxr-x---  2      gokcehan  gokcehan  4096  Eki    26   22:02       awk
# -rw-r--r--  1      gokcehan  gokcehan  8120  Eki    27   19:33       regex.sh
# -rw-r--r--  1      gokcehan  gokcehan  2043  Eki    26   22:01       sed.sh
# -rw-r--r--  1      gokcehan  gokcehan  7873  Eki    27   20:01       text.sh

# display fields separated by ':' in '/etc/passwd' in tabular form
head /etc/passwd | column -t -s:
# root    x  0  0      root    /root            /bin/bash
# daemon  x  1  1      daemon  /usr/sbin        /usr/sbin/nologin
# bin     x  2  2      bin     /bin             /usr/sbin/nologin
# sys     x  3  3      sys     /dev             /usr/sbin/nologin
# sync    x  4  65534  sync    /bin             /bin/sync
# games   x  5  60     games   /usr/games       /usr/sbin/nologin
# man     x  6  12     man     /var/cache/man   /usr/sbin/nologin
# lp      x  7  7      lp      /var/spool/lpd   /usr/sbin/nologin
# mail    x  8  8      mail    /var/mail        /usr/sbin/nologin
# news    x  9  9      news    /var/spool/news  /usr/sbin/nologin

## sort (1) - sort lines of text files

# sort alphabetically line by line
echo -e "aaa\ndd\ncccc\nb" | sort
# aaa
# b
# cccc
# dd

# display disk usage
du -d1 /usr
# 15764   /usr/sbin
# 428     /usr/local
# 21388   /usr/include
# 1680220 /usr/lib
# 867640  /usr/share
# 261852  /usr/src
# 158680  /usr/bin
# 684     /usr/games
# 1432    /usr/libexec
# 3008092 /usr

# sort disk usage numerically
du -d1 /usr | sort -n
# 428     /usr/local
# 684     /usr/games
# 1432    /usr/libexec
# 15764   /usr/sbin
# 21388   /usr/include
# 158680  /usr/bin
# 261852  /usr/src
# 867640  /usr/share
# 1680220 /usr/lib
# 3008092 /usr

# display disk usage in humanized form
du -d1 /usr -h
# 16M     /usr/sbin
# 428K    /usr/local
# 21M     /usr/include
# 1,7G    /usr/lib
# 848M    /usr/share
# 256M    /usr/src
# 155M    /usr/bin
# 684K    /usr/games
# 1,4M    /usr/libexec
# 2,9G    /usr

# sort disk usage numerically in humanized form
du -d1 /usr -h | sort -h
# 428K    /usr/local
# 684K    /usr/games
# 1,4M    /usr/libexec
# 16M     /usr/sbin
# 21M     /usr/include
# 155M    /usr/bin
# 256M    /usr/src
# 848M    /usr/share
# 1,7G    /usr/lib
# 2,9G    /usr

# display users and shells sorted by shells
cat /etc/passwd | cut -d: -f1,7 | column -t -s: | sort -k2
# root                 /bin/bash
# gokcehan             /bin/bash
# gdm                  /bin/false
# hplip                /bin/false
# ...
# speech-dispatcher    /bin/false
# gnome-initial-setup  /bin/false
# sync                 /bin/sync
# lp                   /usr/sbin/nologin
# bin                  /usr/sbin/nologin
# ...
# systemd-network      /usr/sbin/nologin
# systemd-resolve      /usr/sbin/nologin

## uniq (1) - report or omit repeated lines

# display shells used in the system
cat /etc/passwd | cut -d: -f7 | sort | uniq
# /bin/bash
# /bin/false
# /bin/sync
# /usr/sbin/nologin

# display shell counts
cat /etc/passwd | cut -d: -f7 | sort | uniq -c
#       2 /bin/bash
#      22 /bin/false
#       1 /bin/sync
#      16 /usr/sbin/nologin

# sort shell counts
cat /etc/passwd | cut -d: -f7 | sort | uniq -c | sort -rn
#      22 /bin/false
#      16 /usr/sbin/nologin
#       2 /bin/bash
#       1 /bin/sync

## wc (1) - print newline, word, and byte counts for each file

# count lines in '/etc/passwd'
cat /etc/passwd | wc -l
# 41

# count words in '/etc/passwd'
cat /etc/passwd | wc -w
# 70

# count bytes in '/etc/passwd'
cat /etc/passwd | wc -c
# 2286

# count lines/words/bytes in '/etc/passwd'
cat /etc/passwd | wc
#      41      70    2286

## shuf (1) - generate random permutations

# select random words
cat /usr/share/dict/words | shuf | head
# crabbed
# raceway
# scare's
# altercations
# multiple
# eerier
# doubling
# donned
# Browne
# caucussed

## fmt (1) - simple optimal text formatter

# format given text
shuf /usr/share/dict/words | fmt | head
# Collins kindergärtner Soyuz Cinderella pricklier laxness Domesday
# Darryl's lighted departments delphiniums envisioned grapevine's
# Noel's Casablanca's eyrie's seeker's Brendan exhorted Septembers
# flea dyke burgers hashing laminating mattock servicewoman approving
# Basho's crusade omits filthy pare performance's peccadillo certainly
# retributions hinterland tresses displace moonlights exposed mellows
# careened syncopating spotter's divas Ruby's measurement's waterfowls
# curvatures Reunion moistening playback hooped Man equators cable's
# thumbing camerawoman's Tenochtitlan's somersault's pizazz's Nestle
# reformation Senior overshot enlivened impugns Kroc kickoffs rocketry

# format given text with a width of 40
shuf /usr/share/dict/words | fmt -w 40 | head
# eyesore's cagy Shorthorn outrider's
# capaciousness flaunt's unbelief
# Gaul's unselfishness fetishist's
# Serbian's UNICEF's eventually
# impenetrability bodkin eclectic's
# researches smokeless geriatrics dowdily
# Roth misidentify Congregationalist
# trails saying disciplines Congreve
# mystery's offends Astrakhan tinseling
# dearness disengages glinted leprous

## colrm (1) - remove columns from a file

# display first 40 characters from each line
shuf /usr/share/dict/words | fmt | head | colrm 40
# simile trappers jawbone hurdle's sorori
# Mongolia's hallway's misquotation measu
# Harrington nomination's guessers risk s
# belie Charlemagne's upside's mines enve
# Dannie tightwad fruitfulness's sensuali
# scrapbook's asters dicta coexistence Lu
# squeaked expressionists undies dingy ja
# Baldwin Ricky librettos sac lobbyist pa
# meddler teachable silversmiths percolat
# broadsword's advisers multiplier robs m

# remove characters betweeen 20 and 40 from each line
shuf /usr/share/dict/words | fmt | head | colrm 20 40
# busting Ahab ligatuions mammoths etymologists
# crams logging's toddu's vacillation's VLF's
# Prius bitterly sheln's italic interstice Rickey's
# Shavian's cylindersfhound's hoarders psychokinesis
# Pb devil's mumbled enlightens pleasantries wicker
# timidity gelding amath portraiture's motion's
# blowers Bolshevist erring song's cash diameter
# juiciness bearded G obeisances mob's inkwell's wok
# paddocked jiffies Crneyron's Napster tidiness's
# need's aureola leg nnelling Mohawk craftsmanship's

```

Related:

* <https://gokcehan.github.io/>

Tags:

    #linux #bash #text #commands #cheatsheet
