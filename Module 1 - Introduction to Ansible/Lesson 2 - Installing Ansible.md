# 2.1 Understanding What to install
- Install 3 VM's, using RHEL (recommended)  or CentOS Stream
  - control.example.local (Server with GUI)
  - ansible1.example.local (minimal install)
  - ansible2.example.local (minimal install)
- The latest version of RHEL can be obtained for free from `https://developers.redhat.com`
- The machines require Internet access
- Clone git repository: `git clone https://github.com/sandervanvugt/rhce9`

## Required Lab
- The _control node_ is where you install Ansible
- For RHCE 9, you'll install on top of RHEL 9 (preferred) or CentOS Stream
- Make sure a fixed IP address is set up, as well as a hostname
- The _managed nodes_ are managed by Ansible
- Managed nodes can be anything: servers running RHEL, but also other Linux distributions, Windows, Network Devices, and much more
- For RHCE 9, you'll need 2 managed nodes pre-installed with RHEL 9 or CentOS Stream
- Ensure host name lookup is configured, `/etc/hosts` is good enough

# 2.2 Understanding Required Components
- To use Ansible, the following is needed:
  - Host name resolution to address managed hosts by name
  - Python on all nodes
  - SSH running on the managed servers
  - A dedicated user account with sudo privileges
  - Optional: SSH jeys to make logging in esier
  - An inventory to identify managed nodes on the control node
  - Optional: an ansible.cfg to specify default parameters

# 2.3 Installing Ansible on the Control Node
1. On the control node: use `sudo subscription-manager repos --list` to verify the name of the latest available repository
2. Use `sudo subscription-manager repos --enable=rhel-9-for-x86_64-appstream.rpms` to enable the repository
3. `sudo subscription-manager repos --enable ansible-automation-platform-2.2-for-rhel-9-x86_64-rpms`
4. Use `sudo dnf install ansible-core -y` to install the Ansible software
5. Use `sudo dnf install ansible-navigator -y` to install Ansible Navigator
6. `ansible --version` to verify

# 2.4 Using Ansible to Set up Managed Nodes
```bash
cat >> inventory >> EOF
    ansible1
    ansible2
    EOF

ssh ansible1 # to create a finger print 
ssh ansible2 # to create a finger print
ansible -i inventory all -u student -k -b -K -m user -a "name=ansible"
ansible -i inventory all -u student -k -b -K -m shell -a "echo password | passwd --stdin ansible"
ssh-keygen # (for user student, as well as for user ansible)
for i in ansible1 ansible2; do ssh-copy-id $i; done # (user student & ansible)
ansible -i inventory all -u ansible -b -m command -a "ls -l /root" -K
ansible -i inventory all -u student -k -b -m shell -a 'echo "ansible ALL=(ALL) NOPASSWD:ALL"' > /etc/sudoers.d/ansible
```

# 2.5 Managing Static Inventory
## Managin Static Inventory
- In a minimal form, a static inventory is a list of host names and/or IP addresses that can be managed by Ansible
- Hosts can be grouped in inventory to make it easy to address multiple hosts at once
- A host can be a member of multiple groups
- Nested groups are also available
- It is common to work with project-based inventory files
- Variables can be set from the inventory file - but this is deprecated practice.
- Ranges can be used:
  - server[1:20] matches server1 up to server20
  - 192.168.[4:5].[0:255] matches two full class C subnets

## Inventory Files Locations
- `/etc/ansible/hosts` is the default inventory
- Alternative inventory location can be specified through the ansible.cfg configuration file
- Or use the `-i inventory` option to specify the location of the inventory file to use
- It is common practice to put the inventory file in the current project director option to specify the location of the inventory file to use
- It is common practice to put the inventory file in the current project directory

## Static Inventory Example
```yaml
[webservers]
web[1:9].example.com
web12.example.com

[fileservers]
file1.example.com
file2.example.com

[servers:children]
webservers
fileservers
```

## Host Groups Usage
- Functional host groups
  - web
  - lamp
- Regional host groups
  - europe
  - africa
- Staging host groups
  - test
  - development
  - production

## Testing Inventory
- `ansible -i inventory all --list-hosts`
- `ansible -i inventory file --list-hosts`
- `ansible-navigator inventory -i inventory -m stdout --list`