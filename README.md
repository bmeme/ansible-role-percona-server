# Ansible Role: Percona Mysql Server
![Workflow](https://github.com/bmeme/ansible-role-percona-server/actions/workflows/ci.yml/badge.svg?branch=main)
[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://GitHub.com/Naereen/StrapDown.js/graphs/commit-activity)
[![MIT license](https://img.shields.io/badge/License-MIT-blue.svg)](https://lbesson.mit-license.org/)

Installs and configures Percona MySQL Server on Enterprise Linux and Ubuntu systems.

| Role Series | Percona Server version | Supported OS | Python | Ansible | Status |
|-------------|------------------------|--------------|--------|---------|--------|
|1.x|5.7/8.0|EL 7 / 8 · Ubuntu 20.04 / 22.04|3.6–3.9|<2.15|_Legacy_ (EOL for Percona 5.7)|
|2.x|8.x only|EL 9 · Ubuntu 24.04 (noble)|3.10 / 3.11|>2.15|_Current / Maintained_

> Version 2.x drops support for Percona 5.7 and older platforms. EL 9 and Ubuntu 24.04 (noble) are the only supported targets. Python 3.10 / 3.11 are required on the controller.

## Requirements
No special requirements.

This role must run with root privileges, so either set a global `become: yes` or call it as:

```yaml
- hosts: database
  roles:
    - role: bmeme.percona_server
      become: yes
```

## Installation
```shell
ansible-galaxy install bmeme.percona_server
```

To update an existing installation:
```shell
ansible-galaxy install --force bmeme.percona_server
```

## Role Variables

### General

```yaml
## Percona enable mysql at startup
percona_enabled_on_startup: true
```
Enable the Percona MySQL Server service at boot. Default:`true`

### Root account

```yaml
## Percona root user/password 
percona_set_root_password: true
percona_root_user: "root"
percona_root_password: "S3cr3tS/$"
```
Change the root password during role execution (useful on EL where a random password is initially generated).

### Optional database creation

```yaml
## Percona database settings
percona_db_enabled: false
percona_db_name: "default_db"
percona_db_user: "default_user"
percona_db_password: "Ch4ng3m3/*"
percona_db_host: "localhost"
```
### Base configuration

```yaml
# This variable specifies the server ID. 
# In MySQL 5.7, server_id must be specified if binary logging is enabled, otherwise the server is not allowed to start.
# server_id is set to 0 by default
mysql_server_id: ""

# Datafile directory.
mysql_datadir: "/var/lib/mysql"

# Address on which mysql bind to
mysql_bind_address: "127.0.0.1"

# Mysql symbolic links configuration
mysql_symbolic_links: 0
```
### Extra mysqld settings

```yaml
# All specific configuration items
# Example:
#
# mysql_server_configuration: |-
#   max_allowed_packet=128M
#
mysql_server_configuration: ""
```
These options are appended to the mysqld.cnf inside the Percona configuration include directory (/etc/percona-server.conf.d on EL, /etc/mysql/percona-server.conf.d on Ubuntu).

Example:

```yaml
mysql_server_configuration: |-
  max_allowed_packet=64M
  some_other_conf=some_other_value
```

## How to select Percona Server version
Role 1.x supported both `5.7` and `8.0` through the variable:
```yaml
percona_version: "80" # or "57"
```
Role 2.x installs only Percona Server 8.x, so the variable is no longer required.

## Supported Platforms
- EL 9 (Rocky Linux 9 / Alma Linux 9 / RHEL 9)
- Ubuntu 24.04 LTS (noble)

Older releases (EL 7/8, Ubuntu 20.04/22.04) are supported only by the 1.x role branch.

## Dependencies
None

## Example Playbook
    - hosts: db-servers
      become: yes
      vars_files:
        - vars/main.yml
      roles:
        - { role: bmeme.percona_server }

Example vars/main.yml:

    percona_db_enabled: true
    percona_set_root_password: true
    percona_version: "80" # To install Percona Mysql Server 8

## License
MIT

## Author Information
This role was created in 2022 by [Bmeme](https://www.bmeme.com). It is actually maintained by [Daniele Piaggesi](https://github.com/g0blin79) and [Roberto Mariani](https://github.com/jean-louis).

## Credits
Inspired by [geerligguy Ansible Role Mysql](https://github.com/geerlingguy/ansible-role-mysql).