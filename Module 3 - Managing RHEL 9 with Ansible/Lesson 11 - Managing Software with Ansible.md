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

## Gathering Package Facts
- Facts about packages are not gathered by the `ansible.builtin.setup` module
- To gather facts about packages, use the `ansible.builtin.package_facts` module
- When gathered, package facts are written to the `ansible_facts['packages']` variable
- As packages are stored in an array, while addressing the package you need to address the correct array index value, which is `[0]` in many cases

## packagefacts.yml
```yml
---
- name: use debug to check package facts
  hosts: ansible2
  tasks:
    - name: get information about packages
      package_facts:
        manager: auto
    - name: list installed packages
      debug:
        var: ansible_facts.packages
    - name: show Bash version
      debug:
        msg: "Version {{ ansible_facts.packages['bash'][0].version }}"
      when: "'bash' in ansible_facts.packages"
```

# 11.2 Managing Repositories and Repository Access
## Accessing Repositories
- To access repositories, the `ansible.builtin.yum_repository` module is used 
- This module creates a repository file in the `/etc/yum.repos.d` directory
- If the module argument `gpgcheck: yes` is used, the `ansible.builtin.rpm_key` module must be used to install the GPG key

# 11.3 Managing Subscriptions
## Managing RHEL Subscriptions
- To work with RHEL, you need to use your subscription
- Free RHEL subscriptions are available through https://developers.redhat.com
- To register a RHEL server from the Linux command line, you would use the following:
  - `subscription-manager register --username=yourname --password=password`
  - `subscription-manager attach`
  - `subscription-manager repos list`
  - `subscription-manager repos --enable "repo name"`

## Using Modules to Manage Subscriptions
- The `redhat_subscription` module enables you to perform subscription and registration in one task
- The `rhsm_repository` module is used to enable subscription manager repositories 

# Lesson 11 Lab: Managing Repositories