# Bash: Split variable|string into array

## Method 1: Split string with parenthesis

> ⚠️  Do not quote `$myvar` when you create an array with parenthesis. This will
not use whitespace as IFS and as a result your array will have all elements in
[0].

```bash
myvar="string1 string2 string3"
myarray=($myvar)
## WRONG:
myarray=("$myvar")
```

## Method 2: Split string with `read`

The advantage of this is you do not come up with the silly idea of quoting your
variable when you use `read`.

```bash
myvar="string1 string2 string3"
read -a myarray <<< $myvar
```

## Method 3: Use custom delimiter

Both read and simply creating an array can use a custom IFS in front as
a delimiter to create elements.

```bash
myvar="string1|string2|string3"
IFS='|' myarray=($myvar)
IFS='|' read -a myarray <<< $myvar
```

Tags:

    #linux #bash #array #caveats #split #string
