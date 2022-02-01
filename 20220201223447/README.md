# youtube-dl commands


### Download video

youtube-dl URL from video
`youtube-dl https://www.youtube.com/watch?v=OLK49ZTbmWM&list=PLtK75qxsQaMLZSo7KL-PmiRarU7hrpnwK&index=3&t=0s`

### Download playlist

youtube-dl URL from playlist
`youtube-dl https://www.youtube.com/playlist?list=PLtK75qxsQaMLZSo7KL-PmiRarU7hrpnwK`

### Download Channel

youtube-dl URL from channel
`youtube-dl https://www.youtube.com/channel/UC2eYFnH61tmytImy1mTYvhA`

### Download in 720p

`youtube-dl --title -f "bestvideo[width=1280]+bestaudio" URL`

### Download partial Video (only sequence)

`youtube-dl-section https://www.youtube.com/watch?v=6f0y1Iaorug 00:00:00 00:02:00 intro_test.mp4`

### Audio

`youtube-dl -x URL` will download video and convert it to audio in a second step (for real music often needed)
`youtube-dl -f bestaudio URL`

### video/playlist/channel+thumbnail

`youtube-dl --write-thumbnail URL`

### video/playlist/channel+metadata

`youtube-dl --add-metadata URL`

### Options

`youtube-dl -F URL` to see a list of available formats to download: choose the format code
`youtube-dl -f 136 URL` to download specific format

### only thumbnails of a playlist or video

`youtube-dl --write-thumbnail --skip-download URL`

### only metadata

`youtube-dl --add-metadata --skip-download URL`

### video/playlist/channel+json (for scripts), e.g. offline YT-library

`youtube-dl --write-info-json URL`

***HINTS***
* cancelled videos can be restared where you left off
* works on many platforms not just YT, even if there is a paywalled content just use the username option (see `man youtube-dl`)
* option -i ignores errors if you download from a country where not all videos are available

Tags:

    #download #youtube #videos
