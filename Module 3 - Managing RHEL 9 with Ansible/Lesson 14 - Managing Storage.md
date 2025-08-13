# 14.1 Managing Partitions and Mounts
## Modules to manage Storage
- `ansible.posix.mount` is used to mount existing filesystems
- `community.general.parted` is used to manage partitions
- `community.general.lvg` manages volume groups
- `community.general.lvol` manages logical volumes
- `community.general.filesystem` can be used to create filesystems on the new devices
- Notice that the community.general content collection is unsupported. Use the `redhat.rhel_system_roles.storage` role if you want to use a supported solution 

# 14.2 Managing Logical Volumes
# 14.3 Developing Advanced Playbooks
# Lesson 14 Lab: Managing Storage