Etcd
=========

Ansible role to install Etcd.

Requirements
------------

Ansible >= 1.2

Role Variables
--------------

| Name | Description | Default value |
|:-----  | :----- | :----- |
| etcd_bin_directory | Path where binaries files are located | ~/etcd/bin
| etcd_git_repository | Url to Etcd Git Repository file | https://github.com/coreos/etcd.git |
| etcd_install | Boolean to define if installation should be done | true |
| etcd_install_create_symlink | Boolean to define if symlinks to the binary files should be created | true |
| etcd_install_symlink_path | Path where to create symlinks | /usr/local/bin |
| etcd_install_version | Tag used for checkout | HEAD |
| etcd_target_directory | Directory where to checkout Etcd and run the build command | ~/etcd |

Dependencies
------------

Git must be installed on the system.

Example Playbook
----------------

Install Etcd on local machine.

    - hosts: 127.0.0.1
      connection: local
      sudo: yes
      roles:
         - { role: etcd }

License
-------

BSD

Author Information
------------------
[Thomas Krahn]

[Thomas Krahn]: mailto:ntbc@gmx.net
