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
| etcd_git_repository | Url to Etcd Git Repository file | git@github.com:coreos/etcd.git |
| etcd_target_directory | Directory where to checkout Etcd and run the build command | /opt/etcd |
| etcd_bin_directory | Path where binaries files are build | /opt/etcd/bin

Dependencies
------------

Git must be installed on the system.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

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
