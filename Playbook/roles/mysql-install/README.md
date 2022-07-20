mysql-install
=========

This role can install MySQL 8 and PyMySQL on CentOS 7 \
Also set root password with `mysql_native_password` authentication plugin, removes all anonymous user accounts and the MySQL test database.

Requirements
------------

Role uses `community.mysql` modules. Requirements for this collection must to be used:
`mysqlclient` (Python 3.5+) or \
`PyMySQL` (Python 2.7 and Python 3.x) or \
`MySQLdb` (Python 2.x)

Also there is only support for the `mysql_native_password` encrypted password hash module

Role Variables
--------------

| Var  | Description  | Path  | Default value  |
|--- |--- |--- |--- |
| install_mysql  | Parameter for MySQL installation. If `True` MySQL packages will be installed.  | defaults/main.yml  | true  |
| initial_setup  | Parameter for initial setup MySQL. If `True` role will check `/var/log/mysqld.log` for temporary password and change it with `mysql_root_password` value. Also all anonymous user accounts and  the MySQL test database will be removed.  | defaults/main.yml  | true  |
| mysql_rpm  | MySQL RPM package name.  | defaults/main.yml  | mysql80-community-release-el7-6.noarch.rpm  |
| mysql_root_password  | Password that will be set for root while initial setup. **Important!** It`s highly recommended to override this value and use encryption in your vars. Do not store it as plain text.  | defaults/main.yml  | PassW0rd_  |

Example Playbook
----------------

```yaml
- name: Install MySQL
  hosts: mysql
  roles:
    - mysql-install
```

License
-------

MIT

Author Information
------------------

Nikita Kiselyov \
nkiselyov94@gmail.com
