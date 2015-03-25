# Etcd

This role can be used to do the following tasks:
- Checkout and build Etcd
- Install binaries on a system
- Setup an Etcd Cluster
- Manage key/value pairs of in Etcd

By default the role does nothing because _etcd_build_ and _etcd_install_ are set to false.

# Requirements

- Ansible >= 1.2
- Git and Go must be installed
- Requirements of [ansible-library-etcd] must be fulfilled if using variable __etcd_keys__.

# Role Variables
## Build variables

| Name | Description | Default value |
|:-----  | :----- | :----- |
| etcd_build | Boolean to define if binaries should be build | false |
| etcd_build_repo_url | String to define the Git repo URL | https://github.com/coreos/etcd.git |
| etcd_build_repo_update | Boolean to define if update should be checked out | false |
| etcd_build_version_default | String which equals the Git tag to checkout | v2.0.5 |
| etcd_build_path | String to define a directory where the repository will be cloned into | $HOME/etcd |

## Install variables
| Name | Description | Default value |
|:-----  | :----- | :----- |
| etcd_install | Boolean to define if the installation should be done | false |
| etcd_install_path | String to define where binaries will be installed | /usr/local/bin |
| etcd_install_owner | String to define the binary owner | root |
| etcd_install_group | String to define the binary group | root |
| etcd_install_mode | String to define the binary mode | '0755' |

## Etcd Cluster variables
| Name | Description | Default value |
|:-----  | :----- | :----- |
| etcd_cluster_name | String to define the Etcd Cluster name | default |
| etcd_cluster_data_dir  | String to define the directory where Etcd can store data | /var/lib/etcd |
| etcd_install_systemd_service | Boolean to define if Systemd service should be installed | false |
| etcd_install_sysvinit_service | Boolean to define if SysVinit service should be installed | false |

# Dependencies

# Example Playbook

Checkout and build Etcd
```
- hosts: 127.0.0.1
  roles:
  - role: etcd
    etcd_build: true
```
Install binaries which are build in $HOME/etcd/bin into /usr/local/bin/{etcd,etcdctl}
```
- hosts: 127.0.0.1
  sudo: true
  roles:
  - role: etcd
    etcd_install: true
```
Checkout and build Etcd locally but install binaries on etcd cluster nodes and setup an Etcd cluster
named test using Systemd.

    - hosts: 127.0.0.1
      gather_facts: false
      roles:
      - role: etcd
        etcd_build: true
        etcd_build_path: /tmp/etcd
    - hosts: etcd-nodes
      gather_facts: true
	  sudo: true
      roles:
      - role: etcd
        etcd_build_path: /tmp/etcd
        etcd_install: true
        etcd_install_path: /usr/bin
        etcd_install_systemd_service: true
        etcd_cluster_name: test

## Etcd Key/Values
```
---
- hosts: 127.0.0.1
  roles:
  - role: etcd
    etcd_keys:
    - name: mykey
      value: myvalue
```

# License

BSD

# Author Information

[Thomas Krahn]

[Thomas Krahn]: mailto:ntbc@gmx.net
