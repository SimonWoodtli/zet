# How to configure weechat with twitch

https://weechat.org/files/doc/stable/weechat_quickstart.en.html
https://gist.github.com/noromanba/df3d975613713f60e6ae

1.
open weechat:
`/server add twitch irc.twitch.tv/6667 -password=oauth:*** -nicks=TWITCH_NAME -username=TWITCH_NAME`

replace TWITCH_NAME to your lowercase Twitch Name

https://www.reddit.com/r/Twitch/comments/2uqews/anybody_here_using_weechat/


2. /connect twitch
3. /join #CHANNEL_NAME
4. /save


## Customize Weechat


hide nickname-list and channel-list columns:
1. find name of bars: `/bar list`
2. hide it: `/bar hide <bar name>`
3. show it: `/bar show <bar name>` or toggle: `/bar toggle <bar name>`

hide timestamp:
`/set weechat.look.buffer_time_format ""`

[more info:](https://weechat.org/files/doc/devel/weechat_faq.en.html#small_terminal)

```
/set buflist.look.enabled off
/bar hide nicklist
/set weechat.look.buffer_time_format "%H:%M"
/set weechat.look.prefix_align none
/set weechat.look.align_end_of_lines prefix
/set weechat.look.nick_suffix ">"
/set weechat.look.nick_prefix "<"
```
