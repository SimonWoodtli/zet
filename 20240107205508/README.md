# Server Setup: Add domain name to your Server

1. Login to your favourite domain name provider
2. Go to DNS records
3. Add an A record with the Value of your VPSs IPV4 and AAAA for IPV6. Since we want to configure the root domain we use "@" as the Host.
4. Add a CNAME record with the Value of your domain name and "\*" as Host for your sub domains.

> 🧐 Using * as Host value, represents a wild card that means every records or subdomains. This can be a problem if you decide to create another instance in the future.


Related:

* <https://www.namecheap.com/support/knowledgebase/article.aspx/434/2237/how-do-i-set-up-host-records-for-a-domain/>
