# 16.1 Before you Start: Essential Tips
- All Ansible documentation is available on the exam. Make sure you practice finding the appropriate documentation; it sometimes is well hidden.
- You don't have to use `ansible-navigator`. All assignments can also be done through `ansible-playbook` and other older tools.
- According to Red Hat exam philosophy, it doesn't matter how you do it, as long as you're doing the right thing.
- Avoid non-idempotent behavior as much as you can
- You think better behind your coffe machine. During the exam you may have 3 breaks, use them!
- Read the "essential information" section very carefully; it has essential information.
- Make suer you are very comfortable with the following:
  - Using roles, collections, and requirements files
  - Using conditionals
  - Using block and rescue
- All tasks expect you to create a working solution. If needed, install packages, create users, start services, and more to ensure the playbook solution will work.

# 16.2 Setting up the Environment
## Task 1: Setting up the Environment
- To work through the assignments in this exam, you need the following:
  - 3 virtual machines running either RHEL 9 or CentOS Stream:
    - A control host
    - A node1 host
    - A node2 host
- Add a second disk to node1, not node2
- Install with GUI on the control host and use the minimal installation pattern on the other nodes
- If running RHEL 9, make sure your control host is (manually) registered using subscription manager.
- Ensure that the httpd package is copied to the control host while installing (or after installation)

## Task 1: Solution
```yml
sudo subscription-manager register
sudo subscription-manager attach
sudo dnf repolist
vim /etc/hosts
# add the below to /etc/hosts
192.168.29.199 control.example.com control
192.168.29.191 ansible1.example.com ansible1
192.168.29.192 ansible2.example.com ansible2
# exit /etc/hosts
ssh ansible1
ssh ansible2
```

# 16.3 Configuring the Control Node
# 16.4 Setting up a Repository Server
# 16.5 Setting up Repository Clients
# 16.6 Installing Collections
# 16.7 Generating an /etc/hosts file
# 16.8 Creating a Vault Encrypted File
# 16.9 Creating Users
# 16.10 Creating a Role
# 16.11 Creating a Cron Job
# 16.12 Creating a Logical Volume
# 16.13 Generating a Report