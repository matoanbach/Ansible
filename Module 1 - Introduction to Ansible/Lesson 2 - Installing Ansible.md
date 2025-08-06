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