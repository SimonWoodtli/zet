# LDAP Client Authentication Process

The graphic shows the information path used by LDAP/SSSD. Connecting to an LDAP
server starts with PAM, the pluggable authentication module. PAM is the default
authentication mechanism for Linux; LDAP and SSSD are extension modules added
to the default authentication configuration.

* PAM is configured to use the pam.sss.so module
* sss.so and sssd use a combined sssd and LDAP configuration file.

> 🧐 The /etc/sssd/conf.d/00-sssd.conf file has a 2 digit prefix to allow for
> sequencing if more than one configuration file is being used.

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

When you configure a client system for LDAP authentication, the following files are changed:

* /etc/sssd/conf.d/00-sssd.conf
* /etc/pam.d/common-session.conf (Ubuntu)
* /etc/pam.d/system-auth (Cent OS)

> 🧐 In previous distribution releases, there were specific utilities used for
> configuration. The adoption of sssd (System Security Services Daemon) and
> friends has greatly reduced the complexity of the client configuration and
> eliminated the need for specialized configuration tools. Text file
> configuration information may be configured with your favorite text editor.

Related:

* [20240321165126](/20240321165126/) 🖼️ LDAP

Tags:

    #linux #sysadmin #LDAP #authentication #sssd
