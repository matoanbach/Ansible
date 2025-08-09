# 7.1 Understanding Conditionals
- `loop`:  allows you to loop over a list of items instead of calling the same task repeatedly
- `when`: performs conditional task execution based on the value of specific variables
- `handlers`: used to perform a task only if triggered by another task that has changed something

# 7.2 Writing Loops
- The `loop` keyword allows you to iterate through a simple list of items
- Before Ansible 2.5, the `with_` keyword was used instead
```yaml
- name: start some services
  service:
    name: "{{ item }}"
    state: started
  loop:
    - vsftpd
    - httpd
```

## Using Dictionaries in Loops
- Each item in a loop can be a hash/dictionary with multiple keys in each hash/dictionary
```yaml
- name: create users using a loop
  hosts: all
  tasks:
  - name: create users
    user:
        name: "{{ item.name }}"
        state: present
        groups: "{{ item.groups }}"
    loop:
        - name: anna
          groups: wheel
        - name: linda
          groups: users
```
## loop versus with_
- The `loop` keyword is the current keyword
- In previous versions of Ansible, the `with_*` keywords were used for the same purpose
- Using `with_X` often is easier, but using plugins and filters offers more options
  - `with_items`: equivalent to the `loop` keyword
  - `with_file`: the `item` contains a file, which content is used to loop through
  - `with_sequence`: generates a list of values based on a numeric sequence
- Loop up "Migrating from with_X to loop" in the Ansible documentation for instructions on how to migrate

# 7.3 Using When
## Example Conditionals
- `ansible_machine == "x86_64"` - Variable contains string value
- `ansible_distribution_version == "8"` - Variable contains string value
- `ansible_memfree_mb == 1024` - Variable value is equal to integer
- `ansible_memfree_mb < 256` - Variable value is smaller than  integer
- `ansible_memfree_mb > 256` - Variable value is bigger than  integer
- `ansible_memfree_mb <= 256` - Variable value is smaller than or equal to integer
- `my_variable is defined` - Variable value exists (nice for facts) 
- `my_variable is not defined` - Variable value does not exist
- `my_variable` - Variable is Boolean true
- `ansible_distribution in supported_distros` - Variable contains another variable

## Variable Types
- String: sequence of characters - the default variable type in Ansible
- Numbers: numeric value, treated as integer or float. When placing a number in quotes it is treated as a string
- Booleans: true/false values (yes/no, y/n, on/off also supported)
- Dates: calendar dates
- Null: undefined variable type
- List or Arrays: a sorted collection of values 
- Dictionary or Hash: a collection of key/value pairs

## Using Filter to Enforce Variable Types
- While working with variable in a when statement, the variable type may be interpreted wrongly
- To ensure that a variable is treated as specific type, filters can be used
  - int (integer) `when vgsize | int > 5`
  - bool (boolean) `when runme | bool`

- Using a filter does not change the variable type, it only changes the way it is interpreted

## Demo on how to use When:
```yml
--
- name: when demo
  hosts: all
  vars:
    supported_distros:
      - Ubuntu
      - CentOS
      - Fedora
  tasks:
    - name: install RH family specific packages
      yum:
        name: "{{ mypackage }}"
        state: present
      when: ansible_distribution in supported_distros
```

# 7.4 Using When and Register to Check Multiple Conditions
## TEsting Multiple Conditions
- `when` can be used to test multiple conditions as well
- Use `and` or `or` and group the conditions with parentheses
```yaml
when: ansible_distribution == "CentOS" or \
ansible_distibution == "RedHat"
when: ansible_machine == "x86_63" and \
ansible_distribution == "CentOS"
```
- The `when` keyword also supports a list and when using a list, all of the conditions must be true
- Complex conditional statements can group conditions using parentheses

```yaml
---
- name: conditionals test 
  hosts: all
  vars:
    package: nmap
  tasks:
  - name: install vsftpd if sufficient space on /boot
    package:
      name: "{{ package }}"
      state: latest
    loop: "{{ ansible_mounts }}"
    when: item.mount == "/boot" and item.size_available > 100000000
```

## Using Register Conditionally
- The `register` keyword is used to store the results of a command or tasks
- Next, `when` can be used to run a task only if a specific result was found

```yaml
- name: show register on random module
    user:
        name: "{{ username }}"
    register: user
- name: show register results
    debug:
        var: user
```

# 7.5 Conditional Task Execution with Handlers
# 7.6 Using Blocks
# 7.7 Managing Task Failure
# 7.8 Managing Changed Status
# 7.9 Including and Important Files