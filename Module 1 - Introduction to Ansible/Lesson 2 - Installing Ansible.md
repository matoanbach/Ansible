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