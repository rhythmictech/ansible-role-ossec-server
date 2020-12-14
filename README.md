# ansible-role-ossec-server
![Molecule Test](https://github.com/rhythmictech/ansible-role-ossec-server/workflows/Molecule%20Test/badge.svg)

This role will install the [OSSEC Server](https://www.ossec.net/).

## Requirements
This role requires Ansible 2.8 or higher. It will run on:

 * Amazon Linux 2
 * CentOS/RedHat 7/8
 * Debian 7/8
 * Ubuntu 18.04 LTS

## Dependencies
Postfix must be installed and configured. This module is tested with the [ansible-role-postfix](https://github.com/rhythmictech/ansible-role-postfix) role.

## Example Playbook
```
- hosts: ossec_server
  vars_files:
    - vars/main.yml
  roles:
    - ansible-role-ossec-server

ossec_server_config:
      mail_to:
        - noone@rhythmic.net
      mail_smtp_server: localhost
      mail_from: noreply@rhythmic.net
      frequency_check: 604800
      directories:
        - check_all: 'yes'
          dirs: /etc,/usr/bin,/usr/sbin
        - check_all: 'yes'
          dirs: /bin,/sbin
      localfiles:
        - format: 'syslog'
          location: '/var/log/syslog'
        - format: 'syslog'
          location: '/var/log/auth.log'
      globals:
        - '127.0.0.1'
      connection: 'secure'
      log_level: 1
      email_level: 7
      ignore_files:
        - /etc/mtab
        - /etc/mnttab
        - /etc/hosts.deny
    ossec_agent_configs:
      - type: os
        type_value: linux
        frequency_check: 79200
        ignore_files:
          - /etc/mtab
          - /etc/mnttab
          - /etc/hosts.deny
          - /etc/mail/statistics
          - /etc/svc/volatile
        directories:
          - check_all: 'yes'
            dirs: /etc,/usr/bin,/usr/sbin
          - check_all: 'yes'
            dirs: /bin,/sbin
        localfiles:
          - format: 'syslog'
            location: '/var/log/syslog'
          - format: 'syslog'
            location: '/var/log/auth.log'
          - format: 'syslog'
            location: '/var/log/mail.log'
```

## Testing
This module is tested with [Molecule](https://molecule.readthedocs.io/en/latest/). Tests run against the following OS releases using Docker images provided by [geerlingguy](https://www.jeffgeerling.com/).

* Amazon Linux 2
* CentOS 7/8
* Debian 8
* CentOS 7

## License
MIT

## Attribution
This role was originally forked from [dj-wasabi](https://github.com/dj-wasabi/ansible-ossec-server).
