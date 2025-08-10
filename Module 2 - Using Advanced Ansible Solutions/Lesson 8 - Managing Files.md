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

# 8.2 Copying Files to and From Managed Hosts
# 8.3 Using Jinja2 Templates
# 8.4 Applying Conditionals in Jinja2 Templates
# 8.5 Managing SELinux File Context
# Lesson 8 Lab: Applying Conditionals in Templates