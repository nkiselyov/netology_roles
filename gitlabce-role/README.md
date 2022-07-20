gitlabce-role
=========

This role install and configure Gitlab-CE on CentOS 7
Bundled prometheus_monitoring is disabled.

Role Variables
--------------

| Var | Description | Path | Default value |
|---|---|---|---|
| external_url | URL to reach the repository | defaults/main.yml | http://example.com |
| reverse_proxy_address | Address of reverse proxy server. If specified - the IP address of the proxy will not show up as the client address | defaults/main.yml |  |
| gitlab_initial_pass | Password that will be set for default `root` user. Only applies on first run `gitlab-ctl reconfigure`.**Important!** It`s highly recommended to override this value and use encryption in your vars. Do not store it as plain text. | defaults/main.yml |  |
| gitlab_initial_runner_registration_token | Initial token for shared runners registration. **Important!** It`s highly recommended to override this value and use encryption in your vars. Do not store it as plain text. | defaults/main.yml |  |
| gitlab_listen_port | Port on which gitlab listen requests | defaults/main.yml | 80 |
| gitlab_listen_https | Disable or enable bundled NGINX to handle SSL termination | defaults/main.yml | "false" |

Example Playbook
----------------

`group_vars/gitlabce.yml`

```yml
gitlab_initial_runner_registration_token: "5A2jF7ka5bY"
reverse_proxy_address: "192.168.122.33"
external_url: "https://gitlab.example.com"
```

`site.yml`

```yml
- name: gitlab
  hosts: gitlabce
  roles:
    - gitlabce-role
```

License
-------

MIT

Author Information
------------------

Nikita Kiselyov \
nkiselyov94@gmail.com
