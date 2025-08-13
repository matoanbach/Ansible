# 14.1 Managing Partitions and Mounts
## Modules to manage Storage
- `ansible.posix.mount` is used to mount existing filesystems
- `community.general.parted` is used to manage partitions
- `community.general.lvg` manages volume groups
- `community.general.lvol` manages logical volumes
- `community.general.filesystem` can be used to create filesystems on the new devices
- Notice that the community.general content collection is unsupported. Use the `redhat.rhel_system_roles.storage` role if you want to use a supported solution 

## create_partition.yml
```yml
---
- name: create partition
  hosts: all
  tasks:
  - name: create partition
    parted:
      device: /dev/sdb
      number: 1
      state: present
      part_end: 4GiB
    when: ansible_facts['devices']['sdb'] is defined
```

# 14.2 Managing Logical Volumes
- Currently, the `community.general` modules are not supported
- To manage storage devices in a supported way, either use `ansible.builtin.command`, or `redhat.rhel_system_roles.storage` (which are not ideal)

## Using redhat.rhel_system_roles.storage
- The `storage` role only supports the following
  - unpartitioned devices, where it creates a file system on the whole disk device (which is a bad idea)
  - LVM on whole devices

## Creating LVM with the Storage Role
- To use the RHEL system role to create LVM, the `storage_pools` variable must be defined
- Within `storage_pools` you define the LVM environment, using the following variables:
  - `name`: the name of the VG
  - `disks`: the complete disk to use
  - `volumes`: the LVM logical volumes and their properties

# 14.3 Developing Advanced Playbooks
# Lesson 14 Lab: Managing Storage