Nginx-role
=========

This role can install and configure NGINX as reverse proxy on Centos 7

Role Variables
--------------

| Var                  | Description                                                                                                                                                                                 | Path            | Default value                                                                                                         |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|-----------------------------------------------------------------------------------------------------------------------|
| email                | Used by Certbot for certificate generation. Email address for important account notifications                                                                                              | defaults/main.yml | someone@example.com                                                                                                   |
| reverse_proxy_config | Config file template for reverse proxy upstreams. For each upstream the role creates config file /etc/nginx/conf.d/{{ domain }}.conf. By default NGINX include all /etc/nginx/conf.d/*.conf | defaults/main.yml | see below this table                                                                                                                       |
| sites                | List of parameters for reverse proxy upstreams. Specified parameters will be used for reverse_proxy_config                                                                                   | defaults/main.yml |                                                                                                                       |
| domain               | Domain name for upstreams behind reverse proxy                                                                                                                                               | defaults/main.yml | www.example.com                                                                                                       |
| upstreams            | Backend settings for upstreams (backend pool servers). Should contain address and port                                                                                                                            | defaults/main.yml | - { backend_address: 192.168.101.22, backend_port: 80 }<br> - { backend_address: 192.168.101.22, backend_port: 8080 } |
| proxy_pass           | Point address where to proxy requests. Usually protocol + `domain` (backend group name)                                                                                                                                                      | defaults/main.yml | <http://www.example.com>                                                                                                 |
| new_cert             | Boolean. Specifies that a Let`s Encrypt certificate will be issued for this domain name. Certbot also configures nginx settings to redirect HTTP to HTTPS                                                                                                         | defaults/main.yml | false                                                                                                                 |
| add_config           | Boolean. Specifies that there will be created config file for upstream in directory /etc/nginx/conf.d/. If set to `False` no config files will be created or updated                       | defaults/main.yml | false                                                                                                                 |

Variable `reverse_proxy_config`

``` text
  upstream {{ item.domain }} {
  {% for upstream in item.upstreams %}
      server {{ upstream.backend_address }}:{{ upstream.backend_port }};
  {% endfor %}
  }

  server {
      listen 80;
      server_name {{ item.domain }};
      location / {
         proxy_pass {{ item.proxy_pass }};
         proxy_set_header        X-Real-IP $remote_addr;
         proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
         proxy_set_header        X-Forwarded-Proto $scheme;
         proxy_set_header        Host $host;
      }
  }

```

Example Playbook
----------------

`group_vars`

```yaml
---
email: nkiselyov94@gmail.com

sites:
  - domain: www.example.com
    upstreams:
      - { backend_address: host1.example.com, backend_port: 80 }
    proxy_pass: http://www.example.com
    new_cert: true
    add_config: true
  - domain: gitlab.example.com
    upstreams:
      - { backend_address: host2.example.com, backend_port: 80 }
      - { backend_address: host3.example.com, backend_port: 80 }
    proxy_pass: http://gitlab.example.com
    new_cert: false
    add_config: false
```

```yaml
- name: Install Nginx
  hosts: nginx
  roles:
    - nginx-role
```

License
-------

MIT

Author Information
------------------

Nikita Kiselyov \
nkiselyov94@gmail.com
