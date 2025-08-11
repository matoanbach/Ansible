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

## Using a Requirements File
- If specific roles are needed in a project, they can be specified in the `roles/requirements.yml` file in the project directory
- By using a `requirements.yml` file, you'll have one location in the project to manage which roles are needed
- In a requirements.yml file, different sources can be used
  - Git: `https://github.com/mygit/myrole/scm: git`
  - files: `file://tmp/myrole.tar`
  - web: `https://www.example.local/myrole.tar`
- If a role is hosted in Git, the `scm: git` attribute is required

## Sample Requirements File
```yml
- src: https://github.com/myaccount/myrole.role
  scm: git
  version: "2.0"
- src: file:///tmp/myrole.tar
  name: mytarrole
- src: https://example.local/myrole.tar
  name: mywebrole
```

# 9.3 Understanding Roles in Ansible Content Collections
## Roles and Ansible Content Collections
- Roles may be distributed through Ansible Content Collections
- This is not commonly done
- When roles are provided through collections, you'll find them using `ansible-navigator collections`
- Select `redhat.insights` to see some roles (use **:21** to address numbers higher than 9)

# 9.4 Writing Playbooks that Use Roles
## Using Roles in Playbooks
- Roles can be used in different ways
  - Using a `roles:` section in the play header
  - Using the `import_role:` or `include_role:` module in a task
- Roles listed in the `roles:` section are executed before the tasks in the play
- Use `pre_tasks`: to trigger tasks to run before the roles
- Use `post_tasks`: to force tasks to run after the roles and `tasks:`
- Notice that you don't have to use a `tasks:` section in the play, it will also work if you just have a `roles:` section
- Best practice: if roles should be executed after some tasks, define the tasks and use `import_role` where needed

## include_role versus import_role
- `include_role` dynamically includes the role when it is referred to in a task
- `import_role` is processed when the playbook is parsed
- While using `import_role` the role handlers and variables are exposed to all tasks in the play, even if the role has not run yet
- Best practice: in most cases, using `include_role` is better than using `import_role`

## Variables in Roles
- Roles are often pre-configured with standard variables
  - Variables in the defaults directory in the role provide default variables that are intended to be changed in plays
  - Variables in the vars directory in the role are used for internal purposes in the role and they are not intended to be overwritten in the playbook
- Site-specific information should be set through playbook variables, which will be picked up by the role
- Best practice: Don't define local variables or vault encrypted variables in the role, they should always be defined locally in the playbook

## Defining Variables are Role Parameters
- Variables can be set as a role parameter while calling the role from the playbook
- This is helpful if the same role is used multiple times, with different values for the same variable
```yml
- hosts: webservers
  roles:
    - role: myrole
      message: hello
    - role: myrole
      message: bye
```

## Role Variable Precedence
- Role variables in the vars directory are not supposed to be overwritten, but will be overwritten by facts, registered variables, and variables loaded with `include_vars`
- Role variables in the default directory are overwritten by any other variable definition
- If a variable is declared as a role parameter, it has the highest precedence

## apache-vhost-role.yml
```yml
---
- name: create apache vhost
  hosts: ansible1
  pre_tasks:
  - name: pre_tasks message
    debug:
      msg: setting up web server
  roles:
    - mywebhost
  post_tasks:
    - name: install contents from local file
      copy:
        src: files/html
        dest: "/var/www/vhosts/{{ ansible_hostname }}"
    - name: post_tasks message
      debug:
        msg: web server is configured
```

# 9.5 Writing Custom Roles
## Creating Custom Roles
- To create a custom role, just make sure to create the required directory structure somewhere in the `roles_path`
- Consider using `ansible-galaxy role init myrole` to create the default directory structure for the `myrole` role
- Next, complete the main.yml files to provide role content

```bash
   1  mkdir temp
    2  cd temp
    3  ansible-galaxy init myrole
    4  cd ..
    5  cp ../rhce9/roles/motd roles
    6  cp -r ../rhce9/roles/motd roles
    7  vim roles
    8  cp ../rhce9/motd-role.yml .
    9  vim motd-role.yml 
   10  history
```

# 9.6 Using RHEL Sytem Roles
- RHEL system roles are provided as an easy interface to manage specific parameters between different RHEL versions
- Use `dnf install -y rhel-system-roles` to install them
- RHEL system roles come with sample playbooks in `/usr/share/doc/rhel-system-roles` if installed from the RPM package
- If you need to use RHEL system roles, copy a sample playbook from `/usr/share/doc/rhel-system-roles` to your project directory and modify it instead of create a new one


## Install RHEL System Roles
- For Ansible Automation Platform customers, the RHEL system roles are available as a content collection: `redhat.rhel_system_roles`
- Alternatively, they are available in `rhel-system-roles.rmp` for Ansible Core users
- Tip: Best install rhel-system-roles using `dnf install rhel-system-roles`. This installs them to `/usr/share/ansible/roles` and `/usr/share/ansible/collections`, which ensure easy access from `ansible-navigator` as well as the `ansible-playbook` utility and also provides useful examples in `/usr/share/doc` 

# 9.7 Configuring Ansible Roles and Colletion Sources
# 9.8 Using the TimeSync RHEL System Role
# 9.9 Using the SELinux RHEL System Role
# Lesson 9 Lab: Using Roles