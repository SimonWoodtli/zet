# FFMPEG basic commands

convert mp3 to ogg
`ffmpeg -i input.mp3 output.ogg`

convert mp4 to webm
`ffmpeg -i input.mp4 output.webm`

flag to select specific codec: audio
`ffmpeg -i input.mp3 -c:a libvorbis output.ogg`

flag to select specific codec: video&audio
`ffmpeg -i input.mp4 -c:v vp9 -c:a libvorbis output.mkv`


This command copies the video stream from input.webm into output.mkv and encodes the Vorbis audio stream into a FLAC. The -c flag is really powerful.
`ffmpeg -i input.webm -c:v copy -c:a flac output.mkv`

Prior command applied to video and audio
`ffmpeg -i input.webm -c:av copy output.mkv`

Extract audio from videofile:
`ffmpeg -i input.mkv -vn audio_only.ogg`

Delete all audio tracks:
`ffmpeg -i input -map 0 -map -0:a -c copy output`

Create GIF from video
`ffmpeg -i input.mkv output.gif`

check ffmpeg [Documentation]

Tags:

    #ffmpeg #video #converter #CLI

[Documentation]: <https://ffmpeg.org/documentation.html>
