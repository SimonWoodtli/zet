# Resolve DNS names with the `dig` command

* name resolution: `dig yourDomain.com`
* reverse resolution and less verbose: `dig -x 1.1.1.1 +short`

Use cloudflares public name resolver to resolve a name with a specific rule:

```
dig @1.1.1.1 git.simonwoodtli.com a
dig @1.1.1.1 *.simonwoodtli.com a
dig @1.1.1.1 simonwoodtli.com mx
```

## Example

Output of: `dig linuxfoundation.org`

This output is showing a lookup for an A record (IPv4), flags, transport, and timing information

## Some useful options/flags

* `+trace` Check the entire path, from the root servers (by default)
* `-t [record type]` specify the record (including A, TXT, CNAME, MX)

Tags:

    #linux #sysadmin #networking #internet #DNS #resolver
