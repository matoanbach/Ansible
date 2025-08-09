# 5.1 Understanding Variables
- A variable is a label that is assigned to a specific value to make it easy to refer to that value throughout the playbook
- Variables can be defined by administrators at different levels
- A fact is a special type of variable, that refers to a current state of an Ansible-managed system
- Variables are pariticularly useful when dealing with managed hosts where specifics are different
    - Set a variable `web_service` on Ubuntu and Red Hat
    - Refer to the variable `web_service` instead of the specific service name

## Defining Variables
- In Ansible, different types of variables can be used and defined
- Variables can be defined in playbooks
- Alternatively, include files can be used
- Variables can be specifiied as command line arguments
- The output of a command or task can be used in a variable using `register`
- `vars_prompt` can be used to ask for input and store that as a variable
- `ansible-vault` is used to encrypt sensitive values
- Facts are discovered host properties stored as variables
- Host variables are host properties that don't have to be discovered
- System variables are a part of the Ansible system and cannot be changed

## Variable Precedence
- Variables can be set with different types of scope
  - Global Scope: this is when a variable is set from inventory or the command line
  - Play Scope: this is applied when it is set from a play
  - Host Scope: this is applied when set in inventory or using a host variable inclusion file
- When the same variable is set at different levels, the most specific level gets precedence
- When a variable is set from the command line, it has highest precedence
  - `ansible-playbook site.yml -e "web_package=apache"`

# 5.2 Using Variables in Playbooks
- Variables can be defined in a vars section in the beginning of a play
    ```yaml
    - hosts: all
      vars:
        web_package: httpd
    ```
- Alternatively, variables can be defined in a variable file, which will be included from the playbook
    ```yaml
    - hosts: all
      vars_files:
        - vars/users.yml
    ```
## Using Variables
- After defining the variables, they can be used later in the playbook
- Refer to a variable `{{ web_package }}`
- In conditional statements (discussed later), no curly braces are needed to refer to variable values
  - `when: 'not found' in command_result.err`
    ```yaml
    {% if ansible_facts['devices']['sdb'] is defined %} 
        Secondary disk size: {{
            ansible_facts['devices']['sdb']['size'] }}
    ```
- If the variable is the first element, using quotes is mandatory: `"{{ web_package }}"`

# 5.3 Including Variables
# 5.4 Managing Host Variables
# 5.6 Using Register to Set Variables
# 5.7 Using Vault to Manage Sensitive Values
# Lesson 5 Lab: Using Ansible Vault