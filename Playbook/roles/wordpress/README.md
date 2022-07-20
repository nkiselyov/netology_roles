wordpress
=========

This role configure Wordpress with Apache web server on CentOS 7

Role Variables
--------------

| Var | Description | Path | Default value |
|---|---|---|---|
| wordpress_install | Bool. Indicate that wordpress archive will be downloaded and unarchive (replace current files) | defaults/main.yml | true |
| wordpress_root_dir | Folder where to store Wordpress files | defaults/main.yml | /var/www/wordpress |
| wordpress_server_name | `ServerName` directive for Apache virtual host | defaults/main.yml | localhost |
| wordpress_db_name | DB name for Wordpress | defaults/main.yml | wordpress |
| wordpress_user_name | Username to connect to Wordpress DB | defaults/main.yml | wordpress |
| wordpress_user_pass | Password for username to connect to Wordpress DB. **Important!** It`s highly recommended to override this value and use encryption in your vars. Do not store it as plain text. | defaults/main.yml | wordpress |
| wordpress_db_address | Server with Wordpress DB | defaults/main.yml | localhost |
| wordpress_site_url | Address of WordPress (which visitors enter in browser) | defaults/main.yml | <https://www.example.com> |

Example Playbook
----------------

`inventory\prod.yml`

```yaml
wordpress:
  hosts:
    wordpress-01:
      ansible_host: 192.168.101.3
```

`group_vars\wordpress.yml`

```yaml
wordpress_site_url: "https://www.example.ru"
wordpress_db_address: 192.168.101.7
wordpress_server_name: 192.168.101.3
```

`playbook`

```yaml
- name: Wordpress Install
  hosts: Wordpress
  roles:
    - wordpress
```

License
-------

MIT

Author Information
------------------

Nikita Kiselyov \
nkiselyov94@gmail.com
