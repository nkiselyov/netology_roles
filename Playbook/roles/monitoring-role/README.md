monitoring-role
=========

This role can

- install and configure `Prometheus` server with [Awesome Prometheus alerts](https://awesome-prometheus-alerts.grep.to/rules.html)on CentOS 7
- install and configure `Alertmanager` on CentOS 7
- install and configure `Grafana` with Prometheus as default datasource on CentOS 7
- install and configure `Node Exporter` with [Node Exporter Full](https://grafana.com/grafana/dashboards/1860) dashboard on CentOS 7
- install and configure `Mysql Exporter` with [MySQLd Mixin](https://github.com/prometheus/mysqld_exporter/tree/main/mysqld-mixin) dashboard on CentOS 7

Role Variables
--------------

 **Var**                         | **Description**                                                                                                                                                              | **Path**          | **Default value**
---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------|--------------------------------------
 prometheus_folder               | Location to store Prometheus db files                                                                                                                                        | defaults/main.yml | /var/lib/prometheus
 prometheus_config_folder        | Location to store Prometheus config files                                                                                                                                    | defaults/main.yml | /etc/prometheus
 prometheus_install              | If `true` - Prometheus will be downloaded and installed                                                                                                                      | defaults/main.yml | true
 prometheus_version              | Prometheus version to be installed                                                                                                                                           | defaults/main.yml | 2.37.0
 prometheus_scrape_configs       | Scrape config                                                                                                                                                                | defaults/main.yml | see below this table |
 is_prometheus                   | If `false` - no tasks for Prometheus will be started. This variable is used to mark servers which wiil be host Prometheus                                                    | defaults/main.yml | false
 alertmanager_folder             | Location to store Alertmanager files                                                                                                                                         | defaults/main.yml | /var/lib/alertmanager
 alertmanager_config_folder      | Location to store Alertmanager config files                                                                                                                                  | defaults/main.yml | /etc/alertmanager
 alertmanager_install            | If `true` - Alertmanager will be downloaded and installed                                                                                                                    | defaults/main.yml | true
 alertmanager_version            | Alertmanager version to be installed                                                                                                                                         | defaults/main.yml | 0.24.0
 is_alertmanager                 | If `false` - no tasks for Alermanager will be started. This variable is used to mark servers which wiil be host Alertmanager                                                 | defaults/main.yml | false
 nodeexporter_folder             | Location to store Node Exporter files                                                                                                                                        | defaults/main.yml | /var/lib/nodeexporter
 nodeexporter_config_folder      | Location to store Node Exporter config files                                                                                                                                 | defaults/main.yml | /etc/nodeexporter
 nodeexporter_install            | If `true` - Node Exporter will be downloaded and installed                                                                                                                   | defaults/main.yml | true
 nodeexp_version                 | Node Exporter version to be installed                                                                                                                                        | defaults/main.yml | 1.3.1
 is_nodeexporter                 | If `false` - no tasks for Node Exporter will be started. This variable is used to mark servers which wiil be host Node Exporter                                              | defaults/main.yml | false
 grafana_dashboards_location     | Location where to store Grafana dashboards files                                                                                                                             | defaults/main.yml | /etc/grafana/provisioning/dashboards
 is_grafana                      | If `false` - no tasks for Grafana will be started. This variable is used to mark servers which wiil be host Grafana                                                          | defaults/main.yml | false
 mysqldexporter_folder           | Location to store Mysql Exporter files                                                                                                                                       | defaults/main.yml | /var/lib/mysqldexporter
 mysqldexporter_config_folder    | Location to store Mysql Exporter config files                                                                                                                                | defaults/main.yml | /etc/mysqldexporter
 mysqldexporter_install          | If `true` - Mysql Exporter will be downloaded and installed                                                                                                                  | defaults/main.yml | true
 mysqldexporter_version          | Mysql Exporter version to be installed                                                                                                                                       | defaults/main.yml | 0.14.0
 mysqldexporter_mysql_user       | MySQL user that will be created for monitoring                                                                                                                               | defaults/main.yml | mysqldexporter
 mysqldexporter_mysql_pass       | Password for MySQL user for monitoring. **Important!** It`s highly recommended to override this value and use encryption in your vars. Do not store it as plain text         | defaults/main.yml | mysqldexporter
 mysqldexporter_mysql_host       | MySQL host which exporter wiil use to connect                                                                                                                                | defaults/main.yml | localhost
 mysqldexporter_mysql_port       | MySQL port for `mysqldexporter_mysql_host`                                                                                                                                   | defaults/main.yml | 3306
 mysqldexporter_mysql_login_user | MySQL user for connection to MySQL instance with rights to create accounts and grant permissions. Used to create MySQL account for monitoring                                | defaults/main.yml | root
 mysqldexporter_mysql_login_pass | Password for `mysqldexporter_mysql_login_user`. **Important!** It`s highly recommended to override this value and use encryption in your vars. Do not store it as plain text | defaults/main.yml | PassW0rd_
 is_mysqldexporter               | If `false` - no tasks for Mysql Exporter will be started. This variable is used to mark servers which wiil be host Mysql Exporter                                            | defaults/main.yml | false

`prometheus_scrape_configs`

```yml
  - job_name: "prometheus"
    static_configs:
      - targets: ["localhost:9090"]
  - job_name: "nodeexporter"
    file_sd_configs:
      - files:
          - {{ prometheus_config_folder }}/targets/node.yml
  - job_name: "mysqldexporter"
    file_sd_configs:
      - files:
          - {{ prometheus_config_folder }}/targets/mysql.yml
```

Example Playbook
----------------

`group_vars/all.yml`

```yml
is_nodeexporter: true
```

`group_vars/mysql.yml`

```yml
is_mysqldexporter: true
```

`group_vars/monitoring.yml`

```yml
is_alertmanager: true
is_prometheus: true
is_grafana: true
```

`inventory/prod.yml`

```yml
mysql:
  hosts:
    prod-db01:
      ansible_host: 192.168.136.11
    prod-db02:
      ansible_host: 192.168.136.12
monitoring:
  hosts:
    prometheus-01:
      ansible_host: 192.168.136.13
```

`site.yml`

```yml
- name: monitoring
  hosts: all
  roles:
    - monitoring-role
```

License
-------

MIT

Author Information
------------------

Nikita Kiselyov \
nkiselyov94@gmail.com
