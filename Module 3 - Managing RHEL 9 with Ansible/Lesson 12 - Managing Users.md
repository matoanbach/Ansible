# 12.1 Understanding User-related Tasks
- Most of the user oriented tasks can be performed with the `ansible.builtin.user` module
- If additional groups need to be created, use `ansible.builtin.group` before running the user module
- The `user` module can also take care of less common tasks, like generation of ssh keys using the `generate_ssh_key`: boolean

## Creating Users
- The `ansible.builtin.user` module contains all that is needed to manage basic user properties
- If a user needs to be a member of additional (secondary) groups, ensure these groups exist before using the user module
- While adding users to secondary groups, use the `append: true` option
- Ansible can also be used to manage SSH keys and create sudo configuration
- For setting passwords, you'll have to ensure that encrypted passwords are generated before providing them to the user module

## User Management Modules Overview
- `ansible.builtin.user`: manages core user properties
- `ansible.builtin.group`: creates and manages groups
- `ansible.builtin.known_hosts`: updates the `/etc/ssh/ssh_known_hosts` file with the host key of a managed host
- `ansible.builtin.authorized_key`: manages authorized keys for user accounts on managed hosts
- `ansible.builtin.lineinfile`: used to configure sudo access by adding a line to a configuration file

# 12.2 Managing SSH Keys
- The `ansible.builtin.user` module can use the `generated_ssh_key` argument to generate to an SSH key for a user on a managed host
- The `ansible.builtin.known_host` module copies host keys from managed hosts
  - This ensures that users are not prompted to verify the remote host SSH key fingerprint before connection to it
- The `ansible.builtin.authorized_keys` module can be used to copy a control host user public key to the corresponding user account on a managed host
  - To use it, the public key must be in a public location, where it is readable: if it is a hidden directory in the user home directory it cannot be used 

## userkey.yml
```yml
---
- name: create remote management user
  hosts: ansible2
  tasks:
  - name: create user ansible
    user:
      name: ansible
  - name: give them sudo privileges
    lineinfile:
      path: /etc/sudoers.d/ansible
      state: present
      create: true
      mode: 0440
      line: 'ansible ALL=(ALL) NOPASSWD: ALL'
      validate: /usr/sbin/visudo -cf %s
  - debug:
      msg: the remote management user is now created and has sudo privileges

- name: manage user keys
  hosts: localhost
  become: true
  tasks:
  - name: create a directory to store the file that authorized_keys is goign to distribute
    file:
      name: ansiblekey
      state: directory
  - name: copy the local user ansible ssh key to this directory
    shell: 'cat /home/ansible/.ssh/id_rsa.pub > ansible/id_rsa.pub'
  - debug:
      msg: the local management user key is now in a place where it can be used

- name: create another remote user with a key
  hosts: ansible2
  tasks:
  - name: copy the management user authorized key to the management host
    authorized_key:
      user: ansible
      key: "{{ lookup('file', './ansiblekey/id_rsa.pub') }}" 
  - name: create a remote user with an SSH key pair
    user:
      name: anna
      generate_ssh_key: true
      ssh_key_bits: 2048
      ssh_key_file: .ssh/ansiblekey_rsa
```

# 12.3 Managing Encrypted Passwords
- On linux, the hash of the encrypted  user password is stored in `/etc/shadow`
- This hash looks like `$6$G9dU2zwfEJn2gQD/$TmlbTElEKom/DLxs604vMIjZVDvWLKzC8TmQiBMSmLWurpdyu1DJxUnM4C0ncaoCD/uWn/8AyZxcvwN5JNt4x0`
- It consists of 3 parts:
  - The hashing algorithm that is used
  - The random salt that was used to encrypt the password
  - The encrypted hash of the user password
- To create an encrypted password, a random salt is used to ensure that two users that have identical passwords would not have identical entries in `/etc/shadow`
- This salt and the unencrypted password are combine and encrypted, which generates the encrypted hash that is stored in `/etc/shadow`

# 12.4 Creating Encrypted Passwords in Ansible
- The `ansible.builtin.user` module does not generate encrypted passwords
- To generate an encrypted password, an external utility must be used to generate an encrypted string
- Next, the encrypted string can be used in a variable to set the password
- For enhanced security, store the password hash in a vault encrypted file
- One method is to use the `password_hash` filter to generate an encrypted password
- Another option would be to use the `shell` module to run `passwd --stdout` and capture the result of that command in a variable using `register`