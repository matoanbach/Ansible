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
- To develop advanced Ansible (exam lab) solutions, it's recommended to use a phased approach
  - First, set up a generic framework that shows how you are going to take care of any conditional task execution. In this phase don't use specific modules but put as much as you can in the `debug` module and focus on structure.
  - Next, work out the conditionals and facts you may want to check. Still no details about specific modules but check the facts using the `debug` module.
  - Finally, work out module specifics and produce the working solution. In this phase, consider using tags to allow you to test specific parts only.

## setup-storage-test.yml
```yml
---
- name: creating storage configuration
  hosts: all
  vars_files:
    - vars/storage.yml
  tasks:
    - name: verify partition existence
      parted:
        device: /dev/sdb
        state: present
        number: "{{ item.numer }}"
        part_start: "{{ item.start }}"
        part_end: "{{ item.end }}"
      loop: "{{ partitions }}"
    - name: verify VG existence
      debug:
        msg: TODO
      loop: "{{ vgs }}"
    - name: create LVs if needed
      debug:
        msg: TODO
      loop: "{{ lvs }}"
      when: true
    - name: verify XFS filesystem on each LV
      debug:
        msg: TODO
    - name: mount LVs
      debug:
        msg: TODO
      loop: "{{ lvs }}"
```

## setup-storage-lvm.yml
```yml
---
- name: configure Apache storage configuration
  hosts: all
  vars_files:
    - vars/storage.yml
  tasks:
    - name: verify partition existence
      parted:
        device: /dev/sdb
        state: present
        number: "{{ item.numer }}"
        part_start: "{{ item.start }}"
        part_end: "{{ item.end }}"
      loop: "{{ partitions }}"
    - name: verify VG existence
      lvg:
        vg: "{{ item.name }}"
        pvs: "{{ item.devices }}"
      loop: "{{ vgs }}"
    - name: create LVs if needed
      lvol:
        vg: "{{ item.vgroup }}"
        lv: "{{ item.name }}"
        size: "{{ item.size }}"
      loop: "{{ lvs }}"
      when: item.name not in ansible_lvm["lvs"]
    - name: verify XFS filesystem on each LV
      debug:
        msg: TODO
      loop: "{{ lvs }}"
    - name: mount LVs
      debug:
        msg: TODO
      loop: "{{ lvs }}"
```

# Lesson 14 Lab: Managing Storage