# 15.1 Using Roles for Network Management
- The `redhat.rhel_system_roles.network` system role allows for the configuration of network related settings
- After installing the `rhel-system-roles.rpm` package, many examples are provided in the `/usr/share/doc/rhel-system-roles` directory
- Based on these examples, many different network interface types can be configured
- To configure the role, the `network_provider` and the `network_connections` variables must be set
  - `network_provider` variables must be set
  - `network_connection` defines the network connection and its properties

## example_simple_auto-playbook.yml
```yml
---
- name: Manage autoconnect
  hosts: all
  vars:
    network_provider: nm
    network_connections:
      - name: ens161
        type: ethernet
        ip:
          address:
          - 192.168.4.231/24
        zone: external
  roles:
    - rhel-system-roles.network
```

# 15.2 Managing Network Settings with Facts and Modules
- `ansible.posix.firewalld` allows you to create rules for firewalld
- `ansible.builtin.hostname` allows for setting the hostname
- These modules can be used with ansible facts as stored in `ansible_facts['intefaces']`