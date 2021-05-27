# Create github gist from CLI

1. create personal access token on github name it: "create gist"
1. make sure to add scope: select gist
1. Use this command to post a gist:
`curl -X POST -d '{"public":true,"files":{"addgist.txt":{"content":"This is a cool way to add a gist!"}}}' -u MyUser:194be7ef18ae5c539cc75bd830f03d3487b118fc https://api.github.com/gists`

* This is all one command you will copy into your command line.
* You can modify the “public”:true, to false if you wish to add a secret gist.
* You can modify the “Addgist.txt” to whatever you would like to name your gist.
* We can also add specific content to your gist by modifying the “This is a cool way to add a gist!” section.
* Lastly, change the ‘MyUser’ option to your GitHub username, and then enter the personal access token we copied in the last step as your password.

[checkout](https://www.liquidweb.com/kb/little-known-ways-to-utilize-github-gists/)
