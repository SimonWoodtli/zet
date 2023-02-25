# IRC setup for Twitch.tv

## Basic Idea

1. [Create] token to connect with IRC:


1. Config in Weechat/Hexchat:
nickname: twich username
server: irc.chat.twitch.tv
port: 6667 or 6697 (SSL)
password: OAuth token

[Create]: <https://twitchapps.com/tmi/>

## Weechat

1. start weechat: (replace TWITCH_NAME to your lowercase Twitch Name)
`/server add twitch irc.twitch.tv/6667 -password=oauth:*** -nicks=TWITCH_NAME -username=TWITCH_NAME`
`/server add twitch irc.twitch.tv/6697 -password=oauth:*** -nicks=TWITCH_NAME -username=TWITCH_NAME`

2. /connect twitch
3. /join #channelName (with hash)

Related:

* https://weechat.org/files/doc/stable/weechat_quickstart.en.html
* https://gist.github.com/noromanba/df3d975613713f60e6ae

Tags:

    #weechat #irc #twitch
