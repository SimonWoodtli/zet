# What are some basic commands used with Reddio?

## reading

* `reddio` gives you a list of tops from all your subreddits
* `reddio print -l 10 r/linux` top 10 of r/linux
* `reddio print -f '$num. $url$nl' -l 5 -t month r/linux/top` top 5 of the month numbered
* `reddio print -l 50 -t week -f '$url$nl' r/commandline/controversial` top 50 of the week
* id = t3_j92ciz or similliar: `reddio print -f '$url' by_id/t3_j92ciz` get url from 1 post
* `reddio print -f '$title' by_id/t3_j92ciz`  get title from 1 post
* id = j92ciz (ignore t3) `reddio print comments/j92ciz` print comments of particular post

## user

* `reddio print -l 1 -f '${new:+New message(s)!$nl}' message/unread
New message(s)!` Check for new messages and comment replies
* `reddio print -f '${subscribed:+you are subscribed to $name$nl}' r/linux/about` check if you are subscribed to subredit
* `reddio subscribe commandline linux` subscribe to multiple subreddits (commandline and linux)

## submit

* `reddio submit -t 'Hello, World!' r/test 'Test submission using reddio - CLI reddit reader'` submit post to r/test
* `reddio submit -l http://localhost r/test "Testing a link submission using reddio"` submit a link to r/test
