# Youtube-dl commands

* download YT video: `youtube-dl-section https://www.youtube.com/watch?v=6f0y1Iaorug 00:00:00 00:02:00 intro_test.mp4` # only downloads 2mins

* remove audio: `ffmpeg -i input_file.mp4 -vcodec copy -an output_file.mp4`
