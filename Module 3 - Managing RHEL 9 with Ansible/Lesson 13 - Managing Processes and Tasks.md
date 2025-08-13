# 13.1 Managing Services and Targets
- `ansible.builtin.service` provides a generic interface used to manage the state of services in different service management systems
- `ansible.builtin.systemd` manages the state of services in systemd, as well as additional systemd specific properties
- There are no modules to manage the default target, use `ansible.builtin.command` instead
- `ansible.builtin.reboot` will reboot a managed host. It can use a `reboot_timeout` and a `test_command` to verify that the host is available again

- Best Practice: Using the command module is not neccessarily bad. Just configure it to be idempotent