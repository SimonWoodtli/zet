# Server Security: Add password/http auth and whitelists via ACCESS List for Nginx Proxy Manager

> 🧐 By default both conditions the right password and the right IP need to
> match in order to have access to a site. However if you only need or want one
> feature enable 'Satisfy Any'

Access List in NPM allow for password authentication using the same http auth
as plain nginx under the hood. Additionally you can also allow certain IPs only
(whitelist) to have access to your site or block them (blacklist).

1. Login to NPM
1. Go to Access Lists and click 'Add Access List'
1. Give it a name 'Pw Auth' or the name of the service or whatever
1. Enable Satisfy Any in case you don't bother with the IPs or vice versa
1. Pass Auto Host? Research
1. In the Authorization Tab add a user and password (don't click the add button, it's not working as you might think. Just hit safe)
1. Go to the Proxy Hosts click edit the site you want to have http auth. Where it says 'Access List' select 'Pw Auth' that you just created.

About the Access Whitelist:

* Accepts only publicly available IPs
* Does not accept domains (really?test)
* If you want to select an individual local IP it requires subnet too so '192.168.0.55/32'
* By default every IP is denied
* Allows whole networks to be white/blacklisted e.g. '192.168.0.0/24'
* The order of the block/allow list matters if you allow all first but then block a specific one that you allowed first, it's not getting blocked

> ⚠️  If you update the whitelist after initial creation of the access list you
> need to go the affected proxy hosts and click edit and hit safe again to
> update/refresh the actual sites with this new configuration.

Related:

* [20240104161046](/20240104161046/) Fresh Server: First Steps
* [20240110232911](/20240110232911/) LAN Server Install: Setup and Install dietpi
* [20240108005229](/20240108005229/) Server Security: Add password/http authentication to nginx site

Tags:

    #linux #server #security #http #authentication #password #nginx
