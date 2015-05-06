Etcd
==========

This role can be used to do the following tasks on RHEL based systems:
- Checkout and build Etcd
- Install binaries
- Setup an Etcd Cluster
- Manage create, update and delete key/value pairs in Etcd

By default the role does nothing because __etcd_build__ and __etcd_install__ are set to false.

# Requirements

- Ansible >= 1.2
- To checkout and build binaries Git and Go must be installed. This will be done if parameter __etcd_build_install_packages__ is set to _true_.
- If Key/Value pairs within an Etcd Cluster have to be managed the [ansible-library-etcd] must be available within your Ansible __library_path__.

# Notes

Some tasks are required to run with sudo. Ensure password less sudo access is possible or provide
sudo password to Ansible when running your playbook.

# Role Variables
## Build variables

| Name | Description | Default value |
|:-----  | :----- | :----- |
| etcd_build | Boolean to define if binaries should be build | false |
| etcd_build_repo_url | String to define the Git repo URL | https://github.com/coreos/etcd.git |
| etcd_build_repo_update | Boolean to define if an update should be done on checkout | false |
| etcd_build_version_default | String which equals the Git tag to checkout | v2.0.5 |
| etcd_build_path | String to define a directory where the repository will be cloned in | $HOME/etcd |
| etcd_build_packages | List of package names to install | git, go |
| etcd_build_install_packages | Boolean to define if packages which are required to checkout and build the binaries have to be installed | true |


## Install variables
| Name | Description | Default value |
|:-----  | :----- | :----- |
| etcd_install | Boolean to define if the installation should be done | false |
| etcd_install_path | String to define where binaries will be installed | /usr/local/bin |
| etcd_install_owner | String to define the binary and data dir owner | root |
| etcd_install_group | String to define the binary and data dir group | root |
| etcd_install_mode | String to define the binary file mode | '0755' |

## Service variables
| Name | Description | Default value |
|:-----  | :----- | :----- |
| etcd_service | Boolean to define if a service (sysvinit or systemd depending on ansible_distribution_major_version) should be installed | false |
| etcd_service_state | Service state | running |
| etcd_service_enabled | Boolean to define if service should start on boot | true |
| etcd_service_file_template | Filename of Jinja2 template to be used to generate the service file | (RHEL < 7) = etcd.init.j2, (RHEL >= 7) = etcd.service.j2

## Etcd Cluster variables
| Name | Description | Default value |
|:-----  | :----- | :----- |
| etcd_cluster_name | String to define the Etcd Cluster name | default |
| etcd_cluster_data_dir  | String to define the directory where Etcd can store data | /var/lib/etcd |
| etcd_cluster_group_name | Name of Ansible host group which contains the Cluster members | null |

# Example Playbook

Checkout and build Etcd v2.0.10. Ensure all required packages are installed.

```yaml
- hosts: 127.0.0.1
  roles:
  - role: etcd
    etcd_build: true
    etcd_build_install_packages: true
    etcd_build_version: v2.0.10
```

Install binaries which are build in $HOME/etcd/bin into /usr/local/bin/{etcd,etcdctl}

```yaml
- hosts: 127.0.0.1
  sudo: true
  roles:
  - role: etcd
    etcd_install: true
```
Checkout and build Etcd binaries on all members of group etcd-cluster. Setup an Etcd cluster
named _test_ storing data in /var/lib/etcd.
```yaml
- hosts: etcd-cluster
  gather_facts: false
  roles:
  - role: etcd
    etcd_build: true
    etcd_install: true
    etcd_service: true
    etcd_cluster_name: test
    etcd_cluster_data_dir: /var/lib/etcd
    etcd_cluster_group_name: etcd-cluster
```
## Etcd Key/Values
Requires [ansible-library-etcd] to be in Ansible's __library_path__.
```yaml
- hosts: 127.0.0.1
  connection: local
  gather_facts: false
  roles:
  - role: etcd
    etcd_keys:
    - name: mykey
      value: myvalue
      state: present
      etcd_host: etcd.example.com
```

# License

BSD

# Author Information

[Thomas Krahn]

[ansible-library-etcd]: https://github.com/Nosmoht/ansible-library-etcd.git
[Thomas Krahn]: mailto:ntbc@gmx.net
