Ansible role: GlusterFS
=========

This role is used to install glusterfs cluster.

For now, it does the following:
- disables SELinux and swap
- does all necessary LVM configurations for every volume
- creates directories for glusterfs volumes and mounts LVM logical volumes
- installs glusterfs repo and packages
- creates cluster and joins peers to it
- creates and configures glusterfs volumes (currently supported volume types are distributed and replicated ones)
- installs client tools only if need via include of separate tasks file


Requirements
------------

This is not strict requirements and it may not work with other versions than tested ones.
Anyway. feel yourself free to test by yourself, suggest addition of new functionality and contribute.

Role is tested with:
- Ansible version >= 2.8.6 <= 2.10
- CentOS version >= 7.6 (1803) <= 8


Role Variables
--------------

Variables and their descriptions copied from defaults/main.yml

```bash

# LVM options to create and mount:
glusterfs_lvm: []
# - lv: ""
#   vg: ""
#   pvs:
#   - ""
#   fstype: ""
#   fsopts: ""
#   mount_opts: ""
#   mount_dump: ""
#   mount_passno: ""

# Root directory, where all glusterfs volumes will be mounted:
glusterfs_dir: /data/glusterfs

# Filesystem type to format every logical volume with,
# used if "glusterfs_lvm.fstype" is missing for single logical volume:
glusterfs_fstype: xfs

# Filesystem options for every logical volume, used if
# "glusterfs_lvm.fsopts" is missing for single logical volume:
glusterfs_fsopts: -i size=512

# Mount options for every logical volume, used if
# "glusterfs_lvm.mount_opts" is missing for single logical volume:
glusterfs_mount_opts: rw,inode64,noatime,nouuid

# /etc/fstab mount "dump" parameter to use for every logical volume,
# used if "glusterfs_lvm.mount_dump" is missing for single logical volume:
glusterfs_mount_dump: '1'

# /etc/fstab mount "passno" parameter to use for every logical volume,
# used if "glusterfs_lvm.mount_passno" is missing for single logical volume:
glusterfs_mount_passno: '2'

# Version of glusterfs repo package:
glusterfs_version: 6

# Open firewall ports for bricks, this port range determines
# the number of bricks will be accessible throught firewall:
glusterfs_brick_ports: 49152-49155

# Glusterfs volumes to create:
gluster_volumes:
# - name: ""
#   brick_dirs:
#   - ""
#   replicas:
#   clusters:
#   - ""
#   auth_allow:
#   - ""

```


Dependencies
------------

none


Example Playbook
----------------

```bash
---
- hosts: localhost
  gather_facts: false
  become: no
  tasks:
  - name: Check ansible version >=2.8.6
    assert:
      msg: Ansible must be v2.8.6 or higher
      that:
      - ansible_version.string is version("2.8.6", ">=")
    tags:
    - check
  vars:
    ansible_connection: local

- hosts: all
  become: yes
  tasks:
  - import_role:
      name: glusterfs
  # import this if you want to install client tools only:
  - import_role:
      name: glusterfs
      tasks_from: client

```

More detailed examples ( inventories, playbooks etc. ) of this and other roles can be found [here](https://github.com/caermeglaeddyv/examples/tree/dev/ansible).

It's highly recommended to start you test deploys from there, especially if you use Google Cloud Platform or VMware vCenter as your infrastructure, for now that [repository](https://github.com/caermeglaeddyv/examples) contains [Packer](https://github.com/caermeglaeddyv/examples/tree/dev/packer) and [Terraform](https://github.com/caermeglaeddyv/examples/tree/dev/terraform) examples to build templates and deploy machines on this platforms.


License
-------

[Apache 2.0](https://github.com/caermeglaeddyv/ansible-role-rear/blob/dev/LICENSE)


Author Information
------------------

Copyright 2020 [caermeglaeddyv](https://github.com/caermeglaeddyv)
