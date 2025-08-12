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

