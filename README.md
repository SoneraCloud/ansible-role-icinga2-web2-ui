Role Name
=========

Ansible role to install Icinga2 Web2 Ui based on the original: https://github.com/Icinga/icinga2-ansible

This fork extends the CentOS/RHEL functionality to include:
 - Fully automated install of web2 with no setup wizzard steps (thanks to https://github.com/bennythejudge/icinga2-ansible/tree/master/icinga2-ansible-web2-ui for the inspiration)
 - Fully automated install of director (thanks to https://github.com/mamercad/ansible-icinga2-server for the inspiration)

Requirements
------------

On CentOS/RH: EPEL

On Fedora from version 22: `python-dnf`

Mysql or MariaDB - Httpd

PHP (is going to be installed as a dependency of icingaweb2)

Dependencies
------------

Roles:
 - https://github.com/SoneraCloud/ansible-role-icinga2-no-ui

Example Playbook
----------------

```yaml
---
- hosts: MonitoringServers
  roles:

   - role: icinga2-ansible-no-ui
     icinga2_conf_global: |
       include "constants.conf"
       include "zones.conf"
       include <itl>
       include <plugins>
       include "features-enabled/*.conf"
       include_recursive "conf.d"
     check_commands:
       check_nrpe: |
          "-H", "$address$",
              "-c", "$remote_nrpe_command$",
     tags: icinga2-no-ui

   - role: icinga2-ansible-web2-ui
     icinga2_db_pass: "CHANGEME"
     icinga2_web2_db_pass: "CHANGEME"
     icinga2_ido_mysql_configuration: |
       library "db_ido_mysql"

       object IdoMysqlConnection "ido-mysql" {
         user = "{{ icinga2_db_user }}"
         password = "{{ icinga2_db_pass }}"
         host = "localhost"
         database = "{{ icinga2_db }}"
       }
     tags: icinga2-ansible-web2-ui

```

Role Variables
--------------

See `defaults/main.yml`

License
-------

GNU General Public License Version 2

Author Information
------------------

Valentino Gagliardi - Icinga Dev Team
Peter Jenkins
