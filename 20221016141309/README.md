# Some basics of jq to process JSON

## Make JSON readable: prettifier

* If you pipe it: `curl URL | jq '.' > pretty.json`
* If you have a local file: `jq '.' foo.json > pretty.json`

## Filter JSON data


### TLDR

* filter only within collections: `jq '.menu.popup.menuitem' foo.json`
* filter within collections with a nested array `jq '.menu.popup.menuitem | .[]' foo.json`
* filter within collections with a nested array and get all the elements key-pair values: (.value is just a name given in the example) `jq '.menu.popup.menuitem | .[] | .value' foo.json`
* filter within collections with a nested array and get only element 1 key-pair value: `jq '.menu.popup.menuitem | .[1] | .value' foo.json`
* you often want your end result in an unquoted output. Use `jq -r`

### Explained

Collections{} get filtered with .nameOfCollection. Often with JSON you have one giant collection wrapping all your data in more collections and often arrays as well. Since the data you are looking for is often nested inside you use `jq .nameOfFirstCollection.nameOfSeconCollection... etc.`

example: what if you want to get "menuitem"

```json
{
  "menu": {
    "id": "file",
    "value": "File",
    "popup": {
      "menuitem": [
        {
          "value": "New",
          "onclick": "CreateNewDoc()"
        },
        {
          "value": "Open",
          "onclick": "OpenDoc()"
        },
        {
          "value": "Close",
          "onclick": "CloseDoc()"
        }
      ]
    }
  }
}
```

* command: `jq '.menu.popup.menuitem' foo.json`

output:

```json
[
  {
    "value": "New",
    "onclick": "CreateNewDoc()"
  },
  {
    "value": "Open",
    "onclick": "OpenDoc()"
  },
  {
    "value": "Close",
    "onclick": "CloseDoc()"
  }
]
```

If you want to get in any of these elements which are another set of collections your will face the next issue of arrays.
To do this you would want to iterate over all elements and spit them out again. This can be done with `jq .[] foo.json`.

However in this example we want to create one single command so we would need to use a pipe first to let jq filter the collections first and then take this output to in a piped command be able to iterate over the array.

command: `jq '.menu.popup.menuitem | .[]' foo.json`

Contrary to normal pipes make sure to have your quotes wrapped around the first collection filter and the pipe and the array filter. Simply because jq needs to interpret this command and not your shell.

output:

```json
{
  "value": "New",
  "onclick": "CreateNewDoc()"
}
{
  "value": "Open",
  "onclick": "OpenDoc()"
}
{
  "value": "Close",
  "onclick": "CloseDoc()"
}
```

Now from here lets say we want to get the values:

command: `jq '.menu.popup.menuitem | .[] | .value' foo.json`

output:

```json
"New"
"Open"
"Close"
```

But hold on what if you actually only want the data from the second collection "open"?

command: `jq '.menu.popup.menuitem | .[1] | .value' foo.json`

output:

```json
"Open"
```

One last detail is that a JSON value in a key-value pair is quoted. Now for any further processing of this information you might not want that.
You can use the -r flag for that.

command: `jq -r '.menu.popup.menuitem | .[1] | .value' foo.json`

Related:

* <https://www.linode.com/docs/guides/using-jq-to-process-json-on-the-command-line/>

Tags:
    
    #linux #jq #json #prettifier #filter #dataProcessing #api
