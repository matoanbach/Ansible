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
## Running Your First Playbook
- Run `ansible-playbook vsftpd.yml` to run the playbook
- Notice that a successfuly run requires the inventory and become parameters to be set correctly, and also requires access to an inventory file
- The output of the `ansible-playbook` command will show what exactly has happened
- Playbooks should be idempotent, which means that running the same playbook again should lead to the same result
- Notice there is no easy way to undo changes made by a playbook

## Using ansible-navigator
- `ansible-navigator run -m stdout --pp never vsftpd.yml` will run the playbook using navigator
  - `-m stdout` will write output to STDOUT instead of using iterative mode
  - `--pp never` will not check for newer container images
- `ansible-navigator run --pp never vsftpd.yml` will run the playbook in iterative mode
  - Use the indicated numbers to get more details about each step
  - Use Esc to get out of any interface
  - Set `policy: missing` in `.ansible-navigator.yml` to avoid using the `--pp never` option
  - `~/.ansible-navigator.yml`
    ```yml
    ansible-navigator:
    execution-environment:
        image: ee-supported-rhel8:latest
        pull:
        policy: missing
    playbook-artifact:
        enable: false
    ```

# 4.3 Verifying Playbook Syntax

# 4.4 Using Multiple-Play Playbooks