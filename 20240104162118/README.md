# Add https cert for nginx

> 🧐 on debian/ubuntu server you should already have a systemd timer running by
> default after installing certbot. Check with `systemctl list-timer` or `systemctl status
> certbot.timer`, as a backup there is even `/etc/cron.d/certbot` a cronjob that
> runs if systemd service is disabled or failed.

1. install `apt install python3-certbot-nginx`
1. enable https port: `ufw allow 443`
1. run auto-config: `certbot --nginx` (select the site you want or all, if it fails try a couple of times)
1. restart service: `systemctl restart nginx`

Depending on whether or not auto cert renewal is already enabled:

1. add cronjob for auto cert renewal: `crontab -e`

```
#run twice a day at 12:23 00:23
23 0,12 * * * certbot renew --quiet --deploy-hook "systemctl reload nginx"
```

2. check if added: `crontab -l`

Related:

* TODO: add hovs vs container zet, and a bunch of others here
* <https://serverfault.com/questions/790772/best-practices-for-setting-a-cron-job-for-lets-encrypt-certbot-renewal>

Tags:

      #linux #server #webserver #nginx
