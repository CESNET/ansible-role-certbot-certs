certbot_certs
=========

This role creates a Let's Encrypt TLS certificate and its private key using the certbot tool.

The following files are created:

- /etc/letsencrypt/live/{{ certbot_certname }}/privkey.pem
- /etc/letsencrypt/live/{{ certbot_certname }}/cert.pem
- /etc/letsencrypt/live/{{ certbot_certname }}/chain.pem
- /etc/letsencrypt/live/{{ certbot_certname }}/fullchain.pem

which are symlinks to files in /etc/letsencrypt/archive/{{ certbot_certname }} directory.

The ownership is set to root:ssl-cert for all of them, and permissions are set to 0750 for directories
and 0640 for files, so that PostgresSQL database server can use the files.

Role Variables
--------------

* **certbot_hostname** - the host name for the certificate, default is {{ inventory_hostname }}, i.e. the hostname used for accessing the machine by Ansible
* **certbot_certname** - name used for the live and archive directories where files are stored, default is {{ certbot_hostname }}
* **certbot_account_options** - default is "--register-unsafely-without-email", replace with "--email &lt;your email>" for registering properly
* **certbot_prehook** - file with prehook, default is apache_prehook.sh stopping Apache web server
* **certbot_posthook** - file with posthook, default is apache_posthook.sh starting Apache web server
Simple Example Playbook
----------------

```yaml
    - hosts: all
      roles:
         - cesnet.certbot_certs
```


Example of More Complex Playbook
----------------

```yaml
    - hosts: 
        - "some-random-name.my-cloud.org"
      vars: 
        certbot_hostname: www.mynicename.net
        certbot_certname: mycert
        certbot_account_options: --email john@gmail.com
      roles:
         - cesnet.certbot_certs
```
License
-------

Apache

Author Information
------------------

Martin Kuba <makub@cesnet.cz>, CESNET
