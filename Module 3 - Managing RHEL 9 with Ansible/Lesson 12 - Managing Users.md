# 12.1 Understanding User-related Tasks
- Most of the user oriented tasks can be performed with the `ansible.builtin.user` module
- If additional groups need to be created, use `ansible.builtin.group` before running the user module
- The `user` module can also take care of less common tasks, like generation of ssh keys using the `generate_ssh_key`: boolean

## Creating Users
- The `ansible.builtin.user` module contains all that is needed to manage basic user properties
- If a user needs to be a member of additional (secondary) groups, ensure these groups exist before using the user module
- While adding users to secondary groups, use the `append: true` option
- Ansible can also be used to manage SSH keys and create sudo configuration
- For setting passwords, you'll have to ensure that encrypted passwords are generated before providing them to the user module

## User Management Modules Overview
- `ansible.builtin.user`: manages core user properties
- `ansible.builtin.group`: creates and manages groups
- `ansible.builtin.known_hosts`: updates the `/etc/ssh/ssh_known_hosts` file with the host key of a managed host
- `ansible.builtin.authorized_key`: manages authorized keys for user accounts on managed hosts
- `ansible.builtin.lineinfile`: used to configure sudo access by adding a line to a configuration file

# 12.2 Managing SSH Keys
- The `ansible.builtin.user` module can use the `generated_ssh_key` argument to generate to an SSH key for a user on a managed host
- The `ansible.builtin.known_host` module copies host keys from managed hosts
  - This ensures that users are not prompted to verify the remote host SSH key fingerprint before connection to it
- The `ansible.builtin.authorized_keys` module can be used to copy a control host user public key to the corresponding user account on a managed host
  - To use it, the public key must be in a public location, where it is readable: if it is a hidden directory in the user home directory it cannot be used 