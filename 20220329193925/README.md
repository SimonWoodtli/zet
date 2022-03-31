# Setup and Customize Weechat IRC Client

All settings preconfigured in ~/Private/bin/weechat
Weechat config dir: ~/.config/weechat

Autojoin channels if connected to server: `/set irc.server.libera.autojoin "#weechat,#weechat-fr"`

## Add servers

* Freenode: `/server add freenode chat.freenode.net/7000 -ssl -password=yourPassword -nicks=yourNickName -username=yourUserName`
* Twitch: `/server add twitch irc.twitch.tv/6697 -ssl -password=oauth:*** -nicks=TWITCH_NAME -username=TWITCH_NAME`
* Libera:
* Spotchat:
* Slack:
* Discord:

### Auto join channels

Make sure: `/set irc.server.freenode.autoconnect on` is on

* Freenode: `/set irc.server.freenode.autojoin "#weechat,#ubuntu,#reddit-sysadmin,#puppet,#theforeman,#lopsa,#illumos,#opensolaris,#linux,#r_netsec,#lopsa`

## Scripts

To see all available scripts: `/script` 
-> to search scripts: Type in the input bar
To see some info about script: `/script show <scriptname>`
To install: `/script install <scriptname>`
To remove it first: `/script unload <scriptname>` then `/script remove <scriptname>`

* Install autosort: `/script install autosort.py`

## Spell checking

* enable spell checking: `/spell enable`
* list all dictionaries: `/spell listdict`
* select dictonary: `/set spell.check.default_dict "en_US,en"`

## Colors



## Some more sane settings:

• See a list of all settings to be changed: `/set`
• See a list of all buflist settings: `/set buflist`


To change boolean values in there: `/fset -toggle` or Alt-Space
To change a string value in there: `/fset -setnew` or Alt-Enter
To Undo the latest change: `/fset -reset` or Alt-r
To See a list of all changed values: `/set d`
Save settings: `/save`

Use `/set keyword` to search settings:


* Close window when leaving channel: `irc.look.part_closes_buffer` turn on
* Have channels be related to the server: `/set irc.look.server_buffer independent`
* Give nicknames a different color: `/set irc.look.color_nicks_in_nicklist on`
* Activate mouse: `/set weechat.look.mouse on`
* Create default username: `/set irc.server_default.username "name you want to use"`
* Create username fallback list if not available: `/set irc.server_default.nicks "name,name1,name2,name3,name4"`
* Better readable titlebar `/set weechat.bar.title.size 0`
* Change channels easy: 
`/trigger add numberjump modifier "2000|input_text_for_buffer" "${tg_string} =~ ^/[0-9]+$" "=/([0-9]+)=/buffer *${re:1}=" ""` navigate with /1 ... /2 ... /n
* Disable join/leave message from all users: `/set irc.look.smart_filter on` and 
`/filter add irc_smart * irc_smart_filter *`
* Disable join msg: `/set irc.look.display_join_message` and 
`/filter add irc_join_names * irc_366,irc_332,irc_333,irc_329,irc_324 *`

## Logs

* Don't log the core weechat application: `/set logger.level.core.weechat 0`
* log all channels and messages from slack: `/set logger.level.python.xxxx.slack.com 1`
* log channels and messages to me: `/set logger.level.irc 1`
* Don't log specific channel: `/set logger.level.irc.freenode.##news 0`

store logs each year/server different file:

```
/set logger.file.path "%h/logs/%Y/"
/set logger.file.mask "$plugin.$name.log"
/set logger.file.nick_suffix "]"
/set logger.file.nick_prefix "["
```

## Switch off file Transfer

```
/set irc.ctcp.clientinfo ""
/set irc.ctcp.finger ""
/set irc.ctcp.source ""
/set irc.ctcp.time ""
/set irc.ctcp.userinfo ""
/set irc.ctcp.version ""
/set irc.ctcp.ping ""
/plugin unload xfer
```

Don't Forget to save settings: `/save`

## Only load default Plugins that you need (wait until you configured Plugins)

```
/plugin unload lua
/plugn unload tcl
/plugin unload ruby
/plugin unload javascript
/plugin unload guile
/plugin unload relay
```

* disable exec, fifo, guile, javascript, lua, ruby, tcl, xfer:
`/set weechat.plugin.autoload "*,!exec,!fifo,!guile,!javascript,!lua,!ruby,!tcl,!xfer"`

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
* <http://www.futurile.net/2020/11/29/weechat-configuration-for-irc-and-slack/>
* <https://hugo.md/post/the-perfect-weechat-setup-2/>
* <https://netsplit.de/networks/top10.php>
* <https://www.bitlbee.org/main.php/news.r.html>

Tags:

    #weechat #irc #setup
