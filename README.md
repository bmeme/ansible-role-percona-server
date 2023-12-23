# Ansible Role: Percona Mysql Server
![Workflow](https://github.com/bmeme/ansible-role-percona-server/actions/workflows/ci.yml/badge.svg?branch=main)
[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://GitHub.com/Naereen/StrapDown.js/graphs/commit-activity)
[![MIT license](https://img.shields.io/badge/License-MIT-blue.svg)](https://lbesson.mit-license.org/)

Installs and configures Percona MySQL server on EL 8/9 and Ubuntu 20.4/22.04.

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
Global variables are:

```yaml
## Percona enable mysql at startup
percona_enabled_on_startup: true
```
Enable Percona MySQL Server daemon at boot. Default at `true`

```yaml
## Percona root user/password 
percona_set_root_password: true
percona_root_user: "root"
percona_root_password: "S3cr3tS/$"
```
Update root password during the role execution. This role will only change root user password when MySQL is configured. This could be particularly useful in EL environments, where at first run a random password is created and stored in the logfile. Default at `true`.

```yaml
## Percona database settings
percona_db_enabled: false
percona_db_name: "default_db"
percona_db_user: "default_user"
percona_db_password: "Ch4ng3m3/*"
percona_db_host: "localhost"
```
Create a database during the role execution. Default at `false`

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
Basic Mysqld configuration. 

```yaml
# All specific configuration items
# Example:
#
# mysql_server_configuration: |-
#   max_allowed_packet=128M
#
mysql_server_configuration: ""
```
This allows to add custom configuration to `mysqld.conf` file, stored in `/etc/percona-server.conf.d` (RedHat) or `/etc/mysql/percona-server.conf.d` (Ubuntu) directory. Uncomment `mysql_server_configuration` line and add `mysqld` configuration items exactly as if you were putting them in `mysqld.conf` file.

Here an example of advanced configurations:

```yaml
mysql_server_configuration: |-
  max_allowed_packet=64M
  some_other_conf=some_other_value
```

## How to set Percona Mysql Server version to install
This role allows you to install both `5.7` and `8.0` version of Percona Mysql Server. Choose software version using `percona_version` variable. Default at `57`. 

## Supported Linux distro and versions
- EL 8
- EL 9
- Ubuntu 20.04
- Ubuntu 22.04

EL 7 is no longer officially supported. As far as we know, the role can currently continue to work properly on EL 7.9; however, we're uncertain whether it works on lower versions.

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

    percona_db_enabled: true
    percona_set_root_password: true
    percona_version: "80" # To install Percona Mysql Server 8

## License
MIT

## Author Information
This role was created in 2022 by [Bmeme](https://www.bmeme.com). It is actually maintained by [Daniele Piaggesi](https://github.com/g0blin79) and [Roberto Mariani](https://github.com/jean-louis).

## Credits
This role was really inspired by [geerligguy Ansible Role Mysql](https://github.com/geerlingguy/ansible-role-mysql).