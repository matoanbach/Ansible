# 8.1 Modifying Files
- Different Modules are available for managing files
  - `ansible.builtin.lineinfile`: ensures that a line is in a file, useful for changing a single line in a file
  - `ansible.builtin.blockinfile`: manipulates multi-like blocks of text in files
  - `ansible.builtin.file`: sets attributes to files, and can also create and remove files, symbolic links and more
  - `ansible.builtin.stat`: used to request file statistics. Useful when combined with register

## file.yml
```yaml
---
- name: create a file
  hosts: all
  tasks:
    - name: create a file
      file:
        path: /tmp/removeme
        owner: ansible
        mode: 0640
        state: touch
        setype: public_content_rw_t
```

## addcustomfacts.yml
```yml
---
- hosts: ansible1
  gather_facts: no
  tasks:
    - name: Create a custom fact on ansible1
      blockinfile:
        path: /etc/ansible/facts.d/local.fact
        create: true
        block: |
          [localfacts]
          type: production
- hosts: ansible2
  tasks:
    - name: Create custom fact on ansible2
      blockinfile:
        path: /etc/ansible/facts.d/local.fact
        create: true
        block: |
          [localfacts]
          type: testing
```

# 8.2 Copying Files to and From Managed Hosts
- Different Modules are available for managing files
  - `ansible.builtin.copy`: copies a file from a local machine to a location on a managed host
  - `ansible.builtin.fetch`: used to fetch a file from a remote machine and store in on the management node
  - `ansible.posix.synchronize`: synchronizes files `rsync` style. Only works if the linux `rsync` utility is available on managed hosts
  - `ansible.posix.patch`: applies patches to files

## copy.yml
```yml
---
- name: file copy modules
  hosts: all
  tasks:
  - name: copy file demo
    copy:
      src: /etc/hosts
      dest: /tmp/
  - name: add some lines to /tmp/hosts
    blockinfile:
      path: /tmp/hosts
      block: |
        192.168.4.111 host111.example.com
        192.168.4.112 host112.example.com
      state: present
  - name: verify file checksum
    stat:
      path: /tmp/hosts
      checksum_algorithm: md5
    register: result
  - debug:
      msg: "The checksum of /tmp/hosts is {{ result.stat.checksum }}"
  - name: fetch a file
    fetch:
      src: /tmp/hosts
      dest: /tmp/
```

# 8.3 Using Jinja2 Templates
# 8.4 Applying Conditionals in Jinja2 Templates
# 8.5 Managing SELinux File Context
# Lesson 8 Lab: Applying Conditionals in Templates