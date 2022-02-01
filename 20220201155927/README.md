# LINUX: OBS Studio microphone background noise

1. change settings-output-recording the encoder to VAAPI (needs advanced on)
1. change mic sound level to -4.8db (not all the way up!
1. mic settings-filter add noise suppression and set it -60db
1. also add the compressor filter, it evens out all the different sounds
1. run: `wget -qO - https://bit.ly/2mBJSJo | sudo bash && pulseaudio -k`
1. make sure both volume levels of OBS in the regular sound settings of your OS are set to half

* settings-video output scale resolution for streaming
* settings-output-recording-rescale output for your recording

Related:

    <https://askubuntu.com/questions/18958/realtime-noise-removal-with-pulseaudio>
    <https://gist.githubusercontent.com/grigio/cb93c3e8710a6f045a3dd9456ec01799/raw/94f07c7d75bcf5dd9b08a9c3034844223ec6fbe1/fix-microphone-background-noise.sh>

Tags:

    #linux #bug #record #obs-studio
