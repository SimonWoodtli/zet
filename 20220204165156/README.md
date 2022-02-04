# LINUX: remove PPA

1. Method:
`sudo add-apt-repository --remove ppa:PPA_Name/ppa`

1. Method:
Pop-Store settings

1. Method:
`sudo ls /etc/apt/sources.list.d`
pick one
`sudo rm -i /etc/apt/sources.list.d/PPA_Name.list`

1. Method: (delete ppa + software)
`sudo apt-get install ppa-purge`
`sudo ppa-purge ppa-url`

Tags:

    #linux #ppa #packages #software
