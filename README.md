ansible-role-nginx_revproxy
=========

Install and configures Nginx as reverse proxy for multiple website.

|GitHub|Quality|Downloads|Galaxy|Version|
|------|-------|---------|-------|-------|
|[![CI](https://github.com/hispanico/ansible-role-nginx_revproxy/actions/workflows/ci.yml/badge.svg)](https://github.com/hispanico/ansible-role-nginx_revproxy/actions/workflows/ci.yml)|[![quality](https://img.shields.io/ansible/quality/53382)](https://galaxy.ansible.com/hispanico/nginx_revproxy)|[![downloads](https://img.shields.io/ansible/role/d/53382)](https://galaxy.ansible.com/hispanico/nginx_revproxy)|[![Galaxy](https://img.shields.io/badge/galaxy-hispanico.nginx_revproxy-blue.svg)](https://galaxy.ansible.com/hispanico/nginx_revproxy)|[![Version](https://img.shields.io/github/release/hispanico/ansible-role-nginx_revproxy.svg)](https://github.com/hispanico/ansible-role-nginx_revproxy/releases/)|

Requirements
------------

This role requires Ansible 2.4 or higher.

Role Variables
--------------

Default values:

```yaml
nginx_revproxy_sites:                                         # List of sites to reverse proxy
  default:                                                    # Set default site to return 444 (Connection Closed Without Response)
    ssl: false                                                # Set to True if you want to redirect http to https
    letsencrypt: false

  example.com:                                                # Domain name
    domains:                                                  # List of server_name aliases
      - example.com
      - www.example.com
    upstreams:                                                # List of Upstreams
      - { backend_address: 192.168.0.100, backend_port: 80 }
      - { backend_address: 192.168.0.101, backend_port: 8080 }
    auth:                                                     # Define this block for a single HTTP user/password, or leave undefined for unauthenticated vhosts
      login: myusername
      password: mysecretpassword
    listen: 9000                                              # Specify which port you want to listen to with clear HTTP, or leave undefined for 80
    ssl: false                                                # Set to True if you want to redirect http to https
    letsencrypt: false                                        # Set to True if you want to use letsencrypt
    conn_upgrade: true                                        # Set the Connection upgrade header values

  example.org:                                                # Domain name
    domains:                                                  # List of server_name aliases
      - example.org
      - www.example.org
    upstreams:                                                # List of Upstreams
      - { backend_address: 192.168.0.200, backend_port: 80 }
      - { backend_address: 192.168.0.201, backend_port: 8080 }
    listen: 9000                                              # Specify which port you want to listen to with clear HTTP, or leave undefined for 80
    listen_ssl: 9001                                          # Specify which port you want to listen to with HTTPS, or leave undefined for 443
    ssl: true                                                 # Set to True if you want to redirect http to https
    ssl_certificate: /etc/ssl/certs/ssl-cert-snakeoil.pem     # ssl certificate, used if letsencrypt is false
    ssl_certificate_key: /etc/ssl/private/ssl-cert-snakeoil.key # ssl certificate key, used if letsencrypt is false
    letsencrypt: false                                        # Set to True if you want use letsencrypt
    letsencrypt_email: ""                                     # Set email for letencrypt cert

nginx_revproxy_certbot_auto: false                             # Set to true to install certbot-auto

nginx_revproxy_certbot_packages:                              # Install these packages from repo, when not using certbot-auto
  - certbot
  - python3-certbot-nginx
```

Dependencies
------------

None.

Example Playbook
----------------

```yaml
  - hosts: all
    roles:
      - hispanico.nginx_revproxy
    vars:
      nginx_revproxy_sites:
        default:
          ssl: false
          letsencrypt: false

        example.com:
          domains:
            - example.com
            - www.example.com
          upstreams:
            - { backend_address: 192.168.0.100, backend_port: 80 }
            - { backend_address: 192.168.0.101, backend_port: 80 }
          ssl: true
          letsencrypt: false
```

License
-------

Licensed under the GPLv3 License. See the LICENSE file for details.

Author Information
------------------

Hispanico
