# Add https cert for nginx

> 🧐 on debian/ubuntu server you should already have a systemd timer running by
> default after installing certbot. Check with `systemctl list-timer` or `systemctl status
> certbot.timer`, as a backup there is even `/etc/cron.d/certbot` a cronjob that
> runs if systemd service is disabled or failed.

1. Install `apt install python3-certbot-nginx`
1. Enable https port: `ufw allow 443`
1. Run auto-config: `certbot --nginx`
1. Restart service: `systemctl restart nginx`

Depending on whether or not auto cert renewal is already enabled:

1. Add cronjob for auto cert renewal: `crontab -e`

```
#run twice a day at 12:23 00:23
23 0,12 * * * certbot renew --quiet --deploy-hook "systemctl reload nginx"
```

2. Check if added: `crontab -l`

Related:

* <https://serverfault.com/questions/790772/best-practices-for-setting-a-cron-job-for-lets-encrypt-certbot-renewal>
* [20240104161046](/20240104161046/) Fresh Server: First Steps
* [20240104194645](/20240104194645/) Grok nginx
* [20240104154437](/20240104154437/) Server Service: Add nginx
* [20240104161534](/20240104161534/) How to run nginx? Host vs Container

Tags:

      #linux #server #webserver #nginx
