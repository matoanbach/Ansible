# 4.1 Using YAML to Write Playbooks
## Why playbooks
- Ad-hoc commands can be used to run one or a few tasks
- Ad-hoc commands are convenient to test, or when a complete managed infrastructure hasn't been set up yet
- Ansible playbooks are used to run multiple tasks against managed hosts in a scripted way
- In playbooks, one or multiple plays are started
  - Each play runs one or more tasks
  - In these tasks, different modules are used to perform the actual work
- Playbooks are written in YAML and have the .yml or .yaml extension

## Understanding YAML
- YAML is Yet Another Markup Language according to some
- According to others it stands for YAML Ain't Markup Language
- Anyway, it's an easy-to-read format to structure tasks/items that need to be created
- In YAML files, items are usign identation with white spaces to indicate the structure of data
- Data elements at the same level should have the same indentation
- Child items are indented more than the parent items
- There is no strict requirement about the number of spaces that should be used, but 2 is common

## Writing Your First Playbook
```yml
- name: deploy vsftpd
  hosts: ansible2
  tasks:
  - name: install vsftpd
    yum: name=vsftpd
  - name: enable vsftpd
    service: name=vsftpd enabled=true
  - name: create readme file
    copy:
        content: "free downloads for everybody"
        dest: /var/ftp/pub/README
```

# 4.2 Running Playbooks

# 4.3 Verifying Playbook Syntax

# 4.4 Using Multiple-Play Playbooks