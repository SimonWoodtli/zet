# When to use bash 'set -e'?

* If it's a small script, because if 'set -e' fails you debugging won't be
  a headache
* If you don't want it to exit use logical OR: `... || true`

E.g:

```bash
#!/usr/bin/bash
set -e
mkdir -p ~/foo || true
echo hello world
```

If ~/foo already exists it would throw an error and set -e would exit the
script. However a logical OR conditional set to true would skip it and not exit
your script. So wherever necessary you can skip 'set -e' exiting your script. 

But If your script is getting bigger I am of the opinion that creating an own
error checking system makes more sense. Because you most likely include
external programs which might not handle things as expected. And 'set -e' does
have quite a few gotchas. See related

Related:

* http://mywiki.wooledge.org/BashFAQ/105

Tags:

    #bash #debug #test #error #checking
