# Ansible Role: Percona Mysql Server

[![CI](https://github.com/bmeme/ansible-role-percona-server/workflows/CI/badge.svg?event=push)](https://github.com/bmeme/ansible-role-percona-server/actions?query=workflow%3ACI)

Installs and configures Percona MySQL server 5.7 on RHEL/CentOS (Debian/Ubuntu is here to come...).

## Requirements

No special requirements; note that this role requires root access, so either run it in a playbook with a global `become: yes`, or invoke the role in your playbook like:

```yaml
- hosts: database
  roles:
    - role: bmeme.percona_server
      become: yes
```

## Installation

This is an Ansible role distributed using Ansible Galaxy. In order to install this role you can use the following command.

`$ ansible-galaxy install bmeme.percona_server`

## Update

If you want to update the role, you need to pass --force parameter when installing. Please, check the following command:

`$ ansible-galaxy install --force bmeme.percona_server`

## Role Variables

Basic variables are:

Available variables are listed below, along with default values (see `defaults/main.yml`):

Packages settings:

```yaml
percona_repo_baseurl: "https://repo.percona.com/yum"
percona_repo_release: "percona-release-latest"
percona_version: 57
percona_packages:
  - "Percona-Server-server-{{ percona_version }}"
  - "Percona-Server-devel-{{ percona_version }}"
  - "Percona-Server-client-{{ percona_version }}"
```

Percona `mysqld` daemon settings and startup management

```yaml
percona_mysql_daemon: mysqld
percona_enabled_on_startup: true
```

Percona MySQL base configuration settings

```yaml
mysql_config_file: /etc/my.cnf
mysql_config_include_dir: /etc/percona-server.conf.d
mysql_server_id: ""
mysql_socket: "/var/lib/mysql/mysql.sock"
mysql_datadir: "/var/lib/mysql"
mysql_bind_address: "127.0.0.1"
mysql_log_error: "/var/log/mysqld.log"
mysql_pid_file: "/var/run/mysqld/mysqld.pid"
mysql_symbolic_links: 0
```

Percona free configurations

```yaml
mysql_server_configuration: ""
```

Here an example of free configurations:

```yaml
mysql_server_configuration: |-
  max_allowed_packet=64M
  some_other_conf=some_other_value
```

To change root password during the execution of this role. 

```yaml
## Percona root user/password
percona_set_root_password: true
percona_root_user: "root"
percona_root_password: "S3cr3tS/$"
```

To create a database during the execution of this role.

```yaml
## Percona database settings
percona_db_enabled: false
percona_db_name: "default_db"
percona_db_user: "default_user"
percona_db_password: "Ch4ng3m3/*"
percona_db_host: "localhost"
```

## Dependencies

N/A

## Example Playbook

    - hosts: db-servers
      become: yes
      vars_files:
        - vars/main.yml
      roles:
        - { role: bmeme.percona_server }

*Inside `vars/main.yml`*:

    percona_db_enabled: false
    percona_db_name: "my_new_db"
    percona_db_user: "my_new_db_user"
    percona_db_password: "Ch4ng3m3/*"
    percona_db_host: "%"

## License

MIT / BSD

## Author Information

This role was created in 2022 by [Bmeme](https://www.bmeme.com).

## Credits

 Thanks to the great works of [geerligguy Ansible Role Mysql](https://github.com/geerlingguy/ansible-role-mysql).