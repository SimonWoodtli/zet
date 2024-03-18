# Commands to display metadata for various files

## Admin Metadata

* `file foo`
* `stat foo`

## Image Metadata

* `identify -verbose foo.jpeg`
* `exif foo.jpg` || `exiftool foo.jpg`
* `mediainfo foo.jpg`

## Video Metadata

You may also use `exif` or `mediainfo`

* `ffprobe foo.mp4 -hide_banner`

## Audio Metadata

You may also use `exif` or `mediainfo`

* `ffprobe -i foo.mp3`

## PDF Metadata

You may also use `exif` or `mediainfo`

* `pdfinfo foo.pdf`

Tags:

    #linux #sysadmin #videos #music #song #mp3 #pdf #images #pictures #jpeg #png #mp4
