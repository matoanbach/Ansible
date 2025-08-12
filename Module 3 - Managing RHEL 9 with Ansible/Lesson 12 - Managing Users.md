# 12.1 Understanding User-related Tasks
- Most of the user oriented tasks can be performed with the `ansible.builtin.user` module
- If additional groups need to be created, use `ansible.builtin.group` before running the user module
- The `user` module can also take care of less common tasks, like generation of ssh keys using the `generate_ssh_key`: boolean