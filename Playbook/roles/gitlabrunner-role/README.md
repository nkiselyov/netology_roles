gitlabrunner-role
=========

This role can install and register GitLab shared runner (with docker executor) on CentOS 7

Role Variables
--------------

| Var | Description | Path | Default value |
|---|---|---|---|
| gitlab_url | Gitlab url | defaults/main.yml | http://example.com |
| gitlab_token | Gitlab shared runners token | defaults/main.yml | exampleToken |
| runner_tags | Tags that are associated with runner | defaults/main.yml | - docker <br> - prod |
| runner_executor | Specify your execution environment | defaults/main.yml | docker |
| runner_description | Runner description | defaults/main.yml | runner-{{ ansible_hostname }} |
| runner_image | Specify default docker image | defaults/main.yml | alpine:latest |
| gitlab_external_host | Specify gitlab external host. Uses for hosts file and extra_host parameter as runner resolve gitlab host with internal IP | defaults/main.yml | "example.com" |
| gitlab_external_ip | Specify gitlab external IP. Uses for hosts file and extra_host parameter as runner resolve gitlab host with internal IP | defaults/main.yml | "" |

Example Playbook
----------------

`group_vars/gitlabrunner.yml`

```yml
gitlab_url: "http://192.168.35.12"
gitlab_token: "5A2jF7ka5bY"
gitlab_external_host: "gitlab.example.com"
gitlab_external_ip: "1.1.1.1"
```

`site.yml`

```yml
- name: gitlabrunner
  hosts: gitlabrunner
  roles:
    - gitlabrunner-role
```

License
-------

MIT

Author Information
------------------

Nikita Kiselyov \
nkiselyov94@gmail.com
