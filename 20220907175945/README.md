# ImageMagick commands

* open image magick GUI: `display` , buggy but funny it even exists. Maybe useful for an image viewer: `display image.png`
* remove all the metadata: `convert -strip`
* distorts and twists screen.png and puts picture2 in the center in front of it:`convert /tmp/screen.png -paint 1 -swirl 180 /tmp/picture2.png -gravity center -composite -mattte /tmp/output.png`


Kris Occhipinit on YT: https://yewtu.be/playlist?list=PL9135DE9747FB6F88
