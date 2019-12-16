certbot_certs
=========

This role creates a Let's Encrypt TLS certificate and its private key using the certbot tool.

The following files are created:

- /etc/letsencrypt/live/{{ certbot_hostname }}/privkey.pem
- /etc/letsencrypt/live/{{ certbot_hostname }}/cert.pem
- /etc/letsencrypt/live/{{ certbot_hostname }}/chain.pem
- /etc/letsencrypt/live/{{ certbot_hostname }}/fullchain.pem

which are symlinks to files in /etc/letsencrypt/archive/{{ certbot_hostname }} directory.

The ownership is set to root:ssl-cert for all of them, and permissions are set to 0750 for directories
and 0640 for files, so that PostgresSQL database server can use the files.

Role Variables
--------------

* **certbot_hostname** - the host name for the certificate, default is {{ inventory_hostname }}, i.e. the hostname used for accessing the machine by Ansible
* **certbot_account_options** - default is "--register-unsafely-without-email", replace with "--email &lt;your email>" for registering properly

Example Playbook
----------------


    - hosts: all
      vars: 
        certbot_hostname: cloud1.perun-aai.org
        certbot_account_options: --email john@gmail.com
      roles:
         - cesnet.certbot_certs

License
-------

Apache

Author Information
------------------

Martin Kuba <makub@cesnet.cz>, CESNET
