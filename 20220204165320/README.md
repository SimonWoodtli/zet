# LINUX: How to write Pinyin/Chinese

1. Install ibus-m17n with all dependencies
1. Open ibus preferences and add English US and hanyu pinyin (m17n)
1. Export variables: `vi ~/.bashrc`

Content:

```sh
#IBUS settings
export GTK_IM_MODULE=ibus
export XMODIFIERS=@im=ibus
export QT_IM_MODULE=ibus
```

4. open settings - region & language and add hanyu pinyin there too
5. log out and login again
6. select keyboard and you can type: e4 e3 e2 e1 for tones

# Some fonts to write in Office:

`sudo apt install fonts-wqy-microhei fonts-wqy-zenhei fonts-arphic-ukai fonts-arphic-uming fonts-cns11643-sung`
