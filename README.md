# Etcd

This role can be used to do the following tasks:
- Checkout and build Etcd
- Install binaries on a system
- Setup an Etcd Cluster

The process of checking out and building Etcd binaries can be delegated to another system, called _build host_.
Therefore Git and Go must be installed on the _build host_.

By default the role does nothing because _etcd_build_ and _etcd_install_ are set to false.

# Requirements

- Ansible >= 1.2
- Git and Go must be installed on the _build host_

# Role Variables
## Build variables

| Name | Description | Default value |
|:-----  | :----- | :----- |
| etcd_build | Boolean to define if binaries should be build | false |
| etcd_build_repo_url | String to define the Git repo URL | https://github.com/coreos/etcd.git |
| etcd_build_repo_update | Boolean to define if update should be checked out | false |
| etcd_build_version_default | String which equals the Git tag to checkout | v2.0.5 |
| etcd_build_host | String to define the host where build tasks will be delegated to | 127.0.0.1 |
| etcd_build_path | String to define a directory where the repository will be cloned into | '{{ lookup(''env'', ''HOME'') }}/etcd' |

## Install variables
| Name | Description | Default value |
|:-----  | :----- | :----- |
| etcd_install | Boolean to define if the installation should be done | false |
| etcd_install_path | String to define where binaries will be installed | /usr/bin |
| etcd_install_owner | String to define the binary owner | root |
| etcd_install_group | String to define the binary group | root |
| etcd_install_mode | String to define the binary mode | '0755' |
| etcd_install_sudo | Boolean to define if binary installation should be done with sudo. Set to true if installing binaries into a directory which is owned by root (like /usr/bin) | false |
| etcd_install_systemd_service | Boolean to define if Systemd service should be installed | false |
| etcd_install_sysvinit_service | Boolean to define if SysVinit service should be installed | false |

## Etcd Cluster variables
| Name | Description | Default value |
|:-----  | :----- | :----- |
| etcd_cluster_name | String to define the Etcd Cluster name | default |
| etcd_cluster_data_dir  | String to define the directory where Etcd can store data | /var/lib/etcd |

# Dependencies


# Example Playbook

Checkout and build Etcd 2.0.5 on local machine in /tmp/etcd

    - hosts: 127.0.0.1
      roles:
      - role: etcd
        etcd_build: true
        etcd_build_version: v2.0.5
        etcd_build_path: /tmp/etcd

Install binaries into /usr/bin/{etcd,etcdctl}

    - hosts: 127.0.0.1
      roles:
      - role: etcd
        etcd_install: true
        etcd_install_path: /usr/bin

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

# License

BSD

# Author Information

[Thomas Krahn]

[Thomas Krahn]: mailto:ntbc@gmx.net
