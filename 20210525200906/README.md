# Basic IRC commands

If you want to change channels/windows with the mouse `/mouse enable` will help.

## Common IRC servers:

• irc.freenode.net/7000 (with SSL)
• irc.spotchat.org/ (SSL?)
* irc.libera.chat/ (SSL?)
* irc.twitch.tv/6697 (with SSL)

## Settings

* See a list of all settings to be changed: `/set`
* See a list of all buflist settings: `/set buflist`

To change boolean values in there: `/fset -toggle` or Alt-Space
To change a string value in there: `/fset -setnew` or Alt-Enter

## Commands

buffer = windows
ubuntu = channel name

* connect unsaved server: `/connect [irc.servername.foo]`
* connect saved server (~/.config/weechat/irc.conf): `/connect [servername]`
* join channel: `/join #ubuntu`
* search servers channels: `/list -re #linux*`
* change channel: `/buffer #ubuntu` (use autocomplete) or /1 ... /2 ... /n
* change channelname when joined on two servers: `/buffer freenode.#ubuntu`
* leave channel: `/part foo` or `/leave foo`
* close buffer: `/close`
* leave server: `/disconnect serverName`
* toggle nickname list `/bar toggle nicklist`  or Alt-Shift-n
* get nickname info: `/whois userName`
* toggle channel list: `/buflist toggle` or `/bar show buflist` or Alt-Shift-b
* Bare window with no extras: `/window bare` or Alt-l
* to mark user in chat @nickname (tab-autocomplete works)
* whisper private msg: `/query nickname`

* help: `/help`
* exit: `/exit`
