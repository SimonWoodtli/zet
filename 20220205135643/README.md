# Zettelkasten Notes and Hints

* have a system that allows for online/computer and offline/handwritten notes
* make handwritten notes whenever a good idea comes to mind. add your unique identifert date++second but make sure to always use GPT time. Otherwise if you switch between different timezones its no longer unique (if you travel or whatever) so always GPT for online notes and offline notes.

* has a unique identifier as folder
* has a README.md as a file in it
* the file is only 50 chars long or 25 lines
* the file has a title
* git commit message should be title of your file
* license for notes: https://github.com/rwxrob/zet/blob/main/LICENSE
* method to link to notes: https://github.com/rwxrob/zet/blob/a4458625e1707a62c4a7879141e7a4986bc2b3c6/20210502045853/README.md
* if you link to the search instead of a specific zettel identifier  about a topic. You will get all the old things related to it. But also all the new ideas that have the topic/keyword in it. -> ofcourse you can still link to a unique identifier if its just about a specific connection to a zettel.
but in general its better to link to a search so you can get an idea about the whole topic. and hardlinking to a specific zettel can break if you for example delete this specific zettel because it became redundant because you wrote another zettel with the same idea.
* at the end you will have mutlipe repos that are zettelkasten. But a main one for all your notes and learning and knowledge. Only have another one for a specific project you want to share with the world like the beginner boost from rob.
* the readme.md that comes in the starting point of a  repo is your register where you can start making notes how things are linked together (hyperlinks to the unique identifers or searches)
*  vlc when 1:05 left on day 2 boost video ipad watch again: idea how to write zettels
-> you start writing on a zettel and when you come up with a new idea that has something to do but is not directly about what you currently write you open a new tmux window and create another zettel and start writing there. and this migth lead to another idea and so on…
* dont use tags like i had in my old system
* to search notes from commandline: $ git grep -i foo
* adjust my zettel script to robs zet script to create zettels.

* you want mainly to link zettels from your registery to a a topic search of zettels. sometimes also to an individual zettel. And even less often link from within a zettel directly to another zettel (identifier to identifier)
* if a zettel is longer than 25 lines create a new zettel and link them together via ttheir identifier. Also this makes sure to keep your zettels short and to the point. and multiple zettels should that belong to each other should always be questioned whether or not they actually should be an independent zettel as a new idea.
* in a good system you should be able to delete a zettel and not break everything. that is why linking from one zettel to another zettel directly should be done only when absolutly necessary.
* search every title with git directly: $ head -1 \*\*/README.md
* only use a title if you have to it is not necessary. If you dont have a title for a zettel its fine too. (don’t think too long/hard to come up with one)
* if you link to an image with markdown: always put the link on its own line. https://rwx.gg/lang/md/basic/

Tags:

    #knowledge #notes #zettelkasten #knowledgeManagement
