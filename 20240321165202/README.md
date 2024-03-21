# LDAP Client Authentication Process

The graphic shows the information path used by LDAP/SSSD. Connecting to an LDAP
server starts with PAM, the pluggable authentication module. PAM is the default
authentication mechanism for Linux; LDAP and SSSD are extension modules added
to the default authentication configuration.

* PAM is configured to use the pam.sss.so module
* sss.so and sssd use a combined sssd and LDAP configuration file.

example of /etc/sssd/conf.d/00-sssd.conf

```
[sssd]
config_file_version = 2
domains = example.com
services = nss, pam,autofs 

[domain/example.com]
enumerate = true
id_provider = ldap
autofs_provider = ldap
auth_provider = ldap
chpass_provider = ldap
ldap_uri = ldap://192.168.122.154/
ldap_search_base = dc=example,dc=com
ldap_id_use_start_tls = true
cache_credentials = True
ldap_tls_reqcert =allow
```


Related:

* [20240321165126](/20240321165126/) 🖼️ LDAP

Tags:

    #linux #sysadmin #LDAP #authentication
