# Termux Setup

## General

1. `pkg update`
1. `pkg upgrade`
1. `pkg install vim git openssh`

### Install streamlink

1. install dependencies: `pkg update && pkg install clang python ffmpeg libxslt`
1. install streamlink: `pip install streamlink`
1. create config: (make sure its on 4 lines)

```
echo 'player=am start -n org.videolan.vlc/.StartActivity -a android.intent.action.VIEW -d 
player-external-http
player-external-http-port 4567
player-args "vlc://{playerinput}"' > ~/.streamlinkrc
```

4. start stream: `streamlink <URL> best`
5. watch stream, open VLC player and stream from: `127.0.0.1:4567`

Tags:

    #android #termux #terminal #linux #phone
