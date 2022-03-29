# Customize Weechat

All settings preconfigured in ~/Private/bin/weechat

Autojoin channels if connected to server: `/set irc.server.libera.autojoin "#weechat,#weechat-fr"`

## Some appearance changes

Hide nickname-list and channel-list columns:

1. find name of bars: `/bar list`
2. hide it: `/bar hide <bar name>`

hide timestamp: `/set weechat.look.buffer_time_format ""`

[Small View]

```
/set buflist.look.enabled off
/bar hide nicklist
/set weechat.look.buffer_time_format "%H:%M"
/set weechat.look.prefix_align none
/set weechat.look.align_end_of_lines prefix
/set weechat.look.nick_suffix ">"
/set weechat.look.nick_prefix "<"
```

[Small View]: <https://weechat.org/files/doc/devel/weechat_faq.en.html#small_terminal)>

Related:

* <http://www.futurile.net/2020/11/09/weechat-getting-started-for-irc-and-slack/>

Tags:

    #weechat #irc #setup
