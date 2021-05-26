# bboost20: Day 19

##  Wednesday, May 28, 2020, 01:31:30PM

### Topic

***HINTS***
* single quotes in bash script mean that you translate this sequence into binary data. (nice for pen testing, overthewire).
* if you want to see if your script has color bleed change your prompt: `export PS1="sero$ "` and then you get a plain white prompt. Now run your script if it changes your prompt color you know you have bleed going on.

waffles15:
declare -r clear: now you don't need the `clear` command anymore because you added the constant that you can echo to clear the screen. line 3/6. Reset puts color back to white otherwise it'd be green.

waffle16: to stop color bleed you have to reset the last red line. and make \$yes variable. The redundance is important to see why subroutines matter. so new Q added.

Create a color script to paste colors from HEX to RGB for your scripts:
![color1](img/2020-05-28_16-54.png)

exit 1: (bash specific) when you exit a program, programs in UNIX that run successfully exit with a return value of 0. 0 exit value or return value means everything went fine. Because if it is not 0 you can return up to 255. So returning not with a 0 means something went wrong. In the old days you every number stand for a specific bug/error. The exit value is stored in a special character called darr question mark. Darr question mark returns the last exit value of the last thing. And that can be a return value (if it is a function, subroutine) or a exit value (script).![color2](img/2020-05-28_17-31.png)
![color3](img/2020-05-28_17-24.png)


Line 11: be able to support the 3 letters too, than you get those awesome greyscale values. And you don't have to do the doubles.

![color4](img/2020-05-28_17-44.png)

exit 1 or exit 0 at the end: you have to make a decision when your script reaches the end if is a fail (1) or a success (0). And the whole script design is orientated towards that. Default case fail or default case success. This can simplify your code if you make the right decision because you won't have a ton of else statements everywhere.

Exit 0 after a function will already end the script (bail). If exit 1 is reached it will kill the script.
![color5](img/2020-05-28_18-02.png)

Only accept f, fff, ffffff not ff otherwise error:
![color6](img/2020-05-28_18-14.png)
----

lets define the colors and build them in:
```
declare -r clear=$'\033[H\033[2J'
declare -r red=$'\033[38;2;255;0;0m'
declare -r green=$'\033[38;2;0;255;0m'
declare -r blue=$'\033[38;2;0;0;255m'
declare -r white=$'\033[38;2;255;255;255m'
declare -r reset=$'\033[0m'
```

----

This will be the if version of our script:

----

```
## CSS Hex Value
if [[ $# == 1 ]]; then
  if [[ ${#1} == 6 ]]; then
    echo six
  elif [[ ${#1} == 3 ]]; then
    echo 3
  elif [[ ${#1} == 1 ]]; then
    echo 1
  else
    echo "${red}Must be 1, 3, or 6 in length.$reset"
    exit 1
  fi
  exit 0
fi
```

----

Better for our script and this situation is the case statement instead of the if version we build:
![color7](img/2020-05-28_18-49.png)

lets write a test script so we don't need to use our test commands manually:

----

```
#!/bin/bash

# single argument (hex)

for i in f ff fff ffff fffff ffffff; do
  echo '------------------------------------------'
  echo Testing $i
  ./color $i; echo $?
done
```

# three arguments (rgb)


echo '------------------------------------------'
echo Testing white
./color 255 255 255; echo $?


echo '------------------------------------------'
echo Testing red
./color 255 0 0; echo $?

echo '------------------------------------------'
echo Testing green
./color 0 255 0; echo $?

echo '------------------------------------------'
echo Testing blue
./color 0 0 255; echo $?

#RBG edge cases

echo '------------------------------------------'
echo Testing Testing red out of bounds
./color 300 0 0; echo $?
./color -2 0 0; echo $?

echo '------------------------------------------'
echo Testing Testing green out of bounds
./color 0 300 0; echo $?
./color 0 -2 0; echo $?

echo '------------------------------------------'
echo Testing Testing blue out of bounds
./color 0 0 300; echo $?
./color 0 0 -2; echo $?

echo '------------------------------------------'
echo Testing Just 2 arguments
./color 0 0; echo $?

echo '------------------------------------------'
echo 'Testing >3 arguments'
./color 0 0 0 0; echo $?

echo '------------------------------------------'
echo 'Testing none numeric arguments'
./color abc 0 0; echo \$?
./color 0 abc 0; echo $?
./color 0 0 abc; echo $?

----

Lets finish the \# RGB Value:
![color8](img/2020-05-29_10-21.png)
This escape sequence is only for foregrounds not backgrounds.
if you escape this it won't print anything. `./color 244 244 244`
why not? It didn't print the text it converted the txt with these escape sequences into an invisible ansi terminal sequence, which did not print.
if you do `echo \$(./color 244 34 244) something` it will color it and also bleeds.


Lets create another function in a new script to get background too. name: hex2rgb
![hex2rgb1](img/2020-05-29_10-55.png)

Now we add this function back to our color script. (last line not needed)

Let's add a case statement to hex2rgb to make the conversion. (3 and 1 is what you cant see)
![hex2rgb2](img/2020-05-29_11-21.png)

still not working lets add a recursive function call and correct some errors:
![hex2rgb3](img/2020-05-29_11-31.png)
The Hexcode gets translated in another way then Rob was thinking to fix this:
![hex2rgb4](img/2020-05-29_11-36.png)
still not working...

now it works: (hex2rb in your scriptfolder)
![hex2rgb5](img/2020-05-29_12-31.png)

Explanation:

----

### General Notes

* Bluesky= you code for the most common cases and you detect those cases first. It is an approach to programming that says put the most relevant the most common thing that your code is going to be doing first. And bail out when something is going wrong. And put the secondary cases (exceptional path) at the end.
* printf can take two arguments one with "" and then you put special little strings in there that have % in front. And then your arguments after that correspond to the positions in the format. So it is print formatted.
* Printf is in every language. In other languages than bash it is more powerful. Because you can pass an argument that is a string and do a % whatever. And then whatever the % thing is it will convert it.
* Test driven development is when you write the tests first. And then you write the actual code.

#### Common Practice

1. UNIX philosophy is: Do one thing well.

#### Tips

1. use \## for comments in your scripts and \# to disable code.
1. do some automated testing but don't eliminate user testing (manually by yourself, by running it). Don't get too obsessed with automated testing it will shrink your productivity. Find a middle path between the two.
1. don't write bash scripts that depend on imports. Otherwise if you want to share them you also have to share the import file otherwise the script won't work. Write self containing files!
1. `!!ix 2nCu` in vim will paste stuff in vim.
1. writing test code is mandatory: you will get fired if you don't do it
