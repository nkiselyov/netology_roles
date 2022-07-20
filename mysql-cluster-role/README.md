mysql-cluster-role
=========

This role can

- install MySQL 8 and PyMySQL on CentOS 7 \
- set root password with `mysql_native_password` authentication plugin
- removes all anonymous user accounts and the MySQL test database
- configures two-node Primary - Replica cluster.
- create DB and user with full rights for this DB

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
| mysql_login_username  | Usename for MySQL instance connection  | defaults/main.yml  | root  |
| mysql_login_password  | Password for user to connect to MySQL instance. **Important!** It`s highly recommended to override this value and use encryption in your vars. Do not store it as plain text.  | defaults/main.yml  | PassW0rd_  |
| mysql_create_db  | Parameter for MySQL DB creation. If `True` DB with name `mysql_new_db_name`, user `mysql_new_user_name` with password `mysql_new_user_pass` will be created  | defaults/main.yml  | true  |
| mysql_new_db_name  | DB, that will be created if `mysql_create_db` set to `True`  | defaults/main.yml  | wordpress  |
| mysql_new_user_name  | User, that will be created if `mysql_create_db` set to `True`  | defaults/main.yml  | wordpress  |
| mysql_new_user_pass  | Password for user, that will be created if `mysql_create_db` set to `True`. **Important!** It`s highly recommended to override this value and use encryption in your vars. Do not store it as plain text.  | defaults/main.yml  | wordpress  |
| mysql_replication_username  | User for replication  | defaults/main.yml  | replica  |
| mysql_replication_password  | Pasword for replication user. **Important!** It`s highly recommended to override this value and use encryption in your vars. Do not store it as plain text.  | defaults/main.yml  | replica  |
| mysql_replication_role  | Role of server if cluster (`replica` or `primary`)  | defaults/main.yml  | this variable is empty  |
| mysql_replication_primary_server  | Address of `primary` node  | defaults/main.yml  | this variable is empty  |
| mysql_replication_user_host  | Host for replication user  | defaults/main.yml  | % (any host)  |
| mysql_my_cnf_location  | Location of MySQL config file  | defaults/main.yml  | /etc/my.cnf  |
| mysql_replica_server_id  | Replica server ID (will be used in config file)  | defaults/main.yml  | 101  |
| mysql_primary_server_id  | Primary server ID (will be used in config file)  | defaults/main.yml  | 1  |
| mysql_my_cnf_config  | Template for MySQL config file  | defaults/main.yml  | see below this table  |
| install_mysql  | Parameter for MySQL installation. If `True` MySQL packages will be installed.  | defaults/main.yml  | true  |
| initial_setup  | Parameter for initial setup MySQL. If `True` role will check `/var/log/mysqld.log` for temporary password and change it with `mysql_root_password` value. Also all anonymous user accounts and  the MySQL test database will be removed.  | defaults/main.yml  | true  |
| mysql_rpm  | MySQL RPM package name.  | defaults/main.yml  | mysql80-community-release-el7-6.noarch.rpm  |
| mysql_root_password  | Password that will be set for root while initial setup. **Important!** It`s highly recommended to override this value and use encryption in your vars. Do not store it as plain text.  | defaults/main.yml  | PassW0rd_  |

Example Playbook
----------------

`inventory\prod.yml`

```yaml
mysql:
  children:
    primary:
      hosts:
        mysql-01:
          ansible_host: 192.168.101.18
    replica:
      hosts:
        mysql-02:
          ansible_host: 192.168.101.19
```

`group_vars\primary.yml`

```yaml
mysql_replication_role: "primary"
```

`group_vars\replica.yml`

```yaml
mysql_replication_primary_server: "192.168.101.18"
mysql_replication_role: "replica"
```

`playbook`

```yaml
- name: Create MySQL cluster
  hosts: mysql
  roles:
    - mysql-cluster-role
```

License
-------

MIT

Author Information
------------------

Nikita Kiselyov \
nkiselyov94@gmail.com
