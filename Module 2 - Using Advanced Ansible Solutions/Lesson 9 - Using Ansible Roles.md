# 9.1 Understanding Roles
- Roles are about reusable code
  - Many Ansible projects exist, and often the same things need to be done
  - Roles provide standardized solutions
  - Community roles are provided through `galaxy.ansible.com` 
  - Custom roles can be created and distributed through an organizations local Git repositories
  - Roles are included in tasks, using a `roles:` section in the playbook play header

# 9.2 Using Ansible Galaxy to Get Roles
- Roles can be provided in different ways
  - Through Ansible Galaxy, either as roles or as a part of a content collection
  - As tar balls
  - From RPM packages
  - Through Ansible Content Collection from Red Hat Automation hub at `https://console.redhat.com`

## Using Galaxy Roles
- Community roles are provided through Ansible Galaxy
- From there, find the role that you would like to use and fetch it, using `ansible-galaxy role install`
- While installing roles, the `roles_path` setting is used
- The default `roles_path` will use roles in the following order of precedence
  - A roles directory in the current project directory
  - The `~/.ansible/roles` directory
  - `/etc/ansible/roles`
  - `/usr/share/ansible/roles`
- Optionally, use `ansible-galaxy role install -p mypath` to install in an alternative path

## Using the ansible-galaxy Command 
- The `ansible-galaxy` command has a few useful options
- `ansible-galaxy [role] search 'docker' --author geerlingguy --platforms EL` will search for roles containing the keyword docker, written for usage on RHEL and related by author geerlingguy
- `ansible-galaxy [role] info geerlingguy.docker` shows information about the role
- `ansible-galaxy [role list]` will list roles
- `ansible-galaxy [role] remove geerlingguy.docker` will remove the role

# 9.3 Understanding Roles in Ansible Content Collections
# 9.4 Writing Playbooks that Use Roles
# 9.5 Writing Custom Roles
# 9.6 Using RHEL Sytem Roles
# 9.7 Configuring Ansible Roles and Colletion Sources
# 9.8 Using the TimeSync RHEL System Role
# 9.9 Using the SELinux RHEL System Role
# Lesson 9 Lab: Using Roles