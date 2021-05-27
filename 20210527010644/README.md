# What's the command to display the 10 most used commands?

`history | awk '{print $2}' | sort | uniq -c | sort -nr | head -n 10`

