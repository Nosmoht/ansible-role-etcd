Etcd
=========

Ansible role to install Etcd and set key/values.

Requirements
------------

Ansible >= 1.2

Role Variables
--------------

| Name | Description | Default value |
|:-----  | :----- | :----- |
| etcd_bin_directory | Path where binaries files are build | /opt/etcd/bin
| etcd_git_repository | Url to Etcd Git Repository file | git@github.com:coreos/etcd.git |
| etcd_install | Boolean to define if installation should be done | true |
| etcd_target_directory | Directory where to checkout Etcd and run the build command | /opt/etcd |

Dependencies
------------

Git must be installed on the system.

Example Playbook
----------------

Install Etcd on local machine into /opt/etcd

    - hosts: 127.0.0.1
      connection: local
      roles:
         - { role: etcd }

License
-------

BSD

Author Information
------------------
[Thomas Krahn]

[Thomas Krahn]: mailto:ntbc@gmx.net
