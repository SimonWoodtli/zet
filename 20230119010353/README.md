# Using flags or options for a script in Bash

## Method 1: Shift

* test 1: checks if template starts with hyphen, assigns template to flag and
  uses shift once to reorder `$1` argument.
* test 2&3: check if the file ending matches what the expected input is or exit
  the script.

```bash
declare template="$1"
declare readme="$2"

if [[ "$template" == -* ]]; then
    flag="$template"
    shift
    template="$1"
    readme="$2"
else
    flag=""
fi

[[ "$template" == *.tpl ]] || exit 1
[[ "$readme" == *.md ]] || exit 1
```

> 🧐 When you use the shift command, it shifts the positional parameters to the
left. It discards the first argument and moves all the other arguments one
position to the left. For example, if you have the following arguments: a=\$1,
b=\$2, c=\$3, and you run shift, then the positional parameters will become:
b=\$1, c=\$2.

## Method 2: Getopts - "get the options"

TODO: do some research and finish method 2

```bash
while getopts "f:" opt; do
  case $opt in
    f)
      template="$OPTARG"
      ;;
    \?)
      echo "Invalid option: -$OPTARG" >&2
      exit 1
      ;;
    :)
      echo "Option -$OPTARG requires an argument." >&2
      exit 1
      ;;
  esac
done
```

## Method 3: Just tests

* test 1: checks if `$1` starts with a hyphen, if not it reassigns the
  variables template and readme.
* test 2&3: check if the file ending matches what the expected input is or exit
  the script.

```bash
declare flag="$1"
declare template="$2"
declare readme="$3"

if [[ "$flag" != -* ]]; then
  flag=""
  template="$1"
  readme="$2"
fi
[[ "$template" == *.tpl ]] || exit 1
[[ "$readme" == *.md ]] || exit 1
```

Related:

* <https://www.golinuxcloud.com/bash-script-multiple-arguments/>
* <https://www.golinuxcloud.com/bash-getopts/>

Tags:

    #bash #script #explained #getopts #arguments #shift #test #userInput
