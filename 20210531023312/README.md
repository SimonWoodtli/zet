# How to customize your prompt?

Prompt Statement (PS) is used to customize your prompt string in your terminal windows to display the information you want.

PS1 is the primary prompt variable which controls what your command line prompt looks like. The following special characters can be included in PS1:

\u - User name
\h - Host name
\w - Current working directory
\! - History number of this command
\d - Date

They must be surrounded in single quotes when they are used, as in the following example:

$ echo $PS1
$
$ export PS1='\u@\h:\w$ '
student@example.com:~$ # new prompt

To revert the changes:

student@example.com:~$ export PS1='$ '
