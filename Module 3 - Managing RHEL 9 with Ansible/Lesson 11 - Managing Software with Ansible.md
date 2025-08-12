# 11.1 Managing Packages
## Understanding Software Management Tasks
- To manage software on RHEL systems, different tasks need to be managed
- Systems need to be subscribed
- Repositories and software channels need to be configured

## Managing Software Packages
- Different modules are available for managin software packages
  - `ansible.builtin.dnf`: used on recent Red Hat and family and backwards compatible with the ansible.builtin.yum module
  - `ansible.builtin.yum`: used on older Red Hat and family
  - `ansible.builtin.apt`: used to manage packages on Ubuntu and family
  - `ansible.builtin.package`: a generic module that can install and remove packages on different Linux distributions
  - `ansible.windows.win_package`: manages packages on Windows
- While managing packages, the recommendation to use the most specific module applies
- Also realize that package names are not always the same between different distributions

## Managing Package Groups
- The Linux `dnf` command supports working with package groups
- Use `dnf group list` to show a list of available groups
- To install a package group, put a @ in front of the group name:

```yml
- name: install virtualization software
  ansible.builtin.dnf:
    name: '@Virtualization Host'
    state: present
```
## Managing Package Modules
- The `dnf` command also supports working with package modules
- Use `dnf module list` for a list of available modules
- To install a module, put a @ in front of its name, optionally followed by the module stream version and profile

```yml
- name: install the php module
  ansible.builtin.dnf
    name: 'php:7.3/minimal'
    state: latest
```

# 11.2 Managing Repositories and Repository Access
# 11.3 Managing Subscriptions
# Lesson 11 Lab: Managing Repositories