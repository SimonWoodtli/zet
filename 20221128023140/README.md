# Task Spooler `ts` cheatsheet

job = task

* add job to queue: `tsp curl -LJ https://foo.com -o ~/Downloads/foo`
* remove job: `tsp -r <id>`
* remove last job: `tsp -r`
* list all jobs: `tsp`
* get info of specific job: `tsp -c <id>`
* remove finished jobs: `tsp -C`
* create multiple queue lists: `TS_SOCKET=/tmp/tasklist1 tsp echo hello; TS_SOCKET=/tmp/tasklist2 tsp echo world`
* list jobs from first list: `TS_SOCKET=/tmp/tasklist1 tsp`
