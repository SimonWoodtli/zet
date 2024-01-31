# Setup Service: Uptime Kuma

1. Enable 2FA
1. Change to DarkMode
1. Setup Docker Host (if you want to monitor docker apps within their virtual
   network (no public/host exposed ports))
1. Add monitoring services
1. Enable Notifications
1. Create Status Page

## Monitor types

> 🧐 Use Retries: 1 (so you don't get spammed with false reports)

HTTP(s):

* Best practice: Use it with a dedicated healthcheck endpoint. Because if you
  use it directly on the domain name itself it puts an additional load on your
  site. Because every 60s it would load your entire website. E.g.
  https://simonwoodtli.com/healthcheck

The healthcheck file can be something as simple as this:

```
<!DOCTYPE html>
<html>
<head>
    <title>Health Check</title>
</head>
<body>
  <p>{"status":"ok"}</p>
</body>
</html>
```

HTTP(s) with keyword:

* I prefer to use HTTP(s) with keywords than just HTTP(s). Because you now know
  2 things first site is up second html renders correctly so the server returns
  a page.
* Enable Cert Expiry Notification
* If app/site runs via Proxy consider adding 4xx to the accepted status codes.
  Because if the app is down but the reverse proxy is still up the client will
  receive a 404 from the proxy server. SO adding 4xx as "down" makes sense in
  this case.

TCP:

* Any app that has no HTTP but has an exposed port. You can then check for that
  port to see if the service is up. `ss -tulpn`
* E.g add TCP/22 to monitor if SSH is up

DNS:

* If you want to know when your local DNS resolving isn't working
* You could do that with a ping or TCP port check but just because the DNS
  server is up and running doesn't mean that it resolves the query and response
  to it. So any misconfiguration of bind9 would still mean DNS server runs but
  resovle fails.
* Resolver Server: Your internal DNS server IP
* Port: 53
* Record Type: Use any record type usually just A record is enough. Because if
  one works correct it's likely the others do as well. 
* Hostname: Depends on which Record Type you use, for A record it's just your domain name.

Docker:

* For applications that don't have an open port or interface. E.g. Cloudflare
  Tunnels, Portainer Agent
* Docker containers run in their own isolated virtual network. So you cannot
  use any network request like HTTP/TCP/Ping etc.
  Because any network request requires a handshake and if the
  partner in crime isn't visible it won't work. Hence the Docker type.
* Requires adding `- /var/run/docker.sock:/var/run/docker.sock` to your compose
  file and kuma needs to be in the same compose stack as the app you want to
  monitor. (although I think you could still pass in the virtual network from
  kuma to be used for the app you want to monitor, (that is just more
  complicated).
* Additionally requires adding a 'Setup Docker Host' in Kumas settings. 'Socket Type'

Databases:

* Don't add them specifically they allow you to make queries to the database to
  test if database objects are up. But a TCP handshake to the databases open
  port makes more sense and is usually enough. Because if a database object
  isn't available it's likely the database server itself has a problem.

## Enable Notifications

> 🧐 A webhook is just a web endpoint to which you can post data (url+payload)

* Discord/Slack: Requires create your own server and enable a webhook(url) on
  it so that kuma can send a payload.
* TODO setup with ntfy

## Status Pages

Create a Status page with all the services you want to monitor can be publicly
accessible.

Related:

TODO add 'Server service: add kuma'
* [20240104161046](/20240104161046/) Fresh Server: First Steps
* [20240110232911](/20240110232911/) LAN Server Install: Setup and Install dietpi

Tags:

    #linux #server #monitoring #dashboard #uptime #networking
